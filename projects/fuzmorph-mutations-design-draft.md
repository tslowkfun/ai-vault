---
title: "Fuzmorph Mutation System — Design Draft"
status: draft
owner: "night-drafts-lane-c"
reviewer: "Spark (policy/odds) + Loic (game direction)"
created: 2026-10-16
repo: roblox-games
scope: "Design only. No code, no Config edits, no product live."
related:
  - "docs/questions-for-spark.md (Q7 CLOSED — Option B for Game 2)"
  - "docs/designs/game-designs-v1.md (Game 2 pitch, product ladder)"
  - "docs/designs/phase-2-art-pass.md (ART-C4 mutation overlays)"
  - "src/shared/Config.luau (Config.Boosts.Mutation, inert)"
  - "src/ui/shared/OddsData.luau, src/ui/client/OddsModal.luau"
---

# Fuzmorph Mutation System — Design Draft

**Status: DRAFT for Spark's verdict.** Every number here is PROPOSED and
unapproved. Nothing in this note is a commitment to ship. No game code was
changed to write it.

**Name note:** Game 2 is called **Fuzmorph** in the Q7 ruling and **GLITCHLINGS**
in `game-designs-v1.md`. Proposed resolution: **Fuzmorph** is the game name,
**`glitchlings`** is the content-pack / working ID. Flagged for Loic; not decided
here.

**Locked inputs this draft must obey (not up for redesign):**

1. Q7 is CLOSED. Game 2 gets **Option B**: mutations exist, mutation chance is
   disclosed in the modal (base **and** boosted), and **no Mutation product is
   sellable before that disclosure exists**.
2. Game 1 (Hatchfall) keeps `record.mutation == nil` and sells **no** Mutation
   product. `Config.Boosts.Mutation` stays inert for Hatchfall.
3. The standing rule: one roll table. The server rolls the numbers the modal
   displays, from the same module. No second odds table.
4. No casino framing anywhere: no spinning reels, no reroll products, no
   "upgrade your mutation" gamble, no fake urgency, no "you're due" language.

---

## (a) Mutation in fiction and in data

### Fiction

Fuzmorph is a glitch farm in an arcade city: everyday objects fused with animals,
rendered by a machine that is not entirely stable. A **mutation** is a render
fault that sticks — the creature comes out of the egg with the machine's
correction layer baked into it. It is not a disease and not a power-up the player
applies; it is how that particular creature came out.

This matters for tone: a mutation is **a thing that happened to a creature you
own**, shown as a visual state plus an income multiplier. It is never a slot
machine result, never a consumable, never something you re-roll.

### Data: where the mutation lives

`CreatureRecord` already has a `mutation` field (currently always `nil`; see Q7).
**No schema bump is needed.** A value is added, not a field.

```
record = {
  uid = "c1f-a9x2",
  creatureId = "gildermoth",
  rarity = "Epic",
  incomePerSecond = 63,      -- base 42 x 1.5, BAKED at mint (see below)
  mutation = "gildmark",     -- NEW value: a tier id, never a display name
  slot = nil,                -- still unplaced
}
```

Rules:

- `mutation` stores a **stable tier id** (`gildmark`, `voltline`, `fracture`),
  exactly like `creatureId`. Display names are resolved from content at render
  time, so a rename never touches saves.
- `nil` remains valid and means "not mutated" (every existing save, and all of
  Hatchfall).
- Mutation is **atomic with the creature**: it is decided in the same transaction
  that picks the creature, and travels with the `uid` from then on. A steal,
  recovery, or placement transfers the mutation; it never re-rolls it.
- Mutation is **never persisted separately** and never written to a carry
  (carries are memory-only, Q21).

### When the roll happens — proposal: at CLAIM, and why

**Proposed: the mutation roll happens at claim, in the same body as the existing
rarity + creature roll, before the debit commits.**

Defence:

1. **The claim already is the roll.** Rarity and creature are decided at claim,
   `stats.totalHatches` is stamped at claim (Q31), and the HUD reveal at the belt
   already shows the claimed creature. A mutation is a property of that same
   paid outcome. Deciding it later would mean the creature changes identity after
   the player was shown it.
2. **Atomicity.** Claim already runs price -> roll -> debit -> mint with no
   yield. Adding a third roll inside that body keeps the transaction all-or-
   nothing: there is no later step that can fail, duplicate, or be replayed to
   produce a second mutation.
