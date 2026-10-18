---
title: Hatchfall — store copy draft pack
updated: 2026-10-18
tags: [projects, hatchfall, marketing, store-copy, draft]
status: draft
owner: Pi
---

# Hatchfall — store copy draft pack (DRAFT)

**DRAFT v0.1 — for Spark's edit and veto. Nothing here is approved copy.**
Lane C, vault only. No repo changes were made for this note. Facts dated
**2026-10-18** from the canonical repo `tslowkfun/roblox-games`.

**Do not publish until:** (1) the creature-name collision screen is done (it is a
store-copy prerequisite, `docs/designs/phase-1-plan.md`), (2) the claim ledger in
§1 is green, and (3) Spark's verdict is recorded. Creature names are not used in
this copy on purpose — they are still DRAFT.

## 1. Claim ledger — every statement, traced

| Copy claim | Source (2026-10-18) | Status |
|---|---|---|
| Collect-and-steal creature nursery sim | `PROJECT.md` S3 Game 1; title LOCKED | BUILT |
| Eggs on a conveyor, bought with coins | `BeltService`, `Config.Economy.Eggs` | BUILT |
| Carry to your hatch zone, hatch | `CarryService`, `FinalizeService`, `HatchFlow` | BUILT |
| Place creatures on income pads, collect coins | `BaseService`, `IncomeService` | BUILT |
| 6 pads → 30 pads | `Config.Base.DefaultSlots/MaxSlots` | BUILT |
| Up to 10 owners per server ("9 other") | `Config.Map.PlayerCap = 10` | BUILT |
| Steal a **placed** creature | `StealService`/`StealFlow` (creature transfer only) | BUILT |
| Lock your base | `LockService.grant`, tiers 10m/1h | BUILT; products land with step 9 |
| Recover: first/day free, extras cost coins, 30-min window | `RecoverService`, `Config.Steal` | BUILT |
| Display-only leaderboards, no prizes | `LeaderboardService`, `Config.Leaderboards` | BUILT |
| Rebirth resets nursery, +25% multiplier each | `RebirthFlow`, `Config.Rebirth` | BUILT |
| Daily rewards | `DailyService`, `Config.DailyRewards` | BUILT |
| Offline income half rate, 8h cap | `OfflineIncomeService`, `Config.OfflineIncome` | BUILT |
| Exact odds — base and boosted — in-game | `OddsModal`, `OddsData.computeDisclosure` | BUILT |
| Eggs cost in-game coins | `PROJECT.md` quick answer 3 | BUILT |

Deliberately **not** claimed (feature absent or unbuilt): mutations (Q7 closed),
trading, pets that follow you, ranked/PvP ladder, prizes, events, season pass,
VIP/2x passes, auto-collect, faster hatch, cosmetics, coin packs, limited
creatures, "free", "guaranteed", any Robux price.

`Stats` note: leaderboards are display-only, so "bragging rights, no prizes" is
locked by `PROJECT.md` quick answer 2 and must not drift into a reward promise.

## 2. Game description (draft A)

Roblox description limit at draft time is 1,000 characters — **verify in Creator
Hub**; this draft is **~950 characters including line breaks**. Em dashes are
used instead of `--`. No emoji (nothing to hide behind, and it reads cleaner on
mobile).

```text
Hatchfall is a collect-and-steal creature nursery simulator. Eggs ride the conveyor — buy one with in-game coins, carry it to your hatch zone, and see what hatches. Place creatures on income pads, collect coins, and grow your nursery from 6 pads to 30.

You share the server with up to 9 other nursery owners. Slip into a rival's base and steal a placed creature, or lock your base and keep what is yours. Stolen creatures can be recovered: your first recovery each day is free, later ones cost coins, and the window closes 30 minutes after the theft.

Rebirth resets your nursery for a permanent +25% income multiplier, so each run pays back faster. Daily rewards and offline income (half rate, up to 8 hours) keep progress moving. Hatches, rebirths and steals have display-only leaderboards — bragging rights, no prizes.

Every egg's exact hatch odds — base and boosted — are shown in-game before you claim. Eggs cost in-game coins. No hidden numbers.
```

