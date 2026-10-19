---
title: "Hatchfall — Phase 2 content plan (draft)"
updated: 2026-10-18
tags: [projects, hatchfall, phase-2, content, build-order, draft]
status: draft
owner: Pi (Lane C)
---

# Hatchfall — Phase 2 content plan (DRAFT)

**Status: DRAFT. Vault only. No repo changes, no game code, no odds edits, no
product live.** This note **synthesizes** four Lane C drafts already in this
vault into one Phase 2 **build order** for Hatchfall. It edits none of them; it
sequences them. Facts dated **2026-10-18**; canonical repo is
`tslowkfun/roblox-games`, master `PROJECT.md`.

**Inputs synthesized (not edited here):**

- `projects/hatchfall-creature-roster-expansion-draft.md` — 10 season-neutral rows.
- `projects/hatchfall-seasonal-calendar-draft.md` — the event envelope + NEW-1…NEW-7.
- `projects/hatchfall-christmas-event-draft.md` — the Christmas slot contents.
- `projects/hatchfall-store-copy-draft.md` — storefront copy + claim ledger.

Each input still carries its own "draft for Spark's edit and veto" face. This
plan treats them as approved **inputs**, not as published decisions, and inherits
every unruled number as **PROPOSED**.

**Reading conventions used throughout:**

1. **Names are UNVERIFIED** until Spark's collision screen clears them (the first
   sixteen names had six collisions and renames). No name enters UI, store copy,
   or an ad before clearance.
2. **Money rule.** Anything priced, granted, or sold is marked
   **"Lane B / Spark-ruled — not decided here."** Lane C drafts shapes and counts;
   Spark owns every number that touches money, income, or odds.
3. **Describe, do not build.** Reskins and art direction are described in words.
   Nothing here is a spec for code; the builds that must be written are called out
   as NEW items with their gates.
4. **One table.** Odds are one table and only the hatch roll rolls (standing rule).
   No item below may add a second roll table or per-creature weights.

---

## 1. Build order — NEW-2 first

The Christmas event is the **proof build**: every "new" surface an event needs has
to exist before the first event, because after Christmas an event is a **content
swap only** (new rows + Config entries + product ids). Anything a later event
would need is therefore a Christmas build item or it is cut.

### 1.1 First item: NEW-2 — per-egg creature pool + one shared resolver

**This is the first build item of the Christmas phase, and the only item that can
silently damage the live game.**

Today, rarity → creature is **global per content pack**. Adding six Christmas rows
to a rarity table would make them rollable from the **base and mythic eggs too**,
and would change those eggs' **disclosed** odds the instant the event starts. That
is not a reskin — it is a silent odds change on live eggs. The fix is a per-egg
`pool` tag that the hatch roll **and** the odds disclosure read from **one shared
resolver**:

- One resolver, used by both the roll and the disclosure — **never fork the
  table.**
- Event rows carry the event tag; base eggs resolve only base rows.
- The disclosure keeps showing base **and** boosted odds, from the same source.
- Gate: Spark review (odds surface) + `odds_disclosure_test`, plus a new
  assertion that **base-egg disclosed odds are unchanged while an event is
  active**.

**Approved fallback if NEW-2 slips — ship the limited creature + cosmetics with
NO event egg.** An event with no egg is a valid event (the calendar's micro slots
are precedent: zero hatchable rows, limited creature only). The fallback is
**never** "add event rows to the global rarity table." If the resolver is not
green, the Christmas egg does not ship.

### 1.2 The rest of the Christmas-phase build

