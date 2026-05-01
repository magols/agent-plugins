# csharp-code-review

Structured PR review and security review for **C# and Blazor** codebases, with OWASP-mapped findings.

## Agents

| Agent | Description |
|-------|-------------|
| `C# Code Reviewer` | Thorough C#/Blazor code review agent. Covers SOLID, async correctness, Blazor anti-patterns, DI lifetime mismatches, EF Core, and test coverage. Produces Critical / Warning / Suggestion findings. |

## Skills

| Skill | Description |
|-------|-------------|
| `csharp-pr-review` | Structured PR review checklist for C#/Blazor. Naming, null safety, async/await, DI lifetimes, EF Core N+1, Blazor render modes, disposal patterns, test coverage. |
| `blazor-security-review` | Security-focused review mapped to OWASP Top 10. XSS via MarkupString, auth/authorization, CSRF, WASM client-side secrets, SignalR hub security, input validation, CSP headers. |

## Instructions

| File | Applies To | Description |
|------|-----------|-------------|
| `csharp-review-standards.instructions.md` | `**/*.{cs,razor}` | Review severity levels, quick-reference Critical/Warning/Suggestion flags, and output template. |
