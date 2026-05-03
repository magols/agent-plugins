# agent-orchestrator

Orchestrator plugin for confirmation-first, multi-agent delivery workflows.

This plugin is designed for a primary orchestration flow:

1. Understand and classify the user goal.
2. Propose delegation to the right specialist.
3. Wait for explicit user confirmation before delegation.
4. Merge specialist outputs into a single delivery summary.

Execution behavior:

- The orchestrator launches approved specialist subagents.
- It supports sequential and parallel execution modes.
- It uses parallel mode only for independent tasks with no write/resource conflicts.

## Agents

| Agent | Description |
|-------|-------------|
| `PLUGIN Orchestrator` | Primary router and coordinator for build, feature, review, and issue workflows. Enforces confirmation-first delegation. |
| `PLUGIN Build Specialist` | Handles build plans, CI/CD workflow design, build diagnostics, and release-check recommendations. |
| `PLUGIN Feature Specialist` | Implements or updates application features and tests in iterative, verifiable steps. |
| `PLUGIN Review Specialist` | Performs review-focused analysis with severity-based findings and merge recommendations. |
| `PLUGIN Issue Specialist` | Manages work items and issue flows for GitHub Issues and Azure DevOps Work Items. |

## Skills

| Skill | Description |
|-------|-------------|
| `orchestration-router` | Deterministic routing matrix, delegation envelope, and confirmation checkpoint protocol. |
| `delivery-workflow-checkpoints` | Standardized handoff checkpoints for build, feature, and review sequences. |
| `dual-tracker-issue-management` | Provider-neutral issue actions mapped to GitHub Issues and Azure DevOps Work Items. |
| `multi-subagent-execution` | Execution protocol for running multiple subagents in sequential or parallel mode with conflict checks and merge rules. |

## Instructions

| File | Applies To | Description |
|------|-----------|-------------|
| `orchestration.instructions.md` | `**/*.{md,json,yml,yaml,cs,csproj,sln,slnx}` | Coordination standards for routing, confirmations, output contracts, and final summaries. |

## Delegation Conventions

- Orchestrator must propose delegation and request approval before specialist execution.
- Handoffs must use canonical specialist names:
	- `PLUGIN Build Specialist`
	- `PLUGIN Feature Specialist`
	- `PLUGIN Review Specialist`
	- `PLUGIN Issue Specialist`
- Delegation payload must include: `TargetAgent`, `CorrelationId`, `TaskId`, `IntentCategory`, `Inputs`, `AcceptanceCriteria`, and `ExpectedOutputFormat`.

## Validation

- Scenario tests: [SCENARIO-TESTS.md](SCENARIO-TESTS.md)
- Validate routing, confirmation gating, non-circular execution, and dual-tracker issue results.

## Example Prompts

- Build a new web API and React frontend; propose your delegation plan first.
- Add authentication and role-based authorization to this app; confirm before each handoff.
- Run a review pass on pending changes and summarize critical findings first.
- Create matching issues in GitHub and Azure DevOps for the defects from this review.
- Run feature implementation and issue pre-triage in parallel, then do final review after both complete.
