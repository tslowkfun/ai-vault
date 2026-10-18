---
title: Hatchfall — seasonal calendar (draft)
updated: 2026-10-18
tags: [projects, hatchfall, seasonal, events, content, monetization, draft]
status: draft
owner: Pi
---

# Hatchfall — seasonal calendar (DRAFT)

**DRAFT — for Spark's edit and veto.** Lane C, vault only. This note ships no
code and changes no repo. Facts dated **2026-10-18** and read from the canonical
repo `tslowkfun/roblox-games` (local `C:\dev\roblox-games`).

Companion: `projects/hatchfall-christmas-event-draft.md` — being drafted by a
**separate session** (not created or edited here). This note does not define
Christmas content; it fixes the envelope Christmas must fit and the exact
surfaces every later event swaps.

## 1. The one rule

After Christmas, an event is a **content swap only**: new data rows + Config
entries + product ids. No new services, remotes, UI flows, schema fields, or roll
logic.

Anything that would need one is marked **NEW** below. The NEW items must land in
the **Christmas build**, or be cut — they cannot be introduced by a later event
without breaking the rule.

## 2. Inherited constraints (not negotiable here)

| Constraint | Source |
|---|---|
| Seasonal Limited Creature is a **guaranteed direct purchase, never a random roll**; 299–999 Robux; no casino framing | `PROJECT.md` quick answer 3 + S4.3 |
| Eggs are bought with **in-game coins**, never directly with Robux; because coin packs are sold for Robux, hatching is an **indirect paid random reward**, so **exact odds — base AND boosted — are shown in-game** | `PROJECT.md` quick answer 3 |
| Odds are one table; only `HatchRoll` rolls; no second roll table | `PROJECT.md` S4.5 + phase-1 standing rule 1 |
| No casino framing, no feed mechanics, no pay-to-win in ranked contexts | `PROJECT.md` S4 |
| Events run on **UTC**; exact times shown, no manipulation | `PROJECT.md` Section D + world brief §7 |
| Mutation does **not** exist in Hatchfall v1 (Q7 closed); no Mutation product | `docs/questions-for-spark.md` Q7 |
| Season pass + event scheduler are **Phase 2**, not v1 | `PROJECT.md` scheduling override |
| Cosmetics are cosmetic: no income, no odds, no steal/lock effect | `PROJECT.md` monetization |
| Data changes additive + versioned, never a wipe | `PROJECT.md` S4.6 |

## 3. The fixed deck (what exists today)

| Surface | State on 2026-10-18 | Consequence for events |
|---|---|---|
| `src/content/shared/Creatures.luau` | `PACKS["mythic-hatchery"][Rarity]` rows; `CreatureDef = {id, displayName, rarity, incomePerSecond}` | Rows are the only place a creature can be named/minted |
| `src/ui/shared/OddsData.luau` | `RARITY` weights + `Eggs` (`eggId`, `rows`); stage 2 is **uniform within rarity** | A new egg can only choose *which rarities* it rolls; it **cannot** choose a creature pool |
| `src/shared/Config.luau` → `Economy.Eggs` | `costCoins` + `spawnWeight`; join key with `OddsData.Eggs`; mismatch = startup error | Every new egg must add **both** sides |
| `src/world/server/BeltService.luau` | Builds belt tiers from `Config.Economy.Eggs[*].spawnWeight`; weight 0 = never spawns | Disabling an event egg is a number, not a code branch |
| `src/data/shared/Template.luau` | `inventory[itemId]`, `equipped.{nurserySkin, trail, hatchEffect}` already exist | Cosmetics need **no schema change** |
| Cosmetics catalogue / equip applier | **Does not exist** | See NEW-4 |
| Monetization config / shop | Step 9, not built yet | Product ids arrive with step 9 |
| Event scheduler / calendar | **Does not exist** (Phase 2) | See NEW-1 |
| Pavilion / showcase / event banner | **Does not exist** in `src/world` (world-brief intent only) | See NEW-5 |
| Steal / lock / recover | Built; creature transfer only; 30-min recovery; no Robux recovery | Content-neutral, but see D1/D2 |

**The load-bearing finding:** rarity → creature is **global per content pack**.
Adding event creatures to a rarity table makes them rollable from the **basic and
mythic eggs too**, and changes those eggs' disclosed odds the moment the event
starts. That is not a content swap — it is a silent odds change on live eggs.
Christmas must decouple egg → pool once (NEW-2), or every later event inherits
the problem.

## 4. NEW — the Christmas build (one time, then never again)

