---
title: "Fuzmorph — Store Copy Draft (description, taglines, icon/thumbnail, keywords)"
status: draft
owner: "night-drafts-lane-c"
reviewer: "Spark (claims/policy) + Loic (game direction, uploads)"
created: 2026-10-18
repo: roblox-games
scope: "Copy only. No code, no Config edits, no product live, no feature invented."
tags: [projects, fuzmorph, glitchlings, marketing, store-copy, draft]
related:
  - "projects/fuzmorph-mutations-design-draft.md (mutations: DESIGN STAGE, not shipped)"
  - "projects/hatchfall-store-copy-draft.md (the claim-ledger pattern this follows)"
  - "PROJECT.md (Game 2 — Glitchlings pitch, production order)"
  - "src/content/shared/Creatures.luau (glitchlings pack is registered and EMPTY)"
  - "docs/designs/game-designs-v1.md (Game 2 product ladder — unruled for Game 2)"
---

# Fuzmorph — store copy draft (DRAFT)

**Status: DRAFT. Nothing here is approved copy, and nothing here is publishable
yet.** Lane C, vault only. No repo changes were made for this note. Facts dated
**2026-10-18** from the canonical repo `tslowkfun/roblox-games`.

## 0. Pre-publication gates (read before the copy)

This note is written under **invent-no-features** discipline: it may only say
what the **shared template actually does today**, plus one clearly-marked
statement that mutations are **in design and not playable**. Six gates stand
between this draft and a storefront:

