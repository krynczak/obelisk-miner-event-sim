# Obelisk Miner Event Sim

Single-file HTML/JS/CSS app at `index.html`. No build system. Deployed via GitHub Pages. Open in browser to run.

## Rules
- Do not rebuild the upgrade DOM on value changes - it breaks Tab focus
- Upgrade names, costs, and prestige unlocks must match the [official wiki](https://shminer.miraheze.org/wiki/Events)
- All user-facing text in English
- Version bump (semver) and CHANGELOG.md entry on every release
- After every push: monitor GitHub Pages deploy with `gh run watch $(gh run list --limit 1 --json databaseId -q '.[0].databaseId') --interval 5` and notify when the change is live

## Reference
- `ARCHITECTURE.md` - code structure, game formulas, gain calculation system, variable names
- `events.md` - local copy of wiki Events page
- `CHANGELOG.md` - version history

## Pending Redesign: Max Wave Only Mode

The sim currently has two optimization modes (res/min and max wave) plus a "Cycle Resource" button to select which of the 4 resources to optimize for. This is overly complex and confusing. The planned redesign:

**Goal:** Remove res/min mode and Cycle Resource. Make "max wave" the single optimization target.

**Rationale:** The user's real goal is always to push the highest wave possible. Resources are just a means to buy upgrades. Optimizing for "resources per minute" is an intermediate concern - what matters is which upgrade advances wave count the most.

**Key design questions:**
1. Game Speed upgrades (+3% Event Game Speed, +5% Event Game Speed) don't increase wave directly but accelerate farming. When should the sim recommend them? They have indirect value: faster farming = more resources per run = more upgrades = higher waves.
2. +3% 5x Drop Chance similarly has indirect value (more resources per wave).
3. Cap upgrades (+1 Tier N Upgrade Caps, +1 Cap of Cap Upgrades) unlock further upgrades. The sim needs to detect when a cap is blocking progress and prioritize it.
4. The Budget Planner currently uses the same gain logic. It needs to work with a single mode too.

**What to remove:** Run Sim button (already removed), Cycle Resource button, res/min toggle, `optimizeMode` variable, `currentResource` variable, `resourcesPerMinute()` usage in gain calculations.

**What to keep:** The Monte Carlo simulation, the per-upgrade gain calculation framework, the Budget Planner UI.

**Approach:** Enter plan mode. Read ARCHITECTURE.md for gain calculation details. Design the new single-mode ranking that accounts for direct wave gain AND indirect value of speed/drop/cap upgrades. Implement incrementally.

## Roadmap
Tracked in GitHub Issues.