| # | System | Why it cannot wait | Minimal shape | Gate |
|---|---|---|---|---|
| **NEW-1** | `SeasonalCalendar` + `SeasonService` (UTC window check) + belt filter | Nothing reads event state; per-event code would otherwise be needed | `SeasonalCalendar.events[id] = { startUtc, endUtc, eggId?, limitedCreatureId, cosmeticIds }`; pure `active(now)` / `beltWeight(eggId, now)`; `BeltService` asks it instead of a literal | Spark review; Lune suite for window edges |
| **NEW-2** | Per-egg creature pool (`pool = "event:<id>"`) read by `HatchRoll` and `OddsData.computeDisclosure` | Without it, event rows leak into base eggs and dilute disclosed live odds | Add `pool` to the egg row; the `event` tag on each creature row is what the resolver filters on; one resolver used by both roll and disclosure (never fork the table) | Spark review (odds surface) + `odds_disclosure_test` |
| **NEW-3** | Limited direct-pick purchase + idempotent mint | No non-egg path mints a creature; a purchase must mint exactly once, with no roll | Product id → `SeasonalLimitedService` receipt handler → append one `CreatureRecord` (unplaced, `slot = nil`); `stats.totalHatches` untouched; no `OddsData` involvement | **Money path — Spark audit before any sale** |
| **NEW-4** | Cosmetic catalogue + equip/apply runtime | `Template.equipped` is a promise with no catalogue and no applier | `Cosmetics.luau` rows keyed by id → `nurserySkin` / `trail` / `hatchEffect`; equip writes the existing fields; no new fields | Spark review (monetization) |
| **NEW-5** | Event banner + showcase pedestal | Nothing in-world reads event state | Signage at spawn plaza / one pedestal; display-only; direct "Buy" card, **never** a roll visual; `check_map` door-column pass in the same commit | Spark + `check_map` / `check_parts` |
| **NEW-6** | Generic seasonal analytics names | Per-event names would make every event a taxonomy change | `SeasonalEventStart`, `SeasonalEggClaimed`, `SeasonalLimitedPurchased`, `SeasonalCosmeticPurchased`; attribute by SKU or date (confirm the exact `AnalyticsService` signature at build) | Analytics review |
| **NEW-7** | (Optional, recommend DEFER) Seasonal pass, free + paid tracks | Phase 2 by `PROJECT.md`; not needed for the limited-creature model | If built, one framework for all events; **never** puts the limited creature behind randomness | Spark + Loic |

NOT new, do not rebuild: roll logic, steal/lock/recover, economy aggregation,
rebirth math, slots, data schema, leaderboards.

## 5. The swap contract (every event after Christmas)

An event may touch **only** these surfaces:

1. `src/content/shared/Creatures.luau` — N hatchable rows (tagged `event = "<id>"`) and exactly one `Creatures.Limited` row. `CreatureDef` gains the optional `event` field in the Christmas build (additive; no schema impact).
2. `src/ui/shared/OddsData.luau` — one new `Eggs` entry with `rows` limited to rarities that actually have event creatures, plus `pool = "event:<id>"`. **No weight changes** to `RARITY`.
3. `src/shared/Config.luau` → `Economy.Eggs["<event>"] = { costCoins = <positive whole number>, spawnWeight = <0 outside window / N inside> }`.
4. `src/content/shared/SeasonalCalendar.luau` (NEW-1 file) — one `events["<id>"]` entry (window, ids, banner text, colors).
5. `src/content/shared/Cosmetics.luau` (NEW-4 file) — three rows: `<id>_skin`, `<id>_trail`, `<id>_effect`.
6. `Config.Monetization` (step 9) — product ids for the limited creature (299–999) and the three cosmetics (49–249 band), created by Loic in Creator Hub.
7. Optional: a `Theme` accent used **only** by the event banner. `PlotPalette` is **never** touched — plot colors are permanent identity/readability, not seasonal decoration.

Never touched by an event: `HatchRoll`, `StealFlow`, `StealService`, `LockService`,
`RecoverService`, `Economy`, `RebirthFlow`, `Template`, `MapLayout`, `PlotPalette`,
`DataSchemaVersion`, any remote, any checker.

Per-event checklist (all must be green before publish):

- Lune suites: `odds_disclosure_test`, `economy_test`, belt test, plus a new
  "base-egg disclosure is unchanged while an event is active" assertion.
- `check_ui` (odds affordance + seasonal card rules), `check_map` (if anything
  enters a door column), `check_parts` (showcase part budget).
