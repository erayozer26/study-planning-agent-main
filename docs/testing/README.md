# Week 10 - Testing and AI-Agent Evaluation

This folder documents the Week 10 verification of the integrated Study Planning Agent.
The project uses standard-library `unittest`, disposable SQLite databases, and four
deterministic agent-evaluation scenarios.

## Documents

- [Test strategy](test-strategy.md) - scope, isolation, and the nine automated tests.
- [TDD cycle](tdd-cycle.md) - RED, GREEN, and REFACTOR using `PlanningPolicy`.
- [Acceptance scenarios](acceptance-scenarios.md) - Given/When/Then outcomes.
- [Agent evaluation](agent-evaluation.md) - metrics, traces, and interpretation.

## Run from repository root

```powershell
python -m unittest discover -s tests -v
python evaluate_agent.py
python run.py --reset-db
python inspect_database.py
```

Expected: 9 passing tests; evaluation with 2/4 task successes, 4/4 correct
behaviors, and 2/2 reliable completions under feasible conditions.

SQLite `.db` files are runtime data and must not be committed. `tests/` uses
disposable database files. The main application's business and error
behavior is preserved from Week 9.
