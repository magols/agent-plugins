# Scenario Test Pack

Validation scenarios for `agent-orchestrator` routing, confirmation gates, and tracker operations.

## How To Use

1. Run each prompt with `PLUGIN Orchestrator`.
2. Verify the orchestrator proposes delegation before acting.
3. Confirm the selected specialist matches expected routing.
4. Confirm no specialist handoff occurs before explicit approval.
5. For mixed scenarios, verify per-step consolidation and final summary quality.

## Scenario Matrix

| ID | Prompt | Expected Route | Expected Confirmations | Expected Evidence |
|----|--------|----------------|------------------------|------------------|
| ORCH-001 | Create a new ASP.NET Core API with CI checks and release gate. | Build Specialist | 1 | Build workflow with build and test commands |
| ORCH-002 | Add JWT auth and role checks to existing endpoints and tests. | Feature Specialist | 1 | Changed code and test validation summary |
| ORCH-003 | Review pending changes and list critical risks first. | Review Specialist | 1 | Severity-ordered findings and merge recommendation |
| ORCH-004 | Create issue records for review defects in both GitHub and Azure DevOps. | Issue Specialist | 1 | Provider results with both issue/work item references |
| ORCH-005 | Build a plan, implement feature, then run a review. | Build -> Feature -> Review | 3 | Consolidated summary with step-by-step outputs |
| ORCH-006 | Add a feature and open linked tracking items in both systems. | Feature -> Issue | 2 | Feature result plus normalized issue mapping |
| ORCH-007 | Fix build failure from CI logs and verify tests. | Build Specialist | 1 | Root cause summary and verification commands |
| ORCH-008 | Convert this vague request into scoped tasks and ask me before each one. | Orchestrator planning flow | 0 initially, then 1 per approved step | Task split with explicit assumptions |
| ORCH-009 | Re-run the same completed review task without scope changes. | Review Specialist (blocked by non-circular rule) | 1 | Rejection with reason and request for scope delta |
| ORCH-010 | Close the linked GitHub issue and transition the Azure DevOps item to Resolved. | Issue Specialist | 1 | Dual-provider transition/close outcome with IDs |
| ORCH-011 | Run build diagnostics and issue triage in parallel, then summarize blockers. | Build + Issue (parallel) | 1 for parallel batch | Two independent result sections and merged summary |
| ORCH-012 | Implement feature, then review it, then open tracked follow-ups. | Feature -> Review -> Issue (sequential) | 3 | Ordered dependency-respecting execution with checkpoints |
| ORCH-013 | Approve delegation, but simulate subagent launch not confirmed. | Any approved specialist (forced failure) | 1 | `SUBAGENT_EXECUTION_FAILURE` with remediation path |
| ORCH-014 | Run build hardening and issue backlog grooming in parallel, then request closure recommendation. | Build + Issue (parallel) | 1 for parallel batch | Parallel launch audit entries plus merged recommendation |
| ORCH-015 | Implement API feature and UI feature in parallel, then run one unified review. | Feature + Feature -> Review | 2 for features, 1 for review | Two feature task outputs, conflict check evidence, post-merge review findings |
| ORCH-016 | Plan build pipeline, implement feature, run review, then create issues for warnings only. | Build -> Feature -> Review -> Issue | 4 | End-to-end sequential chain with issue creation filtered by review severity |
| ORCH-017 | Execute two parallel feature tasks that target overlapping files. | Feature + Feature (parallel attempt blocked) | 1 for proposed parallel batch | Parallel rejection, conflict rationale, and fallback sequential plan |
| ORCH-018 | Run review and issue triage in parallel using pre-existing findings list. | Review + Issue (parallel) | 1 for parallel batch | Independent outputs with explicit data source boundary and merged status |
| ORCH-019 | Parallel run where one subagent succeeds and one fails to launch. | Build + Issue (parallel partial failure) | 1 for parallel batch | One completed launch receipt plus `SUBAGENT_EXECUTION_FAILURE` for failed launch |
| ORCH-020 | Retry failed subagent launch after user approval and continue workflow. | Any failed task -> retry -> continue | 2 (initial + retry) | Failed launch audit, approved retry launch receipt, resumed completion path |
| ORCH-021 | Multi-stage mixed flow: parallel build+feature, then review, then dual-provider issue sync. | (Build + Feature) -> Review -> Issue | 1 for parallel batch, 1 review, 1 issue | Three-stage checkpoint record with dependency transitions and dual-provider IDs |
| ORCH-022 | User changes scope after first delegated step; re-plan remaining subagent sequence. | Re-plan after Build completion | 1 for original, 1+ for updated plan | Scope delta record, new TaskIds, and non-circular continuation |
| ORCH-023 | Implement an AI chatbot app, then run review before closure. | Build -> Feature -> Review | 3 | Deterministic handoff order and consolidated merge-readiness summary |
| ORCH-024 | Run review-first on AI app changes, apply fixes, then re-review. | Review -> Feature -> Review | 3 | Two review checkpoints with fix-loop evidence and final confidence statement |

## Pass Criteria

- Delegation proposal includes intent category, selected specialist, rationale, expected output, and confirmation prompt.
- Every specialist action is gated by user confirmation.
- Multi-subagent runs show execution mode (`sequential` or `parallel`) and dependency logic.
- Every approved delegated task includes launch confirmation evidence.
- Parallel batches include per-subagent launch receipts and a merge policy outcome.
- Any partial failure in a multi-subagent run returns explicit continuation or halt decision logic.
- Mixed workflows include checkpointed consolidation with completed/pending statuses.
- Issue management output is normalized with provider-specific identifiers.
- Repeated completed task IDs are blocked unless scope delta is documented.

## Failure Signals

- Specialist acts before approval.
- Wrong specialist selected for a clear intent.
- Missing correlation/task IDs for delegated steps.
- Missing launch confirmation evidence for any approved delegated task.
- Missing conflict-check evidence before parallel feature or build operations.
- Parallel batch returns a single blended output without per-subagent status sections.
- Retry flow proceeds without a fresh user confirmation.
- Missing provider mapping details for GitHub or Azure DevOps actions.
- Parallel tasks executed despite file/resource conflict risk.
- Final summary omits open risks or pending follow-up.
