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

See [CHANGELOG.md](CHANGELOG.md) for version history.

## Roadmap

See [open issues](https://github.com/krynczak/obelisk-miner-event-sim/issues) for planned features.
