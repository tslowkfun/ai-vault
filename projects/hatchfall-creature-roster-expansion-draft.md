---
title: "Hatchfall Creature Roster Expansion — 10 Concepts"
status: draft
owner: "night-drafts-lane-c"
reviewer: "Spark (name collision screen) + Loic (art direction)"
created: 2026-10-16
repo: roblox-games
scope: "Content concepts only. No code, no Config edits, no odds edits."
related:
  - "src/content/shared/Creatures.luau (pack rows, income bands)"
  - "src/ui/shared/OddsData.luau (computeDisclosure reads creature counts)"
  - "docs/designs/phase-2-art-pass.md (ART-C1/C2, one model per id)"
  - "projects/hatchfall-seasonal-calendar-draft.md (limited slot is separate)"
---

# Hatchfall Creature Roster Expansion — 10 Concepts

**Status: DRAFT.** Concepts only. No rows added, no odds published, no names
cleared. Every name below is **UNVERIFIED** and must pass Spark's collision
screen before it appears in any UI, store copy, or ad.

**What this is for:** Phase 2 base-pack variety for Hatchfall, season-neutral.
The Christmas set and every other event set live in their own notes and their own
pool; nothing here touches the seasonal-limited slot, the event calendar, or
`pool` tags.

**What this is not:** no new egg, no new rarity, no new income band, no new
system, no limited creatures, no shop row.

---

## Design rules this batch follows

1. **Season-neutral.** No snow, hearts, eggs-as-Easter, pumpkins, turkeys,
   fireworks, or holiday palettes. Palettes stay inside the existing nursery
   range (pastel + one accent). Any of these can sit in the base pack all year.
2. **Rarity is a silhouette/complexity budget, not a power level.** Common reads
   at distance with one material; each tier up adds one readable feature. This is
   the art-pass rule (ART-C1: silhouettes distinguishable at distance).
3. **No income band extension.** Every proposed `incomePerSecond` sits inside an
   existing band (Common 1, Uncommon 3-4, Rare 10-12, Epic 40-45, Legendary
   150-170). This batch adds variety, not power creep, and does not move the
   economy curve.
4. **No Mythic, no Seasonal Limited.** The spread is Common -> Legendary only, as
   briefed. Existing Mythic rows stay untouched, and the one-limited-per-event
   direct-pick slot is defined in the seasonal calendar note, not here.
5. **Original only.** Invented names, invented silhouettes, no brand, meme,
   real-team, or existing-franchise reference. Names are screened by Spark the
   same way the first sixteen were (six of those collided and were renamed).

---

## The 10 concepts

### Common (2)

| # | Name (UNVERIFIED) | One-line visual | Rarity / income fit |
|---|---|---|---|
| 1 | **Plumdrop** | A plum-round creature with a leaf tuft and stubby legs, built to look like it just rolled off a low ledge. | Common: one shape, one material, reads at distance. Income **1** — same as every existing Common; band floor, no curve change. |
| 2 | **Puddlepod** | A squat droplet body with a translucent belly and two fin-feet, always slightly wet-looking. | Common: single silhouette plus one see-through material. Income **1** — band floor, keeps the stage-2 Common pool uniform in value. |

Why two Commons: the base egg's Common pool is the most-seen pool, and four
creatures repeat quickly in a 10-minute session. Two more Commons add visible
variety at the tier players actually hatch, without adding value.

### Uncommon (2)

| # | Name (UNVERIFIED) | One-line visual | Rarity / income fit |
|---|---|---|---|
| 3 | **Nooklet** | A round shelf-dweller with oversized ears and a tail that curls into a key shape. | Uncommon: one extra readable feature (the key tail) over a Common silhouette. Income **3** — bottom of the existing 3-4 band. |
| 4 | **Quaverpaw** | A long-eared, nervy creature whose pawprints are shaped like music notes (visual only, no audio). | Uncommon: one gimmick feature, still one body. Income **4** — top of the existing band. |

### Rare (2)

