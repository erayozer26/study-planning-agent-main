# Week 10 - Acceptance Scenarios

## AC-01 - Valid assignments

**Given** a student with a valid assignment and a working calendar adapter,
**When** the student requests a study plan,
**Then** the request reaches `Completed`, a nonempty plan is returned,
and the shorter 1.5-hour estimate is preserved.

Evidence: `test_given_valid_assignment_when_plan_requested_then_plan_completes`.

## AC-02 - Missing assignments

**Given** a student supplies no assignments,
**When** the student requests a study plan,
**Then** the agent enters `WaitingForInput`, returns no plan items,
and does not call the calendar tool.

Evidence: `test_given_no_assignments_when_plan_requested_then_agent_waits_for_input`.

## AC-03 - Calendar unavailable

**Given** valid input and an unavailable calendar adapter,
**When** the student requests a study plan,
**Then** the agent enters `Failed`, returns no misleading plan,
and persists one failed tool-call trace.

Evidence: `test_given_calendar_outage_when_plan_requested_then_failure_is_controlled`.

## Interpretation

All three acceptance tests can pass even though two scenarios deliberately
do not complete the requested goal. Passing means the observed behavior
matches the expected specification, not that every request succeeds.