| # | Item | Why it sits here | Gate |
|---|---|---|---|
| 1 | **NEW-2** — per-egg pool, one shared resolver | Above. First, because its absence is a silent live-odds change | Spark (odds) + `odds_disclosure_test` |
| 2 | **NEW-1** — `SeasonalCalendar` + UTC window check + belt filter | Nothing in-game reads event state; disabling the egg must be a **number** (spawn weight 0), not a code branch | Spark + Lune window-edge suite |
| 3 | **NEW-3** — limited direct-pick purchase + idempotent mint | The only non-egg path that mints a creature; must mint **exactly once**, never roll. **Money path** | **Spark audit before any sale** |
| 4 | **NEW-5** — event banner + showcase pedestal | Display-only; direct Buy card, never a roll visual. Anything entering a door column needs clearance in the same commit | Spark + `check_map` / `check_parts` |
| 5 | **NEW-6** — generic seasonal analytics names (`SeasonalEventStart`, `SeasonalEggClaimed`, `SeasonalLimitedPurchased`, `SeasonalCosmeticPurchased`) | Per-event names would make every event a taxonomy change; attribute by SKU/date | Analytics review |
| 6 | **NEW-4 SPEC — the cosmetics system spec (Spark)** | **No cosmetics system exists** (see §4). No cosmetics build may start before this spec | Spark (monetization) |
| 7 | **NEW-4 BUILD** — cosmetic catalogue + equip/apply runtime | Ships **after** the spec; ship three first (§4). Writes the three fields that already exist; no new schema field | Spark review |
| 8 | **Christmas content** — 6 hatchable rows + 1 limited + described reskin (§3) | Lands with the build; content is names/art rows, not code | Name clearance + art budget |
| 9 | **Roster expansion** — 10 season-neutral rows (§2), **one drop** | Separate from the event pool; timing vs Christmas is an open question (§7) | Name clearance + one-drop discipline |
| 10 | **Store copy publish** — description, taglines, icon/thumbnail | Gated last, on name clearance + ledger + step-9 product status (§6) | Spark + Loic uploads |

Not in this build, deliberately: no new rarity, no new income band, no mutation
(Q7 closed), no seasonal pass (recommend DEFER), no second roll table, no
per-creature weights, no change to `CarryVisual` / `PlotPalette` / steal / lock /
recover / rebirth logic.

---

## 2. Roster expansion rows vs the current roster actuals

The current base pack (`mythic-hatchery`) actually holds **16 creatures**, split
**Common 4, Uncommon 3, Rare 3, Epic 2, Legendary 2, Mythic 2**. The roster draft
proposes **10** season-neutral rows (two per rarity, Common → Legendary; no
Mythic, no Seasonal Limited). Adding them gives **26** total.

**Current actuals → after the drop:**

| Rarity | Current count | New rows | After | Existing income band |
|---|---|---|---|---|
| Common | 4 | +2 | 6 | 1 |
| Uncommon | 3 | +2 | 5 | 3–4 |
| Rare | 3 | +2 | 5 | 10–12 |
| Epic | 2 | +2 | 4 | 40–45 |
| Legendary | 2 | +2 | 4 | 150–170 |
| Mythic | 2 | 0 | 2 | 600–650 |
| **Total** | **16** | **+10** | **26** | — |

**The 10 proposed rows** (all names **UNVERIFIED**; incomes inside the existing
bands, no band extended):

| # | Name (UNVERIFIED) | Rarity | Proposed income | Inside band |
|---|---|---|---|---|
| 1 | Plumdrop | Common | 1 | Common 1 |
| 2 | Puddlepod | Common | 1 | Common 1 |
| 3 | Nooklet | Uncommon | 3 | Uncommon 3–4 |
| 4 | Quaverpaw | Uncommon | 4 | Uncommon 3–4 |
| 5 | Ceruleaf | Rare | 11 | Rare 10–12 |
| 6 | Mireglass | Rare | 12 | Rare 10–12 |
| 7 | Tempestrel | Epic | 42 | Epic 40–45 |
| 8 | Nebulynx | Epic | 44 | Epic 40–45 |
| 9 | Asterune | Legendary | 155 | Legendary 150–170 |
| 10 | Gravitore | Legendary | 165 | Legendary 150–170 |

**The one real odds effect — stage-2 dilution, not weight change.** The rarity
weights are untouched, so the chance of rolling each rarity is identical. But
stage 2 is **uniform within rarity**, and the disclosure computes each creature's
chance as rarity chance ÷ count in that rarity. Adding rows therefore lowers each
**existing** creature's disclosed share inside its rarity:

| Rarity | Count before → after | Each creature's share of its rarity |
|---|---|---|
| Common | 4 → 6 | 25.0% → 16.7% |
| Uncommon | 3 → 5 | 33.3% → 20.0% |
| Rare | 3 → 5 | 33.3% → 20.0% |
| Epic | 2 → 4 | 50.0% → 25.0% |
| Legendary | 2 → 4 | 50.0% → 25.0% |

