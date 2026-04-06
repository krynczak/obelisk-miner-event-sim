# Czak Event Sim

## Architecture
Single-file HTML/JS/CSS app at `index.html`. No build system, no external JS dependencies (only Google Fonts CDN). Designed for GitHub Pages deployment.

## How to Run
Open `index.html` in a browser. No server needed. GitHub Pages deployment: Settings > Pages > main branch / root.

## Key Code Sections
Line numbers are approximate - search for function names if they drift.
- **Game logic** (~190-340): `parseUpgrades`, `eventTest`, `FullEventSim`, `avgEvent_short`, `GiveApproxWithUpgraded`
- **UI data** (~340-370): `upgradeNames`, `maxLevels`, `prestigeUnlocked`, `capUpgIdx`
- **Persistence** (~370-385): `saveState`/`loadState` using localStorage key `shminer_state`. State format: `{upgrades, gemUp, prestigeCount, baseCost, currentResource}`
- **DOM rendering** (~400-600): build-once pattern (`buildUpgradesDOM`/`buildGemsDOM`) + update-in-place (`refreshUpgradeValues`/`refreshGemValues`)
- **Budget planner** (~620-680): greedy optimizer using `avgEvent_short` for fast gain estimation

## Game Formula Reference
- Upgrade cost: `baseCost * 1.25^(level-1)`
- Prestige multiplier: `1 + prestigeBonusScale * prestigeCount`
- Gem 3 event speed: `+1.25 per gem level` (not +1.0)
- T1[4] event speed: `+3% per level` (not +2%)
- T3[3] event speed: `+5% per level` (not +3%)
- Materials formula: `(w*w+w)/2 * multiplier` where w = floor(wave/waveReq)

## DOM Pattern
Inputs are built once with stable IDs (`upg-inp-{ti}-{uj}`, `gem-inp-{i}`, `pres-input`). Values update in-place via `refreshUpgradeValues()`. Never rebuild the upgrade DOM on value changes - it breaks Tab focus.

## Simulation
- `FullEventSim`: 1000-run Monte Carlo, accurate but slow (~200ms)
- `avgEvent_short`: deterministic approximation, used for per-upgrade gain calculations and budget planner
- `resourcesPerMinute`: converts wave/time results to resource efficiency metric

## Performance Notes
- Budget planner caps at 600 iterations to avoid hanging
- Sim results panel uses CSS opacity transition (`.simulating` class) instead of innerHTML wipe to prevent flicker
- `storedApprox` cache is cleared on any upgrade/prestige change

## TODO
- Visual overhaul to match game's pixel art assets (pending wiki screenshots)
- Gem upgrades not yet included in budget planner optimizer
- Export/import save data