- Odds modal shows the event egg's **base and boosted** odds; the limited
  creature appears **nowhere** in the odds modal.
- Product test purchase mints exactly one creature; replayed receipt mints zero.
- Showcase/banner shows the exact start and end time in UTC.

## 6. The year at a glance (UTC, window is `[start, end)`)

| Slot | Window | Theme (one line) | Tier | New hatchable rows (by rarity) | Limited (PROPOSED price) | Cosmetics | Event egg |
|---|---|---|---|---|---|---|---|
| **Christmas** | Dec 1 → Jan 2 | The worked example: frost-lit nursery, gifts on the belt (content owned by the Christmas note) | A+ | **6**: 3 Common, 1 Uncommon, 1 Rare, 1 Epic | 1 — **799** | 3 | yes |
| **New Year** | Jan 2 → Jan 8 | First Light: countdown sparks, aurora ribbons, a fresh ledger | micro | **0** | 1 — **299** | 3 | no |
| **Valentine's** | Feb 7 → Feb 22 | Sweetheart Hollow: ribboned nests, warm pinks, paired charms | B | **3**: 1 Common, 1 Uncommon, 1 Rare | 1 — **399** | 3 | yes |
| **Spring** | Mar 20 → Apr 13 | Bloomline: petals on the belt, pastel greens, new growth | B | **3**: 1 Common, 1 Uncommon, 1 Rare | 1 — **399** | 3 | yes |
| **Summer** | Jun 21 → Aug 11 | Sunspill: heat shimmer, beach props, long bright sessions | A | **4**: 2 Common, 1 Uncommon, 1 Rare | 1 — **699** | 3 | yes |
| **Back-to-School** | Aug 15 → Sep 11 | Study Hollow: satchels, chalk doodles, packed-lunch energy | B | **3**: 1 Common, 1 Uncommon, 1 Rare | 1 — **349** | 3 | yes |
| **Halloween** | Oct 1 → Nov 3 | Hollow Lanterns: original lantern shapes, cobwebbed stalls, dusk lighting | A | **4**: 2 Common, 1 Uncommon, 1 Rare | 1 — **799** | 3 | yes |
| **Thanksgiving** | Nov 18 → Dec 1 | Harvest Table: cornucopia, warm amber, a shared feast (global framing, not US-only) | micro | **0** | 1 — **349** | 3 | no |

Annual content: **23 hatchable creatures, 8 limited creatures, 24 cosmetics,
6 event eggs.** About 201 event days of 365; the base game is always on and is
never disabled for an event.

Micro events (New Year, Thanksgiving) deliberately add **no egg and no hatchable
row**: no odds table change, no pool dilution, no disclosure review. Their new
content is the limited creature + three cosmetics.

## 7. Per-slot detail

Prices below are **PROPOSED** and sit inside the mandated 299–999 band. Final
prices are **Spark's** (money rule: never guessed). Event egg coin prices and
`spawnWeight` are economy tuning and also Spark-audited.

### 7.1 Christmas — Dec 1 → Jan 2 (worked example, Tier A+)
- Theme: frost-lit nursery; the proof build for every NEW above.
- Creatures: 6 rows — 3 Common, 1 Uncommon, 1 Rare, 1 Epic. Rows carry `event = "christmas"`.
- Egg: `eggId = "christmas"`; rows `Common, Uncommon, Rare, Epic` (existing weights: 600/250/100/40 → ≈60.6% / 25.3% / 10.1% / 4.0% at Luck ×1); `pool = "event:christmas"`.
- Limited: one direct-pick creature, no egg, no pool membership; **proposed 799**. Price band and final number: Spark.
- Cosmetics: `christmas_skin`, `christmas_trail`, `christmas_effect` (49–249 band).
- Surfaces: `Creatures` (6 + 1 limited), `OddsData.Eggs` (+1), `Config.Economy.Eggs.christmas`, `SeasonalCalendar.events.christmas`, `Cosmetics` (3), `Monetization` (4 products), `Theme` banner accent.
- Exact content (names, art, story) is the Christmas note's job — this note only fixes counts, rarities, band, and surfaces.

### 7.2 New Year — Jan 2 → Jan 8 (micro)
- Theme: First Light — countdown sparks, aurora ribbons.
- Creatures: **0** hatchable rows. One limited: `newyear_limited`.
- Limited: direct pick, **proposed 299**.
- Cosmetics: `newyear_skin`, `newyear_trail`, `newyear_effect`.
- Surfaces: `Creatures.Limited` (+1), `SeasonalCalendar.events.new_year`, `Cosmetics` (3), `Monetization` (4). **No `OddsData` change, no `Economy.Eggs` change.**
- Notes: 6-day window immediately after Christmas; it must not reuse or discount Christmas rows.