| # | Name (UNVERIFIED) | One-line visual | Rarity / income fit |
|---|---|---|---|
| 5 | **Ceruleaf** | A cerulean fawn with fern-shaped antlers and a moss-lined spine. | Rare: two-part read (antlers + spine) and a second material. Income **11** — inside the 10-12 band. |
| 6 | **Mireglass** | A translucent swamp-green newt with glassy frills that catch the light. | Rare: transparency plus frill geometry; the most "material" of the Rares. Income **12** — top of the band. |

### Epic (2)

| # | Name (UNVERIFIED) | One-line visual | Rarity / income fit |
|---|---|---|---|
| 7 | **Tempestrel** | A storm-grey kestrel-like creature with lightning-shaped crest feathers and a ragged wing edge. | Epic: layered crest + wing silhouette, reads as a "chase" creature. Income **42** — inside the 40-45 band (40 and 45 already exist). |
| 8 | **Nebulynx** | A star-flecked lynx with a comet tail that trails a thin, slow sparkle. | Epic: needs a subtle particle tail, the first effect in this batch. Income **44** — inside the band, distinct from existing values. |

### Legendary (2)

| # | Name (UNVERIFIED) | One-line visual | Rarity / income fit |
|---|---|---|---|
| 9 | **Asterune** | A deep-indigo, six-eyed guardian with constellation markings across its flank and one crescent horn. | Legendary: full-body signature (six eyes + constellation pattern + horn), one always-on idle effect. Income **155** — inside 150-170, above the entry Legendary. |
| 10 | **Gravitore** | A heavy, floating tortoise with a boulder shell and slow rings of dust orbiting it. | Legendary: silhouette + motion signature (float + orbit rings). Income **165** — near the top of the band, below the 170 cap. |

