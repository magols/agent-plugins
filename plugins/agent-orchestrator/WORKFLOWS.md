# Unified Orchestrator Workflows

Use `PLUGIN Orchestrator` as the single entry point and approve each proposed delegation.

## Workflow A: Implementation First, Then Review

1. Orchestrator plans build + implementation + review steps.
2. `PLUGIN Build Specialist` validates build/test workflow.
3. `PLUGIN Feature Specialist` implements the feature and tests.
4. `PLUGIN Review Specialist` performs merge-readiness review.

Copy/paste prompt:

```text
Using PLUGIN Orchestrator, implement issue #<id> in this repo.
Propose a deterministic delegation plan first, then wait for my confirmation before each handoff.
Run `PLUGIN Build Specialist`, then `PLUGIN Feature Specialist`, then `PLUGIN Review Specialist`.
Use CorrelationId corr-impl-review-001 and sequential TaskIds.
Return one final consolidated summary with evidence and open risks.
```

## Workflow B: Review First, Iterate, Re-Review

1. Orchestrator routes initial pass to `PLUGIN Review Specialist`.
2. Findings are delegated to `PLUGIN Feature Specialist` for fixes.
3. Orchestrator re-runs `PLUGIN Review Specialist` for confirmation.

Copy/paste prompt:

```text
Using PLUGIN Orchestrator, run a review-first workflow on current changes.
Delegate first to PLUGIN Review Specialist, then implement fixes with PLUGIN Feature Specialist,
then re-run PLUGIN Review Specialist.
Require explicit confirmation before each delegation and include launch receipts.
```

## AI App Scenario Prompts (Copy/Paste)

### 1) Customer Support Chatbot API

```text
Using PLUGIN Orchestrator, create a .NET AI customer support chatbot API with streaming responses,
conversation memory, and unit/integration tests. Use build -> feature -> review workflow.
Ask for confirmation before each delegation to `PLUGIN Build Specialist`, `PLUGIN Feature Specialist`,
and `PLUGIN Review Specialist`, and include deterministic TaskIds.
```

### 2) RAG Knowledge Assistant

```text
Using PLUGIN Orchestrator, implement a retrieval-augmented generation app:
document ingestion, vector search, and grounded answer generation with citations.
Run `PLUGIN Feature Specialist` for implementation and `PLUGIN Review Specialist` for risk review.
```

### 3) Agentic Workflow App

```text
Using PLUGIN Orchestrator, build an agentic workflow service with planner + executor + tool calls.
First validate pipeline/readiness with `PLUGIN Build Specialist`, then implement with `PLUGIN Feature Specialist`,
then run `PLUGIN Review Specialist` and summarize critical findings first.
```

### 4) Multi-Model Evaluation App

```text
Using PLUGIN Orchestrator, create an AI evaluation app that compares two models on quality, latency,
and cost. Implement test harness + report generation, then run `PLUGIN Review Specialist`
and propose follow-up issues with `PLUGIN Issue Specialist`.
```

## Handoff Naming Rule

Always use exact specialist names in delegation and summaries:

- `PLUGIN Build Specialist`
- `PLUGIN Feature Specialist`
- `PLUGIN Review Specialist`
- `PLUGIN Issue Specialist`