### 7.3 Valentine's — Feb 7 → Feb 22 (Tier B)
- Theme: Sweetheart Hollow — ribboned nests, warm pinks.
- Creatures: 3 rows — 1 Common, 1 Uncommon, 1 Rare.
- Egg: `eggId = "valentines"`; rows `Common, Uncommon, Rare` (≈63.2% / 26.3% / 10.5% at ×1); `pool = "event:valentines"`.
- Limited: direct pick, **proposed 399**. Cosmetics: 3 (`valentines_*`).
- Surfaces: `Creatures` (3 + 1), `OddsData.Eggs` (+1), `Config.Economy.Eggs.valentines`, `SeasonalCalendar.events.valentines`, `Cosmetics` (3), `Monetization` (4).
- Notes: hearts/ribbons only; no real-world brands, no copied character motifs.

### 7.4 Spring — Mar 20 → Apr 13 (Tier B)
- Theme: Bloomline — petals on the belt, pastel greens.
- Creatures: 3 rows — 1 Common, 1 Uncommon, 1 Rare. Egg `spring` with the same three rarity rows and `pool = "event:spring"`.
- Limited: direct pick, **proposed 399**. Cosmetics: 3 (`spring_*`).
- Surfaces: same six as 7.3 with `spring` ids.

### 7.5 Summer — Jun 21 → Aug 11 (Tier A)
- Theme: Sunspill — heat shimmer, beach props, long bright sessions.
- Creatures: 4 rows — 2 Common, 1 Uncommon, 1 Rare. Egg `summer`, rows `Common, Uncommon, Rare`, `pool = "event:summer"`.
- Limited: direct pick, **proposed 699**. Cosmetics: 3 (`summer_*`).
- Surfaces: same six with `summer` ids.
- Notes: longest window (peak traffic). One swap only; no mid-event second drop, which would be a second content swap and is out of scope for v1.

### 7.6 Back-to-School — Aug 15 → Sep 11 (Tier B)
- Theme: Study Hollow — satchels, chalk doodles.
- Creatures: 3 rows — 1 Common, 1 Uncommon, 1 Rare. Egg `school`, three rarity rows, `pool = "event:school"`.
- Limited: direct pick, **proposed 349**. Cosmetics: 3 (`school_*`).
- Surfaces: same six with `school` ids.

### 7.7 Halloween — Oct 1 → Nov 3 (Tier A)
- Theme: Hollow Lanterns — original lantern shapes, cobwebbed stalls, dusk lighting.
- Creatures: 4 rows — 2 Common, 1 Uncommon, 1 Rare. Egg `halloween`, rows `Common, Uncommon, Rare`, `pool = "event:halloween"`.
- Limited: direct pick, **proposed 799**. Cosmetics: 3 (`halloween_*`).
- Surfaces: same six with `halloween` ids.
- Notes: no gore, no existing horror/monster IP, no jump-scare overlays; lighting change stays inside the art-pass budget.

### 7.8 Thanksgiving — Nov 18 → Dec 1 (micro)
- Theme: Harvest Table — cornucopia, warm amber, a shared feast; framed globally so it does not read as US-only.
- Creatures: **0** hatchable rows. One limited: `harvest_limited`.
- Limited: direct pick, **proposed 349**. Cosmetics: 3 (`harvest_*`).
- Surfaces: `Creatures.Limited` (+1), `SeasonalCalendar.events.harvest`, `Cosmetics` (3), `Monetization` (4). **No odds change.**
- Notes: ends the day Christmas starts; no overlap, no combined "double event" offer.

## 8. The limited-creature contract (identical every slot)

1. **Exactly one per event**, sold as a **direct pick**: you see the creature, the
   price, and the end time, then buy it. It is never in an egg, never in a pool,
   never in the odds modal, never re-rolled.
2. **Price inside 299–999 Robux**, one SKU, no randomized bundle, no "chance at".
3. **Deterministic mint:** one receipt → one `CreatureRecord`. Replayed or
   duplicated receipts mint nothing. No coins move; `stats.totalHatches` is not
   incremented (a purchase is not a hatch). Decide the stat separately if Spark
   wants purchase analytics in the profile.
