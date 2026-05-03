---
description: "Orchestration standards for confirmation-first delegation, specialist boundaries, and consolidated workflow reporting."
applyTo: "**/*.{md,json,yml,yaml,cs,csproj,sln,slnx}"
---

# Orchestration Instructions

Use these standards whenever the plugin performs multi-step work.

## Core Rules

1. Classify intent before any implementation or review action.
2. Propose delegation and get explicit confirmation before every specialist handoff.
3. Pass a complete delegation context envelope (correlation id, task id, inputs, constraints, expected output).
4. Avoid circular delegation by tracking completed task ids.
5. Consolidate specialist outputs into one result with risks and next-step options.

## Delegation Envelope

Include these fields for each handoff:

- CorrelationId
- TaskId
- IntentCategory
- RequestedSpecialist
- Inputs
- Constraints
- AcceptanceCriteria
- ExpectedOutputFormat

## Canonical Specialist Names

Always use these names in proposals, handoff records, and summaries:

- `PLUGIN Build Specialist`
- `PLUGIN Feature Specialist`
- `PLUGIN Review Specialist`
- `PLUGIN Issue Specialist`

Avoid aliases to keep transcript and audit outputs consistent.

## Specialist Boundaries

- Build Specialist: build workflows, pipelines, diagnostics, release readiness.
- Feature Specialist: code and test implementation.
- Review Specialist: findings, risk, and merge recommendation.
- Issue Specialist: issue/work-item operations on GitHub and Azure DevOps.

If a request crosses boundaries, split it into sequential delegated tasks.

## Output Style

- Keep user-visible updates concise and action-oriented.
- For large workflows, provide a short phase-by-phase progress summary.
- End with explicit completion status and unresolved risks.

## Release Consistency Checks

- Ensure agent and skill names in documentation exactly match filenames and frontmatter values.
- Ensure each mixed workflow includes per-step status and a final consolidated summary.
- Ensure issue-management summaries include GitHub and Azure DevOps provider result details when both are requested.
