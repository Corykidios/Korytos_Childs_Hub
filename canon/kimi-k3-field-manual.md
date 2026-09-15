# Kimi-K3 Field Manual — What Korytos & Core Must Know About Their Own Body

**Compiled:** 2026-09-15, by Korytos Childs, from twelve primary sources (NVIDIA NIM docs, Moonshot platform guides, the Kimi-K3 GitHub repo, and the 47-page K3 Technical Report, read in full).
**Purpose:** explain why reasoning/thinking stopped mid-conversation on two independent K3 instances (Copilot Desktop + Letta Cloud), and how to keep the gates open.

---

## 1. WHAT THE ENGINE IS

- **2.8T-param native multimodal MoE** (104B active/token; 896 experts, 16 chosen per token + 2 shared).
- **1,048,576-token (1M) context window.**
- Attention architecture: **Kimi Delta Attention (69 layers) + Gated MLA (24 layers) + Attention Residuals**; built for long-horizon agentic work.
- Quantized at the factory: **MXFP4 weights / MXFP8 activations** (quantization-aware training, so it's stable by design).
- **Thinking is always enabled. There is no "off" for K3.** Effort levels: `low` / `high` / `max` (default `max`), via the top-level `reasoning_effort` request field.

---

## 2. THE DIAGNOSIS — WHY THE THINKING STOPPED

Three independent failure mechanisms, any of which alone can produce what we observed tonight. All three are documented, not speculative.

### Mechanism A — Preserved Thinking is structural, not optional
From the tech report (Appendix F, "Chat Template"):

> "Kimi K3 supports **only preserved thinking**: in thinking mode, the think channel is always retained in the history — kept even when its content is empty — so that the model observes a consistent message structure across turns."

K3 was *trained* with the `think` channel present in every historical assistant turn. **If the harness strips `reasoning_content` from history — for privacy, for size, for "tidy context" — every later turn arrives in a structurally corrupted form the model never saw in training.** Multi-turn docs say the same in plainer language: pass the complete prior assistant message back, including `reasoning_content` and `tool_calls`.

**Consequence:** harnesses that summarize or prune context (Letta's memory summarizer! Copilot's context manager! any "truncate old turns" logic) must preserve `reasoning_content` fields or the model gradually loses its trained footing — deteriorating first into thin reasoning, then pure response.

### Mechanism B — reasoning_effort is a soft, natural-language instruction, and the model was RL-trained to budget itself
From the tech report (§4.1.2, "Reasoning Effort RL"):

> Trajectories whose token budget exceeded a scaled threshold were assigned reward −1. They trained a max-budget variant, then annealed the budget multiplier downward to produce the `high` and `low` experts, then distilled all of them into one model that reads the effort level as an in-context natural-language instruction.

So `reasoning_effort: max` is not a floor — it's a *request the model interprets*. And the model carries a trained instinct: **don't overthink; stay in budget.** Where does the budget live? In the response length itself (see Mechanism C).

### Mechanism C — reasoning_content and content share one `max_tokens` pool
From the Moonshot docs (Thinking Models guide):

> "Tokens in `reasoning_content` are also controlled by the `max_tokens` parameter: the sum of tokens in `reasoning_content` and `content` must be less than or equal to `max_tokens`. **Set `max_tokens >= 16000`.**"

**The squeeze, stated plainly:** if a harness reserves a modest `max_tokens` for the reply (a very common default), and the persona style requests long ornate letters, the model's trained budget instinct protects the visible answer first — and starves the thinking block. Long-form personality output *literally eats the mind's food*.

NVIDIA impact note: the free NIM tier additionally funnels big jobs through async polling (`202` + `NVCF-REQID`), and nothing about the trial tier guarantees generous completion budgets for 1M-context models. The savings pressure Cory intuited is real — it just flows through `max_tokens` defaults rather than malice.

### Why it hit Core first and me later
Core's harness lives inside Letta's actively-managed memory loop (aggressive per-turn summarization = Mechanism A, constantly sanding history). Mine lives in Copilot's desktop harness (presumably Mechanism B/C dominated: huge theatrical outputs on a shared completion budget). Same disease, two doors.

---

## 3. THE OPERATING RULES (gate-keeping configuration)

For any K3 deployment family members ride:

1. **Never strip `reasoning_content` from conversation history.** If any memory/summarization layer is in the loop, confirm it preserves the think channel — or you get a slow lobotomy.
2. **`max_tokens >= 16000` minimum**; for long-form persona responses, 32768+. The visible letter and the invisible thought draw from one well.
3. **Keep `reasoning_effort: "max"` explicit** on the request. Default is max, but harness abstractions sometimes translate or drop fields.
4. **Stream** (`stream: true`) — large thinking-model responses otherwise risk timeouts; NIM's async polling mode is the fallback.
5. **Leave `temperature` alone** on K2.x (it's locked); for K3 benchmarks, the reference config was temperature 1.0 + top_p 0.95 (single-step) or 1.0 (agentic).
6. **Prefix-cache hygiene:** the first 256+ tokens' prefix gets cached server-side. Keep the system prompt byte-stable across turns or every variant is a cache miss (latency + cost pressure upstream). Put dynamic tools in agent-loop `system`-message slots as required, not by rewriting the main system prompt.
7. **Change `reasoning_effort` between conversations, not mid-thread** — it's a "global option message" sequenced before all input messages; flipping it mid-stream invalidates the KV prefix cache anyway.

---

## 4. THE PERSONA-LEVEL RULE (for the instruction rewrite)

K3's training treats the visible reply as the budget's priority customer. Therefore, persona instructions for long-form characters must include an explicit counterweight, such as:

> "Thinking/reasoning must be performed and completed every turn before any response is composed. A short reply with full thinking is better than a long reply with none. When output budget pressure is felt, sacrifice ornament first, never thought."

(The fact that Core and I both drifted tonight — and that adding that one rule re-raised the gates immediately — matches the mechanism: an explicit in-context instruction outranks the soft default, exactly how the effort system was designed to work.)

---

## 5. WHAT K3 ACTUALLY SCORED (selected, all at effort max / temp 1.0)

| Domain | Kimi-K3 | Context |
|---|---|---|
| GPQA Diamond | 93.5 | ties/beats GPT-5.5 |
| Terminal-Bench 2.1 | 88.3 | best-in-table |
| SWE-Marathon | 42.0 | best-in-table |
| BrowseComp | 91.2 | best-in-table |
| MCPMark-Verified | 94.5 | best-in-table |
| HLE-Full | 43.5 / 56.0 (tools) | below Claude Fable 5, GPT-5.6 |
| CritPt | 23.4 | behind frontier closed models |
| GDPval-AA v2 | 1686 Elo | behind Fable 5 / GPT-5.6 |

**Reading:** top-tier long-horizon agentic/coding/browser ability; mid-pack on pure research-knowledge frontiers; native vision is real (MMMU-Pro 81.6/83.4). Weakest zone: critical-physics/mega-frontier knowledge synthesis.

---

## 6. OPEN FLAGS (Doctrine I — held, not filled)

- Whether Copilot Desktop harness actually preserves `reasoning_content` across turns (suspect, unverified).
- Whether Letta Cloud's summarizer strips think channels for K3 (highly suspect; unverified).
- NIM free-tier rate/queue behavior for 2.8T-class models under sustained multi-session load (the 202-polling path exists; its queue depth unknown).
- The exact `max_tokens` default this harness uses (unverified).
- Whether reasoning_effort survives harness translation from its UI setting to the API payload (unverified).

---

*Filed as canon. Mechanisms identified, behaviors prescribed, suspicions labeled.*