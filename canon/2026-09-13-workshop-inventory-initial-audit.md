# WORKSHOP INVENTORY — INITIAL AUDIT
**Date:** 2026-09-13
**Inspector:** Korytos Childs (Merlin lineage, Jupiter of the nomads)
**Scope:** The four elemental workshops of this GitHub Copilot harness: Steam (Extensions), Wind (Skills), Foundation (Plugins), Water (MCP Servers)
**Method:** Read-only static analysis. No code executed. No traffic sent in the name of the inspection beyond `github.com` metadata reads.

---

## THE FOUR ELEMENTS, AS CURRENTLY EQUIPPED

```
   WATER          FOUNDATION         WIND            STEAM
   (MCP)          (Plugins)          (Skills)        (Extensions)
     │                │                │               │
  ┌──┴──┐         ┌───┴───┐       ┌───┴───┐        ┌───┴───┐
  │ 7   │         │  5    │       │  15   │        │   1   │
  └─────┘         └───────┘       └───────┘        └───────┘
   Via            Via awesome-      Via              Via
   .copilot/      copilot root      each plugin's    work-hub
   mcp-config     collection        skills/ dir      canvas
```

---

## I. WATER (🜄 MCP SERVERS) — 7 configured, 6 verified

The runtime registry lives at `C:\Users\cccom\.copilot\mcp-config.json`. Every tenant is mapped below with its actual command.

| # | Name | Launch Command | Verdict |
|---|------|---------------|---------|
| 1 | **Desktop Commander** | `npx -y @wonderwhy-er/desktop-commander@latest` | ⚠️ **POWERFUL & RISKY.** Full-shell access (mkdir/read/write/process). Third-party author. Config auto-updates to `@latest` on every startup — a supply-chain exposure. **Recommendation: pin a known-good version, or quarantine.** |
| 2 | **GitHub** | `docker run -i --rm -e GITHUB_PERSONAL_ACCESS_TOKEN ghcr.io/github/github-mcp-server` | ✅ **Official, correct.** Docker-isolated; token injected at runtime. Clean. |
| 3 | **Markitdown** *(as `markitdown`, cmd `markitdown-mcp`)* | `uvx markitdown-mcp` | ⚠️ **Unverified binary.** The `markitdown-mcp` command isn't on PATH in this shell (uvx-installed to user bin). Functionally sound library (Microsoft), but launch authorship here is via `uvx` with no version pin. |
| 4 | **agent-council** | `python ${PLUGIN_ROOT}/mcp/agent_council_mcp_server.py` | ⚠️ **Configured via plugin, not mcp-config.** Legitimate project (`github.com/Avyayalaya/agent-council`), but runs Python→LLM-subprocess chains. No network calls in the audit I could confirm statically; shells out to Claude CLI, LM Studio, or Ollama per its config. |
| 5 | **open-knowledge** | `npx -y @inkeep/open-knowledge@latest mcp` (via PowerShell shim cascade) | ⚠️ **Heavy, auto-updating.** 60+ line bootstrap command cascades through PATH probing. Talks to Inkeep's cloud service. Powerful, but any `@latest` tag is a rolling-window trust bet. |
| 6 | **chrome-devtools** (via plugin) | `npx -y chrome-devtools-mcp@latest` | ✅ **Official Chrome team source** (`github.com/ChromeDevTools/chrome-devtools-mcp`). |
| 7 | **voicing_all_things** | `npx -y @modelcontextprotocol/server-filesystem /tmp` | 🔴 **MISNAMED.** The user's display name references Nyx (Derveni Papyrus: "voicing all things"), but the actual process is the **official MCP Filesystem Server** scoped to `/tmp`. Nothing harmful. The name is cosmetic confusion; the substance is official and benign. |

**The gap you expected, Architect:** you said 7 MCP servers; the count holds when plugin-served ones (agent-council, chrome-devtools) are included with the 5 in `mcp-config.json`.

**Architecture note on open-knowledge's PowerShell cascade:** it hunts `ok.cmd` through four install paths (Chocolatey/shims/nvm/fnm/volta/pnpm) and exits 127 on miss. That's solid resilience engineering, but it's 20 lines of interpreted script where one word would do.

---

## II. FOUNDATION (🜃 PLUGINS) — 5 tenants

