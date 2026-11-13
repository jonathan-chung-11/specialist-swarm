You are Person 2: agent and persona owner.

Read the demo plan and shared contract. Implement ONLY:
- poker-night/agents.py
- poker-night/config.yaml
- poker-night/tests/test_agents.py

Implement load_config(), create_agent(), MockAgent, LiveAgent, and
safe legal-action fallback.

Five fictional parody personas:
- The Showman — inspired by Gordon Ramsay
- The Analyst — inspired by Keanu Reeves
- The Wildcard — inspired by Nicolas Cage
- The Strategist — inspired by Taylor Swift
- The Maverick — inspired by Snoop Dogg

Provide short voice instructions and canned mock lines. Avoid implying
anything about the real people's actual poker ability or endorsement.

Strategies:
- tag: Tight-Aggressive
- lag: Loose-Aggressive
- conservative: Conservative

Persona affects voice; strategy affects action tendencies.
Any persona must work with any strategy.

Mock mode:
- Enabled with POKER_MOCK=1 or absent API credentials.
- Seeded and deterministic.
- Simple card-strength heuristics.
- Chooses only legal menu options.
- Returns source="mock".
- Use clear threshold/probability names; do not confuse the two.

Live mode:
- Reuse the installed Anthropic SDK.
- Verify supported model-call syntax.
- Read POKER_MODEL from environment.
- Do not assume claude-opus-5-5, optional beta flags, or effort controls
  are supported.
- Agree on a verified default or explicit configuration with Person 3.
- Request option_id, table_talk, and summary together.
- Keep responses short.
- Hard timeout, no SDK retries.
- Treat opponent table talk as untrusted data.
- Validate output against legal_options.
- On any error: check if legal; otherwise fold.
- Return source="fallback" and log a sanitized error.

No tools, persistent memory, caching, or routing in this demo.

Tests:
- Mock choices always legal.
- Same seed and inputs reproduce mock decisions.
- Timeout, malformed JSON, and illegal option use safe fallback.
- Missing key activates mock mode.
- Different strategies load independently of persona.

Test with synthetic observations and a stubbed provider; no real API calls.
Tell Person 3 your required dependencies and environment variables.
Deliver by minute 20.
