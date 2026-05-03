# agent-orchestrator

Unified orchestrator plugin for confirmation-first, multi-agent delivery workflows.

This plugin now includes migrated capabilities from:

- `blazor-dev`
- `csharp-code-review`
- `dotnet-ai-developer`
- `fullstack-hybrid-developer`

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

The orchestrator is the only workflow entry point. It routes deterministic handoffs to specialist subagents after explicit user approval.

## Deterministic Delegation Rules

1. Classify request intent (`build`, `feature`, `review`, `issue-management`, `mixed`).
2. Map intent to canonical specialist via `subagents.json`.
3. Propose delegation with required request shape and ask `Approve delegation? (yes/no)`.
4. Launch approved subagent(s) in sequential or safe parallel mode.
5. Consolidate outputs with evidence, risks, and next-step recommendation.

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