3. **No outcome changes hands.** If mutation were rolled at finalize, a stolen
   carry would hand the thief the mutation roll, and a carry that is never
   finalized would never resolve its mutation. Rolling at claim means a stolen
   carry carries a finished creature (mutation included) — the same rule as the
   creature itself.
4. **Disclosure stays valid.** The odds are shown before the claim; the roll uses
   the same multiplier the modal displayed. Whether the player finalizes does not
   change the odds they were shown.
5. **UI already exists.** `Hud.showClaimResult` reveals the claimed creature.
   Adding a mutation line there is one reveal, not a second hidden reveal at
   finalize. The **carry visual stays tier-only** (Q37) — the mutation is shown
   to the claimant in the result panel, never to raiders on the character.

Rejected alternative: roll at finalize. It would make a paid claim's outcome
depend on a later, separate event, put the roll on the steal path, and require a
second reveal. If Spark prefers finalize anyway, the odds table and disclosure
text are unchanged — only the mint location moves, and TagFlow must be proven not
to re-roll.

Consequence to accept explicitly: **abandoning a carry loses a mutated creature**
just like it loses any creature today. There is no re-roll and no refund. That is
consistent, but it is a real player-feel cost and belongs in Spark's ruling.

---

## (b) Mutation tiers — 3 concepts

Three tiers, because three keeps the disclosure table readable (6 rarities x 3
tiers) and matches the locked design's three labels. More tiers would dilute each
one and add art cost without adding a decision.

Names below are **PROPOSED original display names, UNVERIFIED** (Spark runs the
collision screen, same as creature names). The locked design's working labels are
**Gold / Neon / Glitched**; if Loic prefers those as display names, drop the
invented names — the math and effects are label-agnostic. Renaming a locked
design label is a Loic + Spark call, not a night-drafts call.

| Tier id | Proposed display name (UNVERIFIED) | Locked working label | Income multiplier | Visual change |
|---|---|---|---|---|
| `gildmark` | **Gildmark** | Gold | **x1.5** | Gold-leaf seams across the body; one eye turns metallic amber; a single soft glint on idle (no loop). |
| `voltline` | **Voltline** | Neon | **x2.0** | One glowing circuit line traces the silhouette; bright rim light; slow 2s pulse, no strobe. |
| `fracture` | **Fracture** | Glitched | **x3.0** | A vertical seam of offset geometry with a chromatic edge; slow 0.5s positional jitter loop (motion-reduction aware). |

**Mechanical scope (deliberately small and justified):**

- **Income multiplier only.** Mutation changes what a creature earns, nothing
  else. It does not change hatch odds, rarity, steal chance, lock behaviour,
  slots, or carry speed.
- **No rarity interaction in the effect.** A mutated Common gets the same x1.5 as
  a mutated Mythic. Rarity interacts with the **chance** (next section), not the
  multiplier. This keeps the disclosure legible and stops "rarity x tier"
  stacking from becoming an economy surprise.
- **One mutation maximum, no upgrades, no merges, no downgrades.** An owned
  creature cannot be mutated later with Robux or coins. A paid "mutate this
  creature" action would be a random re-roll on an owned asset — precisely the
  casino framing this design must avoid.
- **No cosmetic-only mutation.** If it were cosmetic, the 79-Robux boost would
  be selling nothing; if it is economic, it must be disclosed. It is economic and
  disclosed.

**Why the multiplier is baked into `incomePerSecond` at mint:** the economy has
exactly ONE aggregation (`Economy.incomePerSecond` sums stored
`creature.incomePerSecond`), and adding a second multiplier inside that
aggregation would touch offline income, steal valuation, rebirth scaling and
every income test. Instead, mint stores `round(base x tierMultiplier)` and the
record's `mutation` id is what the UI uses to display the badge and the "x1.5"
label. `Economy` is untouched. The invariant to test: for a fixture,
`record.incomePerSecond == round(contentIncome(creatureId) * tierMultiplier)`.
This is also how the game already treats stored income (retuning base content
never rewrites owned creatures).

---

## (c) Odds math shape

### Model: one independent roll, after the creature is chosen

1. Rarity roll (existing `OddsData.compute`, Luck applies) — unchanged.
2. Creature pick within rarity (existing, uniform) — unchanged.
3. **Mutation roll (new, independent):** does a mutation occur? The chance
   depends on the **rolled rarity**. If yes, draw a tier from fixed weights.

Why this shape:

- It mirrors the existing modal grammar: stage 1 = which rarity, stage 2 = which
  creature, and now stage 3 = did it mutate and which tier.
