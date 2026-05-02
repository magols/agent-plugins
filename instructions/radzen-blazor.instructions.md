---
description: 'Radzen Blazor Components usage patterns and best practices'
applyTo: '**/*.razor, **/*.razor.cs, **/*.cs'
---

# Radzen Blazor Components

## Overview

This project uses [Radzen.Blazor](https://blazor.radzen.com/) v8 for UI components. Radzen provides a comprehensive set of enterprise-grade Blazor components with Material 3 theme support.

## Configuration

### Theme Setup

The project uses Material 3 theme configured in `App.razor`:
```razor
<RadzenTheme Theme="material3" @rendermode="InteractiveServer"/>
```

### Service Registration

Radzen services are registered in `Program.cs`:
```csharp
builder.Services.AddRadzenComponents();
```

## Common Components

### Form Controls

#### RadzenTextBox
```razor
<RadzenTextBox @bind-Value="@model.Name" 
               Placeholder="Enter name" 
               MaxLength="100" />
```

#### RadzenNumeric
```razor
<RadzenNumeric @bind-Value="@model.Age" 
               TValue="int" 
               Min="0" 
               Max="120" />
```

#### RadzenDropDown
```razor
<RadzenDropDown @bind-Value="@selectedValue"
                Data="@items"
                TextProperty="Name"
                ValueProperty="Id"
                Placeholder="Select an option" />
```

#### RadzenCheckBox
```razor
<RadzenCheckBox @bind-Value="@model.IsActive" 
                Name="isActive" />
<RadzenLabel Text="Active" Component="isActive" Style="margin-left: 8px;" />
```

#### RadzenDatePicker
```razor
<RadzenDatePicker @bind-Value="@model.DateOfBirth" 
                  DateFormat="MM/dd/yyyy" 
                  ShowCalendarWeek="true" />
```

### Buttons and Actions

#### RadzenButton
```razor
<RadzenButton Text="Save" 
              ButtonStyle="ButtonStyle.Primary" 
              Click="@OnSaveAsync" 
              IsBusy="@isSaving" />
              
<RadzenButton Icon="delete" 
              ButtonStyle="ButtonStyle.Danger" 
              Click="@OnDeleteAsync"
              Variant="Variant.Outlined" />
```

#### RadzenSplitButton
```razor
<RadzenSplitButton Icon="save" 
                   Text="Save" 
                   Click="@OnSaveAsync">
    <ChildContent>
        <RadzenSplitButtonItem Text="Save and Close" Icon="save" Click="@OnSaveAndCloseAsync" />
        <RadzenSplitButtonItem Text="Save as Draft" Icon="drafts" Click="@OnSaveDraftAsync" />
    </ChildContent>
</RadzenSplitButton>
```

### Layout Components

#### RadzenCard
```razor
<RadzenCard>
    <RadzenStack Orientation="Orientation.Vertical" Gap="1rem">
        <RadzenText TextStyle="TextStyle.H6">Card Title</RadzenText>
        <RadzenText>Card content goes here.</RadzenText>
    </RadzenStack>
</RadzenCard>
```

#### RadzenStack
```razor
<RadzenStack Orientation="Orientation.Horizontal" 
             Gap="1rem" 
             AlignItems="AlignItems.Center">
    <RadzenIcon Icon="home" />
    <RadzenText>Home</RadzenText>
</RadzenStack>
```

#### RadzenRow and RadzenColumn
```razor
<RadzenRow Gap="1rem">
    <RadzenColumn Size="12" SizeMD="6">
        <RadzenCard>Left Column</RadzenCard>
    </RadzenColumn>
    <RadzenColumn Size="12" SizeMD="6">
        <RadzenCard>Right Column</RadzenCard>
    </RadzenColumn>
</RadzenRow>
```

### Data Display

#### RadzenDataGrid
```razor
<RadzenDataGrid Data="@items" 
                TItem="MyItem" 
                AllowFiltering="true" 
                AllowSorting="true"
                AllowPaging="true" 
                PageSize="10">
    <Columns>
        <RadzenDataGridColumn TItem="MyItem" Property="Id" Title="ID" Width="80px" />
        <RadzenDataGridColumn TItem="MyItem" Property="Name" Title="Name" />
        <RadzenDataGridColumn TItem="MyItem" Property="CreatedDate" Title="Created" FormatString="{0:MM/dd/yyyy}" />
        <RadzenDataGridColumn TItem="MyItem" Title="Actions" Sortable="false" Filterable="false">
            <Template Context="item">
                <RadzenButton Icon="edit" ButtonStyle="ButtonStyle.Light" Size="ButtonSize.Small" 
                              Click="@(() => OnEditAsync(item))" />
                <RadzenButton Icon="delete" ButtonStyle="ButtonStyle.Danger" Size="ButtonSize.Small" 
                              Click="@(() => OnDeleteAsync(item))" />
            </Template>
        </RadzenDataGridColumn>
    </Columns>
</RadzenDataGrid>
```

#### RadzenDataList
```razor
<RadzenDataList Data="@items" TItem="MyItem">
    <Template Context="item">
        <RadzenCard Style="margin-bottom: 1rem;">
            <RadzenText TextStyle="TextStyle.Subtitle1">@item.Name</RadzenText>
            <RadzenText>@item.Description</RadzenText>
        </RadzenCard>
    </Template>
</RadzenDataList>
```

### Navigation

#### RadzenMenu
```razor
<RadzenMenu>
    <RadzenMenuItem Text="Home" Icon="home" Path="/" />
    <RadzenMenuItem Text="Products" Icon="shopping_cart">
        <RadzenMenuItem Text="All Products" Path="/products" />
        <RadzenMenuItem Text="Categories" Path="/categories" />
    </RadzenMenuItem>
    <RadzenMenuItem Text="Settings" Icon="settings" Path="/settings" />
</RadzenMenu>
```

#### RadzenPanelMenu
```razor
<RadzenPanelMenu>
    <RadzenPanelMenuItem Text="Dashboard" Icon="dashboard" Path="/" />
    <RadzenPanelMenuItem Text="Reports" Icon="assessment" Expanded="true">
        <RadzenPanelMenuItem Text="Sales Report" Path="/reports/sales" />
        <RadzenPanelMenuItem Text="Inventory Report" Path="/reports/inventory" />
    </RadzenPanelMenuItem>
</RadzenPanelMenu>
```

#### RadzenBreadCrumb
```razor
<RadzenBreadCrumb>
    <RadzenBreadCrumbItem Path="/" Text="Home" Icon="home" />
    <RadzenBreadCrumbItem Path="/products" Text="Products" />
    <RadzenBreadCrumbItem Text="Product Details" />
</RadzenBreadCrumb>
```

#### RadzenSteps
```razor
<RadzenSteps @bind-SelectedIndex="@selectedStep">
    <Steps>
        <RadzenStepsItem Text="Step 1" />
        <RadzenStepsItem Text="Step 2" />
        <RadzenStepsItem Text="Step 3" />
    </Steps>
</RadzenSteps>
```

### Dialogs and Overlays

#### RadzenDialog Service
```csharp
@inject DialogService DialogService

private async Task ShowDialogAsync()
{
    await DialogService.OpenAsync<MyDialogComponent>("Dialog Title",
        new Dictionary<string, object>() { { "Parameter", value } },
        new DialogOptions() { Width = "600px", Height = "400px" });
}
```

#### RadzenNotification Service
```csharp
@inject NotificationService NotificationService

private void ShowNotification(NotificationSeverity severity = NotificationSeverity.Success)
{
    NotificationService.Notify(new NotificationMessage
    {
        Severity = severity,
        Summary = "Success",
        Detail = "Operation completed successfully.",
        Duration = 4000
    });
}
```

#### RadzenTooltip Service
```csharp
@inject TooltipService TooltipService

<RadzenButton Text="Hover me" 
              MouseEnter="@(args => ShowTooltip(args))" />

@code {
    void ShowTooltip(ElementReference elementReference)
    {
        TooltipService.Open(elementReference, "Tooltip content", 
            new TooltipOptions() { Duration = 3000 });
    }
}
```

#### RadzenContextMenu Service
```csharp
@inject ContextMenuService ContextMenuService

<div @oncontextmenu="@ShowContextMenu" @oncontextmenu:preventDefault="true">
    Right-click me
</div>

@code {
    void ShowContextMenu(MouseEventArgs args)
    {
        ContextMenuService.Open(args, ds =>
            @<RadzenMenu Click="OnMenuItemClick">
                <RadzenMenuItem Text="Edit" Icon="edit" Value="edit" />
                <RadzenMenuItem Text="Delete" Icon="delete" Value="delete" />
            </RadzenMenu>);
    }
}
```

### Feedback Components

#### RadzenProgressBar
```razor
<RadzenProgressBar Value="@progress" 
                   Max="100" 
                   ShowValue="true" 
                   Unit="%" />
```

#### RadzenProgressBarCircular
```razor
<RadzenProgressBarCircular Value="@progress" 
                           Max="100" 
                           ShowValue="true">
    <Template>@progress%</Template>
</RadzenProgressBarCircular>
```

#### RadzenAlert
```razor
<RadzenAlert AlertStyle="AlertStyle.Success" 
             Variant="Variant.Flat" 
             AllowClose="true">
    Operation completed successfully!
</RadzenAlert>
```

#### RadzenBadge
```razor
<RadzenBadge BadgeStyle="BadgeStyle.Danger" 
             Text="@notificationCount.ToString()" 
             Shade="Shade.Lighter" />
```

## Best Practices

### Component Initialization

Use lifecycle methods appropriately:
```csharp
@code {
    protected override async Task OnInitializedAsync()
    {
        // Load initial data
        await LoadDataAsync();
    }
    
    protected override async Task OnParametersSetAsync()
    {
        // React to parameter changes
        if (ItemId != previousItemId)
        {
            await LoadItemAsync(ItemId);
        }
    }
}
```

### Performance Optimization

1. **Virtualization for Large Lists**
```razor
<RadzenDataGrid Data="@items" 
                AllowVirtualization="true" 
                Style="height:400px">
    <!-- columns -->
</RadzenDataGrid>
```

2. **Debouncing Input**
```razor
<RadzenTextBox @bind-Value="@searchText" 
               Change="@OnSearchChangedAsync" 
               Placeholder="Search..." />

@code {
    private System.Timers.Timer? debounceTimer;
    
    private void OnSearchChangedAsync(string value)
    {
        debounceTimer?.Stop();
        debounceTimer = new System.Timers.Timer(300);
        debounceTimer.Elapsed += async (sender, e) =>
        {
            await InvokeAsync(async () =>
            {
                await PerformSearchAsync(value);
                StateHasChanged();
            });
        };
        debounceTimer.AutoReset = false;
        debounceTimer.Start();
    }
}
```

### Form Validation

Integrate with Blazor's validation:
```razor
<EditForm Model="@model" OnValidSubmit="@OnValidSubmitAsync">
    <DataAnnotationsValidator />
    <RadzenStack Gap="1rem">
        <RadzenFormField Text="Name" Variant="Variant.Outlined">
            <RadzenTextBox @bind-Value="@model.Name" />
            <ValidationMessage For="@(() => model.Name)" />
        </RadzenFormField>
        
        <RadzenFormField Text="Email" Variant="Variant.Outlined">
            <RadzenTextBox @bind-Value="@model.Email" />
            <ValidationMessage For="@(() => model.Email)" />
        </RadzenFormField>
        
        <RadzenButton ButtonType="ButtonType.Submit" Text="Submit" />
    </RadzenStack>
</EditForm>
```

### Accessibility

Radzen components are built with accessibility in mind:
- Use appropriate ARIA labels
- Ensure keyboard navigation works
- Provide meaningful text for screen readers

```razor
<RadzenButton Text="Delete" 
              Icon="delete" 
              aria-label="Delete item" 
              Click="@OnDeleteAsync" />
```

### Theming

Customize theme colors by overriding CSS variables:
```css
:root {
    --rz-primary: #1976d2;
    --rz-secondary: #424242;
    --rz-info: #0288d1;
    --rz-success: #388e3c;
    --rz-warning: #f57c00;
    --rz-danger: #d32f2f;
}
```

## Common Patterns

### Master-Detail View
```razor
<RadzenSplitter>
    <RadzenSplitterPane Size="30%">
        <RadzenDataList Data="@items" TItem="MyItem" 
                        SelectionMode="DataListSelectionMode.Single"
                        @bind-Value="@selectedItem">
            <Template Context="item">
                <RadzenText>@item.Name</RadzenText>
            </Template>
        </RadzenDataList>
    </RadzenSplitterPane>
    <RadzenSplitterPane Size="70%">
        @if (selectedItem != null)
        {
            <RadzenCard>
                <h3>@selectedItem.Name</h3>
                <p>@selectedItem.Description</p>
            </RadzenCard>
        }
    </RadzenSplitterPane>
</RadzenSplitter>
```

### Loading State
```razor
@if (isLoading)
{
    <RadzenProgressBarCircular ProgressBarStyle="ProgressBarStyle.Primary" 
                               Value="100" 
                               ShowValue="false"
                               Mode="ProgressBarMode.Indeterminate" />
}
else
{
    <RadzenDataGrid Data="@items" TItem="MyItem">
        <!-- columns -->
    </RadzenDataGrid>
}
```

### Confirmation Dialog
```csharp
private async Task<bool> ConfirmDeleteAsync()
{
    var result = await DialogService.Confirm(
        "Are you sure you want to delete this item?",
        "Confirm Delete",
        new ConfirmOptions() { OkButtonText = "Yes", CancelButtonText = "No" });
    
    return result == true;
}
```

## Resources

- [Radzen Blazor Components Documentation](https://blazor.radzen.com/)
- [Radzen Demos](https://blazor.radzen.com/get-started)
- [Radzen GitHub Repository](https://github.com/radzenhq/radzen-blazor)
- [Theme Customization](https://blazor.radzen.com/themes)

## Troubleshooting

### Common Issues

1. **Components not rendering**: Ensure `AddRadzenComponents()` is called in `Program.cs`
2. **Theme not applied**: Verify `<RadzenTheme>` is in `App.razor` with correct render mode
3. **Services not available**: Check that services (DialogService, NotificationService) are injected
4. **Event handlers not firing**: Ensure components have `@rendermode InteractiveServer` or appropriate render mode