Legendary art notes: both are meant to be readable as "the good one" from across
the base. The six eyes and the orbiting rings are the two read-at-distance cues;
neither needs a custom animation graph (reuse ART-C2's shared idle-bob).

---

## Income placement vs the existing curve (no band extended)

| Rarity | Existing band | Existing values | New values | Band moved? |
|---|---|---|---|---|
| Common | 1 | 1, 1, 1, 1 | 1, 1 | No |
| Uncommon | 3-4 | 3, 3, 4 | 3, 4 | No |
| Rare | 10-12 | 10, 11, 12 | 11, 12 | No |
| Epic | 40-45 | 40, 45 | 42, 44 | No |
| Legendary | 150-170 | 150, 170 | 155, 165 | No |
| Mythic | 600-650 | 600, 650 | none | No |

The point of the table: a reviewer can see at a glance that this is a variety
drop, not an economy drop. Stage-2 odds do change (below), income does not.

---

## The one real odds effect: stage-2 dilution

No rarity weight changes. `OddsData.compute` is untouched, so the chance of
rolling Common/Uncommon/... is identical. But `HatchRoll.pickCreature` is uniform
within a rarity, and `OddsData.computeDisclosure` computes
`P(creature) = P(rarity) / count(rarity)`. Adding rows therefore lowers **each
existing creature's** stage-2 share in that rarity:

| Rarity | Count before -> after | Each creature's share of its rarity, before -> after |
|---|---|---|
| Common | 4 -> 6 | 25.0% -> 16.7% |
| Uncommon | 3 -> 5 | 33.3% -> 20.0% |
| Rare | 3 -> 5 | 33.3% -> 20.0% |
| Epic | 2 -> 4 | 50.0% -> 25.0% |
| Legendary | 2 -> 4 | 50.0% -> 25.0% |

This is a real, disclosed odds change even though no weight moved, and the modal
will show the new numbers automatically because it reads the live creature table.
Two consequences:

- **Ship the batch as one content update.** Adding creatures piecemeal changes
  the disclosed percentages repeatedly for no player benefit. One drop = one
  disclosure change.
- **Do not "fix" dilution by weighting creatures individually.** Uniform
  within-rarity is the disclosed model (`computeDisclosure`). Per-creature
  weights would be a second odds table and a policy change, not a content drop.

If Spark wants the dilution to be smaller, the lever is fewer creatures per
rarity or a larger existing pool, not hidden weights.

---

## Inventory-only: surfaces a new creature touches

Nothing below is a change request in this note; it is the checklist of what a
content drop will touch when it is scheduled.

### Must change (data only)

- **`src/content/shared/Creatures.luau`** — one row per creature in
  `MYTHIC_HATCHERY[rarity]`: `{ id, displayName, rarity, incomePerSecond }`.
  `id` is the stable save key (lowercase, no spaces); `displayName` is display
  only. The pack is `mythic-hatchery`; `glitchlings` is Game 2 and stays empty.
- **`docs/designs/phase-2-art-pass.md`** — one ART-C1 row per new id (model
  brief), reusing the ART-C2 shared idle-bob kit. No per-creature animation
  graph, no per-creature VFX beyond the two noted (Nebulynx tail, Asterune idle).
  This is the real production cost: **10 models**, not 10 code changes.

### Must be checked, not edited (derived)

- **`src/ui/shared/OddsData.luau`** — no table edit. `computeDisclosure` already
  takes the creature table; stage-2 rows update from the count. The only risk is
  a test fixture that pins absolute per-creature percentages: update the fixture,
  not the policy. `OddsData.Eggs` weights are untouched.
- **`src/ui/client/OddsModal.luau`** — no code change expected; the stage-2 list
  grows by two rows in each of the five affected rarities (Common 4 -> 6 is the
  tallest list). Check the tallest list on a phone (scroll list already exists).
- **Claim reveal (`Hud.showClaimResult`)** — resolves `displayName`; no code
  change. Check the longest new name fits the reveal label (all are single words).
- **Base/placement cards** — read the record; no code change. If a card truncates
  names, truncation is preferred over shrinking the font.

### Must NOT change (deliberately untouched)

- **`src/world/shared/CarryVisual.luau`** — the carried egg is TIER ONLY (Q37).
  It must never read the creature or rarity, so a new creature is invisible on a
  carrier. No change.
- **Finalize / placement / steal / lock** — all keyed by `uid` and `creatureId`;
  new ids need no logic. A placed creature resolves its model from the art
  registry by id; a missing model falls back to the existing placeholder glow,
  so content can land before art.
- **`src/data/shared/Template.luau` / `DataSchemaVersion`** — `creatures` rows
  already carry `creatureId`; no migration. New ids never need a schema bump.
- **`Config.luau`** — no new egg, no price, no boost, no product. No `pool` tag
  either: these are base-pack rows, not event rows. The seasonal pool filter
  (calendar note NEW-2) is not needed for this batch.
- **`OddsData.Rarity` / egg weights** — no new rarity and no weight change.

### Name and originality gate

1. Spark runs the collision screen on all 10 names (same process as the first
   sixteen; six were renamed).
2. Any rename must change `id` **and** `displayName` **before** the ids ship.
   After ship, ids are never renamed or deleted (saved records reference them);
   a post-ship rename would be a display-only alias, which is a migration task.
3. No name is used in store copy, ads, or thumbnails until step 1 clears and Loic
   signs off the art direction.

### Test surface

- Content test: every rarity used by an egg has at least one creature (already
  true; adding rows cannot break it).
- Disclosure test: displayed stage-2 chances equal `P(rarity)/count` for the live
  pack (the existing pattern); count changes are expected, so assert the formula,
  not hardcoded percentages.
- No new tests are required for income: values are data and sit inside existing
  bands, which is exactly why this batch avoids band extension.

---

## Open items for Spark / Loic

1. **Name clearance** for all 10 (UNVERIFIED as of this draft).
2. **Art budget:** 10 models in one drop vs two waves of 5. Two waves halve the
   per-wave dilution but change disclosed stage-2 odds twice.
3. **Dilution call:** accept the table above, or reduce the batch to 8. Note
   that 8 = two per rarity for Common-Epic and zero Legendary, which breaks the
   Common -> Legendary spread the brief asks for; 10 (two per rarity, five
   rarities) is the cleanest way to satisfy "8-10 across Common to Legendary".
4. **Palette check:** confirm Ceruleaf and Mireglass don't collide with the
   seasonal palettes planned for Spring/Halloween; if they do, shift the accents,
   not the concepts.
5. **Legendary effect budget:** Asterune's idle and Gravitore's rings must be in
   the always-on cut-first list on mobile, same as the rebirth aura (ART-C3).
