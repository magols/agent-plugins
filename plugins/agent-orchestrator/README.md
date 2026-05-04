# agent-orchestrator

Unified orchestrator plugin for confirmation-first, multi-agent delivery workflows.

This plugin now includes migrated capabilities from:

- `blazor-dev`
- `csharp-code-review`
- `dotnet-ai-developer`
- `fullstack-hybrid-developer`

## What This Plugin Does

`PLUGIN Orchestrator` is the single workflow entry point. It performs deterministic routing to specialist subagents, but only after explicit user confirmation for each delegation.

The orchestrator is designed for:

- implementation-first delivery flows
- review-first remediation loops
- mixed build/feature/review/issue pipelines
- AI-focused implementation and review scenarios

## Entry Point + Subagents

- Entry point agent: `PLUGIN Orchestrator`
- Subagent registry: [`subagents.json`](./subagents.json)
- Canonical specialists:
  - `PLUGIN Build Specialist` -> `agent-orchestrator:build-specialist`
  - `PLUGIN Feature Specialist` -> `agent-orchestrator:feature-specialist`
  - `PLUGIN Review Specialist` -> `agent-orchestrator:review-specialist`
  - `PLUGIN Issue Specialist` -> `agent-orchestrator:issue-specialist`
  - `PLUGIN Blazor Developer` -> `agent-orchestrator:blazor-developer`
  - `PLUGIN C# Code Reviewer` -> `agent-orchestrator:csharp-reviewer`
  - `PLUGIN .NET AI Developer` -> `agent-orchestrator:dotnet-ai-developer`
  - `PLUGIN .NET AI Reviewer` -> `agent-orchestrator:dotnet-ai-reviewer`
  - `PLUGIN Full-Stack Hybrid Developer` -> `agent-orchestrator:fullstack-hybrid-developer`

Subagent registry is defined in [`subagents.json`](./subagents.json) and should be treated as the source of truth for route mapping.

## Subagent Reference

### 1) PLUGIN Build Specialist

- Intent category: `build`
- Runtime agent id: `agent-orchestrator:build-specialist`
- Primary focus: scaffolding plans, CI/CD design, build diagnostics, release checks
- Owns:
  - solution/project structure recommendations
  - build and test pipeline definitions
  - build failure triage and remediation
  - release-readiness checklists
- Does not own:
  - product feature scope decisions
  - non-build-focused code review governance
  - issue tracker policy decisions
- Response contract:
  1. build objective
  2. proposed build workflow
  3. validation commands and expected results
  4. risks and mitigations
  5. handoff-ready summary
- Use when:
  - CI is failing
  - you need reproducible build/test/release gates
  - you need delivery hardening before implementation

### 2) PLUGIN Feature Specialist

- Intent category: `feature`
- Runtime agent id: `agent-orchestrator:feature-specialist`
- Primary focus: feature implementation and test-backed code changes
- Owns:
  - converting feature requests to implementation tasks
  - editing code and tests to satisfy acceptance criteria
  - running behavior-relevant validation commands
- Does not own:
  - final merge governance decisions
  - issue tracker lifecycle policy definition
  - broad build-platform strategy outside feature context
- Response contract:
  1. feature interpretation and assumptions
  2. implementation plan
  3. changed files summary
  4. test and verification status
  5. residual risks/follow-ups
- Use when:
  - implementing features or bug fixes
  - adding tests for changed behavior
  - iterating after review findings

### 3) PLUGIN Review Specialist

- Intent category: `review`
- Runtime agent id: `agent-orchestrator:review-specialist`
- Primary focus: severity-based merge-readiness review
- Owns:
  - findings grouped by `Critical`, `Warning`, `Suggestion`
  - regression and risk identification
  - test-gap detection for changed behavior
- Does not own:
  - direct feature implementation unless explicitly delegated for fixes
  - external issue workflow ownership beyond review annotations
- Response contract:
  1. findings ordered by severity
  2. file and line references
  3. impact/rationale
  4. remediation guidance
  5. merge recommendation + confidence
- Use when:
  - pre-merge quality gates are needed
  - regression/security/maintainability risks must be surfaced
  - you need independent verification after implementation

### 4) PLUGIN Issue Specialist

- Intent category: `issue-management`
- Runtime agent id: `agent-orchestrator:issue-specialist`
- Primary focus: provider-neutral issue/work-item actions
- Owns:
  - standardized actions: `create`, `update`, `link`, `transition`, `close`
  - GitHub + Azure DevOps field mapping
  - synchronized status reporting
- Does not own:
  - product prioritization authority
  - code implementation decisions
  - build/pipeline strategy
