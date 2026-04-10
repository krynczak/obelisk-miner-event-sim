# Architecture

## Key Code Sections
Search by function name - these are the stable anchors.
- **Game logic**: `parseUpgrades`, `eventTest`, `FullEventSim`, `analyticalSim`, `exactHtkMoments`
- **Gain system**: `computeUpgradeGain`, `computeGainFromLevels`, `computeCapCompoundGain`, `computeCapOfCapsCompoundGain`
- **UI data**: `upgradeNames`, `maxLevels`, `prestigeUnlocked`, `capUpgIdx`, `CAP_OF_CAPS_IDX`, `upgradeType`
- **Persistence**: `saveState`/`loadState`/`exportState`/`importState` using localStorage key `shminer_state`
- **DOM rendering**: `buildUpgradesDOM`/`buildGemsDOM` (build-once) + `refreshUpgradeValues`/`refreshGemValues`/`renderPrestigeMilestone`/`renderPrestigeAdvice` (update-in-place)
- **Budget planner**: `budgetPlan`/`renderBudgetResults`/`budgetBuy` - greedy optimizer with 2-step lookahead
- **UX**: `toggleHelp`, `openChangelog`/`closeChangelog`, `showOnboard`/`dismissOnboard`, `prestigeReset`
- **Validation**: `validateAnalyticalSim` (console), `compareBudgetStrategies` (console)

## Game Mechanics
- Upgrades are **sequential**: each requires the previous upgrade in the tier to have at least level 1
- Cap upgrades (+1 Tier X Upgrade Caps) only have value when a tier upgrade is at its max level
- Prestige resets tier upgrades, increases HP/damage multiplier by 10% per tier

## Game Formula Reference
- Upgrade cost: `baseCost * 1.25^(level-1)`, rounded
- Prestige multiplier: `1 + prestigeBonusScale * prestigeCount`
- Prestige wave requirement: `prestige * 5 + 5` (P1=W10, P2=W15, P10=W55)
- Gem 3 event speed: `+1.25 per gem level` (not +1.0)
- T1[4] event speed: `+3% per level` (not +2%)
- T3[3] event speed: `+5% per level` (not +3%)
- Currency formula: `(w*w+w)/2 * multiplier` where w = floor(wave/waveReq)
- Number formatting: variable decimals (>=100: 0dp, >=10: 1dp, <10: 2dp) to match game display

## Key Variable Names
- `approxCache`: cached gain values (number) per upgrade key `{ti}_{uj}`, cleared on any state change. Populated by `refreshUpgradeValues`, consumed by both Next Buy and Budget Planner. Key `'base'` stores the baseline `analyticalSim` result `[wave, sub, time]`.
- `CAP_OF_CAPS_IDX`: T4[6] index - the upgrade that increases cap upgrade max levels
- `capUpgIdx`: index of the "+1 Tier X Upgrade Caps" upgrade within each tier
- `upgradeInstructions[tier][upgrade]`: functions that apply stat modifications, parameter `lv` = level
- `upgradeType[tier][upgrade]`: pre-computed classification: `'combat'`, `'speed'`, `'resource'`, `'prestige'`, `'mixed'`, `'cap'`
- `offenseOnly[tier][upgrade]`: pre-computed flag for pure offense upgrades (gain=0 when player already one-shots)

## Gain Calculation (analytical system)
The gain system uses `analyticalSim` - a CLT-based (Central Limit Theorem) analytical simulation that computes expected wave, subzone, and time deterministically.

### analyticalSim
Computes expected wave by tracking cumulative damage moments (E[S], Var[S]) across subzones. Uses Gaussian survival probability `P(survive) = Phi((HP - cumE) / sqrt(cumVar))` to determine expected wave without randomness. Returns `[expectedWave, sub, time]`.

Key internals:
- `exactHtkMoments`: exact E[htk] and Var[htk] via binomial player crit distribution (replaces ceil approximation)
- Enemy crit damage uses `round()` to match Monte Carlo behavior
- CLT bias: ~1.4 wave underestimation at high-wave states (Gaussian tail for bounded distributions). Cancels in gain deltas.

### Upgrade type classification
Pre-computed at startup (like `offenseOnly`):
- `combat`: directly affects wave via analyticalSim (atk, hp, crit, block, enemy stats)
- `speed`: only affects time (walkSpeed, gameSpeed) - indirect value via time savings
- `resource`: only affects drop rates (x5money, x2money) - indirect value via extra resources
- `prestige`: only affects prestigeBonusScale - at P=0, simulated with P=1
- `mixed`: combat + speed (e.g., T4[3] +atkSpeed +walkSpeed) - atkSpeed held constant for combat gain to avoid overcrediting
- `cap`: cap upgrades - compound gain from all maxed upgrades that +1 cap unlocks

### 2-pass gain computation
`refreshUpgradeValues` populates `approxCache` in two passes:
1. **Pass 1**: combat + prestige (P>0) gains via `analyticalSim` delta
2. **Pass 2**: speed, resource, mixed, cap, prestige (P=0) gains using `avgCombatGain` from pass 1

### Cap compound gain
- Regular caps: sum of wave gains from each maxed upgrade unlocked by cap+1
- Cap of Caps (T4[6]): cascade depth=2 (T4[6]+1 -> cap upgrades -> regular upgrades)
- Post-increment guard: only counts upgrades whose max actually increased

## Budget Planner
Greedy optimizer with 2-step lookahead. Each iteration:
1. Evaluate all affordable/available upgrades via `computeGainFromLevels`
2. For the first 20 iterations (and when top gains are close): simulate buying each of the top 5 candidates, find best follow-up, compare pair scores
3. Override greedy if alternative 2-step path is >=5% better
4. Buy the selected upgrade, deduct cost, repeat (up to 600 iterations)

`computeGainFromLevels` mirrors `computeUpgradeGain` but operates on a local `levels` array instead of global `upgrades`.

## Prestige Advisor
Uses the official wiki's optimal prestige milestones: P1, P2, P5, P10, P20, P40.
Compares current wave against the next milestone's wave requirement. After P40, targets wave 250 (last reward).

## DOM Pattern
Inputs are built once with stable IDs (`upg-inp-{ti}-{uj}`, `gem-inp-{i}`, `pres-input`). Values update in-place via `refreshUpgradeValues()`. Do not rebuild the upgrade DOM on value changes - it breaks Tab focus.

## Display Simulation
`FullEventSim(p, e, 1000)` runs 1000 Monte Carlo event simulations for the results panel (worst/avg/best wave, currencies per run, time). This is separate from the gain calculation system.

## Performance Notes
- Budget planner caps at 600 iterations to avoid hanging
- Sim results panel uses CSS opacity transition (`.simulating` class) instead of innerHTML wipe to prevent flicker
- Hold-to-repeat uses pointer events (touch-compatible): 400ms delay, 80ms interval
- `analyticalSim` is ~0.3ms per call (~20x faster than 200 MC paired runs)

## Validation Harnesses (console only)
- `validateAnalyticalSim(mcRuns)`: compares analyticalSim vs FullEventSim for 7 game states. Pass threshold: <0.5 wave delta.
- `compareBudgetStrategies(budgets)`: compares greedy vs 2-step lookahead. Reports wave, iterations, overrides, timing.

## Data Source
[Official wiki](https://shminer.miraheze.org/wiki/Events), local copy at `events.md`.