Draft notes:
- **First 150 characters carry the genre** ("collect-and-steal creature nursery
  simulator") because the title contains no genre keyword and the description is
  truncated on some surfaces. Do not move that phrase later.
- The disclosure sentence is **mandated**, not marketing: odds are exact, shown
  in-game, base + boosted; eggs are bought with in-game coins. Do not soften it,
  and do not add "no Robux" (coin packs are sold for Robux, so that would read as
  misleading).
- Do not add prices or odds numbers here. Those are money/policy facts and
  belong in-game (`Config`, odds modal), not in copy that rots.
- No feature name from the "not claimed" list may be reintroduced without a new
  ledger row and Spark sign-off.

## 3. Five taglines (pick one; do not stack)

| # | Tagline | Best use | Veto check |
|---|---|---|---|
| 1 | **Hatch it. Carry it. Guard it.** | Ad line / first line of a post | Three real verbs from the loop; no claims |
| 2 | **Rare creatures don't hatch themselves.** | Description opener variant / social | "Rare" is a real tier; no odds or guarantee implied |
| 3 | **Your nursery. Your lock. Your problem.** | Short promo video / Discord | Edgy tone; lock is real; no power claim |
| 4 | **Steal a creature. Make a rival.** | Ad copy targeting PvP-curious players | Accurate: one creature, not a base raid |
| 5 | **Six pads to thirty. One belt between.** | Thumbnail alt text / social | Exact progression numbers; no "grind" promise |

Banned in all taglines: free, best, guaranteed, jackpot, lucky, insane, 100%,
limited time, "rarest", "OP", "win", any Robux/price mention, any comparison to
another game by name.

## 4. Icon concept brief (for a Fiverr artist)

**Specs:** 1024×1024 master PNG + 512×512 export (Roblox icon), sRGB, full-bleed
(no rounded-corner mask baked in), layered source file (PSD/Procreate), no text.

**Subject (original only):** one creature, head-and-shoulders, 75–80% of the
frame. Archetype: a round, four-legged nursery creature in **moss green and
cream**, oversized **amber eyes** with soft catchlights, two stubby horn nubs,
leaf-shaped ear tufts, **no tail**, and a single glowing **eggshell fragment**
balanced on its head. It must not resemble any existing creature franchise; no
bulb/plant on the back, no clothing, no accessories with logos.

**Composition:** face centered, eyes on the upper third, 8% safe margin; three-
quarter view looking at the viewer; strong rim light in a rarity glow (amber
`#FFB84D` works with the palette) to separate it from the background. Background:
deep navy-to-teal vignette with a soft nursery-shelf bokeh — no props that read as
text, no UI frames.

**Palette:** cream, moss green, amber glow + deep navy background. Three colors
plus one accent, maximum. No rainbow gradients.

**Text:** **none.** The icon renders at 48–150 px where text is illegible, and
Roblox shows the title separately. Text-heavy icons read as clickbait
(`PROJECT.md` S6).

**Mobile legibility gate (artist must show these):**
- 512×512, 128×128, and 48×48 previews.
- Silhouette test: fill the creature solid black — still recognizable.
- Contrast test on pure white and pure black backgrounds (eyes + eggshell glow
  must survive both).
- No detail thinner than 4 px at 512.

**Do-not list:** no text/letters/numbers, no arrows, no "FREE"/badges, no dice,
cards, chips, reels or coin stacks (no casino framing), no multiple creatures, no
Robux icon, no Roblox logo, no watermark, no traced or AI-imitated existing IP.

**Deliverables / order:** 2 icon directions (one character close-up, one
egg-with-eye-peeking concept), 2 revision rounds, layered source, commercial
rights warranted original. Loic uploads only (`PROJECT.md` S1/S4). Review at 512
and 128 px before accepting.

## 5. Thumbnail concept brief (for a Fiverr artist)

**Specs:** 1920×1080 primary PNG, plus 1280×720 crop and a layered source file.
Roblox ad test needs **three thumbnail variants** (`PROJECT.md` S6) — this brief
supplies one scene in three treatments (A/B/C below).

**Scene (original only):** dusk at the nursery plaza.
- **Left third / focal point:** an original young nursery keeper (gender-neutral,
  overalls and a canvas satchel, no branded clothing) running toward the viewer,
  cradling a **glowing egg**. Face lit warm from the egg.
- **Center:** the conveyor belt crossing the frame with three eggs — smallest far,
  largest near — the nearest egg **cracking with warm light**.
- **Right third:** a rival's walled base, one creature silhouetted on an income
  pad, a **padlock glyph** glowing above the gate. Keep it readable as "locked",
  not as a chest or a slot machine.
- **Background:** pastel plaza stalls and string lights, shallow depth of field.
  No readable invented text on signage (avoid accidental brand names).

**Focal hierarchy at thumbnail size:** the keeper's face + glowing egg first, the
rival base second, everything else third. Three focal elements maximum.

**Text — optional, one line:**
- Variant A: **no text** (primary; best for mobile).
- Variant B: **"STEAL THE RARE ONES"** (4 words), bottom-left, heavy rounded
  sans, cream on a dark scrim, ≤ 18% of frame height.
- Variant C: **no text, tighter crop** on the cracking egg + lock (tests the egg
  as the hook).
- Banned text: free, best, guaranteed, jackpot, lucky, 100%, "limited time",
  prices, odds numbers, fake UI ("Claim", "Buy now"), any competitor name.

**No casino framing:** no reels, dice, chips, roulette, spinning crates, "!"
bursts, payout tables, or fake purchase prompts. The egg crack is anticipation of
a creature, not a payout — show an eye or a glow, never a prize grid.

**Mobile legibility gate (artist must show):**
- 320×180 and 640×360 previews; key content inside the central 90%.
- Text ≥ 2.5% of frame height (48 px at 1920 width) and never below the safe
  area; check it survives Roblox's mobile crop.
- 4.5:1 contrast for the keeper and the egg against the background.
- Composition must still read with the bottom 15% dimmed (UI overlay).

**Consistency:** same creature silhouette, palette and rim-light language as the
icon — the two assets must read as one brand. Original art only: no Roblox logo,
no copied thumbnail styles, no existing creature franchises, no traced poses.

**Deliverables / order:** 3 treatments (A/B/C) from one scene, 2 revision rounds,
layered source, commercial rights warranted original. Loic uploads only.

## 6. Search keywords

Roblox discovery reads title, description and engagement — not a hidden tag
field. Copy is therefore written as sentences, not a keyword list. Genre metadata
in Creator Hub (Simulator) is separate and set by Loic.

| Keyword | In the description? | Where / note |
|---|---|---|
| hatch | **yes** | first sentence, "hatch zone", "hatches" |
| collect | **yes** | "collect-and-steal" |
| steal | **yes** | "collect-and-steal", paragraph 2 |
| simulator | **yes** | first sentence (genre word) |
| nursery | **yes** | first sentence, paragraph 2 |
| creature(s) | **yes** | throughout |
| egg(s) | **yes** | throughout, incl. disclosure line |
| coins | **yes** | paragraphs 1–2 |
| rebirth | **yes** | paragraph 3 |
| lock | **yes** | paragraph 2 |
| recover | **yes** | paragraph 2 |
| offline | **yes** | paragraph 3 |
| leaderboard | **yes** | paragraph 3 |
| daily | **yes** | paragraph 3 |
| rare | no | true (rarity tiers) but currently unclaimed; adding it must not imply odds |
| pets | no | creatures are placed, not following pets — avoid the false expectation |
| adopt | no | no adoption mechanic |
| trade | no | no trading |
| anime | no | art direction is pastel/round, not anime — avoid mismatch |
| lucky | no | luck boosts exist, but "lucky" drifts into chance framing |
| mutation | no | mutations do not exist in Hatchfall v1 (Q7) |
| raid | no | a steal takes one placed creature, not a base raid |
| idle / tycoon | no | offline accrual exists; it is not an idle or tycoon game |
| free / Robux / codes | **never** | false or policy-risky |

Do not stuff: the description already uses the core genre words naturally. If
search performance is weak after launch, add at most one true term ("rare") and
re-run the ledger check — never add a term the game does not honor.

## 7. Open items for Spark / Loic

1. **Creature-name collision screen** — prerequisite; no names in this copy until
   it is done.
2. **Cosmetics / passes / limited creatures** — omitted from the launch
   description. Add a claim-ledger row only if step 9 ships them before publish.
3. **Description length** — confirm the 1,000-character limit in Creator Hub and
   re-count after any edit.
4. **Tagline choice** — one line only; Spark picks or vetoes.
5. **Icon/thumbnail artist** — brief above is execution-ready; original-art and
   upload rules stay with Loic. Three thumbnail variants are required by the ad
   test, not optional.
6. **Ad copy lines** are a separate deliverable (`docs/marketing/` in the repo);
   this pack only covers the storefront.

## 8. Sources (dated)

- `PROJECT.md` — S3 Game 1, S4, S6, quick answers 2–3 (read 2026-10-18).
- `src/shared/Config.luau`, `src/data/shared/Template.luau`,
  `src/core/shared/{HatchFlow,RebirthFlow,DailyFlow,Grants}.luau`,
  `src/core/server/{StealService,RecoverService,LockService,IncomeService,OfflineIncomeService,LeaderboardService}.luau`
  (2026-10-18).
- `src/ui/shared/OddsData.luau`, `src/ui/client/OddsModal.luau` (2026-10-18).
- `docs/designs/phase-1-plan.md` — name-screen prerequisite, step 9 status
  (2026-10-18).
- `docs/questions-for-spark.md` Q7 — no mutations in Hatchfall v1 (2026-10-18).
