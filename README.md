# Czak Event Sim

Web-based event simulator for [Shminer](https://shminer.miraheze.org/wiki/Events).

Forked from [Kommandant-Julk/shminer_event_sim](https://github.com/Kommandant-Julk/shminer_event_sim) (Lua/Love2D) and rewritten as a single-file HTML/JS/CSS web app - no dependencies, no build step. Open `index.html` in a browser.

## Bug Fixes vs Original

### GitHub Issue #1: Incorrect Event Speed Values
- **T1 upgrade #5** (Event Speed): was `+2%` per level, corrected to `+3%` per level
- **T3 upgrade #4** (Event Speed): was `+3%` per level, corrected to `+5%` per level

### Gem 3 Per-Level Bonus
- Was `+1.0` per gem level to gameSpeed, corrected to `+1.25` per gem level
- Verified against in-game display: `1 + 1.35 + 1.60 + 3*1.25 = 7.70` matches game's Event Speed of 7.70

## New Features

### UI Overhaul
- Complete redesign: dark amber/gold theme ("Mining Terminal" aesthetic)
- Chakra Petch + Space Mono font pairing
- Three-column layout: stats | simulation results | upgrades

### Gain Visualization
- Each upgrade row shows a gain bar proportional to resource/min improvement
- "Next Buy" panel: top 5 upgrades sorted by resource/min gain with cost info
- Resource/min metric with cycle button (res 1-4)

### Wave Distribution Chart
- Bar chart showing wave distribution across 1000 Monte Carlo runs
- Peak wave highlighted in gold

### Budget Planner
- Input available resources (supports shorthand: `4.21m`, `168k`, `13.9k`)
- Greedy optimizer finds the best upgrades to buy within budget
- Shows purchase order with level changes, costs, and remaining resources

### Quality of Life
- **localStorage persistence**: upgrade levels, gems, prestige, base cost saved automatically
- **Editable number inputs**: type values directly instead of clicking +/- repeatedly
- **Tab navigation**: Tab skips +/- buttons, moves between input fields only
- **Click-to-select**: single click on any input highlights the value for quick replacement
- **Shift+click**: +/- buttons move by 10 when holding Shift
- **Flicker-free updates**: simulation panel dims during recalculation instead of clearing

## Usage

1. Open `index.html` in a browser
2. Enter your upgrade levels in the right panel (match your in-game values)
3. Set prestige count and gem levels in the left panel
4. The simulation runs automatically on every change
5. Check "Next Buy" for the most efficient upgrade to purchase next
6. Use Budget Planner to plan a spending spree with your available resources

## TODO

- [ ] Style UI to match game's pixel art visual assets (stone/brown backgrounds, pixel font, in-game icon style)
- [ ] Enable GitHub Pages deployment
- [ ] Add gem upgrades to budget planner optimization
- [ ] Export/import save data as text