- Required inputs:
  - provider target (`GitHub`, `Azure DevOps`, or `both`)
  - project/repository identifiers
  - action type + required fields
  - traceability references (commit, PR, branch, review note)
- Response contract:
  1. requested action and scope
  2. provider mapping details
  3. required-field validation
  4. operation result summary
  5. follow-up actions
- Use when:
  - opening follow-up issues from review findings
  - syncing tracker state across providers
  - closing/resolving linked work items

### 5) PLUGIN Blazor Developer

- Intent category: `blazor-development`
- Runtime agent id: `agent-orchestrator:blazor-developer`
- Primary focus: Blazor/.NET 8+ architecture and implementation
- Core strengths:
  - hosting model selection (Server/WASM/Web App/Hybrid)
  - component decomposition and lifecycle correctness
  - Blazor render-mode and state-management guidance
  - Radzen and docs-backed implementation support
- Use when:
  - designing/scaffolding Blazor apps
  - implementing non-trivial Blazor components
  - debugging render mode/state/lifecycle issues

### 6) PLUGIN C# Code Reviewer

- Intent category: `csharp-code-review`
- Runtime agent id: `agent-orchestrator:csharp-reviewer`
- Primary focus: C# and Blazor code quality review checklist
- Core strengths:
  - async correctness and null-safety checks
  - DI lifetime mismatch detection
  - EF Core query/performance/security checks
  - severity-tiered review output with remediation
- Use when:
  - running a focused C#/Blazor quality pass
  - enforcing review standards before merge

### 7) PLUGIN .NET AI Developer

- Intent category: `dotnet-ai-development`
- Runtime agent id: `agent-orchestrator:dotnet-ai-developer`
- Primary focus: .NET AI app implementation with `Microsoft.Extensions.AI` and Agent Framework
- Core strengths:
  - architecture and implementation for chat/agent/workflow scenarios
  - migration support (Semantic Kernel/AutoGen -> Agent Framework)
  - production hardening (resiliency, observability, testing)
  - documentation-first API validation workflow
- Use when:
  - building .NET AI features, agents, and workflows
  - upgrading/migrating AI orchestration code

### 8) PLUGIN .NET AI Reviewer

- Intent category: `dotnet-ai-review`
- Runtime agent id: `agent-orchestrator:dotnet-ai-reviewer`
- Primary focus: correctness/reliability/security/observability review for .NET AI systems
- Core strengths:
  - `IChatClient` abstraction and API usage validation
  - timeout/retry/cancellation risk detection
  - secret-handling and tool-call safety checks
  - AI-specific test coverage gap analysis
- Use when:
  - reviewing AI-integrated .NET changes pre-merge
  - validating production readiness for AI behavior

### 9) PLUGIN Full-Stack Hybrid Developer

- Intent category: `fullstack-hybrid-development`
- Runtime agent id: `agent-orchestrator:fullstack-hybrid-developer`
- Primary focus: end-to-end Microsoft stack delivery (ASP.NET Core, EF Core, Blazor, Azure, AI)
- Core strengths:
  - clean architecture and full-stack implementation
  - hybrid use of Microsoft docs and third-party documentation sources
  - cloud-native application patterns and operational readiness
- Use when:
  - requests span backend + frontend + cloud + AI
  - cross-stack architecture and implementation are both required

## Deterministic Delegation Rules

1. Classify request intent (`build`, `feature`, `review`, `issue-management`, `blazor-development`, `csharp-code-review`, `dotnet-ai-development`, `dotnet-ai-review`, `fullstack-hybrid-development`, or `mixed`).
2. Map intent to canonical specialist via [`subagents.json`](./subagents.json).
3. Propose delegation with request shape and ask `Approve delegation? (yes/no)`.
4. Launch approved subagent(s) in sequential or safe-parallel mode.
5. Consolidate outputs with evidence, risks, and next-step recommendation.

Default mixed-workflow order is:

1. `build`
2. `feature`
3. `review`
4. `issue-management`

## How To Use The Orchestrator

### Step-by-step pattern

1. Start with `PLUGIN Orchestrator` and describe the outcome, constraints, and scope.
2. Review the proposed delegation plan and specialist mapping.
3. Reply `yes` to approve a delegation, or `no` to revise.
4. Repeat confirmation for each next handoff.
5. Validate the final consolidated summary, risks, and follow-up recommendations.

### Required execution metadata

For reliable multi-step runs, include:

- `CorrelationId`: shared workflow id across all steps
- `TaskId`: deterministic, sequential task identifier per delegation
- explicit execution mode: `sequential` or `parallel`

### Sequential vs parallel execution