| # | Gate | State on 2026-10-18 |
|---|---|---|
| G1 | **The game exists** | **No.** Game 2 ships after Game 1's gate (`PROJECT.md` production order, Phase 5). `Creatures.luau` registers the `glitchlings` pack **intentionally empty** — there is no content, no egg, no product, nothing to play. |
| G2 | **Game name decided** | **No.** The Q7 ruling says **Fuzmorph**; `game-designs-v1.md` says **GLITCHLINGS**. The mutations draft proposes Fuzmorph = game name, `glitchlings` = content-pack id, and flags it unresolved. **This draft uses "Fuzmorph" as a working name only.** A store page cannot ship on a working name. |
| G3 | **Creature names cleared** | **No.** No creature name appears in this copy — deliberately. Names are UNVERIFIED and the collision screen has not run for Game 2 (six of Game 1's sixteen collided). |
| G4 | **Mutation odds approved** | **No.** Tier names, chance table, multipliers and the 79-Robux Mutation Boost are all **PROPOSED** in the mutations draft. Per Q7, **no Mutation product is sellable before disclosure exists** — and disclosure does not exist. |
| G5 | **Game 2 product ladder ruled** | **No.** `game-designs-v1.md` lists Game 2 passes, Trap Packs, Shield/Lock, Mutation Boost 79, Luck Boost, Recover Stolen, Cash Packs — none of it is approved for Game 2. Treat every one as unruled. |
| G6 | **Traps** | **No.** Traps are in the Game 2 pitch, not in the shared template, and not built. **Never claimable**, in any tense. |

**Consequence:** this copy's job is to be *accurate when it finally ships*, not to
sell today. Anything that would need a feature to exist gets cut, not hedged.

## 1. Claim ledger — every statement, traced

| Copy claim | Source (2026-10-18) | Status |
|---|---|---|
| Hatch creatures from eggs on a belt, bought with in-game coins | `BeltService`, `Config.Economy.Eggs` (shared template) | **BUILT** (template) |
| Carry the egg to your own zone and hatch it | `CarryService`, `FinalizeService`, `HatchFlow` | **BUILT** (template) |
| Place creatures to earn income | `BaseService`, `IncomeService` | **BUILT** (template) |
| 6 pads → 30 pads | `Config.Base.DefaultSlots / MaxSlots` | **BUILT** (template) |
| Up to 10 owners per server ("9 others") | `Config.Map.PlayerCap = 10` | **BUILT** (template) |
| Steal a **placed** creature from another farm | `StealService` / `StealFlow` (creature transfer only) | **BUILT** (template) |
| Lock your base | `LockService.grant`, tiers 10m/1h | **BUILT** (template); **products unruled for Game 2 (G5)** |
| Recover: first per day free, extras cost coins, 30-minute window | `RecoverService`, `Config.Steal` | **BUILT** (template) |
| Rebirth resets the farm for a permanent multiplier | `RebirthFlow` | **BUILT** (template) |
| Timed boosts (Luck, Income) | `Config.Boosts` — 15m/1h readers, income-affecting category is Income only | **BUILT** (template, as a system); **no Game 2 product approved** |
| Display-only leaderboards, no prizes | `LeaderboardService`, `Config.Leaderboards` | **BUILT** (template) |
| Offline income at half rate, 8h cap | `OfflineIncomeService`, `Config.OfflineIncome` | **BUILT** (template) |
| Exact odds — base and boosted — shown in-game before claiming | `OddsModal`, `OddsData.computeDisclosure` | **BUILT** (template) |
| Mutations exist in this game | `Questions Q7 CLOSED = Option B` (mutations exist for Game 2 **by design**) but the system is **design-stage only** and `Config.Boosts.Mutation` is inert | **DESIGN STAGE — never "shipped", never "playable"** |

Deliberately **not claimed anywhere in this copy** (absent, unbuilt, or unruled):

- **traps / defending with traps** (G6 — not built, not in the template)
- **mutations as a playable feature** (G4 — design stage; see §3.2 for the one
  permitted sentence and why it is still not publishable)
- **mutation boost**, any mutation product, any mutation odds number or tier name
- passes (2x Income, Auto-Collect, Extra Slots, Steal Speed, VIP, Season Pass)
- Trap Packs, Shield/Lock products, Recover Stolen product, Cash Packs
- event objectives, seasonal content, limited creatures, cosmetics
- trading, pets that follow you, ranked/PvP ladders, prizes, "free",
  "guaranteed", any Robux price, any numbers about odds
- any comparison to another game by name

If any of the above is reintroduced later, it needs **a new ledger row, a green
gate, and Spark sign-off** — not a copy edit.

## 2. Game description (Draft A — the one I recommend)

Roblox's description limit at draft time is 1,000 characters — **verify in
Creator Hub**; this draft is **912 characters including line breaks**. Em dashes
are used instead of `--`. No emoji. No mutation claim (§3.2 explains why).

```text
Fuzmorph is a collect-and-steal farm sim set in an arcade city. Eggs ride the conveyor — buy one with in-game coins, carry it to your zone, and hatch it. Place what you hatch on income pads, collect coins, and grow your farm from 6 pads to 30.

You share the server with up to 9 other farm owners. Slip into a rival's farm and steal a placed creature, or lock your farm and keep what is yours. Stolen creatures can be recovered: your first recovery each day is free, later ones cost coins, and the window closes 30 minutes after the theft.

Rebirth resets your farm for a permanent income multiplier, so each run pays back faster. Daily rewards and offline income (half rate, up to 8 hours) keep progress moving. Leaderboards are display-only — bragging rights, no prizes.

Every egg's exact hatch odds — base and boosted — are shown in-game before you claim. Eggs cost in-game coins. No hidden numbers.
```

Draft notes:

- **First 150 characters carry the genre** ("collect-and-steal farm sim") because
  the title carries no genre keyword and descriptions truncate on some surfaces.
- **"Farm", not "nursery".** Game 1 is a nursery; Game 2 is a glitch **farm** in
  an arcade city (`PROJECT.md`). Sharing vocabulary across two games makes both
  descriptions interchangeable, so the words that differ stay different.
- The disclosure sentence is **mandated, not marketing**: odds exact, in-game,
  base + boosted; eggs cost in-game coins. Do not soften it, and do **not** add
  "no Robux" — coin packs are sold for Robux, so that would read as misleading.
- **No creature names, no tier names, no prices, no odds numbers** in copy. Names
  and money facts live in content and in-game; copy that quotes them rots and
  drifts.
- **No mutation sentence.** A storefront is a claim about a game you can play
  today. Mutations cannot be played today (§3.2).

## 3. Mutations — the honest sentence, and where it goes

### 3.1 What is true

Mutations are **designed** for Game 2 (Q7 CLOSED, Option B: mutations exist,
chance is disclosed base **and** boosted, and **no Mutation product is sellable
before that disclosure exists**). The design — claim-time roll, one of three
tiers, an income multiplier, baked income at mint — exists **as a design draft**.
`Config.Boosts.Mutation` is present in config but **inert**; no odds table, no
overlay art, no product, no disclosure, no test. `glitchlings` content is empty.

So the truthful tenses are: *designed*, *in design*, *not playable yet*. The
false ones are: *exists*, *hatch mutated creatures*, *get Gold / Neon /
Glitched*, *mutation boost*, or any percentage.

### 3.2 The one permitted sentence (and why it still does not ship)

If Loic wants mutations present in the storefront as a signal of direction, the
only acceptable shape is a **clearly future-tense, non-playable** line, placed
last, never in the first paragraph, and never with a number:

> `Mutations are in design for a later update. They are not playable yet, and they are not part of this description.`

That sentence is honest. It is also **not publishable under this draft**, for
three reasons: (1) G1 — there is no game to attach it to; (2) a store page is a
promise surface, and an "in design" line belongs in a devlog or Discord, not in
the place players decide whether to spend; (3) Q7's gate makes any mutation
*mention* in a monetized surface a policy magnet — the moment a player reads
"mutations", the next question is odds, and there is no odds table to show.

**Recommendation: Draft A ships without it.** Revisit only when mutations are
built, disclosed, and Spark has signed the sellability gate.

### 3.3 What the copy must say instead

Nothing. Absence is the honest form of "not shipped". The description already
stands on six built systems. Do not compensate with adjectives ("chaotic",
"glitchy", "unstable machine") that imply a mechanic that is not there — the
*look* may be glitchy in art, but the paragraph may not promise a glitch
**rule**.

## 4. Five taglines (pick one; do not stack)

| # | Tagline | Best use | Veto check |
|---|---|---|---|
| 1 | **Hatch it. Place it. Guard it.** | Ad line / first line of a post | Three real verbs from the built loop; no claim |
| 2 | **Your farm. Your lock. Your problem.** | Short promo / Discord | Lock is real; edgy tone; no power claim |
| 3 | **Something fused. Something yours.** | Brand line / icon pairing | Describes the *look* only (object-animal hybrids), promises no mechanic |
| 4 | **Steal a creature. Make a rival.** | Ad copy for PvP-curious players | Accurate: one creature, not a farm raid |
| 5 | **Six pads to thirty. One belt between.** | Thumbnail alt text / social | Exact template numbers; no "grind" promise |

Banned in all taglines: free, best, guaranteed, jackpot, lucky, insane, 100%,
limited time, "rarest", "OP", "win", "mutate", "trap", any Robux/price mention,
any other game named.

## 5. Icon concept brief (for a Fiverr artist)

**Specs:** 1024×1024 master PNG + 512×512 export (Roblox icon), sRGB, full-bleed
(no baked rounded-corner mask), layered source (PSD/Procreate), **no text**.

**Subject (original only):** one creature, head-and-shoulders, 75–80% of frame.
Game 2's look is **everyday objects fused with animals** (`PROJECT.md`): brief a
hybrid of a **small desk fan** and a **round, wide-mouthed animal** — fan-blade
ear fins that are visibly *not* spinning, a soft plastic body in **mint green and
warm grey**, one big off-centre amber eye, a single **offset seam of geometry**
across one cheek as the only "glitch" cue. Nothing may resemble an existing
franchise, meme character, mascot, or real product brand; no logos on the object
part; no clothing.

**Composition:** face centred, eye on the upper third, 8% safe margin,
three-quarter view looking at the viewer. Rim light in a cool arcade neon (cyan
`#5CE1E6`) against a deep indigo vignette with a faint pixel-grid bokeh — no
props that read as text, no UI frames, no fake badges.

**Palette:** mint green, warm grey, amber eye, cyan rim + indigo background. Three
colours plus one accent, maximum. No rainbow gradients, no CRT-scanline filter
over the whole image (it dies at 48 px).

**Text:** **none** — the icon renders at 48–150 px and Roblox shows the title
separately; text-heavy icons read as clickbait (`PROJECT.md` S6).

**Mobile legibility gate (artist must show):**
- 512×512, 128×128, 48×48 previews.
- Silhouette test: solid black fill — still readable as a creature with ear fins.
- Contrast test on pure white and pure black: eye + rim light must survive both.
- The offset seam must be **one** visible element at 48 px, not a texture.

**Do-not list:** no text/letters/numbers, no arrows, no "FREE"/badges, no dice,
cards, chips, reels or coin stacks (no casino framing), no multiple creatures, no
Robux icon, no Roblox logo, no watermark, no traced or AI-imitated existing IP, no
mutation-tier colour coding (gold/neon/glitch palettes are an unapproved system —
an icon that implies tiers implies a feature).

**Deliverables / order:** 2 directions (one character close-up, one
object-first hybrid where the fan reads before the face), 2 revision rounds,
layered source, commercial rights warranted original. **Loic uploads only.**

## 6. Thumbnail concept brief (for a Fiverr artist)

**Specs:** 1920×1080 primary PNG + 1280×720 crop + layered source. The ad test
needs **three variants** (`PROJECT.md` S6) — one scene, three treatments (A/B/C).

**Scene (original only):** a night-time arcade-city farm.
- **Left third / focal point:** an original young farm keeper (gender-neutral,
  hoodie and a tool satchel, no branded clothing) carrying a **glowing egg**
  toward their own zone. Face lit warm from the egg.
- **Centre:** the conveyor crossing the frame with three eggs, smallest far and
  largest near; the nearest egg **cracking with warm light**.
- **Right third:** a rival's walled farm, one creature silhouetted on an income
  pad, a **padlock glyph** glowing above the gate. Must read as "locked" — not a
  chest, not a slot machine, not a trap. **No trap imagery**: traps are not built.
- **Background:** neon arcade signage, shallow depth of field, no readable invented
  text on signs (avoids accidental brand names).

**Focal hierarchy at thumbnail size:** keeper + glowing egg first, rival farm
second, everything else third. Three focal elements maximum.

**Text — optional, one line:**
- Variant A: **no text** (primary; best on mobile).
- Variant B: **"STEAL THE GOOD ONES"** (4 words) bottom-left, heavy rounded sans,
  cream on a dark scrim, ≤ 18% of frame height.
- Variant C: **no text, tighter crop** on the cracking egg + lock.
- Banned: free, best, guaranteed, jackpot, lucky, 100%, "limited time", prices,
  odds numbers, "MUTATED"/tier names, fake UI ("Claim", "Buy now"), competitor
  names.

**No casino framing:** no reels, dice, chips, roulette, spinning crates, "!"
bursts, payout tables, fake purchase prompts. The crack is anticipation of a
creature, not a payout — show an eye or a glow, never a prize grid.

**Mobile legibility gate (artist must show):**
- 320×180 and 640×360 previews; key content inside the central 90%.
- Text ≥ 2.5% of frame height (48 px at 1920) and inside the safe area; verify it
  survives Roblox's mobile crop.
- 4.5:1 contrast for keeper and egg against background.
- Composition still reads with the bottom 15% dimmed (UI overlay).

**Consistency:** same creature silhouette, palette and rim-light language as the
icon — the two must read as one brand, not two games.

**Deliverables / order:** 3 treatments from one scene, 2 revision rounds, layered
source, commercial rights warranted original. **Loic uploads only.**

## 7. Search keywords

Roblox discovery reads title, description and engagement — not a hidden tag
field, so copy is written as sentences. Genre metadata in Creator Hub (Simulator)
is separate and set by Loic.

| Keyword | In the description? | Note |
|---|---|---|
| hatch | **yes** | "hatch it" |
| collect | **yes** | "collect-and-steal" |
| steal | **yes** | "collect-and-steal", paragraph 2 |
| sim / simulator | **yes** | "farm sim" (first sentence) |
| farm | **yes** | first sentence, paragraph 2 — Game 2's word, not "nursery" |
| creature(s) | **yes** | throughout |
| egg(s) | **yes** | throughout, incl. disclosure line |
| coins | **yes** | paragraphs 1–2 |
| rebirth | **yes** | paragraph 3 |
| lock | **yes** | paragraph 2 |
| recover | **yes** | paragraph 2 |
| offline | **yes** | paragraph 3 |
| leaderboard | **yes** | paragraph 3 |
| arcade | **yes** | first sentence (setting, true in art and framing) |
| mutation | **no** | design stage only (G4) — a searched word with nothing behind it |
| trap / defend | **no** | not built (G6) |
| pets | no | creatures are placed, not following pets |
| trade | no | no trading |
| adopt | no | no adoption mechanic |
| lucky | no | luck boosts exist in template, but "lucky" drifts into chance framing |
| glitch | no | true of the *art*; as a keyword it implies a mechanic — do not stuff |
| raid | no | a steal takes one placed creature, not a farm raid |
| idle / tycoon | no | offline accrual exists; it is neither |
| free / Robux / codes | **never** | false or policy-risky |

Do not stuff. If search underperforms after launch, add at most **one** true term
and re-run the ledger check — never a term the game does not honour.

## 8. What this copy may never say (one-line rule)

**If a sentence needs a system that is not built, the sentence is cut, not
hedged.** Mutations are the only exception in shape (a future-tense line is
permissible to *draft*) and no exception in practice (§3.2: it does not ship in
this draft). Traps have no exception at all.

## 9. Open items for Spark / Loic

1. **Game name** — Fuzmorph vs GLITCHLINGS (G2). Copy cannot ship, and no
   storefront asset can be ordered, until it is one name.
2. **Ship timing** — Game 2 is post-Game-1-gate (Phase 5). Is this copy held
   until then, or is its purpose to be ready? Recommendation: hold, re-ledger
   when the game is real.
3. **Draft A vs the §3.2 line** — recommendation is Draft A with no mutation
   sentence. If Loic wants the direction signal, it belongs in a devlog, not the
   store page.
4. **Creature-name collision screen for Game 2** — prerequisite, and no names in
   copy until it runs (G3).
5. **Description length** — confirm the 1,000-character limit in Creator Hub and
   re-count after any edit.
6. **Tagline choice** — one line, Spark picks or vetoes.
7. **Art order** — icon + three thumbnail treatments are one order; original-art
   and upload rules stay with Loic.
8. **Vocabulary split** — confirm "farm" (Game 2) vs "nursery" (Game 1) so the two
   store pages never read as the same product.

## 10. Sources (dated 2026-10-18)

- `PROJECT.md` — Game 2 gloss (genre, look, core loop, 3–10 min sessions),
  production order ("reskin and extend into Game 2: traps + mutations"), S4 rules,
  S6 ad/thumbnail rules, quick answers 2–3.
- `docs/designs/game-designs-v1.md` — Game 2 pitch and product ladder (unruled for
  Game 2; used only to know what **not** to claim).
- `docs/questions-for-spark.md` Q7 — CLOSED, Option B; mutations for Game 2, no
  Mutation product before disclosure; Hatchfall keeps `mutation == nil`.
- `docs/reviews/spark-verdicts.md` — STEP-9 verdict (Q52–Q65), incl. the
  no-Robux-recovery ruling and Q64.
- `projects/fuzmorph-mutations-design-draft.md` — mutations as **design stage**:
  claim-time roll, three tiers (PROPOSED), sellability gate.
- `src/content/shared/Creatures.luau` — `glitchlings` pack registered, **empty**.
- `src/shared/Config.luau` — `Boosts.{Luck,Mutation,Income}`, `Base.DefaultSlots/
  MaxSlots`, `Map.PlayerCap`, `Steal`, `OfflineIncome`, `Leaderboards`.
- `src/core/server/{StealService,RecoverService,LockService,IncomeService,
  OfflineIncomeService,LeaderboardService}.luau`, `src/core/shared/
  {HatchFlow,RebirthFlow,DailyFlow}.luau`, `src/ui/shared/OddsData.luau`,
  `src/ui/client/OddsModal.luau`.
- `projects/hatchfall-store-copy-draft.md` — the claim-ledger pattern and the
  banned-word list this note reuses.
