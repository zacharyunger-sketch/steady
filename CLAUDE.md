# Steady - Gambling Recovery PWA

Personal recovery companion app. Single-page PWA, vanilla JS, no frameworks, no build step, no backend. All user data lives in localStorage (key `steady_v1`) - nothing ever leaves the device. This is non-negotiable: never add analytics, network calls, or external dependencies.

## Files
- `index.html` - the entire app (HTML + CSS + JS in one file, by design)
- `manifest.json` / `sw.js` / `icons/` - PWA install + offline support
- Bump `CACHE` version in `sw.js` after any change to `index.html`

## Design system
- Colors: bg `#0d1117`, cards `#161d27`, borders `#2a3546`, text `#e8edf4`, muted `#8b98a9`, accent teal `#2dd4a7`, money amber `#f5b04d`, risk red `#e06a5a`
- Rounded 16px cards, >=44px touch targets, bottom tab bar with safe-area inset, system font stack, mobile-first (390px)

## Tone rules (critical - this is a recovery app)
- Zero shame language, ever. A slip is "a data point, not a verdict."
- Slips reset the streak but NEVER erase total clean days, best streak, or history.
- Warm, direct, second person. No clinical jargon, no lecturing.
- The urge button stays first and most prominent on Home.
- Helpline (1-800-GAMBLER) must remain visible in urge mode and Settings.

## Architecture notes
- Dates: stored as `YYYY-MM-DD`, parsed manually via `parseDate()` - never `new Date(string)` (timezone drift)
- Streak = days since max(quit date, last slip); total clean = days since quit - slip count
- All intervals (ticker, breathing, urge timer) are guarded against stacking and cleared on close
- "Other Timeline" sim: 5% house edge, $20 bet size, weekly stdev = sqrt(bet x turnover), Box-Muller gaussian
- Persistence: `save()` also writes a `steady_v1_bak` rolling backup; `load()` restores from it if the primary is missing/corrupt. `migrate()` additively backfills new schema fields so old saves keep working across updates.
- On-device Coach (the "AI" layer): heuristic only, no network, no model. `riskProfile()` mines logged urges for top day/time/trigger/mood + kryptonite combo; `currentRisk()` flags high-risk windows now; `coachBriefing()` composes a tailored Home briefing; urge mode weights the user's own reasons via `buildUrgePool()`. Keep it fully offline - a cloud LLM would need opt-in and break the on-device privacy promise.
- SW is network-first for HTML (updates land on next online launch), cache-first for assets. Bump `CACHE` on every `index.html` change.
