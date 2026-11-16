# Week 10 - Red, Green, Refactor

## Problem

Before Week 10, `agent/planner.py` directly calculated
`min(assignment.estimated_hours, 2.0)`. The two-hour rule was hidden
inside a larger planning algorithm and was difficult to unit-test alone.

## RED - specify behavior before the implementation

1. Create `tests/__init__.py` and `tests/test_planning_policy.py`.
2. Include three test cases: 3.5 -> 2.0; 1.5 -> 1.5; -1.0 -> `ValueError`.
3. Before creating `agent/planning_policy.py`, run:

```powershell
python -m unittest tests.test_planning_policy -v
```

The missing-module import error at this teaching checkpoint is expected.
It demonstrates an absent implementation, not a passing test. Once the module
exists, behavior-level failing assertions would provide a stricter RED example.

## GREEN - smallest correct implementation

Create `agent/planning_policy.py` with class `PlanningPolicy`, constant
`MAX_SESSION_HOURS = 2.0`, a negative-value guard, and a capped result.
Run the three unit tests again and require `Ran 3 tests` and `OK`.

## REFACTOR - move responsibility without changing observable results

Update `agent/planner.py` to accept an injected `policy` and call
`self.policy.session_hours(assignment.estimated_hours)`. Update `run.py`
to create and inject the policy. Do not alter assignment ordering or
`PlanItem` construction. Run the complete suite afterward.

## Reflection

The policy is now cohesive, reusable, and independently testable. The
external plan still caps long estimates at 2.0 hours. A structural
improvement does not justify changing expected user behavior.