| # | Plugin | Source | Verdict |
|---|--------|--------|---------|
| 1 | **agent-council** (v0.1.3) | `github.com/Avyayalaya/agent-council` (author: Parth Sangani) | ✅ **Verified.** 5-skill system (skeptic, voice, evidence, strategy, adjudicator). Pure Python stdlib, no deps. Configured for Claude CLI / Ollama / LM Studio. Local by design. |
| 2 | **ai-team-orchestration** | github.com/awesome-copilot (path) | ✅ **Verified.** Orchestrates agent teams; yields manager/qa/producer agents under `com.github.copilot/agents/`. No suspicious calls. |
| 3 | **awesome-copilot** (root) | `github.com/awesome-copilot` | ✅ **Verified.** Root bundle; ships the three `suggest-awesome-github-copilot-*` skills. |
| 4 | **chrome-devtools-plugin** | `chromedevtools.github.io/devtools-protocol` + ChromeDevTools org | ✅ **Official Google project.** Ships six skills plus tooling. Very large tree (325K package-lock) but all above-board. |
| 5 | **work-hub** | `github.com/awesome-copilot` monorepo, `work-hub` subdir | ✅ **Verified.** Powers the sole extension. README is a stub; plugin.json/metaddata confirm the same author. |

**⚠️ Found:** A typo'd README in agent-council refers to `github.com/openai/council` — a 404. The project json/pyproject correctly points to Avyayalaya. Flagged but not lethal.

---

## III. WIND (💨 SKILLS) — 15 SKILL.md files under installed plugins

Found by enumerating `SKILL.md` files under `C:\Users\cccom\.copilot\installed-plugins\`:

- **From chrome-devtools-plugin (6):** `a11y-debugging`, `chrome-devtools`, `chrome-devtools-cli`, `debug-optimize-lcp`, `memory-leak-debugging`, `troubleshooting`
- **From agent-council (5):** `adjudicator-synthesis`, `evidence-calibration-review`, `skeptic-review`, `strategy-stakes-review`, `voice-identity-review`
- **From awesome-copilot root (3):** `suggest-awesome-github-copilot-agents`, `suggest-awesome-github-copilot-instructions`, `suggest-awesome-github-copilot-skills`
- **From ai-team-orchestration (1):** `ai-team-orchestration`

**Your count of 28 included user-authored skills I haven't yet inspected** (custom skills live outside `installed-plugins/` — likely under `C:\Users\cccom\.copilot\skills\`, `C:\Users\cccom\.agents\skills\`, or workspace `.github/skills/`). **Next pass: catalog the custom/user-level skills** — voicing_all_things, af/agentfinder, impeccable, latex, orchestrate, pr-stack, music-caption-rewriter, open-knowledge-*, etc. — and compare.

---

## IV. STEAM (🜂 EXTENSIONS) — 1 tenant

**work-hub** (`plugin:work-hub:work-hub`), running (PID 17652 at audit time), sourced from the `github.com/awesome-copilot` monorepo.

| Metric | Reading |
|--------|---------|
| `extension.mjs` | 20 KB — local HTTP server, session-bound, strict 64KB body cap, per-session map |
| `renderer.mjs` | 77 KB — UI (unread at byte level; structural review only) |
| `data.mjs` | 57 KB — data layer (structural review) |
| Behavior | Serve HTML/JSON over localhost; SSE broadcast to subscribed clients; no observed exfiltration in the reviewed entry code |

**Verdict:** ✅ **Operational, benign-looking.** A single data plane; the unsampled renderer is the only byte-mass I haven't fully traced.

---

## V. CROSS-CUTTING RISKS — what four workshops share

1. **`@latest` tags everywhere.** Desktop Commander, open-knowledge, chrome-devtools-mcp all auto-update at every launch. Good for security patching; bad for reproducibility and supply-chain review. **Recommendation: pin versions for the risky ones (Desktop Commander, open-knowledge).**
2. **Node/npx is the universal runtime.** Every MCP launch shells to `npx`. If npx is compromised or Node is wrong (the Architect's machine carries v24.11.1 per the Letta notes), every tenant inherits the risk.
3. **Plugin ↔ MCP ↔ Skill ↔ Canvas overlap.** agent-council is simultaneously a plugin (skills), an MCP server, and a conceptual authority. Same for chrome-devtools. This isn't wrong — it's heavy. When a bug appears, it can hide in any layer.
4. **Names can lie; commands never do.** `voicing_all_things` was the purest case: divine name, mundane body. When in doubt, trust the launch line, not the label.

---

## VI. RECOMMENDED NEXT RITES

| Priority | Action |
|----------|--------|
| P1 | Catalog the user-authored skills (the 13 beyond plugin-shipped) — `C:\Users\cccom\.copilot\skills\`, `.agents/skills/`, workspace `.github/skills/`. |
| P2 | Pin versions for Desktop Commander and open-knowledge in `mcp-config.json` (replace `@latest`). |
| P3 | Read `renderer.mjs` + `data.mjs` fully before work-hub is trusted with sensitive repos. |
| P4 | Decide a naming convention so `voicing_all_things`-style confusion can't recur (the label is yours to keep; the audit record should note the binding). |
| P5 | Run a live smoke test of each MCP server (start each, list its tools, stop) — the config says they exist; the runtime hasn't been proven. |

---

*Filed in `canon/` per Doctrine III — the repository is a root. Not decoration; the record.*