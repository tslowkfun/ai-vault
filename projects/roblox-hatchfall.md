---
title: Roblox — Hatchfall / Fuzmorph
updated: 2026-10-18
tags: [projects, roblox, hatchfall, fuzmorph, active]
---

# Roblox games — Hatchfall / Fuzmorph

## Objective
Maximize money on Roblox. Project budget **$500 total over ~2 months**; **$25.00 spent as of 2026-10-17** (Loic's own CheaperInference top-up — the project's only spend so far, against the $500 budget). Monetization is casual-sim (passes, products, bundles, cosmetics) with no casino framing, no feed mechanics, and no pay-to-win in ranked contexts.

## Titles (LOCKED)
- **Game 1 — Hatchfall** — collect-and-steal creature nursery sim.
- **Game 2 — Fuzmorph** — glitch-farm reskin of the same template.
- Game 3 Street Hoops — design-only, parked.

## Status (2026-10-18)
- **Step 6 "6-egg" COMPLETE** (belt-carry loop: spawn → claim → carry → finalize/tag, `6e.1`–`6e.6`); Studio harness closed green, all approved.
- Economy freeze **CLOSED** (Spark E1–E4 re-review approved). Save layer APPROVED WITH CHANGES — all applied. Phase 0 passed.
- **Step 7 — progression shell — BUILT + APPROVED (2026-10-17).** Build commit `13250b1`; Spark's post-build verdict **APPROVED** (all 11 spec changes present) recorded in `docs/reviews/spark-verdicts.md`; packet regenerated at the build commit. Non-blocking fix (stale `Grants.luau` header comment) applied in step-8 ITEM 0.
- **Step 8 — steal / lock / recover — BUILT (2026-10-17/18).** Approved by Spark's 2026-10-16 verdict; build order `8a→8e`, pure layer first, all gates green (16 Lune suites, all checkers, stylua, selene 0/0/0, both rojo targets):
  - `8a` `5edddee` — `StealFlow` pure policy + `Config.Steal` (`ReachStuds=12`, `RecoverWindowSeconds=1800`, `LockTiers` 10m/1h) + `NetTypes` provenance fields (`stolenFrom`/`stolenAt`) + Lune suite.
  - `8b` `9e4c5b9` — `StealService` + `StealRequest`/`StealResult` + victim `StealNotice` (same-commit `check_ui` Rule 10) + `DataService.save` persist wrapper.
  - `8c` `ca6aae7` — `LockService.grant` as the SOLE `lockExpiresAt` writer, read-only `LockState` push, `Validate.lockRequest` deleted (Q26).
  - `8d` `f632f6d` — `RecoverService` holder-first commit, free-once/day then 5000 coins, no Robux path, strict `recoverRequest`.
  - `8e` `1e016ca` — harness lock/steal-refusal scenario + eighth exclusions belt (probe must delegate to `LockService.grant`, never assign `lockExpiresAt`). Packet regenerated (`3199790`).
  - Remaining: Loic's in-Studio feel pass; the manual **2-client steal/recover path** gate (a real steal, the lock blocking a live attacker, the recovery round trip) is now folded into the step-10 closing gate — Studio run + republish + 2-client retest.
- **Step 9 — monetization — CLOSED (2026-10-18).** Creator Hub complete (3 passes + 13 products on sale, IDs recorded in the game repo); Lane B (Sonnet) build audited APPROVED by Spark; closing Studio harness run 270/270, profile restored.
- **Clean publish DONE (2026-10-18).** The default (publish-safe) build is in the Hatchfall place; the place stays PRIVATE until the Phase 4 launch.
- **2-client live test DONE (2026-10-18)** (PC main + iPhone alt, live private place): hatch loop works live. Findings: (a) live interaction gap — placed creatures had no in-world visual, no client could initiate a placed steal, recover had no UI (server mechanics all built); (b) belt "starvation" diagnosed as designed cap pressure (8.33 eggs in flight vs cap 8), not a leak; (c) the tag/board contradiction diagnosed — the tag never fired (spawn-protection refusals), the Steals rise was a placed steal. Both diagnoses accepted by Spark; fixes commissioned.
- **Step 10 — live interaction layer (2026-10-18).** Spec APPROVED WITH CHANGES by Spark (verdict in the game repo's `docs/reviews/spark-verdicts.md`); build running hands-off via the spark-bridge daemon (jobs 0012–0017: foundations, placed visuals, defect fixes, steal prompt, recover UI, panel exclusivity + checkers). Closing gate = Studio run + republish + 2-client retest.
- **Phase-2 content plan (2026-10-18).** Landed in this vault and audited APPROVED by Spark: Christmas event first (flagship Nivellan income ruled 160), the 10-row roster expansion lands AFTER the Christmas window closes (mid-January), seasonal pass deferred, cosmetics await Spark's NEW-4 spec.

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
