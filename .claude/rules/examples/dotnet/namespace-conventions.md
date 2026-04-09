# Namespace Conventions (.NET)

Namespaces must mirror the folder path from the project root. Inconsistent namespaces cause IDE navigation failures and confuse dependency injection.

## Rule

- Namespace = `CompanyName.ProjectName` + folder path from project root
- One class per file, filename matches class name exactly
- No `using static` except for well-known BCL types (`Math`, `Console`)
- Never use the default namespace (the project folder name) if it differs from the intended namespace

## Example

```
MyApp/
  Controllers/
    UsersController.cs     → namespace MyApp.Controllers
  Services/
    UserService.cs         → namespace MyApp.Services
  Services/Interfaces/
    IUserService.cs        → namespace MyApp.Services.Interfaces
  Models/Entities/
    User.cs                → namespace MyApp.Models.Entities
  Data/
    ApplicationDbContext.cs → namespace MyApp.Data
```

```csharp
// CORRECT
namespace MyApp.Controllers
{
    public class UsersController : Controller { }
}

// WRONG — folder is Controllers but namespace is MyApp
namespace MyApp
{
    public class UsersController : Controller { }
}
```

## Why

When the project folder name doesn't match the intended namespace (common in scaffolded projects), the default namespace in `.csproj` can differ from the actual code namespace. Always set `<RootNamespace>` in the `.csproj` explicitly:

```xml
<PropertyGroup>
  <RootNamespace>MyApp</RootNamespace>
</PropertyGroup>
```