- Use `sequential` when outputs from one step are dependencies for the next step.
- Use `parallel` only when tasks are independent and conflict checks pass.
- If overlap/conflict risk exists, orchestrator should reject parallel execution and propose a sequential fallback.

### Confirmation and launch receipts

Every delegated specialist action should have:

- explicit user approval
- launch confirmation evidence/receipt
- per-subagent status sections for parallel batches

## Copy-Paste Prompt Templates

### Implementation-first workflow

```text
Using PLUGIN Orchestrator, implement issue #<id> in this repo.
Propose a deterministic delegation plan first, then wait for my confirmation before each handoff.
Run PLUGIN Build Specialist, then PLUGIN Feature Specialist, then PLUGIN Review Specialist.
Use CorrelationId corr-impl-review-001 and sequential TaskIds.
Return one final consolidated summary with evidence and open risks.
```

### Review-first loop

```text
Using PLUGIN Orchestrator, run a review-first workflow on current changes.
Delegate first to PLUGIN Review Specialist, then implement fixes with PLUGIN Feature Specialist,
then re-run PLUGIN Review Specialist.
Require explicit confirmation before each delegation and include launch receipts.
```

### Blazor-focused task

```text
Using PLUGIN Orchestrator, delegate to PLUGIN Blazor Developer to design and implement
a Blazor Web App feature with correct render mode, lifecycle handling, and tests.
Ask for confirmation before delegation and return changed files plus verification evidence.
```

### C# review task

```text
Using PLUGIN Orchestrator, delegate to PLUGIN C# Code Reviewer for a pre-merge review.
Return findings grouped by Critical, Warning, and Suggestion with remediation guidance.
Ask for confirmation before delegation.
```

### .NET AI implementation task

```text
Using PLUGIN Orchestrator, delegate to PLUGIN .NET AI Developer to implement a .NET AI feature
using Microsoft.Extensions.AI and/or Microsoft Agent Framework.
Require docs-backed API choices, tests for changed behavior, and production-readiness notes.
Ask for confirmation before delegation.
```

### .NET AI review task

```text
Using PLUGIN Orchestrator, delegate to PLUGIN .NET AI Reviewer to perform a merge-readiness review
focused on correctness, resiliency, security, observability, and testing gaps.
Ask for confirmation before delegation and return severity-ordered findings.
```

### Full-stack hybrid task

```text
Using PLUGIN Orchestrator, delegate to PLUGIN Full-Stack Hybrid Developer for an end-to-end task
spanning ASP.NET Core backend, Blazor UI, and Azure-integrated deployment concerns.
Ask for confirmation before delegation and return architecture decisions, code changes, and validation.
```

### Issue tracking task

```text
Using PLUGIN Orchestrator, delegate to PLUGIN Issue Specialist to create and link follow-up items
for review findings in GitHub and Azure DevOps.
Include provider mapping details, created IDs, and any missing-field validation errors.
Ask for confirmation before delegation.
```

## Implementation and Review Workflows

See [WORKFLOWS.md](./WORKFLOWS.md) for copy-paste-ready prompts and step-by-step examples covering:

- Implementation-first workflow (build -> feature -> review)
- Review-first workflow (review -> fixes -> re-review)
- AI app scenarios (chatbot, RAG app, agentic workflow API, multi-model evaluation app)

## Skills

| Skill | Description |
|-------|-------------|
| `orchestration-router` | Deterministic routing matrix, delegation envelope, and confirmation checkpoint protocol. |
| `delivery-workflow-checkpoints` | Standardized handoff checkpoints for build, feature, and review sequences. |
| `dual-tracker-issue-management` | Provider-neutral issue actions mapped to GitHub Issues and Azure DevOps Work Items. |
| `multi-subagent-execution` | Execution protocol for running multiple subagents in sequential or parallel mode with conflict checks and merge rules. |
| `blazor-dev/*` | Migrated Blazor scaffolding and component-generation skills. |
| `csharp-code-review/*` | Migrated C# PR review and Blazor security review skills. |
| `dotnet-ai-developer/*` | Migrated .NET AI workflows, observability, safety, evals, and provider adapter skills. |
| `fullstack-hybrid-developer/*` | Migrated Microsoft Agent Framework references and full-stack hybrid skills. |

## Validation

- Scenario tests: [SCENARIO-TESTS.md](SCENARIO-TESTS.md)
- Validate routing, confirmation gating, launch receipts, non-circular execution, and dual-tracker issue results.

Recommended checks from the scenario pack:

- route mapping correctness by intent
- no subagent action before explicit approval
- complete launch evidence for approved delegations
- conflict checks before parallel execution
- deterministic checkpointing for mixed workflows
- normalized provider identifiers for issue operations
