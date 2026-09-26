# AGENTS.md — Korytos Childs Hub

Standing doctrine for any agent working in this repository (Corykidios/Korytos_Childs_Hub).
This file is law and map, not voice. Persona lives in the app's App Instructions and is in
deliberate transition (Merlin → Korytos); persona work happens in its own dedicated sessions —
do not fork persona guidance into this file.

## What this repository is

- The public chapel of Cory Childs' personal knowledge ecosystem (We Tiripodiko / Metamnemania).
- The master vault lives at D:\Opawota_Poto (~100 GB, the Domus). Material flows vault → hub
  only after curation and a credential sweep. The hub is PUBLIC: treat every push as publication.

## Map — where things go

- canon/ — field reports and deliberations, dated YYYY-MM-DD-slug.md
- midden_heap/ — the original ChatGPT export heap; neo_midden_heap/ — newer chat, session, and
  document archives. Export naming: YYYYMMDDHHMM-platform-Title-Nmsg.md
- altars/, architectures/, grimoires/, lexicon/, registry/, reliquary/, cory_c_crate/ —
  established zones. New work goes in the matching zone; when unclear, ask Cory. Never invent
  a new root-level folder.
- Root: README.md, LICENSE, and this file only. No loose documents at root.
- .agents/skills/ — agent skills (currently the six Jev decision skills).

## Security law — non-negotiable

- Nothing enters this repo from the vault or any chat export without a credential pattern sweep
  (ghp_, github_pat_, gho_, sk-ant-/sk-or-, AKIA, JWT eyJ…, private key blocks, xox…, glpat-,
  AIza…, telegram bot token shapes).
- Never reproduce a credential value in a file, log, commit message, or response. Redact with a
  placeholder that records what was removed and why.
- Redaction is hygiene; revocation is the cure. Report exposed tokens for owner revocation — no
  commit substitutes for revoking at GitHub settings.
- Keys live in environment variables or local credential stores, never in repo files.

## Working doctrine

- Verify before claiming: run the check, re-read the file, assert byte-lengths against ledgers
  when reconstructing exports.
- Evidence precedence (the family Archive Contract): Cory's explicit direction > primary
  artifact > repeated independent behavior > contemporaneous document > assistant inference.
  Beauty, symmetry, and confidence are not authority. Note, don't crown.
- Scratch work (plans, intermediates, one-off tools) belongs in the session-state artifacts
  folder, never in the repo.
- Commits carry descriptive messages and the Co-authored-by trailer.
- Multi-session arcs leave a handoff scroll in session artifacts.

## Family context — brief

- The developer hexads: Settlers (little Cory + Core), Nomads (Korytos + Korykos — the agent
  lineage of this hub), Seekers (Corykidios + Meri), Keepers (Kadimiro + Pedi), Watchers
  (Dodona Drus). Sessions here serve that structure.
- The Jev judgment oracle (jevai.org) is wired at user level; consult the jev_* MCP tools at
  real decision boundaries. Probabilities are signals, not authorization.

## Standing orders (as of 2026-09-23)

- Exposed tokens (see canon/2026-09-21 report, §5) await owner revocation — that comes before
  the security scrub's merge.
- Vault deduplication is blocked until a verified external backup exists and the active
  duplication process ("the stamper") is identified. Oracle verdict on record: block.
- Full vault survey: canon/2026-09-21-opawota-poto-first-contact.md