4. **No casino framing:** a pedestal/card with a Buy button — not a crate, not a
   reel, not a spinning reveal. No "!" bursts, no fake scarcity counters. The
   end time is stated exactly in UTC.
5. **After the window it leaves the store and the belt.** Owned copies stay,
   keep their name, and remain placeable. No re-runs unless a future event
   deliberately re-offers the same SKU (a new decision, not assumed).
6. **Display-only badge** ("Seasonal") on the card and inventory. Do **not** add
   a rarity row to `OddsData.RARITY` — that would renormalize every egg's odds
   and force a disclosure re-review.
7. **Cosmetics are cosmetic:** they never alter income, odds, steal reach, lock
   duration, or slot count.

## 9. Decisions required before the first sale (Spark + Loic)

- **D1 — Rebirth vs a paid creature.** `RebirthFlow` resets creatures (keeps
  cosmetics + permanent multipliers). As built, a 299–999 Robux creature is
  deleted by rebirth. Options: (a) mark limited-purchase creatures as kept by
  rebirth (additive record flag + explicit `REBIRTH_KEEP` rule — NEW semantics,
  Spark), (b) sell the limited as a **cosmetic display** that is kept, or (c)
  re-grant after rebirth. **Do not sell before this is ruled.**
- **D2 — Stealability of a paid creature.** Stealing transfers a placed creature;
  after the 30-minute recovery window it is the thief's permanently. Is a paid
  item stealable? Options: steal-immune, always recoverable, or normal rules with
  explicit pre-purchase disclosure. **Spark + Loic; policy-sensitive.**
- **D3 — Purchase cap per account per event** (1 / N / unlimited). Not specified
  anywhere; do not invent.
- **D4 — Final prices** for the 8 limited SKUs and the cosmetic rows.
- **D5 — Income and rarity label** of the limited creature (proposal: top of the
  Epic band, never above base Mythic; badge is display-only).
- **D6 — Event egg coin price + active `spawnWeight`** (economy audit). Constraint
  only: a positive whole coin amount, obtainable without Robux.
- **D7 — Seasonal pass**: build (one framework, all events) or defer until after
  the launch gate. Recommendation: defer.
- **D8 — Event count**: 8 slots/year vs a leaner 5. Decide from retention data
  after launch, not now.

## 10. Risks this calendar accepts (and how they are contained)

- **Roster growth.** 23 new creatures/year are permanent data rows (saved
  creatures must resolve their names forever). Mitigation: per-event grouping
  headers in `Creatures.luau`, never delete a row, and after year 1 prefer
  palette/accessory **variants** that reuse a body (cheap art) over new bodies.
- **Art cost.** 23 new creatures + 24 cosmetics/year against a $90 art cap is not
  fundable pre-revenue. Mitigation: events are **post-gate**; tiers exist so a
  lean year (Christmas + Summer + Halloween only) still reads as a season.
- **Odds integrity.** The pool filter (NEW-2) is the only thing standing between
  an event and a silent odds change on base eggs. It needs its own test, and the
  odds table must remain one table.
- **Showcase placement.** Any new pedestal/sign must pass `check_map` door-column
  clearance in the same commit (standing rule 8).
- **FOMO drift.** Exact UTC end times, no "last chance" copy, no countdown
  pressure, no fake stock. Honest scarcity is a fixed end date, nothing else.
- **Regional reading.** Thanksgiving is framed as a global harvest event;
  windows are UTC and not tied to any one country's calendar.

## 11. Sources (dated)

- `PROJECT.md` — quick answers 3/4, Sections C, D, 4, 5 (read 2026-10-18).
- `src/shared/Config.luau` — `Economy.Eggs`, `Belt`, `Boosts`, `Steal`, `Carry`, `Tag`, `Analytics` (2026-10-18).
- `src/ui/shared/OddsData.luau` — two-stage roll + disclosure (2026-10-18).
- `src/content/shared/Creatures.luau` — roster shape and pack split (2026-10-18).
- `src/world/server/BeltService.luau`, `src/world/shared/Belt.luau` — spawn cadence from `spawnWeight` (2026-10-18).
- `src/data/shared/Template.luau` — inventory + equipped fields (2026-10-18).
- `docs/designs/phase-1-plan.md`, `docs/designs/world-map-brief-v1.md`, `docs/designs/phase-2-art-pass.md` (ART-M2 showcase) (2026-10-18).
- `docs/questions-for-spark.md` Q7 — no mutations in Hatchfall v1 (2026-10-18).
- `docs/reviews/odds-disclosure.md` — one stage-2 home, base + boosted display (2026-10-18).
