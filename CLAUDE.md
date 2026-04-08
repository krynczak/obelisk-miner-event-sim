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

## Roadmap
Tracked in GitHub Issues.
