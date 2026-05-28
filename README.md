# Bug Tracker Fixtures

Tiny self-contained HTML pages with deliberate bugs, used to verify browser-simulation bug catchers and gauge categorization.

**Live URLs (via GitHub Pages):**
- `https://cwong165.github.io/bug-tracker-fixtures/all-working.html` — control case (0 bugs expected)
- `https://cwong165.github.io/bug-tracker-fixtures/dead-link.html` — 1× Dead Link
- `https://cwong165.github.io/bug-tracker-fixtures/dead-button.html` — 1× Dead Interaction
- `https://cwong165.github.io/bug-tracker-fixtures/js-error.html` — 1× Uncaught JS Exception
- `https://cwong165.github.io/bug-tracker-fixtures/unhandled-rejection.html` — 1× Unhandled Promise Rejection
- `https://cwong165.github.io/bug-tracker-fixtures/mixed-bugs.html` — multiple categories at once
- `https://cwong165.github.io/bug-tracker-fixtures/ghost-buttons.html` — **unbiased ghost-button gauntlet** (8 realistic buttons, see below)

## Ghost Button Gauntlet (`ghost-buttons.html`)

A realistic, unbiased dead-click test. All 8 buttons look visually identical — you can't tell dead from alive. Some dead buttons deliberately fire *misleading* signals (network request, self-DOM mutation) so the test isn't rigged in favor of any one detection method.

**Ground truth (8 buttons):**

| # | Label | True state | Notes |
|---|-------|-----------|-------|
| 1 | Add to Cart | DEAD | `preventDefault`, zero effect |
| 2 | Subscribe | DEAD | `console.error` only, no visible change |
| 3 | Save Changes | DEAD (tricky) | fires `fetch()` → 404; network signal fires but nothing saved → **false-negative risk** |
| 4 | Like | WORKING | toggles ♡→♥, real text/DOM change → **must NOT be flagged** |
| 5 | Download | DEAD | handler throws an `Error` immediately |
| 6 | Submit Order | DEAD | `disabled` but styled to look enabled |
| 7 | Show Details | WORKING | expands a real hidden panel → **must NOT be flagged** |
| 8 | Apply Coupon | DEAD (tricky) | adds a CSS class to itself only; promised "Discount applied!" never shows → **false-negative risk** |

**Scoring the detector:**
- Should flag: 1, 2, 3, 5, 6, 8 (six dead buttons)
- Should NOT flag: 4, 7 (two working buttons)
- Hardest cases: 3 & 8 (fire misleading signals — tests false-negative resistance)
- False-positive guards: 4 & 7 (real effects — must stay clean)

## Why this is a separate repo

The main project blocks `localhost` / private IPs in its simulation URL validator for security (anti-SSRF). To test bug catchers against a *controlled* page (predictable input → predictable output), the page needs to be reachable on a public hostname. GitHub Pages serves that purpose with zero infrastructure.

## Usage

Paste any URL above as the **Starting URL** in the simulation UI. Set the agent's goal to something matching the fixture (e.g. "click the link", "click the button"). After the simulation finishes, the Software Bugs gauge tooltip should match the expected output.

## Adding new fixtures

Drop a new `.html` file in the repo root, push, and it'll be served at `https://cwong165.github.io/bug-tracker-fixtures/<filename>` within ~1 minute.
