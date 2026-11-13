You are Person 3: backend owner and integration lead.

Read the demo plan and shared contract. Implement ONLY:
- poker-night/server.py
- poker-night/requirements.txt
- poker-night/.env.example
- poker-night/README.md
- poker-night/tests/test_api.py

Build:
- GET /api/personas
- POST /api/match
- POST /api/match/{match_id}/step
- Static frontend serving.

Use the engine and agent interfaces without duplicating their logic.

Match creation:
- Representative is seat 0.
- Assign two distinct remaining personas using seeded randomness.
- Assign opponent strategies.
- Create separate agent instances.
- Default to one hand; allow 1–5.

Controller:
- One in-memory active match is sufficient.
- Use asyncio.Lock and a version counter.
- Validate match ID and version inside the lock.
- Stale requests do nothing and return current view.
- One player action maximum per step.
- Separate hand transitions from player actions.
- Validate agent decisions again.
- End after configured hands or first bust-out.
- Keep opponent summaries out of browser payloads.
- Feed agents only seat-scoped observations.
- Return useful errors without exposing credentials.

Configuration:
- Explicitly load .env if README startup relies on it.
- Keep credentials server-side.
- Coordinate model defaults with Person 2.
- Verify dependencies instead of copying unverified versions.
- Include pytest and httpx if needed.

Tests:
- Match creates three distinct personas.
- Representative and strategy are preserved.
- Stale version does not apply an action.
- Unknown match ID fails clearly.
- Opponent summaries do not appear in responses.

Use mocked engine/agent dependencies if teammates are not ready.

Integration:
- Confirm contracts immediately.
- At minute 10, check progress with all three teammates.
- At minute 20, run the full mock app.
- Use final 10 minutes for blockers and smoke testing.
- Coordinate before editing another person's files.
- Prefer reducing to one reliable hand over adding another engine.

README:
- Exact setup, run, and test commands.
- Live versus mock configuration.
- Actual rules and limitations.
- Short demo script.
- Honest test status; no invented success claims.

Do not modify unrelated repository files.
