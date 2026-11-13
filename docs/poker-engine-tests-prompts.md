You are Person 1: deterministic poker engine owner.

Read the demo plan and shared contract. Implement ONLY:
- poker-night/engine.py
- poker-night/tests/test_engine.py

Build the PokerEngine interface exactly as agreed.

Use PokerKit as the authoritative rules engine. Verify its installed
interface rather than trusting unverified examples.

Priorities:
1. One complete three-player hand.
2. Correct legal actions, chip accounting, and payouts.
3. Isolated observations.
4. Reliable showdown and fold-ended results.
5. Seeded cards.

Requirements:
- Integer chips, default stacks 1000, blinds 10/20.
- Stable external seat IDs, independent of PokerKit indices.
- Engine-generated legal action menu:
  fold/check/call/min-raise/pot-raise/all-in when applicable.
- Deduplicate equivalent amounts.
- raise_to means total contribution on the current street.
- Never label a maximum raise "all-in" unless it actually is.
- Advance dealing and all-in runouts automatically.
- Include current-street bets in pot exactly once.
- Hide folded/mucked opponents' cards.
- Never put deck state or opponents' hidden cards in observations.
- Return a result even when everyone but one player folds.

Tests:
- Complete seeded hands with simple legal-action bots.
- Chip conservation after each action.
- Fold-ended hand.
- All-in/runout scenario.
- Observation isolation.
- Representative-view card visibility.

Timebox:
- Minute 5: construct a hand and expose public state.
- Minute 12: complete a hand.
- Minute 20: deliver engine and passing core tests.

Do not build a speculative second poker engine. If PokerKit blocks
progress by minute 8, notify Person 3 immediately with the exact issue.
Coordinate a scope reduction rather than silently changing rules.

Use local dependencies if necessary, but tell Person 3 what must go
in requirements.txt. Do not edit that file yourself.
