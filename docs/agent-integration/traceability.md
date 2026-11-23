# Week 11 - Traceability

| Requirement or design concern | Implementation | Verification | Evidence |
|---|---|---|---|
| Generate a plan from valid assignments | `agent/planning_agent.py`, `agent/orchestrator.py` | `python run.py --reset-db` | Completed; two `PLAN` lines |
| Do not invent missing input | `agent/orchestrator.py`, `agent/coordinator.py` | `python run.py --reset-db --no-assignments` | WaitingForInput; reflection skipped |
| Handle calendar failures safely | `ports/calendar_port.py`, `agent/coordinator.py` | `python run.py --reset-db --calendar-fail` | Failed; reflection skipped |
| Enforce at most two hours per session | `agent/planning_policy.py`, `agent/reflection_agent.py` | `tests/test_planning_policy.py`, `tests/test_reflection_agent.py` | Policy tests and review verdict |
| Preserve reflection evidence | `adapters/sqlite_memory.py`, `agent/coordinator.py` | `python inspect_database.py` | Seven memory events on clean success |
| Detect incomplete candidate plan | `agent/reflection_agent.py` | `python -m agent.reflection_demo` | Missing ID 202; rejected |
| Keep calendar provider replaceable | `factories/calendar_factory.py`, `adapters/logging_calendar.py` | `tests/test_calendar_integration.py` | Factory/Decorator traces |
| Keep regression confidence | `tests/` | `python -m unittest discover -s tests -v` | 13 tests; OK |

## Trace interpretation
A terminal trace proves that an event was printed. An SQLite row gives persistent evidence. A passing unit test proves the asserted rule for its inputs, not that every real calendar provider or every scheduling case is correct.
