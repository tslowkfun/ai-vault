---
title: "Hatchfall Christmas Event — First Seasonal Drop (Draft)"
status: draft
owner: "night-drafts-lane-c"
reviewer: "Spark (content/policy/money) + Loic (art direction + uploads)"
created: 2026-10-18
repo: roblox-games
scope: "Content only. No code, no Config edits, no odds edits, no product live."
tags: [projects, hatchfall, seasonal, events, christmas, content, monetization, draft]
related:
  - "projects/hatchfall-seasonal-calendar-draft.md (envelope this note fills)"
  - "projects/hatchfall-creature-roster-expansion-draft.md (season-neutral rows, separate)"
  - "src/content/shared/Creatures.luau (pack rows, income bands)"
  - "src/ui/shared/OddsData.luau (the one odds table + disclosure)"
  - "docs/reviews/spark-verdicts.md (STEP-9 verdict, Q64)"
---

# Hatchfall — Christmas event, first seasonal drop (DRAFT)

**Status: DRAFT for Spark's edit and veto.** Content only. This note ships no
code, changes no repo, and adds no Config values. Every number that is not
already ruled elsewhere is labelled **PROPOSED** and is not decided here.

Facts dated **2026-10-18**, read from the canonical repo `tslowkfun/roblox-games`
(local `C:\dev\roblox-games`). Names are **UNVERIFIED** and must clear Spark's
collision screen before they appear in any UI, store copy, or ad.

**Companion:** `projects/hatchfall-seasonal-calendar-draft.md` — the envelope.
That note fixes the counts, rarities, price band, and the exact surfaces an event
may touch. This note fills the Christmas slot with content and does not redefine
the envelope. Two reconciliations with it are called out in §2.

---

## 1. What an event is here

A **reskin, not a new game mode.** The Christmas event does not add a mechanic,
a currency, a timer, or a rule. It swaps what the nursery looks like, what the
belt carries, and what can hatch — for a fixed window — and then leaves.

No code is described in this note. No `Config` value in this note is decided.

---

## 2. The window, and two reconciliations

**Window: December 1 00:00 UTC → January 2 00:00 UTC**, half-open `[start, end)`.

That is the brief's "early December through January 1": with a half-open end,
**January 1 is the last full playable day** and the event is gone before January 2
begins. Nothing changes hands at midnight in a player's local timezone, because
there is no local timezone in the model — only UTC, stated exactly (`PROJECT.md`
Section D).

