# Week 11 - Tests, Scenarios and Evidence

## Preconditions
Run from the repository root on `week11-agent-integration`. The nine Week 10 tests should already pass before adding new code.

## Tests
| Scope | Command | Expected |
|---|---|---|
| Reflection unit tests | `python -m unittest tests.test_reflection_agent -v` | 3 tests; OK |
| Coordinator integration | `python -m unittest tests.test_agent_coordination -v` | 1 test; OK |
| All regression tests | `python -m unittest discover -s tests -v` | 13 tests; OK |

## Runtime scenarios
| Scenario | Command | Expected final outcome |
|---|---|---|
| Success | `python run.py --reset-db` | Completed, reflection ACCEPTED, two plan items |
| Missing assignments | `python run.py --reset-db --no-assignments` | WaitingForInput, reflection skipped |
| Tool outage | `python run.py --reset-db --calendar-fail` | Failed, reflection skipped |
| Invalid provider | `python run.py --reset-db --calendar-provider=unknown` | Controlled configuration error, exit code 2 |
| Invalid candidate plan | `python -m agent.reflection_demo` | Missing assignment and session-limit issues; REJECTED |

## SQLite checkpoint
Run a **fresh successful reset** immediately before `python inspect_database.py`. The expected summary includes `students: 1`, `assignments: 2`, `study_plans: 1`, `plan_items: 2`, `tool_calls: 1` and `memory_events: 7`.

## Interpretation and limitations
A controlled non-success path is acceptable behavior when it matches its requirement. The reflection component currently checks exact assignment IDs, non-negative session duration up to two hours and unique slot labels. It does not independently verify an LLM, genuine calendar access, or arbitrary overlapping time ranges.