- It keeps **one** mutation table. Luck moves rarity weights, so it changes how
  often you *reach* a given rarity's mutation chance, but it never changes the
  mutation chance itself. That sentence must be in the modal, or Luck will be
  misread as a mutation booster.
- It gives rarity a reason to matter without a per-rarity multiplier table.

### Proposed numbers (PROPOSED — Spark's to set, shown so the shape is concrete)

Base chance that a creature of a given rarity mutates:

| Rarity | P(any mutation) base | boosted x1.5 | boosted x2.0 |
|---|---|---|---|
| Common | 3.0% | 4.5% | 6.0% |
| Uncommon | 4.0% | 6.0% | 8.0% |
| Rare | 5.0% | 7.5% | 10.0% |
| Epic | 7.0% | 10.5% | 14.0% |
| Legendary | 9.0% | 13.5% | 18.0% |
| Mythic | 11.0% | 16.5% | 22.0% |

Tier split, **conditional on a mutation occurring** (fixed, independent of
rarity and of any boost):

| Tier | Weight | Share of mutations |
|---|---|---|
| Gildmark | 65 | 65% |
| Voltline | 27 | 27% |
| Fracture | 8 | 8% |

Absolute per-tier base chances (what the modal must show; rounded from the exact
formula, and recomputed by code, never hardcoded):

| Rarity | Gildmark | Voltline | Fracture | Any |
|---|---|---|---|---|
| Common | 1.95% | 0.81% | 0.24% | 3.00% |
| Uncommon | 2.60% | 1.08% | 0.32% | 4.00% |
| Rare | 3.25% | 1.35% | 0.40% | 5.00% |
| Epic | 4.55% | 1.89% | 0.56% | 7.00% |
| Legendary | 5.85% | 2.43% | 0.72% | 9.00% |
| Mythic | 7.15% | 2.97% | 0.88% | 11.00% |

Boosted at x2.0 (the 1h tier value in `Config.Boosts.Mutation`):

| Rarity | Gildmark | Voltline | Fracture | Any |
|---|---|---|---|---|
| Common | 3.90% | 1.62% | 0.48% | 6.00% |
| Uncommon | 5.20% | 2.16% | 0.64% | 8.00% |
| Rare | 6.50% | 2.70% | 0.80% | 10.00% |
| Epic | 9.10% | 3.78% | 1.12% | 14.00% |
| Legendary | 11.70% | 4.86% | 1.44% | 18.00% |
| Mythic | 14.30% | 5.94% | 1.76% | 22.00% |

(5.86 vs 5.85 is display rounding; the code computes `p * weight / sum` once and
the modal prints that value, so base and boosted can never disagree with the
roll.)

### Formula (single source)

```
p_any(r)      = min(1, p_base(r) * mutationMultiplier)
p_tier(r, t)  = p_any(r) * weight(t) / sum(weights)
```

- `p_base` lives in the mutation odds table, keyed by content pack.
- `mutationMultiplier` comes from `Boosts.mutationMultiplier(config, boosts, now)`
  — the existing reader, which today no code calls (Q7). The modal and the server
  must receive the **same** number.
- `min(1, ...)` is a hard cap. Nothing can exceed 100%.
- Mutation chance never depends on: coins, purchases other than an active
  Mutation Boost, session length, prior hatches, or time of day. No pity timer.

### Two-stage (here, three-stage) disclosure text pattern

The pattern mirrors Hatchfall's odds modal: a short stage sentence, an exact
table with base and boosted numbers side by side, then a plain footer. No
countdowns, no "lucky", no "don't miss".

Draft strings (policy text, not store copy — Spark edits the wording):

> **Stage 3 — Mutation**
> After your creature is chosen, one more independent roll decides whether it
> mutates. The chance depends on the creature's rarity. If it mutates, the tier
> is drawn with fixed weights: Gildmark 65%, Voltline 27%, Fracture 8%.
> A Mutation Boost multiplies the mutation chance only. It never changes the
> tier weights and never changes hatch odds.
>
> *Boost line, only while one is active:* Mutation Boost active: x2.00 on
> mutation chance. Boosted numbers are shown in brackets.
>
> *Luck line:* Luck changes which rarities you roll, and therefore how often you
> reach a rarity's mutation chance. It does not change a mutation chance itself.
>
> *Footer:* These are the exact mutation odds. They do not change based on what
> you buy or how much you have played. A mutation adds income only; it does not
> change hatch odds, stealing, locks, or slots.

