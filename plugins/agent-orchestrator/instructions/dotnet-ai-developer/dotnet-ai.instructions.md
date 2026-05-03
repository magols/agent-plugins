---
description: 'Conventions for .NET AI development with Microsoft.Extensions.AI and Microsoft Agent Framework.'
applyTo: '**/*.{cs,csproj,sln,slnx,json}'
---

# .NET AI Development Instructions

## Scope

Apply these instructions to code that implements AI features in .NET using `Microsoft.Extensions.AI` and Microsoft Agent Framework.

## Architecture Rules

- Use `IChatClient` as the application boundary for model interactions.
- Keep provider SDK specifics inside infrastructure adapters.
- For deterministic, step-based flows, prefer Agent Framework workflows over autonomous agents.
- If a pure function is enough, avoid introducing agent abstractions.

## Code Quality Rules

- Use async/await end-to-end; do not block with `.Result` or `.Wait()`.
- Pass `CancellationToken` through public async methods and external calls.
- Validate all tool/function inputs before execution.
- Use typed options/config classes for model and provider settings.
- Keep prompts and response mapping logic isolated for easier testing.

## Reliability Rules

- Add explicit timeout handling for model calls.
- Add retry policy for transient failures with bounded attempts.
- Log failures with correlation context and safe payload details.
- Implement graceful fallback behavior for provider outages where feasible.

## Observability Rules

- Use structured logging (event names + key metadata).
- Emit telemetry spans/metrics for model calls and tool calls.
- Track latency, failure rates, and token/cost indicators where available.

## Security Rules

- Never hardcode API keys, secrets, or endpoints in source code.
- Use environment variables, user-secrets, or key vault integrations.
- Treat generated model output as untrusted input unless validated.
- Apply content and policy checks when handling external/user content.

## Testing Rules

- Unit tests should mock `IChatClient` or adapter boundaries.
- Add tests for success, timeout, cancellation, and malformed output paths.
- Add integration tests for provider wiring and DI registration.
- New or changed business logic paths should include test updates.

## Docs Validation Rule

Before implementing new APIs or patterns:

1. Search current docs for the feature.
2. Check official C# sample patterns.
3. Note preview APIs explicitly in comments or PR descriptions.