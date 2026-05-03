---
description: 'Radzen Blazor Components usage patterns and best practices'
applyTo: '**/*.razor, **/*.razor.cs, **/*.cs'
---

# Radzen Blazor Components

## Scope

Use this guidance when generating or editing Blazor UI that should use Radzen components as the default UI toolkit.

## Version Baseline

- Target package: `Radzen.Blazor` latest stable.
- Current baseline for this instruction: `10.3.2`.
- If the project is pinned to another version, keep compatibility with the pinned version.

Package update command:
```bash
dotnet add package Radzen.Blazor --version 10.3.2
```

## Required Setup

### Program.cs
```csharp
builder.Services.AddRadzenComponents();
```

### _Imports.razor
```razor
@using Radzen
@using Radzen.Blazor
```

### App.razor
Use one Radzen theme and include the Radzen script.

```razor
<RadzenTheme Theme="material3" @rendermode="InteractiveServer" />
<script src="_content/Radzen.Blazor/Radzen.Blazor.js"></script>
```

## Authoring Rules For This Repo

- Prefer Radzen components over raw HTML when an equivalent exists.
- Prefer `RadzenStack`, `RadzenRow`, and `RadzenColumn` for layout.
- Use strongly typed components (`TItem`, `TValue`) for `RadzenDataGrid`, `RadzenDropDown`, and `RadzenNumeric`.
- Use async event handlers (`async Task`) for UI actions and service calls.
- Keep forms inside `EditForm` with `DataAnnotationsValidator` and `ValidationMessage`.
- Avoid large inline styles; use CSS classes and token variables.
- Add accessibility attributes (`aria-label`, meaningful button text, keyboard-friendly flows).

## High-Value Patterns

### CRUD Grid Pattern
```razor
<RadzenDataGrid Data="@items"
                TItem="ProductDto"
                AllowFiltering="true"
                AllowSorting="true"
                AllowPaging="true"
                PageSize="20"
                Responsive="true">
    <Columns>
        <RadzenDataGridColumn TItem="ProductDto" Property="Id" Title="ID" Width="90px" />
        <RadzenDataGridColumn TItem="ProductDto" Property="Name" Title="Name" />
        <RadzenDataGridColumn TItem="ProductDto" Property="Price" Title="Price" FormatString="{0:C}" />
        <RadzenDataGridColumn TItem="ProductDto" Title="Actions" Sortable="false" Filterable="false">
            <Template Context="row">
                <RadzenButton Icon="edit" ButtonStyle="ButtonStyle.Light" Size="ButtonSize.Small"
                              Click="@(() => EditAsync(row))" />
                <RadzenButton Icon="delete" ButtonStyle="ButtonStyle.Danger" Size="ButtonSize.Small"
                              Click="@(() => ConfirmDeleteAsync(row))" />
            </Template>
        </RadzenDataGridColumn>
    </Columns>
</RadzenDataGrid>
```

### Form Pattern
```razor
<EditForm Model="@model" OnValidSubmit="@SaveAsync">
    <DataAnnotationsValidator />

    <RadzenStack Gap="0.75rem">
        <RadzenFormField Text="Name" Variant="Variant.Outlined">
            <RadzenTextBox @bind-Value="@model.Name" Name="Name" MaxLength="120" />
            <ValidationMessage For="@(() => model.Name)" />
        </RadzenFormField>

        <RadzenFormField Text="Price" Variant="Variant.Outlined">
            <RadzenNumeric @bind-Value="@model.Price" TValue="decimal" Min="0" Step="0.01" Name="Price" />
            <ValidationMessage For="@(() => model.Price)" />
        </RadzenFormField>

        <RadzenStack Orientation="Orientation.Horizontal" Gap="0.5rem">
            <RadzenButton Text="Save" ButtonType="ButtonType.Submit" IsBusy="@isSaving" />
            <RadzenButton Text="Cancel" ButtonStyle="ButtonStyle.Light" Click="@Cancel" />
        </RadzenStack>
    </RadzenStack>
</EditForm>
```

### Dialog + Notification Pattern
```csharp
@inject DialogService DialogService
@inject NotificationService NotificationService

private async Task<bool> ConfirmDeleteAsync()
{
    var confirmed = await DialogService.Confirm(
        "Delete this item?",
        "Confirm",
        new ConfirmOptions { OkButtonText = "Delete", CancelButtonText = "Cancel" });

    return confirmed == true;
}

private void NotifySuccess(string detail)
{
    NotificationService.Notify(new NotificationMessage
    {
        Severity = NotificationSeverity.Success,
        Summary = "Success",
        Detail = detail,
        Duration = 3000
    });
}
```

### Loading + Empty State Pattern
```razor
@if (isLoading)
{
    <RadzenProgressBarCircular Mode="ProgressBarMode.Indeterminate" ShowValue="false" />
}
else if (items.Count == 0)
{
    <RadzenAlert AlertStyle="AlertStyle.Info" Variant="Variant.Flat">
        No records found.
    </RadzenAlert>
}
else
{
    <RadzenDataGrid Data="@items" TItem="ProductDto">
        <Columns>
            <RadzenDataGridColumn TItem="ProductDto" Property="Name" Title="Name" />
        </Columns>
    </RadzenDataGrid>
}
```

## Performance Defaults

- Use `AllowVirtualization="true"` for large datasets and set a fixed grid height.
- Debounce free-text search to reduce server calls.
- Fetch data in `OnInitializedAsync`; re-fetch in `OnParametersSetAsync` only when a relevant parameter changes.
- Avoid reloading all data after every mutation; patch local state when possible.

Debounce example:
```razor
<RadzenTextBox @bind-Value="@search"
               Change="@OnSearchChange"
               Placeholder="Search..." />

@code {
    private System.Timers.Timer? _debounce;
    private string? search;

    private void OnSearchChange(string value)
    {
        _debounce?.Stop();
        _debounce?.Dispose();

        _debounce = new System.Timers.Timer(300);
        _debounce.AutoReset = false;
        _debounce.Elapsed += async (_, _) =>
        {
            await InvokeAsync(async () =>
            {
                search = value;
                await ReloadAsync();
                StateHasChanged();
            });
        };
        _debounce.Start();
    }
}
```

## Accessibility + UX Checklist

- Every icon-only button must include `aria-label`.
- Keep destructive actions behind a confirmation dialog.
- Use `IsBusy` on submit actions to prevent double submits.
- Use clear empty-state and error-state messages.
- Keep tab order natural; do not break keyboard navigation.

## Theme Tokens

Override Radzen CSS variables instead of component-by-component inline styling:

```css
:root {
    --rz-primary: #005eb8;
    --rz-secondary: #344054;
    --rz-success: #0f9d58;
    --rz-warning: #ed6c02;
    --rz-danger: #d92d20;
}
```

## Common Failure Points

1. Components render without styles or behavior.
Cause: Missing `<RadzenTheme />` and/or missing script reference.

2. Dialog/notification service is null or not responding.
Cause: `AddRadzenComponents()` not registered.

3. UI events do not fire in server apps.
Cause: Missing/incorrect render mode for interactive components.

4. Data grid performance degrades with large datasets.
Cause: No virtualization, no paging, or chatty search calls.

## References

- Radzen docs: https://blazor.radzen.com/docs
- Get started: https://blazor.radzen.com/get-started
- Demos: https://blazor.radzen.com/
- GitHub: https://github.com/radzenhq/radzen-blazor