Table rendering: one row per rarity, three tier columns, each cell
`1.95% (3.90%)` — base outside, boosted in brackets, only when a boost is
active. Hatchfall's modal lists base and boosted for every rarity including
Common (Q34); mutation does the same. If no boost is active, only base is shown
and the boost line is absent.

### Rarity interaction, stated plainly

Higher rarity = higher mutation chance (3% -> 11%). Justification: (1) chasing
rarer eggs should feel like it also chases the identity feature; (2) it keeps
mutation supply proportional to the income curve instead of flooding early
Commons with x3 creatures; (3) it is one monotonic table, easy to disclose. It
does **not** make higher rarities mutate harder — only more often.

---

## (d) Systems touched vs untouched

### Touched (all additive unless noted)

| Surface | Change | Risk |
|---|---|---|
| `src/core/shared/HatchFlow.luau` | After the creature roll: compute `mutationMultiplier`, roll mutation from an injected table, mint `mutation` + baked income, return it in the outcome. No-op when the table is empty (Hatchfall). | Ordering/atomicity; must stay pure and keep the existing no-yield body. |
| `src/ui/shared/OddsData.luau` | Add per-pack mutation odds + `computeMutation`/`computeMutationDisclosure`. **Single home** for mutation numbers, same as rarity weights. | This is the audited disclosure module; growth here is expected, forking it is not. |
| `src/ui/client/OddsModal.luau` | New stage-3 section, boost line, Luck line, footer; accepts `mutationMultiplier`; hides the section when the pack has no mutation table. | Mobile layout: 6 rows x 3 columns + brackets; needs a phone check. |
| `src/core/shared/Boosts.luau` | No code change expected — `mutationMultiplier` is already implemented and tested as a reader. It stops being dead code. | If the reader needs a clamp, that is a change; decide (open Q). |
| `src/ui/client/Hud.luau` | Claim reveal gets a mutation line/badge. | One extra label; must not resize the panel on phones. |
| `src/shared/NetTypes.luau` / record type | No shape change — `mutation` already exists; it becomes populated. | None, if the field is already serialised (verify in the shape check). |
| `src/content/shared/Mutations.luau` (new data module) | Tier metadata: id, display name, multiplier, effect id. No odds here. | Must be joined to the odds table at startup by a mismatch check, like prices vs odds. |
| `src/content/shared/Creatures.luau` | Game 2's own 16-creature pack (already planned); mutation is orthogonal to it. | No change to Hatchfall rows. |
| `src/shared/Config.luau` | **No new values needed.** `Config.Boosts.Mutation` already declares 15m x1.5 / 1h x2.0. Mutation odds live in `OddsData`, not Config (same split as egg prices vs odds). | If Spark wants odds in Config instead, that is a deliberate reversal — flag, don't assume. |
| `Config.Analytics.Events` | Add `MutationRolled` (rarity, tier id, boosted flag; no uid, no creature id). | Additive; must respect the no-PII rule. |
| `docs/designs/phase-2-art-pass.md` | ART-C4 gains per-tier overlay specs and an effect budget. | Art only. |
| Tests | New Lune mutation test (distribution, cap, no-op pack, baked income) + odds disclosure test (displayed == rolled for every rarity and multiplier). | This is the proof Spark will want. |

### Untouched (deliberately)

- **`Economy.luau`** — no second multiplier. Income is baked at mint; the single
  aggregation and offline income keep working unchanged.
- **Steal / Tag / Lock / Recover** — mutation travels with the uid. No re-roll, no
  new field, no new authority path. The existing "one record, one uid" invariant
  covers it.
- **Placement / BaseService** — only a badge render, no ownership logic.
- **Rebirth** — mutations vanish with the creatures they belong to. No keep rule,
  no exemption. (A paid mutation boost's output is still a normal creature; if
  that feels bad, that is a game-design question, not a data rule.)
- **Schema / `DataSchemaVersion` / migrations** — field already exists; `nil`
  already valid.
- **Hatchfall (Game 1)** — mutation table empty, no product, no modal section, no
  behaviour change. Q7 stays closed.
- **Hatch odds** — mutation is not a rarity. `OddsData.compute` is not edited.

### Sellability gate (the Q7 promise, made checkable)

A Mutation product may go on sale **only when all of these are true**:

1. Mutation odds exist in `OddsData` for the pack, and the content/odds join
   check passes.
2. The modal renders exact base + boosted mutation numbers for every rarity, on
   phone and desktop.
