---
description: 'Code review severity standards and quick-reference flags for C# and Blazor files. Applied automatically during review to enforce consistent categorization.'
applyTo: '**/*.{cs,razor}'
---

# C# / Blazor Code Review Standards

## Severity Levels

| Level | Label | Meaning | Merge Impact |
|-------|-------|---------|--------------|
| 🔴 | **Critical** | Security vulnerability, data loss, production crash, or correctness bug | **BLOCK** — must fix before merge |
| 🟡 | **Warning** | Performance problem, maintainability issue, or subtle correctness risk | **FIX SOON** — merge with clear remediation plan |
| 🔵 | **Suggestion** | Style, readability, or modernization opportunity | **OPTIONAL** — author's discretion |

## Always Flag as Critical 🔴

- `async void` method outside an event handler
- `.Wait()`, `.Result`, or `.GetAwaiter().GetResult()` in an async call path
- `catch (Exception)` that swallows the exception without logging
- Scoped DI service injected into a Singleton
- `MarkupString` constructed from unsanitized user input
- String interpolation in `FromSqlRaw` / `ExecuteSqlRaw`
- API key, secret, or connection string hardcoded in Blazor WASM client code
- Missing `[Authorize]` on a page or API endpoint that handles sensitive data
- Blazor component subscribes to events without implementing `IDisposable`/`IAsyncDisposable`

## Always Flag as Warning 🟡

- Missing `CancellationToken` on a public async method signature
- Null-forgiving operator (`!`) without a comment justifying it
- Navigation properties accessed in a loop without `Include()` (N+1)
- Missing `AsNoTracking()` on read-only EF Core queries in high-traffic code paths
- `Action<T>` used for component event callbacks instead of `EventCallback<T>`
- `StateHasChanged()` called excessively (in a loop or on every input change)
- `[Parameter]` property mutated inside the component
- New public business logic with no unit test

## Always Flag as Suggestion 🔵

- Inline `@code` block with more than ~15 lines (suggest code-behind)
- Missing `@key` on `@foreach`-rendered components
- Missing `ConfigureAwait(false)` in library (non-application) code
- Magic literal values in test assertions (suggest named constants)
- Non-file-scoped namespace declaration
- Redundant `using` statements when implicit usings are enabled

## Review Output Template

```
## 🔴 Critical
### [File:Line] Issue Title
> `code snippet`
**Risk**: explanation
✅ **Fix**: corrected code

## 🟡 Warning
...

## 🔵 Suggestion
...

---
## Summary
| Severity | Count |
|----------|-------|
| 🔴 Critical | N |
| 🟡 Warning | N |
| 🔵 Suggestion | N |

**Merge recommendation**: BLOCK / APPROVE WITH WARNINGS / APPROVE
```
