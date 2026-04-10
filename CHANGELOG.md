# Changelog

## Stargazing v1.5.0
- Rename "Best Floor" to "Where to Camp" with subtitle explaining the concept
- Floor cards now show vein type as label instead of "Set 1-5"

## Stargazing v1.4.1
- Fix level controls misaligned across star items (consistent /max label width)
- Hide level controls for locked stars (levels don't exist when locked)
- Minimum level is now 1 for unlocked stars (game starts at level 1, not 0)
- Migrate existing saved data: level 0 promoted to 1 for unlocked stars

## v2.4.0
- UX consistency: viewport meta tag, header/panel box-shadows, modal-close touch target
- Undo toast on prestige reset (4s with undo link)
- Keyboard shortcut: press ? to open changelog
- Click brand title to scroll to top
- Panel entrance animations (staggered fade-in)
- Button active state uses scale(0.96) instead of background change
- Easing CSS variables (--ease-out, --ease-snap)
- Focus-visible outline standardized to 2px
- Touch target insets standardized for 44px minimum

## Stargazing v1.4.0
- UX consistency: custom scrollbar, help-btn touch target expanded to 44px
- Undo toast on Lock All and Reset Levels (4s with undo link)
- Keyboard shortcut: press ? to open changelog
- Click brand title to scroll to top
- Panel entrance animations (staggered fade-in)

## v2.3.0
- Remove zoom:1.33 from body - all CSS values scaled to match previous visual output
- Unified type scale (--fs-micro through --fs-lg) shared with Stargazing tool
- Both tools now use identical CSS variable names and values for font sizes

## Stargazing v1.3.0
- Unified type scale with Event Sim (same CSS variables, same values)
- Modal h3 and button font sizes standardized to match Event Sim
- Version display uses --fs-xs, buttons use --fs-xs for cross-tool consistency

## v2.2.0
- Accessibility: h1 heading, skip link, focus-visible styles, prefers-reduced-motion
- Semantic HTML: `<main>` landmark, modal role/aria-label, budget label `for` attributes
- All upgrade/gem inputs and buttons get aria-labels for screen readers
- Modal focus trap with focus return on close
- Touch targets: .ubtn and .help-btn hit areas expanded to 44px via ::after
- Hover effects gated behind @media (hover: hover) for touch devices
- Fix: transition: all replaced with specific properties (.res-btn, .help-btn, .opt-seg-btn)
- Input font-size bumped to 16px (prevents iOS auto-zoom)
- Version check: notifies when a new version is deployed

## Stargazing v1.2.0
- Accessibility: h1 heading, skip link, modal focus trap with focus return
- Hover effects gated behind @media (hover: hover) for touch devices
- Body font-size bumped to 16px (accessibility minimum)
- Scale on press corrected to 0.96
- Version check: notifies when a new version is available

## Stargazing v1.1.0
- Hold-to-repeat on +/- buttons (pointer events, touch-compatible)
- Hover star name in grid to see perk tooltip
- Progress counter in panel header (unlocked / maxed)
- Reset Levels button to zero all star levels
- Target perk now shows vein types alongside the perk description

## Stargazing v1.0.0
- New tool: Stargazing Floor Picker (stargazing.html) - select unlocked stars, pick a target, see the best floor to camp
- Star level tracking with +/- controls; maxed stars excluded from recommendations
- Top floors ranking when no target is selected
- Vein types and floor debuffs displayed per floor card
- Co-star perk tooltips on hover
- Stars grouped by game phase (Early / Mid / End Game)
- Target dropdown grouped with optgroup
- Click star name to select as target (auto-unlocks if locked)
- State persistence (unlocked stars, levels, target) via localStorage
- Mobile responsive layout
- Accessibility: semantic landmarks, aria-labels, focus-visible, reduced motion, touch targets
- Link from Event Sim header to Stargazing

## v2.1.0
- Enable MC fallback in max wave mode: HP, block, speed, and other survival upgrades now show wave gain even when the deterministic sim can't detect it (marginal gains captured via 200 paired MC runs)
- MC uses fractional wave (wave + sub/5) for precision in wave mode
- Budget planner also benefits from MC-cached gains in wave mode

## v2.0.1
- Fix: wave mode gain threshold removed - small HP/survival gains no longer clipped to 0
- Fix: gain=0 upgrades show blank instead of misleading "< 0.1"
- Fix: Next Buy only shows upgrades with positive gain (no more attack damage at +0 avg wave)

## v2.0.0
- Rewrite gain calculation: pre-compute offense-only upgrades (atk/crit/critDmg) at init
- Attack and crit upgrades correctly show 0 gain when player always 1-shots, in any mode
- Combo upgrades (+atk+HP) show correct gain - sim runs without holding atk constant
- Removed GiveApproxWithUpgraded, atk-holding hack, and wave-mode MC branch
- Single gain function (computeUpgradeGain) used by both Next Buy and Budget Planner
- MC fallback only used in rpm mode when deterministic shows 0 for non-offense upgrades

## v1.9.0
- When player always one-shots every wave they can reach, attack-only upgrades show 0 gain in both res/min and max wave modes
- Combo upgrades (e.g. +Attack +HP) show gain only from the survival portion (atk held constant in sim)
- No MC noise fallback in this scenario - deterministic result is used directly

## v1.8.0
- Fix max wave mode root cause: simulate with player.atk held at base level so attack damage upgrades produce exactly 0 wave gain when already one-shotting before death wave. Only HP, block, speed, and game speed upgrades can move the final wave.

## v1.7.0
- Fix max wave mode: use fractional wave (wave + sub/5) for precision; remove MC noise fallback so attack damage correctly shows 0 gain when player already one-shots
- Budget planner now respects the res/min / max wave mode
- Budget planner inputs: press Enter to calculate
- Optimize mode toggle redesigned as a two-button segmented control for visibility

## v1.6.0
- Optimize mode toggle (res/min vs max wave) in Next Buy and Budget Planner
- res/min mode (default): ranks upgrades by farming efficiency, same as before
- max wave mode: ranks by average wave gain - attack damage deprioritized when player already one-shots, HP/block/speed rise to top
- Toggle state persists across page reloads

## v1.5.0
- Upgrade names always fully visible (multi-line wrap instead of truncation)
- Click feedback: flash animation on Next Buy and Budget Planner rows
- Active/pressed states on all buttons (+/-, Run Sim, Calculate)
- Run Sim button visually distinct (accent color, stands out from other header buttons)
- Simulation panel highlighted with accent border
- Tier headers show resource type (e.g. "Tier 1 - Res 1")
- Prestige Reset requires confirmation dialog

## v1.4.0
- Budget planner now uses Monte Carlo gains from Next Buy cache as fallback when deterministic shows zero gain
- Fixes missing T1/T2 upgrade suggestions (HP, prestige bonus, etc.) in budget planner
- Removes v1.3.3 "cheapest fallback" band-aid in favor of unified gain source

## v1.3.3
- Fix: budget planner now suggests T1/T2 upgrades even when deterministic gain is zero (buys cheapest available as fallback)
- Fix: budget planner respects sequential unlock rule (each upgrade requires previous to be bought)

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