Discipline this forces, per the roster draft:

- **One drop, not a drip.** Shipping rows piecemeal changes the disclosed
  percentages repeatedly for no player benefit. One drop = one disclosure change.
- **Do not "fix" dilution with per-creature weights.** Uniform within rarity is
  the disclosed model; weights would be a second odds table and a policy change.
  If Spark wants smaller dilution, the lever is fewer rows, not hidden weights.
- **Income values are economy data** → **Lane B / Spark-ruled — not decided here.**
- **Legendary idle effects** (Asterune idle, Gravitore rings) go in the mobile
  always-on cut-first list, same as the rebirth aura.
- **Roster rows are base-pack only** — they do **not** join the Christmas event
  pool and overlap no event id.

---

## 3. Christmas — the first seasonal drop

### 3.1 Window

**December 1 00:00 UTC → January 2 00:00 UTC**, half-open `[start, end)`. With a
half-open end, **January 1 is the last full playable day** and the event is gone
before January 2 begins. Everything is UTC and stated exactly; no local time is
printed anywhere, ever.

### 3.2 The flagship — Nivellan (name UNVERIFIED)

One dedicated seventh concept, purpose-built for the limited slot (not a promoted
egg row, not a base Legendary). **Name is UNVERIFIED** and must clear Spark's
collision screen before it appears anywhere.

- **Slot:** Seasonal Limited — direct pick, one per player per event,
  window-gated.
- **Direct pick at 599 Robux** — locked by Spark's STEP-9 verdict, **Q64**:
  one SKU, one price, one creature shown before purchase; never in an egg, never
  in a pool, never in the odds modal, never re-rolled. → **Lane B / Spark-ruled —
  not decided here.**
- **KEPT through rebirth.** Limited-purchase creatures survive rebirth; a paid
  creature is not deleted by a normal progression action. → money-adjacent ruling
  → **Lane B / Spark-ruled — not decided here.**
- **Stealable under normal rules, disclosed before purchase.** No immunity, no
  special case in the steal flow; the buyer is told in plain words, above the Buy
  button, before the prompt, that it can be stolen like any other creature and
  recovered under the normal rules. → money-adjacent **disclosure** →
  **Lane B / Spark-ruled — not decided here.**
- **Deterministic mint:** one receipt → one creature; a replayed receipt mints
  zero; no coins move; a purchase does not count as a hatch.
- **Display-only "Seasonal" badge** — not a rarity row.
- **Income value** — must land inside the **Legendary band 150–170**, never above
  base Mythic; the exact number is Spark's (§7).

### 3.3 Christmas hatchable rows — 6, fixed by the envelope

Calendar-fixed spread: **3 Common, 1 Uncommon, 1 Rare, 1 Epic.** Incomes sit
inside existing bands → **Lane B / Spark-ruled — not decided here.**

| # | Name (UNVERIFIED) | Rarity | Proposed income | Band |
|---|---|---|---|---|
| 1 | Frostnib | Common | 1 | Common 1 |
| 2 | Tinseltoe | Common | 1 | Common 1 |
| 3 | Hearthmoss | Common | 1 | Common 1 |
| 4 | Nutmeggle | Uncommon | 3 | Uncommon 3–4 |
| 5 | Aurorim | Rare | 11 | Rare 10–12 |
| 6 | Chimehorn | Epic | 42 | Epic 40–45 |

These six live **only** in the Christmas event pool (NEW-2). They never appear in
the base or mythic egg. The event egg is bought with **in-game coins**, exactly
like every other egg, with exact base **and** boosted odds shown before the claim.

### 3.4 The festive reskin — described, not built

Three surfaces, in the order a player meets them. All dressing; no income, odds,
steal, lock, carry, or slot behaviour changes:

- **Map dressing.** A light frost pass over plaza ground and stall roofs; static
  warm string lights between stall posts; two frosted evergreen props framing the
  spawn sightline, kept out of the walk path and door columns. Nursery pads get a
  thin frost ring and a faint cool underglow — pad count, layout, and income
  visuals unchanged.
