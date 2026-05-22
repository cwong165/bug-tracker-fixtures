# Bug Tracker Fixtures

Tiny self-contained HTML pages with deliberate bugs, used to verify browser-simulation bug catchers and gauge categorization.

**Live URLs (via GitHub Pages):**
- `https://cwong165.github.io/bug-tracker-fixtures/all-working.html` — control case (0 bugs expected)
- `https://cwong165.github.io/bug-tracker-fixtures/dead-link.html` — 1× Dead Link
- `https://cwong165.github.io/bug-tracker-fixtures/dead-button.html` — 1× Dead Interaction
- `https://cwong165.github.io/bug-tracker-fixtures/js-error.html` — 1× Uncaught JS Exception
- `https://cwong165.github.io/bug-tracker-fixtures/unhandled-rejection.html` — 1× Unhandled Promise Rejection
- `https://cwong165.github.io/bug-tracker-fixtures/mixed-bugs.html` — multiple categories at once

## Why this is a separate repo

The main project blocks `localhost` / private IPs in its simulation URL validator for security (anti-SSRF). To test bug catchers against a *controlled* page (predictable input → predictable output), the page needs to be reachable on a public hostname. GitHub Pages serves that purpose with zero infrastructure.

## Usage

Paste any URL above as the **Starting URL** in the simulation UI. Set the agent's goal to something matching the fixture (e.g. "click the link", "click the button"). After the simulation finishes, the Software Bugs gauge tooltip should match the expected output.

## Adding new fixtures

Drop a new `.html` file in the repo root, push, and it'll be served at `https://cwong165.github.io/bug-tracker-fixtures/<filename>` within ~1 minute.
