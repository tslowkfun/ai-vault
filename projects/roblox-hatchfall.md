---
title: Roblox — Hatchfall / Fuzmorph
updated: 2026-10-17
tags: [projects, roblox, hatchfall, fuzmorph, active]
---

# Roblox games — Hatchfall / Fuzmorph

## Objective
Maximize money on Roblox. Project budget **$500 total over ~2 months**; **$0 spent** as of 2026-10-16. Monetization is casual-sim (passes, products, bundles, cosmetics) with no casino framing, no feed mechanics, and no pay-to-win in ranked contexts.

## Titles (LOCKED)
- **Game 1 — Hatchfall** — collect-and-steal creature nursery sim.
- **Game 2 — Fuzmorph** — glitch-farm reskin of the same template.
- Game 3 Street Hoops — design-only, parked.

## Status (2026-10-17)
- **Phase 1 in progress.** Step-6 **"6-egg" belt-carry egg loop**:
  - `6e.1` plot ownership ✅ built; `6e.2` belt + readability ✅ built.
  - `6e.3` claim (money path) ✅ **BUILT + APPROVED** — Studio harness green (38/38 at the time).
  - `6e.6` odds disclosure ✅ **APPROVED WITH CHANGES**, applied.
  - `6e.4` carry + finalize ✅ **BUILT + APPROVED WITH CHANGES**, all four changes applied. **Closed green in Studio: 53/53 assertions**, pinned order `claim → finalize → offline → placement`, profile restored.
  - `6e.5` tag steal (steal-during-carry) — **BUILT + APPROVED WITH CHANGES** (Q39–Q44), all applied. **Studio harness CLOSED green: 85/85 assertions**, pinned order `claim → tag → finalize → offline → placement`, profile restored. Remaining: Loic's feel pass + the true 2-client tag in the step-8 multi-player pass.
  - **Step 6 "6-egg" COMPLETE.** The whole belt-carry loop (spawn → claim → carry → finalize/tag) is built and approved; the two remaining items (Loic's in-Studio feel pass, the true 2-client tag) ride step 8's multi-player pass.
- **Step 7 — progression shell — SPEC COMMISSIONED (2026-10-17).** Deliverable `docs/reviews/step7-progression-shell-spec.md` (SPEC ONLY; no build). Covers rebirth UI + handler on the frozen `Economy` rebirth, daily rewards (7-day UTC, day-7 free Luck Potion), first-run tutorial, and display-only leaderboards. Open questions **Q45–Q51** await Spark's verdict; the build is authorised only by that verdict.
- Economy freeze **CLOSED** (Spark E1–E4 re-review approved).
- Save layer APPROVED WITH CHANGES — all applied. Phase 0 passed.

## Gates summary (PROJECT.md §5)
- Scale if D1 retention ≥ 25%, avg session ≥ 10 min, ≥ 70% of new players pass minute 5, buyers ≥ 2%.
- Kill/redesign if D1 < 15%, session < 6 min, or buyers < 0.5%.
- **Never judge on visits alone.**
- Phases strictly sequential: 0 (skeleton) → 1 (template v1) → 2 (Game 1 content) → 3 (internal test) → 4 (soft launch + ad test) → **gate (Loic + Spark)** → 5 (Game 2) → 6 (phase-2 features).

## Canonical detail — do not copy here
- Repo: **`tslowkfun/roblox-games`** (canonical). Local: `C:\dev\roblox-games`.
- Master instructions: `PROJECT.md` in that repo (wins on any conflict).
- Build plan + gates: `docs/designs/phase-1-plan.md`.
- Test checklists: `docs/tests/`. Open rulings: `docs/questions-for-spark.md`.
