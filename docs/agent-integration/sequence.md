# Week 11 - Agent Interaction Sequence

The sequence records the successful path and makes the fail-fast boundary visible. Time flows downwards; the Mermaid `alt`/`else` branches are mutually exclusive.

```mermaid
sequenceDiagram
    actor Student
    participant Coordinator as AgentCoordinator
    participant Planning as PlanningAgent
    participant Core as StudyPlanningOrchestrator
    participant Calendar as CalendarPort
    participant Memory as SQLiteMemoryStore
    participant Review as ReflectionAgent
    Student->>Coordinator: request weekly plan
    Coordinator->>Planning: execute(student, assignments)
    Planning->>Core: generate_plan(student, assignments)
    alt assignments missing
        Core->>Memory: append WaitingForInput
        Core-->>Planning: empty StudyPlan
        Planning-->>Coordinator: empty StudyPlan
        Coordinator->>Memory: append coordination stopped
        Coordinator-->>Student: request missing input
    else assignments provided
        Core->>Calendar: get_availability()
        alt calendar tool fails
            Calendar--xCore: CalendarUnavailableError
            Core->>Memory: append Failed
            Core-->>Planning: empty StudyPlan
            Planning-->>Coordinator: empty StudyPlan
            Coordinator->>Memory: append coordination stopped
            Coordinator-->>Student: controlled failure
        else calendar tool succeeds
            Calendar-->>Core: available slots
            Core->>Memory: append state and tool events
            Core-->>Planning: candidate StudyPlan
            Planning-->>Coordinator: candidate StudyPlan
            Coordinator->>Review: review(assignments, plan)
            Review-->>Coordinator: ReflectionResult
            Coordinator->>Memory: append reflection decision
            Coordinator-->>Student: reviewed StudyPlan
        end
    end
```

## Trace checkpoint
The `REFLECTION CHECK` lines must occur **after** the orchestrator prints `STATE -> Completed`. Neither missing-input nor tool-failure scenarios may print `REFLECTION -> ACCEPTED`.
