# Changelog

## v1.3.2
- Move changelog from README.md to dedicated CHANGELOG.md
- Changelog modal now fetches from CHANGELOG.md

## v1.3.1
- Fix: tooltips now open to the right (no longer clipped off-screen on left column)
- Fix: changelog modal scrollable with zoom
- Changelog button (?) next to version number for discoverability

## v1.3.0
- Help tooltips (?) on every panel with contextual explanations
- Changelog modal: click version number in header to view changelog
- First-time onboarding banner for new users
- Tooltips support hover (desktop), click (touch), Escape to dismiss

## v1.2.4
- Budget planner results sorted by tier (T1, T2, T3, T4)

## v1.2.3
- Click budget planner results now applies upgrade, subtracts cost from budget input, and re-calculates remaining buy order

## v1.2.2
- Click budget planner results to apply upgrades (sets level to target)
- Increase budget planner detail font size for readability

## v1.2.1
- Fix: hybrid gain calculation - deterministic first, Monte Carlo only for marginal gains. Prevents false positives where attack damage showed large gains when player already one-shots enemies.

## v1.2.0
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

## v1.1.0
- Rename display to "Obelisk Miner Event Sim"
- Upgrade/gem names match official wiki exactly
- Player stats order and labels match in-game display
- Fix prestige unlock values per wiki (T2[3], T2[4], T4[1])
- Fix T4[4] base cost: 50 to 40 per wiki
- Cost formatting aligned with in-game display (variable decimals)
- Prestige milestone advisor (next unlock wave and upgrade count)
- Locked upgrades show "Unlocks at Prestige X" or "Level Up Previous To Unlock"
- Code refactor: rename ambiguous variables, add comments

## v1.0.0
- Export/import save data
- Hold-to-repeat on +/- buttons (touch-compatible via pointer events)
- Maxed upgrade visual indicator
- No-op guards on value changes
- Body zoom 1.33
- Wave Distribution moved below Budget Planner
