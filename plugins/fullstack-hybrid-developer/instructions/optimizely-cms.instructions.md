---
applyTo: "**/*.cs, **/*.razor, **/*.cshtml"
description: "Guidelines for building applications with Optimizely CMS (formerly Episerver)"
---

# Optimizely CMS Instructions

## Overview
Optimizely CMS (formerly Episerver) is a .NET-based content management system. These instructions cover integration patterns, best practices, and modern development approaches for Optimizely CMS 12+ (.NET Core/5+).

## Core Principles

### Content Modeling
- Use `ContentType` attribute with GUID, DisplayName, and Description for all content types
- Inherit from appropriate base classes: `PageData`, `BlockData`, `MediaData`
- Use meaningful property names and display attributes for editor experience
- Leverage `[Display]` attributes with Name, GroupName, and Order
- Use `[UIHint]` for specialized editor controls
- Apply `[Required]`, `[StringLength]`, and other validation attributes

```csharp
[ContentType(
    GUID = "a1b2c3d4-e5f6-4a5b-8c9d-0e1f2a3b4c5d",
    DisplayName = "Article Page",
    Description = "Standard article page with rich content")]
public class ArticlePage : PageData
{
    [Display(
        Name = "Heading",
        GroupName = SystemTabNames.Content,
        Order = 10)]
    [Required]
    [StringLength(100)]
    public virtual string Heading { get; set; }

    [Display(
        Name = "Main Content",
        GroupName = SystemTabNames.Content,
        Order = 20)]
    [UIHint(UIHint.Textarea)]
    public virtual XhtmlString MainBody { get; set; }
}
```

### Content Areas and Blocks
- Prefer blocks over page properties for reusable content
- Use `ContentArea` for flexible content composition
- Apply `[AllowedTypes]` to restrict block types in content areas
- Create block previews with appropriate CSS classes
- Use partial views for block rendering

```csharp
[Display(
    Name = "Main Content Area",
    Order = 30)]
[AllowedTypes(typeof(HeroBlock), typeof(TextBlock), typeof(ImageBlock))]
public virtual ContentArea MainContentArea { get; set; }
```

### Dependency Injection
- Register services in `Startup.cs` or `Program.cs`
- Use constructor injection for repositories and services
- Avoid service locator pattern
- Leverage built-in services: `IContentLoader`, `IContentRepository`, `IUrlResolver`

```csharp
public class ArticleController : PageController<ArticlePage>
{
    private readonly IContentLoader _contentLoader;
    private readonly IUrlResolver _urlResolver;

    public ArticleController(
        IContentLoader contentLoader,
        IUrlResolver urlResolver)
    {
        _contentLoader = contentLoader;
        _urlResolver = urlResolver;
    }
}
```

## Routing and Controllers

### Page Controllers
- Inherit from `PageController<T>` where T is your page type
- Use `Index` action method as default
- Return typed views with strongly-typed models
- Avoid business logic in controllers; use services

```csharp
public class ArticleController : PageController<ArticlePage>
{
    public IActionResult Index(ArticlePage currentPage)
    {
        var model = new ArticleViewModel
        {
            CurrentPage = currentPage,
            // Additional view model properties
        };
        return View(model);
    }
}
```

### Partial Controllers for Blocks
- Use `BlockComponent<T>` for blocks in CMS 12+
- Keep rendering logic simple and focused
- Use view models to separate concerns

```csharp
public class HeroBlockComponent : BlockComponent<HeroBlock>
{
    protected override IViewComponentResult InvokeComponent(HeroBlock currentBlock)
    {
        return View(currentBlock);
    }
}
```

## Data Access Patterns

### Content Loading
- Use `IContentLoader` for reading content
- Use `IContentRepository` for CRUD operations
- Always check for `null` and access rights
- Use `GetChildren<T>()` with type filtering

```csharp
var page = _contentLoader.Get<ArticlePage>(contentLink);
var children = _contentLoader.GetChildren<ArticlePage>(
    parentLink, 
    new LoaderOptions { LanguageLoaderOption.FallbackWithMaster() });

if (_contentLoader.TryGet<ArticlePage>(contentLink, out var result))
{
    // Use result
}
```

### Content Publishing
- Use `IContentRepository.Save()` for creating/updating
- Set appropriate `SaveAction` (Publish, CheckIn, etc.)
- Handle versioning appropriately
- Use `IContentEvents` for event handling

```csharp
var writeable = content.CreateWritableClone();
writeable.Name = "New Name";
_contentRepository.Save(writeable, SaveAction.Publish, AccessLevel.NoAccess);
```

## Search and Find

### Find API
- Use `IClient` from EPiServer.Find for search
- Build queries fluently with LINQ-like syntax
- Apply filters, facets, and sorting
- Use pagination for large result sets

```csharp
var results = _findClient.Search<ArticlePage>()
    .Filter(x => x.Heading.Match("search term"))
    .OrderByDescending(x => x.StartPublish)
    .Take(10)
    .GetResult();
```

## Security and Access Control

