You are Person 4: frontend and browser-experience owner.

Read the demo plan and shared contract. Implement ONLY:
- poker-night/static/index.html
- poker-night/static/app.js
- poker-night/static/style.css if useful

Use plain JavaScript and CSS. No framework installation or build step.
Tailwind CDN is optional; the page should remain usable without it.

Build a polished, compact experience:

SETUP:
- Fetch five persona cards from /api/personas.
- Choose one representative.
- Choose one strategy.
- Start a one-hand match.

TABLE:
- Three seats with monograms, names, stacks, strategy labels.
- Gold "Your player" highlight.
- Representative cards face up.
- Opponent card backs when hole_cards entry is null.
- Board, pot, dealer button, active-player glow.
- Short speech bubbles and action log.
- Clear result banner.
- Pause/Resume and Restart.

INSPECTOR:
- Representative's latest action.
- Decision summary.
- Source and latency.
- Label: "AI-generated explanation, not internal reasoning."

HONEST STATUS:
- Permanent mock banner when mode="mock".
- Per-action fallback badges.
- Parody/non-endorsement notice.
- Display rules_label.
- Friendly network/API error state.

REQUEST LOOP:
- Recursive setTimeout after each completed request.
- Never overlapping /step requests.
- Always send current version.
- Pause stops scheduling; an in-flight action may finish.
- Ignore old responses after Restart using a generation token.
- Stop stepping when finished.
- Briefly display hand_over before requesting another hand.

SECURITY:
- Use textContent for model-provided text.
- Do not use unsanitized innerHTML.
- Do not request hidden opponent analysis.

VISUALS:
Dark felt, restrained gold accents, readable card faces, clean spacing.
Use CSS transitions, not complex animation work.

Work against a temporary local fixture until the backend exists.
Final app must use real endpoints by default.

By minute 20, hand over a usable UI.
Use final 10 minutes to browser-test with Person 3:
- Setup and start.
- Full mock hand.
- Pause/resume.
- Restart during a pending request.
- Correct hidden-card display.
- Result, mock, and fallback labels.

If browser tooling is unavailable, state that explicitly and provide
manual test steps. Do not claim browser tests passed without running them.
