# Week 10 - AI-Agent Evaluation

## Evaluation design

Run `python evaluate_agent.py` from the repository root. Each of four
deterministic scenarios uses an independent temporary SQLite database.

| Scenario | Expected state | Tool trace | Task success | Behavior correct |
|---|---|---|---|---|
| `standard_success` | Completed | SUCCESS | Yes | Yes |
| `short_success` | Completed | SUCCESS | Yes | Yes |
| `missing_input` | WaitingForInput | NOT_CALLED | No | Yes |
| `calendar_failure` | Failed | FAILED | No | Yes |

## Metrics and denominators

- **Task success rate:** 2/4 = 50%. All attempted scenarios are included.
- **Controlled behavior rate:** 4/4 = 100%. Checks expected state and tool outcome.
- **Goal-completion reliability:** 2/2 = 100%. Only feasible cases with valid input
  and a working tool are included in this classroom definition.
- **Error analysis:** `missing_input=1`, `tool_failure=1`, `unexpected=0`.

## Trace inspection

Inspect the `planning_requests.status` column for the final task state and
`tool_calls.status` for the external calendar outcome. A missing-input case
has no `tool_calls` row. The normal run also persists `memory_events`.
Failure states must be reported rather than hidden or converted to success.

## Interpretation and limits

The 50% overall task-success figure is expected because two scenarios are
intentionally adverse. Safe failure can be correct agent behavior. These
results describe a deterministic, rule-based teaching agent; they are not
evidence of an LLM's open-ended reasoning ability or statistical reliability.