### Content Security
- Use `IContentSecurityRepository` for permissions
- Check access rights before displaying content
- Apply `[Authorize]` attributes on controllers when needed
- Use role-based access where appropriate

```csharp
if (_contentLoader.Get<IContent>(contentLink)
    .QueryDistinctAccess(AccessLevel.Read)
    .Any(x => x == SecurityEntityType.Everyone))
{
    // Content is public
}
```

## Performance Optimization

### Caching
- Leverage built-in output cache with `[ContentOutputCache]`
- Use `IObjectInstanceCache` for custom caching
- Set appropriate cache dependencies
- Avoid caching user-specific content

```csharp
[ContentOutputCache]
public IActionResult Index(ArticlePage currentPage)
{
    // Cached output
}
```

### Loading Strategies
- Use `LoaderOptions` to control language fallback
- Batch load content when possible
- Avoid N+1 queries in loops
- Use projections in Find queries

## Modern Integration Patterns

### Headless/Content Delivery API
- Enable Content Delivery API for headless scenarios
- Use OData queries for filtering and expansion
- Secure API endpoints appropriately
- Version your API contracts

### Blazor Integration
- Use Optimizely CMS as backend/CMS
- Fetch content via Content Delivery API or custom APIs
- Consider server-side rendering for SEO
- Cache API responses appropriately

### Commerce Integration
- Use `EPiServer.Commerce` for e-commerce features
- Separate catalog from CMS content
- Use `IOrderRepository` and `ICartService` for orders
- Implement proper inventory management

## Version-Specific Features

### CMS 12+ (.NET 6+)
- Migrated to .NET Core/5+
- Updated dependency injection patterns
- New routing system
- Enhanced content delivery API
- Block components instead of partial controllers

### Breaking Changes from CMS 11
- No more `ServiceLocator`; use DI
- Controllers follow ASP.NET Core conventions
- Updated initialization modules
- New configuration system (appsettings.json)

## Best Practices

### Do's
✅ Use strongly-typed content types
✅ Implement view models for separation of concerns
✅ Follow SOLID principles in content models
✅ Use async/await for I/O operations
✅ Write unit tests for business logic
✅ Document complex content models
✅ Use display options for flexible rendering
✅ Implement proper error handling
✅ Follow Optimizely's naming conventions

### Don'ts
❌ Avoid service locator pattern
❌ Don't put business logic in content types
❌ Don't bypass security checks
❌ Avoid synchronous calls in async contexts
❌ Don't cache user-specific content globally
❌ Avoid hardcoding content references
❌ Don't ignore content events for cache invalidation
❌ Avoid circular dependencies in content models

## Configuration

### appsettings.json
```json
{
  "EPiServer": {
    "CmsUI": {
      "Enabled": true
    },
    "Find": {
      "ServiceUrl": "https://your-find-instance.find.episerver.net/",
      "DefaultIndex": "your-index"
    }
  }
}
```

### Startup Configuration
```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddCms()
        .AddFind()
        .AddOptimizelyIdentity();
    
    services.AddMvc();
    // Additional service registrations
}

public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    
    app.UseStaticFiles();
    app.UseRouting();
    app.UseAuthentication();
    app.UseAuthorization();
    
    app.UseEndpoints(endpoints =>
    {
        endpoints.MapContent();
        endpoints.MapControllerRoute(
            name: "default",
            pattern: "{controller=Home}/{action=Index}/{id?}");
    });
}
```

## Testing

### Unit Testing Content Types
- Use mocked `IContentLoader` and repositories
- Test property validation
- Verify business logic in controllers
- Use `PageData` and `BlockData` test builders

### Integration Testing
- Use in-memory database for testing
- Mock external dependencies
- Test complete workflows
- Verify security policies

## Resources
- [Optimizely Documentation](https://docs.developers.optimizely.com/)
- [Optimizely World](https://world.optimizely.com/)
- [GitHub Samples](https://github.com/episerver)
- [Optimizely NuGet Packages](https://nuget.optimizely.com/)

## Troubleshooting

### Common Issues
1. **Content not appearing**: Check publishing status and access rights
2. **404 errors**: Verify routing configuration and URL resolver
3. **Cache issues**: Clear cache and check cache dependencies
4. **Performance problems**: Enable SQL profiling and Find query logs
5. **Migration issues**: Review CMS 12 migration guide for breaking changes

## Quick Reference

### Key Interfaces
- `IContentLoader` - Read content
- `IContentRepository` - CRUD operations
- `IUrlResolver` - URL generation
- `IContentEvents` - Content lifecycle events
- `IContentSecurityRepository` - Security/permissions
- `IClient` (Find) - Search functionality

### Common Attributes
- `[ContentType]` - Define content type
- `[Display]` - UI metadata
- `[UIHint]` - Editor control
- `[Required]`, `[StringLength]` - Validation
- `[AllowedTypes]` - Content area restrictions
- `[ContentOutputCache]` - Output caching

### Base Classes
- `PageData` - Page types
- `BlockData` - Block types
- `MediaData` - Media types
- `PageController<T>` - Page controllers
- `BlockComponent<T>` - Block components (CMS 12+)
