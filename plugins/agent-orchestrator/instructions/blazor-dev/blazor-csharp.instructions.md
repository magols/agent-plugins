---
description: 'Coding guidelines for C# and Blazor files. Enforces .NET 8+ conventions: file-scoped namespaces, nullable reference types, code-behind pattern, component disposal, and async lifecycle methods.'
applyTo: '**/*.{cs,razor}'
---

# C# and Blazor Coding Guidelines

## C# Language Conventions

- Use **file-scoped namespaces**: `namespace YourApp.Components;` (not block-scoped)
- Enable **nullable reference types**: `<Nullable>enable</Nullable>` in every `.csproj`
- Use **implicit usings**: `<ImplicitUsings>enable</ImplicitUsings>` — do not add redundant `using System;` etc.
- Use **C# 12+ features**: primary constructors, collection expressions (`[]`), pattern matching
- Prefer `is null` / `is not null` over `== null` / `!= null`
- Use `ArgumentNullException.ThrowIfNull(param)` for null guards at method entry

## Naming

- **Types, methods, properties, events**: `PascalCase`
- **Private and protected fields**: `_camelCase` (underscore prefix)
- **Local variables and parameters**: `camelCase`
- **Interfaces**: `I`-prefix — `IWeatherService`
- **Async methods**: `-Async` suffix — `GetWeatherAsync`
- **Blazor components**: `PascalCase` file and class names matching exactly — `WeatherCard.razor` → `WeatherCard`

## Blazor Component Structure

- Keep `.razor` files focused on **markup only**
- Move logic (fields, lifecycle methods, event handlers) to a **code-behind** `.razor.cs` partial class
- Exception: trivial components with fewer than ~10 lines of logic may use inline `@code`
- The code-behind class must be `partial` and match the component's class name exactly

```csharp
// WeatherCard.razor.cs
public partial class WeatherCard : ComponentBase, IAsyncDisposable { ... }
```

## Component Disposal

- Implement `IAsyncDisposable` (preferred) or `IDisposable` whenever the component:
  - Subscribes to `NavigationManager.LocationChanged`
  - Subscribes to any .NET event on a long-lived object
  - Creates a `PeriodicTimer`, `System.Timers.Timer`, or similar
  - Acquires a JS object reference (`IJSObjectReference`)
- Unsubscribe/dispose in `DisposeAsync()` or `Dispose()`

## Component Parameters

- All `[Parameter]` properties must be **never mutated** inside the component — treat them as read-only
- Use `[EditorRequired]` on mandatory parameters to catch missing values at design time
- Prefer `EventCallback<T>` over `Action<T>` for parent-child callbacks
- Use `[SupplyParameterFromQuery]` for URL query string binding (not manual `NavigationManager` parsing)

## Async Lifecycle Methods

- Always return `Task` (not `void`) from lifecycle overrides: `OnInitializedAsync`, `OnParametersSetAsync`, `OnAfterRenderAsync`
- Never block inside lifecycle methods — no `.Wait()`, `.Result`, `.GetAwaiter().GetResult()`
- Call `StateHasChanged()` after async operations that update state outside of normal lifecycle flow
- Use `CancellationToken` where available for long-running operations

## Dependency Injection

- Register services with the **correct lifetime**:
  - `AddScoped` — per-user state in Blazor Server circuits; per-request in APIs
  - `AddTransient` — stateless, short-lived services
  - `AddSingleton` — thread-safe, app-wide state only
- Use constructor injection in services and code-behind classes for testability
- `[Inject]` property injection is acceptable in `.razor.cs` partial classes

## Rendering and Performance

- Add `@key="item.Id"` on every `@foreach`-rendered component or element
- Avoid calling `StateHasChanged()` in tight loops or on every keystroke — debounce where needed
- Prefer `@rendermode InteractiveServer` or `InteractiveAuto` only where interactivity is genuinely needed; use Static SSR for read-only content

## Error Handling

- Never swallow exceptions silently — always log with `ILogger` before rethrowing or handling
- Use specific exception types; avoid bare `catch (Exception)` except at the top-level boundary
- Wrap external service calls in `try/catch` and handle gracefully in the UI (show error state, not a crash)
