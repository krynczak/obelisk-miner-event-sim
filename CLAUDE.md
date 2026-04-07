# Obelisk Miner Event Sim

## Architecture
Single-file HTML/JS/CSS app at `index.html`. No build system, no external JS dependencies (only Google Fonts CDN). Deployed via GitHub Pages.

## How to Run
Open `index.html` in a browser. No server needed.

## Key Code Sections
Search by function name - these are the stable anchors.
- **Game logic**: `parseUpgrades`, `eventTest`, `FullEventSim`, `avgEvent_short`, `GiveApproxWithUpgraded`, `mcGainForUpgrade`, `quickSim`
- **UI data**: `upgradeNames`, `maxLevels`, `prestigeUnlocked`, `capUpgIdx`, `CAP_OF_CAPS_IDX`
- **Persistence**: `saveState`/`loadState`/`exportState`/`importState` using localStorage key `shminer_state`
- **DOM rendering**: `buildUpgradesDOM`/`buildGemsDOM` (build-once) + `refreshUpgradeValues`/`refreshGemValues`/`renderPrestigeMilestone` (update-in-place)
- **Budget planner**: `budgetPlan`/`renderBudgetResults`/`budgetBuy` - greedy optimizer with hybrid gain (deterministic + MC cache fallback)
- **UX**: `toggleHelp`, `openChangelog`/`closeChangelog`, `showOnboard`/`dismissOnboard`, `prestigeReset`

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
- `approxCache`: cached gain values (number, not simResult) per upgrade key `{ti}_{uj}`, cleared on any state change. Populated by `refreshUpgradeValues`, consumed by both Next Buy and Budget Planner.
- `CAP_OF_CAPS_IDX`: T4[6] index - the upgrade that increases cap upgrade max levels
- `capUpgIdx`: index of the "+1 Tier X Upgrade Caps" upgrade within each tier
- `upgradeInstructions[tier][upgrade]`: functions that apply stat modifications, parameter `lv` = level
- `GAIN_SIM_RUNS`: number of MC runs for `quickSim`/`mcGainForUpgrade` (currently 200)

## Gain Calculation (hybrid system)
The core challenge: `avgEvent_short` (deterministic) gives gain=0 for HP/prestige upgrades when the player already one-shots enemies. Monte Carlo captures these marginal gains via random crits/blocks.

- `GiveApproxWithUpgraded(ti,uj)`: deterministic sim with +1 level. Used for initial gain detection.
- `mcGainForUpgrade(ti,uj)`: 200 paired MC runs (base vs upgraded). Fallback when deterministic shows 0.
- `refreshUpgradeValues`: populates `approxCache` with hybrid gains (det first, MC fallback). Source of truth for both Next Buy and Budget Planner.
- Budget Planner: uses deterministic gain from its mutated `levels` state, falls back to `approxCache` (MC-based) when det=0.

## DOM Pattern
Inputs are built once with stable IDs (`upg-inp-{ti}-{uj}`, `gem-inp-{i}`, `pres-input`). Values update in-place via `refreshUpgradeValues()`. Do not rebuild the upgrade DOM on value changes - it breaks Tab focus.

## Performance Notes
- Budget planner caps at 600 iterations to avoid hanging
- Sim results panel uses CSS opacity transition (`.simulating` class) instead of innerHTML wipe to prevent flicker
- Hold-to-repeat uses pointer events (touch-compatible): 400ms delay, 80ms interval
- Cap upgrades skip MC simulation when no upgrade in the tier is at max level

## Data Source
[Official wiki](https://shminer.miraheze.org/wiki/Events), local copy at `events.md`.

## Roadmap
Tracked in GitHub Issues.
