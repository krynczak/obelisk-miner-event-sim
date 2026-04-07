# Shminer Event Sim

Web-based event simulator for [Idle Obelisk Miner](https://shminer.miraheze.org/wiki/Events) (Shminer).

Forked from [Kommandant-Julk/shminer_event_sim](https://github.com/Kommandant-Julk/shminer_event_sim) (Lua/Love2D) and rewritten as a single-file HTML/JS/CSS web app - no dependencies, no build step. Open `index.html` in a browser.

## Features

### Simulation
- **Monte Carlo**: 1000 runs per simulation, shows average wave, time, and wave distribution
- **Deterministic approximation**: fast per-upgrade gain calculation for ranking and budget planner
- **Resource/min**: efficiency metric per resource type (1-4), cycle with button

### Upgrade Panel
- Names match the game exactly (verified against the [official wiki](https://shminer.miraheze.org/wiki/Events))
- Gain bar proportional to each upgrade's resource/min impact
- Maxed upgrades with visual indicator (accent border)
- Locked upgrades show "Unlocks at Prestige X"
- **Next Buy**: top 5 upgrades sorted by resource/min gain

### Prestige & Gems
- Prestige control with next milestone indicator (wave and upgrades unlocked)
- 4 gem upgrades with dynamic max level per prestige

### Budget Planner
- Input available resources (supports shorthand: `4.21m`, `168k`, `13.9k`)
- Greedy optimizer finds the best buy order within budget
- Shows purchase order with levels, costs, and remaining resources

### Quality of Life
- **Local persistence**: levels, gems, prestige saved automatically via localStorage
- **Export/Import**: copy and paste save data between devices
- **Hold-to-repeat**: holding +/- repeats the action (works on touch and mouse)
- **Shift+click**: +/- moves by 10 levels
- **Tab navigation**: skips buttons, navigates between inputs
- **Editable inputs**: type values directly

## Bug Fixes vs Original

- **T1[4] Event Speed**: was `+2%`, corrected to `+3%` per level
- **T3[3] Event Speed**: was `+3%`, corrected to `+5%` per level
- **Gem 3 Event Speed**: was `+1.0`, corrected to `+1.25` per gem level
- **Prestige unlocks**: T2[3], T2[4], T4[1] corrected per wiki
- **T4[4] base cost**: was 50, corrected to 40 per wiki
- **Cost formatting**: aligned with in-game display

## Usage

1. Open `index.html` in a browser
2. Enter your upgrade levels in the right panel (match your in-game values)
3. Set prestige and gem levels in the left panel
4. The simulation runs automatically on every change
5. Check "Next Buy" for the most efficient upgrade to purchase next
6. Use the Budget Planner to plan spending with your available resources

## Changelog

### v1.2.4
- Budget planner results sorted by tier (T1, T2, T3, T4)

### v1.2.3
- Click budget planner results now applies upgrade, subtracts cost from budget input, and re-calculates remaining buy order

### v1.2.2
- Click budget planner results to apply upgrades (sets level to target)
- Increase budget planner detail font size for readability

### v1.2.1
- Fix: hybrid gain calculation - deterministic first, Monte Carlo only for marginal gains. Prevents false positives where attack damage showed large gains when player already one-shots enemies.

### v1.2.0
- Next Buy shows best upgrade per tier (each tier uses its own currency)
- Next Buy ranks by gain/cost efficiency instead of raw gain
- Click Next Buy picks to increment that upgrade (+1, or +10 with Shift)
- Monte Carlo (200 runs) for per-upgrade gain calculation - captures marginal HP/damage gains
- Upgrades are sequential: each requires previous to be bought first
- Cap upgrades only suggested when a tier upgrade is actually maxed
- Prestige Reset button (zeros tier upgrades, keeps gems)
- Run Sim button in header
- Remove redundant header stats (same info in simulation panel)
- Show "< 0.1" for marginal gains in upgrade panel
- Translate remaining Portuguese strings to English

### v1.1.0
- Rename display to "Obelisk Miner Event Sim"
- Upgrade/gem names match official wiki exactly
- Player stats order and labels match in-game display
- Fix prestige unlock values per wiki (T2[3], T2[4], T4[1])
- Fix T4[4] base cost: 50 to 40 per wiki
- Cost formatting aligned with in-game display (variable decimals)
- Prestige milestone advisor (next unlock wave and upgrade count)
- Locked upgrades show "Unlocks at Prestige X" or "Level Up Previous To Unlock"
- Code refactor: rename ambiguous variables, add comments

### v1.0.0
- Export/import save data
- Hold-to-repeat on +/- buttons (touch-compatible via pointer events)
- Maxed upgrade visual indicator
- No-op guards on value changes
- Body zoom 1.33
- Wave Distribution moved below Budget Planner

## Roadmap

See [open issues](https://github.com/krynczak/obelisk-miner-event-sim/issues) for planned features.