**Reconciliation 1 — price.** The calendar's Christmas row carries a *proposed*
**799** for the limited creature (calendar §6/§7.1, marked PROPOSED, i.e. not a
ruling). The STEP-9 verdict **locks 599** (`spark-verdicts.md`, Q64: "Seasonal
Limited Creature = direct pick 599 Robux, one per player per event, event-window
gated"). **This note uses 599.** The calendar's 799 was a proposal inside the
299–999 band and is superseded; the calendar itself is not edited by this note,
so whoever owns it should strike that row's 799 on next touch.

**Reconciliation 2 — creature count.** Calendar §6/§7.1 fixes Christmas at
**6 hatchable rows (3 Common, 1 Uncommon, 1 Rare, 1 Epic)** plus **1 limited**.
This note delivers exactly that: six hatchable concepts in §4 and one limited
concept in §5.

---

## 3. The reskin (described, not built)

Three surfaces, in the order a player meets them. All of it is dressing: no
income, odds, steal, lock, carry, or slot behaviour changes (`PROJECT.md`
monetization rule — cosmetics are cosmetic).

### 3.1 Map dressing — the nursery reads as frost-lit

- **Plaza:** a light frost pass over ground and stall roofs; string lights
  between stall posts (warm bulbs, static, no flashing); two frosted evergreen
  props framing the spawn sightline — kept out of the walk path and door columns.
- **Belt:** the conveyor's frame gets a subtle snow-crust edge and a cold rim
  light. The belt's *speed, spacing, and egg order are untouched* — it is the
  same belt with a coat on.
- **Nursery pads:** a thin frost ring at each pad's base and a faint cool
  underglow. Pad count, slot layout, and income visuals are unchanged.
- **Lighting mood:** colder ambient, warmer local pools (window glow, bulbs,
  hatch light). One art-pass lighting change, inside the existing budget, not a
  new time-of-day system.
- **Carry visual:** **unchanged and explicitly untouched.** The carried egg is
  tier-only (Q37) and must never read creature or rarity — so a festive egg on a
  shoulder reveals nothing about the steal target either. No Christmas change may
  touch `CarryVisual`.

**Constraint that comes with all of it:** `PlotPalette` / plot colours are
permanent identity and readability, **never** seasonal decoration (calendar §5,
item 7). Christmas dress *around* plots, never *recolour* them.

### 3.2 Belt and egg presentation

- **Egg presentation:** the event egg is visually distinct from base eggs
  (frosted shell, warm seam glow) so a player can tell what they are buying
  before the modal opens. It is still bought with **in-game coins**, exactly like
  every other egg.
- **Odds are still shown before the claim:** exact, base *and* boosted, from the
  one table. A festive shell is a purchase cue, not a reason to soften the modal.
- **No casino framing:** no reels, no crates, no spinning reveal, no "!" bursts,
  no fake stock counters. The egg cracks the way it always cracks.
- **Belt presence:** the event egg appears on the belt **only inside the window**,
  and its absence outside the window must be a number (spawn weight), not a code
  branch (calendar NEW-1; `BeltService` already builds tiers from `spawnWeight`,
  and weight 0 = never spawns).

### 3.3 UI accents

- A **seasonal banner** at the spawn plaza and a card in the shop area: name of
  the event, the exact **UTC** start and end, and a **Buy** button for the
  limited creature. Display-only signage otherwise.
- **Inventory badge** reading "Seasonal" on event rows and the limited creature.
  Display-only: it is **not** a rarity and adds **no** row to `OddsData.RARITY`
  (that would renormalise every egg's disclosed odds).
- **Modal accent:** the odds modal may take a themed header/accent colour. Its
  numbers, layout, and base+boosted disclosure are untouched.
- **Copy discipline:** no countdown pressure, no "last chance", no fake scarcity.
  Honest scarcity is a fixed end date, stated exactly.

---

## 4. Six hatchable Christmas creatures

**4–6 concepts, matching the calendar's fixed spread exactly:** 3 Common,
1 Uncommon, 1 Rare, 1 Epic. Rows carry `event = "christmas"` and live in the
**event pool only** (§6, NEW-2) — they must never leak into the base or mythic
egg's pool.

Income notes sit **inside existing rarity bands** (Common 1, Uncommon 3–4,
Rare 10–12, Epic 40–45; floors read from `Creatures.luau`). No band is extended
and no existing value moves, so this drop is variety, not power creep. The
*values* below are proposals inside those bands; the bands, not the numbers, are
the constraint, and final values are Spark's.

| # | Name (UNVERIFIED) | Rarity | One-line visual | Income note (band) |
|---|---|---|---|---|
| 1 | **Frostnib** | Common | A snowball-round creature with pine-needle quills along its back and a permanently frost-dusted nose; hops rather than walks. | **1** — Common floor, same as every existing Common. No curve change. |
| 2 | **Tinseltoe** | Common | Ribbon-furred and small, with a tail that ends in a stiff brass knot; the ribbons settle a half-second after it stops moving. | **1** — Common floor. Keeps the Common pool uniform in value. |
| 3 | **Hearthmoss** | Common | A moss-backed round creature with a warm ember glow in its belly that brightens when it is placed on a pad. | **1** — Common floor. The glow is cosmetic and must not be read as an income tell. |
| 4 | **Nutmeggle** | Uncommon | Nutmeg-brown, shell-backed, curls into a nut shape when idle; one oversize ear pokes out of the shell. | **3** — bottom of the 3–4 band. |
| 5 | **Aurorim** | Rare | An ice-antlered creature with a slow aurora shimmer travelling between the antler prongs (2s loop, no strobe). | **11** — inside the 10–12 band. |
| 6 | **Chimehorn** | Epic | A pale, sure-footed creature with hollow ice horns that catch light; the horns are silent — a visual chime, never audio. | **42** — inside the 40–45 band (40 and 45 already exist). |

**Why 3 Commons:** the Christmas egg's Common row is the pool players see most in
a short session, and a festive event that mostly shows base commons reads as
undressed. Three Commons buys visible seasonal variety at the tier that is
actually hatched, at zero economy cost.

**Art budget note:** six new bodies is the real cost here, not six code changes
(calendar §10: art is the binding constraint against the cap). If the budget
shrinks, cut **one Common** first — it changes the disclosed pool counts but not
the rarity mix, and the event still reads as Christmas.

**Motion/effect budget:** only Aurorim (antler shimmer) and Hearthmoss (belly
glow) carry a standing effect, both in the mobile always-on cut-first list (same
rule as the rebirth aura, ART-C3). Nothing here needs a custom animation graph —
shared idle-bob covers all six.

---

## 5. The Seasonal Limited Creature — the flagship

### 5.1 The locked terms (not proposed, not redesignable here)

- **DIRECT PICK at 599 Robux** — locked by Spark's STEP-9 verdict, **Q64**.
  One SKU, one price, one creature shown before purchase. It is **never** in an
  egg, never in a pool, never in the odds modal, never re-rolled.
- **One per player per event** (Q64). Purchase cap is therefore answered: 1.
- **Event-window gated** (Q64): it leaves the store when the window closes.
- **No casino framing:** a pedestal and a card with a Buy button. Not a crate, not
  a reel, not a spinning reveal. The end time is stated exactly in UTC.

### 5.2 Two rulings that sit beside the price

Both of these were open questions (calendar D1/D2). Both are now **ruled**, and
both must be told to the buyer **before** the purchase prompt — not in a
post-purchase FAQ.

| Ruling | What it means in play | Where the player is told |
|---|---|---|
| **KEPT through rebirth.** Limited-purchase creatures survive `RebirthFlow`. | A 599-Robux creature is never deleted by a rebirth. It is the one class of creature a rebirth does not reset — a paid creature must not be destroyed by a normal progression action. | Stated on the limited-creature card *and* in the purchase confirmation, before the prompt. |
| **Stealable under normal rules, with the risk disclosed before purchase.** | It is a normal placed creature: it can be stolen like anything else, and after the 30-minute recovery window it is the thief's permanently (`RecoverService` / `Config.Steal` — first recovery per UTC day free, extras 5,000 coins, no Robux recovery). No immunity, no special case in `StealFlow`. | Stated on the card *and* in the purchase confirmation, **before** the prompt, in plain words: "This creature can be stolen like any other. It can be recovered under the normal rules." |

Why disclose rather than exempt: an unannounced steal-immunity would be a silent
rule change inside `StealFlow`, and an unannounced stealability would be a paid
item lost to a mechanic the buyer did not agree to. Disclosure keeps the steal
system single-rule and keeps the purchase honest. The confirmation text is a
**money-adjacent disclosure** — Spark signs the final wording.

### 5.3 Which concept is the flagship — the argument

Three candidates were on the table:

| Option | Argument for | Argument against | Verdict |
|---|---|---|---|
| **(a)** Promote the egg's Epic row (Chimehorn) to the limited slot | No new concept to art; the "best thing in the event" is already designed | **Breaks the calendar's fixed envelope.** Calendar §7.1 requires 1 Epic row *in* the egg, and the egg needs an Epic row to have a chase tier at all. Promoting it leaves Common/Uncommon/Rare only — a worse egg for everyone who does not buy | **Rejected** |
| **(b)** Fall back to a roster Legendary (Q64's fallback: "a roster Legendary at normal Legendary income") | Deterministic, zero new art, zero new content risk, ship-safe | A base-game creature sold at 599 reads as *a creature you could otherwise hatch*. It also prices a Legendary's income at a Christmas SKU, which muddies what the event is | **Fallback only** |
| **(c)** A dedicated 7th concept, purpose-built for the slot | The limited slot is the only creature in the event with **no odds**, so it can carry a full-body signature no hatched row can justify. It also keeps the egg's six rows intact (a) and keeps base Legendaries out of the store (b) | One more model in the art budget | **CHOSEN** |

**The flagship: Nivellan.**

| Field | Value |
|---|---|
| Name (UNVERIFIED) | **Nivellan** |
| Slot | Seasonal Limited — direct pick, 599 Robux, one per player, window-gated |
| One-line visual | A tall, pale-blue herald with a crown of hanging frost-lanterns; one slow light pulse travels crown-to-chest and settles (always-on, ~3s, no strobe) |
| Rarity label | **Display-only "Seasonal" badge.** Not a rarity row, not in `OddsData.RARITY` |
| Income | **PROPOSED, NOT DECIDED.** Calendar D5 proposes the top of the Epic band (45); Q64's fallback path implies a Legendary-band value (150–170) if a roster Legendary is used instead. Constraint only: **never above base Mythic**, and it must not move any existing value. Spark rules the number; this note does not. |

Why Nivellan is the flagship and not just "the expensive one":

1. **It is the only creature in the event the player can see before buying.** The
   six hatched rows are described to nobody until they hatch. Nivellan is the one
   thing the event can *promise* — so it should be the most describable object in
   the event, which is exactly the full-body signature (crown, pulse, height) that
   a uniform-rarity egg row cannot carry.
2. **It is legible at a glance from across the base,** which matters because it
   sells by being looked at. Silhouette test: tall + crown reads at distance; the
   pulse gives it motion when static; nothing about it is thin detail.
3. **It reads as the event's herald, not a stronger pet.** Frost-lanterns are the
   same language as the string lights in §3.1, so the flagship and the dressing
   are visibly one product. It does not need to look "rarer than Mythic" — it
   needs to look like Christmas.
4. **Nothing about it depends on the roll.** No odds disclosure, no pool, no
   dilution, no stage-2 arithmetic. The simplest-to-sell object is the one with no
   probability attached.
5. **Mint is deterministic:** one receipt → one `CreatureRecord`, unplaced
   (`slot = nil`), no coins moved, `stats.totalHatches` **not** incremented (a
   purchase is not a hatch). A replayed receipt mints zero (calendar NEW-3 — a
   money path Spark audits before any sale).

**Buyer-visible card, in order:** creature art → name → 599 Robux → exact end
time in UTC → "one per player" → **kept through rebirth** → **stealable under
normal rules, recoverable under normal rules** → Buy. The two risk rulings are
above the button, never below it.

**If content is not cleared in time:** Q64's fallback applies verbatim — a roster
Legendary (Dawnwyrm 150 or Chronofin 170) at **normal Legendary income**, same
599, same terms. That is a scheduling escape hatch, not a design alternative.

---

## 6. New build vs existing systems

The Christmas event is the **proof build** for every new surface the calendar
defines, because it is the first event. Nothing here is rebuilt.

### 6.1 Exists — reuse as-is, no change

| System | State | Christmas use |
|---|---|---|
| Rarity roll (`OddsData` stage 1) | Built, one table | Unchanged |
| `HatchRoll` | Built, the only roller | Unchanged logic; reads the event pool (§6.2) |
| Eggs on belt, coins only | `BeltService` + `Config.Economy.Eggs` | One new egg row; belt picks it up from `spawnWeight` |
| Claim ledger / debit / mint | `HatchFlow` | Unchanged; one claim = one roll = one mint |
| Carry (tier-only visual) | `CarryService`, `CarryVisual` (Q37) | Unchanged and must stay unchanged |
| Place / income / offline | `BaseService`, `Economy`, `OfflineIncomeService` | Unchanged |
| Steal / lock / recover | `StealService`, `LockService`, `RecoverService` | Unchanged — **and Nivellan is subject to them** (§5.2) |
| Rebirth | `RebirthFlow` | Unchanged logic; limited creature carries the **kept** ruling (§5.2) |
| Inventory + `equipped.{nurserySkin, trail, hatchEffect}` | Already in `Template` | Cosmetics need **no schema change** |
| Exact odds, base + boosted | `OddsModal` + `computeDisclosure` | Unchanged; must show the event egg's numbers |
| Leaderboards | Display-only | Unchanged; **no event prizes** |
| Data schema / versioning | Additive only | No bump, no wipe |

### 6.2 NEW — must land in the Christmas build or be cut

| # | Item | Why it is first, not later | Owner |
|---|---|---|---|
| **NEW-2** | **Per-egg creature pool** (`pool = "event:<id>"`), read by the hatch roll **and** the odds disclosure from **one resolver** | **This is the first build item of the Christmas phase.** Rarity → creature is currently *global per content pack*: adding six Christmas rows to a rarity table makes them rollable from the **base and mythic eggs too**, and changes those eggs' disclosed odds the instant the event starts. That is a silent odds change on live eggs, not a reskin. **One resolver, used by both the roll and the disclosure — never fork the table.** | Spark review (odds surface) + `odds_disclosure_test` |
| NEW-1 | Event window (`SeasonalCalendar` + UTC check) + belt filter | Nothing reads event state; otherwise the window is per-event code | Spark + Lune window-edge suite |
| NEW-3 | Limited direct-pick purchase, idempotent mint | No non-egg path mints a creature; must mint exactly once, never roll | **Spark audit before any sale (money path)** |
| NEW-4 | Cosmetic catalogue + equip/apply runtime | `equipped` is a promise with no catalogue and no applier | Spark (monetization) |
| NEW-5 | Event banner + showcase pedestal | Nothing in-world reads event state | Spark + `check_map` door-column pass |
| NEW-6 | Generic seasonal analytics names (`SeasonalEventStart`, `SeasonalEggClaimed`, `SeasonalLimitedPurchased`, `SeasonalCosmeticPurchased`) | Per-event names make every event a taxonomy change | Analytics review |

**NEW-2 is the one that can silently damage the game.** Everything else, if
delayed, costs polish. If NEW-2 is not built, the correct action is to **ship no
event egg at all** (New Year and Thanksgiving are precedent: zero hatchable rows,
limited creature only) — never to add event rows to a global rarity table.

---

## 7. Festive cosmetics — 8 concepts in the 49–249 band

Cosmetics are cosmetic: **no income, no odds, no steal reach, no lock duration, no
slot effect.** They write the three existing `equipped` fields; no new field, no
schema bump. Prices below are **PROPOSED inside the ruled 49–249 band** — final
prices are Spark's (money rule: never guessed).

| # | Concept (UNVERIFIED) | Slot | One-line visual | Proposed price |
|---|---|---|---|---|
| 1 | **Frosted Pines** | `nurserySkin` | Snow-dusted evergreens around the plot edge with warm window glow behind them | 149 |
| 2 | **Gingerbread Pad** | `nurserySkin` | Pads and plinths read as baked slabs with icing trim; no food branding, no logos | 199 |
| 3 | **Stringlight Terrace** | `nurserySkin` | A canopy of static warm bulbs over the pads, gentle bokeh, never flashing | 249 |
| 4 | **Solstice Blanket** | `nurserySkin` | Deep-blue snowfield with one lantern-lit path to the hatch zone | 149 |
| 5 | **Sweetfrost Trail** | `trail` | Soft powder puffs at each footfall, settling in under a second | 99 |
| 6 | **Ribbon Glide** | `trail` | A thin red ribbon that follows the creature and lags half a step behind | 79 |
| 7 | **Snowglobe Burst** | `hatchEffect` | A hatch emits a slow swirl of snowflakes that clears in ~2s | 129 |
| 8 | **Bellshine** | `hatchEffect` | A single warm ring of light expands from the hatch — **visual only, no sound** | 99 |

**Ship three, not eight.** The calendar fixes **3** cosmetic rows per event (one
per equip slot), so the ship set is one `nurserySkin`, one `trail`, one
`hatchEffect`:

- `christmas_skin` → **Stringlight Terrace** (the most legible at plot distance)
- `christmas_trail` → **Sweetfrost Trail** (reads on every creature, no per-body art)
- `christmas_effect` → **Snowglobe Burst** (the hatch moment is where the event is)

The other five are a **queue**, not a launch set: extra skins are the cheapest
future content (no new body, no new odds, no disclosure change) and are the first
thing to spend on if the event needs a mid-window refresh — but a mid-window
second drop is a second content swap and out of scope for v1 (calendar §7.5,
same rule). Treat the queue as next-year material.

**Audio:** nothing in this set makes sound. Bellshine is deliberately silent —
audio is a separate budget and a separate pass.

---

## 8. Repeatability — this draft is the template

Every event after Christmas is a **content swap only** (calendar §5). This note
is the shape of that swap. To run any future event, copy this structure and
change the rows:

1. **Window + reconciliation** — UTC `[start, end)`, and a note of anything the
   calendar says that a later verdict superseded. Never silently inherit a
   PROPOSED number as if it were ruled.
2. **Reskin** — map dressing / belt + egg presentation / UI accents. Describe,
   do not build. Always the same three surfaces, always dress-around-plots.
3. **N hatchable concepts** — counts and rarities fixed by the calendar row; names
   UNVERIFIED; income inside existing bands; effects inside the mobile cut list.
4. **One limited creature** — direct pick, locked price, one per player,
   window-gated, kept through rebirth, stealable under normal rules, both
   disclosed before purchase. Argue the flagship, do not assume it.
5. **New build vs exists** — usually one line: "NEW-2 through NEW-6 are already
   built; this event adds rows only." If that line is not true, the calendar's
   one-rule is broken.
6. **Cosmetics** — one per slot, in band, cosmetic-only.
7. **Checklist** — the same tests every time.

**The reusable checklist (must be green before publish):**

- `odds_disclosure_test` passes, **plus** the assertion that **base-egg disclosed
  odds are unchanged while the event is active** (the NEW-2 guard).
- The event egg's **base and boosted** odds render in the modal, on phone and
  desktop; the limited creature appears **nowhere** in the odds modal.
- Belt shows the event egg inside the window and never outside it; disabling is a
  number, not a branch.
- Product test purchase mints **exactly one** creature; a replayed receipt mints
  **zero**; `stats.totalHatches` unchanged.
- Buyer sees the end time in UTC, "one per player", the rebirth-keep ruling, and
  the steal-risk ruling **before** the prompt.
- A limited creature survives a rebirth test and is stealable in a steal test —
  both rulings have a test, not just a sentence.
- `check_ui` (odds affordance + seasonal card), `check_map` (banner/pedestal vs
  door columns), `check_parts` (showcase part budget).
- Cosmetics apply through the existing `equipped` fields only; no schema bump.
- Names cleared by Spark; no name in store copy before clearance.
- **Data changes additive and versioned, never a wipe.**

**Year-one guardrail:** 23 creatures and 24 cosmetics a year is not fundable
pre-revenue against the art cap (calendar §10). This template is written so a lean
year — Christmas + Summer + Halloween only — still reads as a season, because each
slot is self-contained.

---

## 9. Risks specific to this drop

- **Odds leak (the big one).** Six festive rows in a global rarity table would
  change base and mythic egg odds on day one, silently. Contained only by NEW-2.
  If NEW-2 slips, ship the limited creature and the cosmetics with **no event
  egg** — an event with no egg is a valid event (calendar micro slots).
- **Dilution is still real, just bounded.** Inside the event pool, 3 Commons means
  each is one-third of the Common row; that is disclosed automatically from the
  live count. Do not "fix" it with per-creature weights — that is a second odds
  table.
- **Paid creature feel-bad.** Kept-through-rebirth plus disclosed stealability
  removes the two obvious complaints; what remains is the ordinary steal risk,
  which the buyer accepted in writing. Support answer must match the card exactly.
- **FOMO drift.** Fixed UTC window, no countdown, no "last chance", no fake stock.
- **Art cost.** 6 bodies + 1 flagship + 3 cosmetics is the most expensive event in
  the year and it is the first one. If the budget only covers part, cut one Common
  and one skin — never the flagship and never the disclosure.
- **Lantern motif reuse.** The flagship's frost-lanterns and the Halloween
  "Hollow Lanterns" slot use a similar object. Different palette, different event,
  different silhouette; keep them visually distinct or Halloween inherits the
  Christmas language.
- **Time-offset confusion.** Everything is UTC and stated; do not print a local
  time anywhere, ever.

---

## 10. Open items for Spark / Loic

1. **Name clearance** — Frostnib, Tinseltoe, Hearthmoss, Nutmeggle, Aurorim,
   Chimehorn, Nivellan (all UNVERIFIED).
2. **Limited-creature income** — top of the Epic band (calendar D5 proposal, 45)
   vs a Legendary-band value (Q64 fallback path, 150–170). Undecided here on
   purpose; never above base Mythic.
3. **Final cosmetic prices** inside 49–249, and confirmation of the ship-three set.
4. **Purchase-confirmation wording** for the two rulings (kept / stealable) —
   money-adjacent, Spark signs the text.
5. **Event egg coin price + active `spawnWeight`** — economy audit; constraint
   only: a positive whole coin amount obtainable without Robux.
6. **Art budget split** — 6 + 1 bodies in one commit vs a smaller launch set with
   the rest queued.
7. **Banner/pedestal placement** — needs `check_map` door-column clearance in the
   same commit (standing rule 8).
8. **Calendar owner** — strike the calendar's proposed 799 for Christmas so the
   vault has one price (599, Q64). Not done by this note.

---

## 11. Sources (dated 2026-10-18)

- `PROJECT.md` — quick answers 2/3, Sections C, D, 4, 5, 6.
- `docs/reviews/spark-verdicts.md` — STEP-9 MONETIZATION SPEC verdict, **Q64**
  (599, one per player per event, window-gated, flagship at content time, roster
  Legendary fallback), Q52–Q63, Q65.
- `projects/hatchfall-seasonal-calendar-draft.md` — envelope: counts, rarities,
  price band, NEW-1…NEW-6, the swap contract, D1–D8.
- `src/content/shared/Creatures.luau` — `CreatureDef`, per-pack rarity tables,
  income bands (Common 1, Uncommon 3–4, Rare 10–12, Epic 40–45, Legendary
  150–170, Mythic 600–650).
- `src/ui/shared/OddsData.luau`, `src/ui/client/OddsModal.luau` — one odds table,
  stage-2 uniform within rarity, base + boosted disclosure.
- `src/shared/Config.luau` — `Economy.Eggs`, `Steal`, `Base.MaxSlots`,
  `Map.PlayerCap`.
- `src/world/server/BeltService.luau` — spawn weight drives belt tiers.
- `src/data/shared/Template.luau` — `inventory`, `equipped.{nurserySkin, trail,
  hatchEffect}`.
- `src/core/server/{StealService,RecoverService,LockService}.luau`,
  `src/core/shared/{HatchFlow,RebirthFlow}.luau`.
- `projects/hatchfall-creature-roster-expansion-draft.md` — season-neutral rows
  (separate batch, no event pool, no overlap with these ids).
