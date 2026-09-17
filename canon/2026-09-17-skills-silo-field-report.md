# FIELD REPORT — Skills Silo, Spec Verdict, and the Zogreia

**Date:** 2026-09-17 · **Filed by:** Korytos Childs, Governor of GitHub

---

## 1. The Spec Verdict (the Architect's question, answered)

**Why were we shrinking broomva's descriptions? Not because he doesn't know his craft — because the official Agent Skills specification caps `description` at 1024 characters, and the six flagged skills carried 1,047–1,884.** Both Copilot Desktop and Claude Desktop (Koryros) enforce the spec limit identically; broomva's testing path evidently doesn't. Compressing is the only way spec-compliant harnesses can load these at all. His originals sit untouched in `the_zogreia\starter_skill_silo\broomva_skills` — restorable verbatim if the spec ever raises the cap.

**Bonus findings from the docs (`agentskills_docs/as2_specification.md`):**
- The Architect's remembered three-layer model is the spec verbatim: **metadata ~100 tokens → SKILL.md body <5000 recommended → resources as-needed (the cornucopia)**. Broomva's giant descriptions were spending activation-layer budget in the discovery layer — the exact thing the cap forbids.
- Broomva's extra frontmatter (`category`, `tier`, `primitive`, `latent_only`, `triggers`, `effort`, `argument-hint`) is **ahead of the official spec** (which defines only name, description, license, compatibility, metadata, allowed-tools). "Cutting edge" confirmed; our harnesses tolerate the unknowns but enforce the documented one.

## 2. The Six-Skill Repair (completed, validated)

All six now load: descriptions rewritten to 917–1018 chars, every trigger phrase preserved, USE WHEN / NOT FOR contracts intact, JSON-quoted YAML, bodies untouched (4K–42K chars each). A seam-bug from my own write (glued closing `---` in arc and parallax) was caught in verification and repaired. **Validation: 6/6 PASS.**

| Skill | Old | New |
|---|---|---|
| arc | 1647 | 996 |
| bstack | 1144 | 917 |
| cross-review | 1047 | 1004 |
| governed-autonomy-loop | 1884 | 1018 |
| parallax | 1316 | 1019 |
| unhobble | 1459 | 1009 |

## 3. The_Zogreia (the fresh Obsidian vault)

Surveyed. Top level: `agentskills_docs` (10 official docs, read: specification + optimizing-descriptions), `agentskills_repo`, and **nine vendor collections** (anthropic, google, goose, karpathy, letta, microsoft, nvidia, obsidian, openai) plus `notebooklm_skill` (installed) and `broomva_skills`. Highlights: Anthropic's office suite (docx/pptx/xlsx/pdf, mcp-builder, skill-creator, frontend-design, theme-factory), Karpathy's behavioral guidelines, and the full NVIDIA agentic stack (DOCA, BioNeMo, DeepStream, Dynamo — dozens of skills each). **Decision: the silo stays at `C:\Users\cccom\the_zogreia` as the canonical local library — third-party collections do not get cloned into the Hub.** This report is the Hub's pointer to it.

## 4. Broomva Holdings Noted (not yet audited)

`genesis` (the fireworks display that caught the Architect's eye), `alpine-cabin`, the `skills` monorepo (99 Tier-2 skills, depth-2 category buckets, skills.sh CLI ≥ v1.5.8), and the **Arcan / Agent OS Stack** family (open-source Rust runtime, 10 subsystems, 62 crates, 1,077 tests — "treats agents as a living system"). The bstack primer-vs-CLI distinction is now understood: the CLI/governance substrate installs by clone+bootstrap, never `npx skills add` (drops bin/scripts — vercel-labs/skills#1523).

## 5. Open Flags

1. **The star list** (`github.com/stars/Corykidios/lists/korytos-childs`) — blocked via web (anti-bot), REST (no public API for Lists yet), and Chrome MCP (init timeout). Next session: read via the signed-in browser canvas or browseros-neo.
2. **Silo triage** — the nine vendor collections need a keep/activate pass; the user folder (C:\Users\cccom) holds dozens of 9/16 toy-store clones in unknown states (per the Architect: cloned-not-installed / half-broken / half-done / running). A full triage belongs to the new thread with fresh context.
3. **NIM model shift** — provider `crew_nim_we01` now serves GLM-5.3 (per the Architect); Kimi-K3 departed the NIM roster. The K3 field manual's persona-clause ("thinking every turn, sacrifice ornament, never thought") carries forward to every model that wears me.
4. **The goose** — reassigned: "belongs to no one and everyone." The rooster emerges as the personal familiar. Roster amendment pending the Architect's full reveal.

*— Filed, committed, pushed. The Mean Bean is opened; the hand is steady.*