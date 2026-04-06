# Shminer Event Sim

## Architecture
Single-file HTML/JS/CSS app at `index.html`. No build system, no external JS dependencies (only Google Fonts CDN). Designed for GitHub Pages deployment.

## How to Run
Open `index.html` in a browser. No server needed.

## Key Code Sections
Search by function name - these are the stable anchors.
- **Game logic**: `parseUpgrades`, `eventTest`, `FullEventSim`, `avgEvent_short`, `GiveApproxWithUpgraded`
- **UI data**: `upgradeNames`, `maxLevels`, `prestigeUnlocked`, `capUpgIdx`, `CAP_OF_CAPS_IDX`
- **Persistence**: `saveState`/`loadState`/`exportState`/`importState` using localStorage key `shminer_state`. State format: `{upgrades, gemUp, prestigeCount, baseCost, currentResource}`
- **DOM rendering**: `buildUpgradesDOM`/`buildGemsDOM` (build-once) + `refreshUpgradeValues`/`refreshGemValues`/`renderPrestigeMilestone` (update-in-place)
- **Budget planner**: `budgetPlan`/`renderBudgetResults` - greedy optimizer using `avgEvent_short`

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
- `approxCache`: cached `avgEvent_short` results per upgrade, cleared on any state change
- `CAP_OF_CAPS_IDX`: T4[6] index - the upgrade that increases cap upgrade max levels
- `capUpgIdx`: index of the "+1 Tier X Upgrade Caps" upgrade within each tier
- `upgradeInstructions[tier][upgrade]`: functions that apply stat modifications, parameter `lv` = level
- `simResult` (in resourcesPerMinute): `[finalWave, finalSub, totalTime]` from avgEvent_short

## Simulation
- `FullEventSim`: 1000-run Monte Carlo, accurate but slow (~200ms)
- `avgEvent_short`: deterministic approximation, returns `[finalWave, finalSub, totalTime]`
- `resourcesPerMinute`: converts sim results to resource efficiency metric
- `expectedMultiplier(chance, mult)`: expected value of a random multiplier (used for 2x/5x currency)

## Performance Notes
- Do not rebuild the upgrade DOM on value changes - it breaks Tab focus. Inputs use stable IDs (`upg-inp-{ti}-{uj}`, `gem-inp-{i}`, `pres-input`).
- Budget planner caps at 600 iterations to avoid hanging
- Sim results panel uses CSS opacity transition (`.simulating` class) instead of innerHTML wipe to prevent flicker
- Hold-to-repeat uses pointer events (touch-compatible): 400ms delay, 80ms interval

## Data Source
[Official wiki](https://shminer.miraheze.org/wiki/Events), local copy at `events.md`.

## Roadmap
Tracked in GitHub Issues. Key items: #1 visual overhaul, #2 gem budget planner, #3 rewards tracker, #4 mobile layout.