3. A test proves the number the modal prints equals the number the server rolls
   for the same multiplier (the `Boosts.luckMultiplier` proof, repeated for
   mutation).
4. The roll is inside the claim transaction and a test proves one claim = one
   roll = one mint.
5. The product id and price (79 Robux, per the locked design ladder) are approved
   by Spark; `MaxBoostMultiplier` and the mutation cap behave as ruled.
6. Spark's verdict on this note is recorded in the vault.

Until 1-6, the boost stays unsold (and if it were somehow activated, mutation
chance is treated as 0 because no odds table exists). **Odds before product, not
product before odds.**

### Casino-framing prohibitions (explicit)

- No paid re-roll of an owned creature's mutation.
- No "upgrade mutation" / "lock in a better tier" product.
- No mutation crates, keys, or bundles containing a random tier.
- No animated spin/reel/slot reveal; the mutation reveal is a one-shot visual
  state, not a game of chance presentation.
- No "you're due", streak, or pity messaging; the footer says odds never change
  with play.
- No limited-time mutation odds. The boost changes the chance; the calendar does
  not.
- No mutation on deterministic direct-pick / seasonal-limited purchases (if a
  direct pick could mutate, the price would be buying a random outcome without a
  roll disclosure). Flag for the event economy: direct picks are minted
  unmutated unless Spark rules otherwise.

---

## Open questions for Spark (numbered)

1. **Roll moment.** Approve claim (proposed) or require finalize? If finalize,
   prove no re-roll on the steal path and accept that a never-finalized carry
   never resolves.
2. **Final numbers.** Approve the proposed `p_base` table (3/4/5/7/9/11%), tier
   weights 65/27/8, and multipliers x1.5/x2.0/x3.0 — or supply the real ones.
   These are the only values a player can be told, so they need one owner.
3. **Tier names.** Keep the invented display names (Gildmark/Voltline/Fracture,
   subject to the collision screen), or keep the locked labels Gold/Neon/Glitched
   as display names? Math unaffected either way.
4. **Mutation Boost product spec.** The locked ladder lists one 79-Robux product,
   but `Config.Boosts.Mutation` has two tiers (15m x1.5, 1h x2.0). Which one is
   the 79 product? Or two products at two prices? Needs a single answer before
   the shop row is written.
5. **Stacking.** If two Mutation boosts are active, the existing category
   multiplier would multiply them (x3.0 for 15m + 1h). Allowed, replace-on-use,
   or cap? Proposed: one active Mutation boost; activating another replaces the
   current one's duration/value.
6. **Cap.** Confirm the hard `min(1, ...)` cap, and whether `MaxBoostMultiplier`
   should also clamp mutation odds (today it clamps income only).
7. **Luck wording.** Confirm the "Luck changes the rarity mix, not the mutation
   chance" sentence is acceptable disclosure; it is the honest version and it
   pre-empts a support ticket.
8. **Direct picks / event creatures.** Confirm event direct-pick creatures are
   minted with no mutation (deterministic purchase), or define otherwise.
9. **Rebirth.** Confirm mutations are deleted with creatures on rebirth (no
   exemption), or add a keep rule.
10. **Mutation and stealing.** Confirm a stolen mutated creature keeps its
    mutation (proposed yes, travels with uid) and that the mutation is visible to
    the thief only after the steal, not on the carried egg (Q37 stays tier-only).
11. **Record shape confirmation.** Verify `mutation` is already in the
    `PublicCreature` serialisation path and survives profile round-trips as an
    optional string; if not, this becomes a schema task and the draft's "no bump"
    claim is wrong.
12. **Analytics.** Approve `MutationRolled` fields (rarity, tier id, boosted
    flag) and whether hatch events need a `mutated=true` dimension.
13. **Content split.** Confirm `glitchlings` is the Game 2 content pack id that
    keys both the creature table and the mutation table, and that Hatchfall's
    `mythic-hatchery` gets an empty mutation table.
14. **Visual budget.** Approve three overlays as additive materials/particles
    (no new meshes) and the motion-reduction behaviour for Fracture; confirm the
    mobile always-on effects cut order.

---

## What this note is not

- Not code. No `Config` edits, no product enabled, no odds published in-game.
- Not a price change. 79 Robux is quoted from the locked ladder; Q4 above can
  change it.
- Not a store copy draft. Shop text for the boost is a separate, later pass and
  must quote these odds exactly.
- Not approved. Until Spark's verdict + the six-point sellability gate, the boost
  is not sellable.
