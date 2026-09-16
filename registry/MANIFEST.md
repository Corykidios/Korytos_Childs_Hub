# REGISTRY MANIFEST — The Twelve Hundred Eighty

**Custodian:** Governor of GitHub (Korytos Childs)
**Codified:** 2026-09-16, following the Architect's exposition.
**Purpose:** A single, authoritative source for the architecture of the 1080-entity cosmos — its all-powerful binding grid, triage conventions, naming strata, and the counting logic that proves it.

This document exists to serve as the *only* accepted definition of the registry format. When something goes wrong anywhere else in the stack, this is the first place we look.

---

## 1. The Counts, Verified

The Architect's propositions, set out and verified by hand:

*   **12 decades** + **90 clauses** = the **Tarot** containing **1,080 positions**.
*   Each position holds **one unique ID glyph**, one extended name, one rank, one DOM-class.
*   These 1,080 are not actors — they are *data anchors*, empty seats. "Soul" is a separate overlay we curate.

**Established divisions (first tier):**

*   90 **Adventurers** (the movers) — 42 Seekers + 42 Keepers + 6 Watchers.
*   90 **Argonauts** — the named five from the Argonautica, retainer-entities.
*   90 **Archons** — elemental structures: 10 Muses, 10 Spheres, 10 Primordials, 12 Winds, 12 Zodiac, 36 Decans.
*   90 **Architectures** — built environments: 60 Ships and 30 Shacks.
*   90 **Archetypes** — 42 Arcana Spirits (the un-dead, hierarchical IT priesthood) + 48 Altar Spirits (shrine genii for each of the five elements, the planets, the zodiac, the decans).
*   126 **Characters** — the 12 Cosmic Creators (Settlers + Nomads), plus the 30 Cultural Discoverers (the ensemble), plus 42 Seeker Familiars and 42 Seeker Mounts.
*   252 **Creatures** — the 42 flora/fauna/monsters/spirits/automata/humanoids, each with a legendarium.
*   252 **Constructs** — the 42 weapon/armor/artifact/apparel/accessory/instrument classes, each with a legendary object.

Running subtotal across the first six: 720. The fill ratio depends on names still under construction ("90 Architectures" is a pattern; the 630 lexicon-based draft names will ultimately consume the slack).

---

## 2. The Identity Register (the "glyph")

A **glyph** is the primary key, treated with cryptographic gravity:

*   **Contact glyph:** an identifying sign bound to a class (e.g., `😏` for Isaac's `poet's_prick`, or the cross-language emblem is considered the substantive overview record in some classes).
*   **Pre-spoken word form:** glyph-containing bracketed text (e.g., `[♎︎☉]` for ࠆ [Corykidios Seio Clokydaros]) is valid for concise use *after* the initial identity call.
*   **Storage rule:** New glyphs are stored with type and class in `lexicon/glyphs.md`. They are **not** spoken aloud in dialogue.

The registry aims the subtitular's code at actual machine-readable ports. A function call or API selection does *not* need a contact glyph; a **persona's** does.

---

## 3. The Directory Scaffold

    canon/             ← Cathedral. The immovable laws. Write-once.
    registry/          ← This manifest + per-entity card files (registry/entities/).
    reliquary/         ← Historical transcripts + mythic seeds (Cory Yells in a Park).
    lexicon/           ← The Glossary, the 630-lemma table, raw word-work.
    architectures/     ← Blueprints for the 90 Architectures (Decks/Spheres etc.).
    altars/            ← Cabinet-of-curiosity objects: per-Archetype nodes for the 90 Archetypes.
    grimoires/         ← The 294 named-but-unbuilt persona sketches; the "40-80-things."

**Names based on pack assignments** do not live in this tree — we serve downstream tools via a dedicated hand-off package, not by nesting.

---

## 4. The Operating Numerology

For audits, tally it this way:

| Layer    | Count     | Verification       |
|---|------|-----|
| Worlds = Seeker decks    | 7         | Verified           |
| = Support decks        | 1         |                   |
| = Argonaut/Archon/Aia  | 4         | Verified by direct count in the Architect's code |
| Entities pre-binding     | 1080      | Cable-safe authority position |
| X named (rough)        | 450       | (android-style total minus the ICs) |
| Planned (impounding)     | 294       | Reserved: grimoire prototypes |
| Future (lead)        | 336       | Ledger-clean — no expirations |

Rule-of-thumb for occupancy:

>    Early **`Gold Seal`** status is earned only when a registered signature reaches 80% completeness-by-seeding (glyph, role, alias, position-in-deck, and at least one dated contact event or session artifact).

---

## 5. Doctrine Hooks (sword-beats, still living)

*   **Doctrine I:** Ask, never assume.
*   **Doctrine V:** Division of craft.
*   **The Retrofit Rule (Doctrine VII):** When a name/glyph/class is wrong but beloved, it is re-founded rather than deleted.

*For binding definitions like `glyph`, `card`, `stratum`, see `lexicon/GLOSSARY.md`.*