- **Belt and egg presentation.** The belt frame gets a subtle snow-crust edge and
  a cold rim light; belt speed, spacing, and egg order untouched. The event egg
  reads visually distinct (frosted shell, warm seam glow) so players know what
  they are buying before the modal opens. No casino framing: the egg cracks the
  way it always cracks — no reels, crates, spinning reveals, "!" bursts, or fake
  stock counters.
- **UI accents.** A seasonal banner at the plaza and a shop card with the exact
  UTC window and a **Buy** button (display-only signage otherwise); a display-only
  "Seasonal" inventory badge; an optional themed header accent on the odds modal —
  its numbers, layout, and base+boosted disclosure untouched.

**Never touched by the reskin:** plot colours are permanent identity/readability,
never seasonal decoration; and the carried egg stays **tier-only** (it must never
read creature or rarity, so a festive egg reveals nothing about a steal target).
Honest scarcity only: a fixed end date, stated exactly — no countdown pressure, no
"last chance", no fake stock.

---

## 4. Cosmetics — 8 drafted, ship three, spec first

**Flag: there is NO cosmetics system in the game yet.** The three equip fields
(`nurserySkin`, `trail`, `hatchEffect`) exist in the save template, but **no
catalogue and no equip/apply runtime exist** — the fields are an unkept promise
today. Therefore:

> **Cosmetics need their own Spark spec (money + monetization) before any
> cosmetics build.** This plan sequences the cosmetics build **after** that spec
> (build item 7 in §1.2). No cosmetic ships while the spec is open.

**The 8 drafted concepts** (all names **UNVERIFIED**; prices are the drafted
49–249 band → **Lane B / Spark-ruled — not decided here**):

| # | Concept | Slot | Proposed price |
|---|---|---|---|
| 1 | Frosted Pines | nurserySkin | 149 |
| 2 | Gingerbread Pad | nurserySkin | 199 |
| 3 | Stringlight Terrace | nurserySkin | 249 |
| 4 | Solstice Blanket | nurserySkin | 149 |
| 5 | Sweetfrost Trail | trail | 99 |
| 6 | Ribbon Glide | trail | 79 |
| 7 | Snowglobe Burst | hatchEffect | 129 |
| 8 | Bellshine | hatchEffect | 99 |

**Ship three first** — one per slot, matching the envelope's 3-rows-per-event rule:

1. `christmas_skin` → **Stringlight Terrace** (most legible at plot distance)
2. `christmas_trail` → **Sweetfrost Trail** (reads on every creature, no per-body art)
3. `christmas_effect` → **Snowglobe Burst** (the hatch moment is where the event is)

The other five are a **queue**, not a launch set — and a mid-window second drop is
a second content swap, out of scope for v1. Treat the queue as next-year material.
Cosmetics are cosmetic: **no income, no odds, no steal reach, no lock duration, no
slot effect**, and no schema bump (they write the three existing fields). Nothing
in the set makes sound; audio is a separate budget.

---

## 5. Money-adjacent register

Every row below is priced, granted, or sold, and each is marked with the standing
marker. **None of it is decided in this plan.**

| Item | What it is | Marker |
|---|---|---|
| Seasonal Limited Creature (Nivellan) | Direct pick, 599 Robux, one per player, window-gated (Q64) | **Lane B / Spark-ruled — not decided here.** |
| Flagship income value | Must land in the Legendary band 150–170; exact number open | **Lane B / Spark-ruled — not decided here.** |
| Event egg coin price + active spawn weight | Economy audit; coin amount must be obtainable without Robux | **Lane B / Spark-ruled — not decided here.** |
| 8 cosmetic prices | Drafted 49–249 band | **Lane B / Spark-ruled — not decided here.** |
| Product ids (limited creature + 3 cosmetics) | SKUs created in Creator Hub | **Lane B / Spark-ruled — not decided here.** |
| Kept-through-rebirth ruling | Protects the value of a paid creature | **Lane B / Spark-ruled — not decided here.** |
| Steal-risk pre-purchase disclosure wording | Money-adjacent disclosure text | **Lane B / Spark-ruled — not decided here.** |
| Purchase cap per account per event | Q64 rules 1; confirm before sale | **Lane B / Spark-ruled — not decided here.** |
| Roster + Christmas creature incomes | Income data inside existing bands | **Lane B / Spark-ruled — not decided here.** |
| Lock / coin-pack products (step 9) | Eggs are coins; coins are sold for Robux → hatching is an indirect paid random reward, so exact odds are disclosed | **Lane B / Spark-ruled — not decided here.** |
| Store-copy odds/disclosure sentence | Mandated disclosure, not marketing; no prices/odds numbers in copy | **Lane B / Spark-ruled — not decided here.** |
| Purchase analytics (`SeasonalLimitedPurchased`) | Purchase telemetry naming | **Lane B / Spark-ruled — not decided here.** |

