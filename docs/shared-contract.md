We are implementing docs/celebrity-poker-night-plan.md as a 30-minute demo.

Product:
- Five celebrity-inspired parody personas.
- User selects one representative and its strategy.
- Two distinct opponents are assigned automatically.
- Three autonomous agents play; no human poker actions.
- Default demo is ONE complete hand.
- Persona controls voice; strategy controls poker tendencies.
- Opponents' hidden information must never reach another agent.

Collaboration:
- Implement, do not produce another plan.
- Edit only assigned files.
- Do not change shared interfaces without coordinating with Person 3.
- Use temporary test doubles rather than waiting on teammates.
- Deliver integration-ready work by minute 20.
- Report changed files, tests run, blockers, and limitations.
- Preserve unrelated changes in the repository.

Application directory: poker-night/
Stable seat IDs: 0, 1, 2.
Seat 0 is the user's representative.

CONFIG:
config.yaml:
personas:
  - id, name, inspired_by, monogram, color, voice, mock_lines
strategies:
  tag: {label, prompt, mock: {...}}
  lag: {label, prompt, mock: {...}}
  conservative: {label, prompt, mock: {...}}

ENGINE CONTRACT:

class PokerEngine:
    def __init__(self, seed: int, starting_stacks: list[int],
                 dealer_seat: int): ...
    def legal_options(self, seat: int) -> list[dict]: ...
    def observation(self, seat: int) -> dict: ...
    def apply_option(self, seat: int, option_id: str) -> dict: ...
    def public_state(self) -> dict: ...
    def view_for(self, representative_seat: int) -> dict: ...

public_state():
{
  "active_seat": int | null,
  "hand_over": bool,
  "street": str,
  "board": [str],
  "pot": int,
  "stacks": [int, int, int],
  "bets": [int, int, int],
  "dealer_seat": int,
  "result": null | {
    "winner_seats": [int],
    "text": str,
    "payoffs": [int, int, int]
  }
}

pot includes ALL committed chips not yet paid out, including
current-street bets. Do not count bets twice.

view_for() adds:
"hole_cards": {"0": [str] | null, "1": [str] | null, "2": [str] | null}

Reveal only the representative's cards and opponents' cards legitimately
shown at showdown. Folded/mucked cards remain hidden.

Constructor and apply_option() advance automatic dealing, runouts,
and payouts until the next player decision or completed hand.

Option:
{
  "id": str,
  "label": str,
  "raise_to": int | null,
  "call_amount": int | null
}

Observation:
{
  "my_seat": int,
  "my_hole_cards": [str],
  "board": [str],
  "street": str,
  "pot": int,
  "stacks": [int, int, int],
  "bets_this_street": [int, int, int],
  "dealer_seat": int,
  "legal_options": [Option]
}

The server may add a sanitized public_action_log.
Never include opponents' hidden cards, deck state, or private summaries.

AGENT CONTRACT:

def load_config() -> dict: ...

def create_agent(persona: dict, strategy: dict, seed: int):
    # returns an object exposing:
    async def decide(self, observation: dict) -> dict: ...

Decision:
{
  "option_id": str,
  "table_talk": str,
  "summary": str,
  "source": "live" | "mock" | "fallback",
  "latency_ms": int
}

Validate option_id against the legal menu.
On failure: check if legal; otherwise fold.
Server validates again before applying.

API CONTRACT:

GET /api/personas:
{
  "personas": [persona metadata without prompts],
  "strategies": [{"id": str, "label": str}],
  "mode": "live" | "mock"
}

POST /api/match
body: {"persona_id": str, "strategy_id": str, "hands": 1, "seed": 42}
returns TableView.

POST /api/match/{match_id}/step
body: {"version": int}
returns TableView.

TableView:
{
  "match_id": str,
  "version": int,
  "mode": "live" | "mock",
  "status": "playing" | "hand_over" | "finished",
  "hand_number": int,
  "max_hands": int,
  "representative_seat": 0,
  "players": [{
    "seat": int,
    "persona_id": str,
    "name": str,
    "inspired_by": str,
    "monogram": str,
    "color": str,
    "strategy_id": str,
    "strategy_label": str
  }],
  "table": {engine.view_for(0) output},
  "action_log": [{
    "seat": int,
    "action": str,
    "table_talk": str,
    "source": str
  }],
  "inspector": null | {
    "action": str,
    "summary": str,
    "source": str,
    "latency_ms": int
  },
  "rules_label": str
}

Only the representative's latest summary appears in inspector.

A stale version returns the current view without acting.
Each /step applies at most one player action.
If more hands remain, transitioning to the next hand is a separate step.
Match ends after configured hands or first bust-out.