---

## 6. Store copy sequencing

The storefront is a **last**-step deliverable, gated on:

1. **Name clearance** by Spark — prerequisite; no creature name appears in copy
   until it clears.
2. **Claim ledger green** — every statement traced to the built game.
3. **Step-9 status.** Cosmetics, passes, and limited creatures are **omitted from
   the launch description** today; add a claim-ledger row only if step 9 ships them
   before publish.
4. **No prices or odds numbers** in copy — those are money/policy facts that belong
   in-game and would rot in copy. The odds-disclosure sentence is mandated and must
   not be softened.
5. **Banned terms** enforced across taglines, icon, and thumbnails: free, best,
   guaranteed, jackpot, lucky, 100%, "limited time", any likelihood use of "rare"
   ("Rare" only as the tier name), any Robux/price mention, and any competitor name.
6. **Icon/thumbnail briefs** are execution-ready; original art only; **Loic
   uploads only.**

---

## 7. Open questions for Spark

1. **Flagship income (blocking).** Must land inside the **Legendary band
   150–170** (never above base Mythic 600–650). Spark rules the exact value. The
   earlier Epic-45 proposal is retired for the flagship.
2. **Roster-drop timing vs the Christmas drop (blocking).** Ship the 10-row
   roster expansion first (a quiet base-pack window: one disclosed stage-2 change,
   no event), then Christmas? Or Christmas first, with the roster drop after the
   window closes? Consider art budget, the one-drop dilution rule, and store-copy
   timing. The two must not land as simultaneous disclosure changes.
3. **Name clearance** — all 17 names (10 roster + 6 Christmas + Nivellan),
   UNVERIFIED. Any rename must change id **and** display name before ship.
4. **NEW-2 review** — the shared pool resolver is the gate for the event egg; if it
   slips, confirm the fallback (limited creature + cosmetics, **no egg**).
5. **Cosmetics system spec (NEW-4)** — required before any cosmetics build; then
   confirm the ship-three set.
6. **Purchase-confirmation wording** for the two rulings (kept / stealable) —
   money-adjacent; Spark signs the final text.
7. **Event egg coin price + active spawn weight** — economy audit before publish.
8. **Art budget split** — 10 roster bodies + 6 Christmas bodies + 1 flagship + 3
   cosmetics against the art cap; what is queued vs launched, and the cut order
   (cut a Common and a skin — never the flagship, never the disclosure).
9. **Banner/pedestal placement** — door-column clearance in the same commit.
10. **Calendar owner** — strike the calendar note's stale proposed Christmas 799
    so the vault carries one price (599, Q64). Not done by this note.
11. **Seasonal pass** — build one framework for all events, or DEFER (recommended
    defer). The limited creature is never behind randomness either way.
12. **Event count** — 8 slots/year vs a leaner 5; decide from retention data after
    launch, not now.

---

## 8. Sources (dated 2026-10-18)

- `projects/hatchfall-creature-roster-expansion-draft.md` — 10 rows, dilution table.
- `projects/hatchfall-seasonal-calendar-draft.md` — envelope, NEW-1…NEW-7, swap contract.
- `projects/hatchfall-christmas-event-draft.md` — Christmas contents, Nivellan, cosmetics.
- `projects/hatchfall-store-copy-draft.md` — copy, claim ledger, publish gates.
- `projects/roblox-hatchfall.md` — phase status, gates, canonical repo pointer.
- `docs/reviews/spark-verdicts.md` Q64 (per the Christmas and calendar notes) —
  599 direct pick, one per player per event, window-gated, roster-Legendary
  fallback.
- `PROJECT.md` — master instructions, monetization and odds rules (per inputs).
- Income bands (per inputs): Common 1, Uncommon 3–4, Rare 10–12, Epic 40–45,
  Legendary 150–170, Mythic 600–650.
