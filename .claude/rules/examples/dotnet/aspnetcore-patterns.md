# ASP.NET Core Patterns

Consistent layering keeps the codebase navigable as it grows. Every new controller, service, and model must follow these patterns.

## Layer Responsibilities

| Layer | Responsibility | What it must NOT do |
|---|---|---|
| Controller | Receive request, call service, return view/JSON | Business logic, direct DB access |
| Service | Business logic, orchestration | Return HTTP results, access `HttpContext` |
| Repository / DbContext | Data access only | Business logic, HTTP concerns |

## Controller Pattern

```csharp
[Authorize]
public class OrdersController : Controller
{
    private readonly IOrderService _orderService;
    private readonly UserManager<ApplicationUser> _userManager;

    public OrdersController(IOrderService orderService, UserManager<ApplicationUser> userManager)
    {
        _orderService = orderService;
        _userManager = userManager;
    }

    public async Task<IActionResult> Index()
    {
        var currentUser = await _userManager.GetUserAsync(User);
        var orders = await _orderService.GetOrdersForUserAsync(currentUser.Id);
        return View(orders);
    }

    [HttpPost, ValidateAntiForgeryToken]
    public async Task<IActionResult> Create(CreateOrderViewModel model)
    {
        if (!ModelState.IsValid) return View(model);

        var currentUser = await _userManager.GetUserAsync(User);
        await _orderService.CreateAsync(model, currentUser.Id);

        TempData["SuccessMessage"] = "Order created successfully.";
        return RedirectToAction("Index");
    }
}
```

## Service Pattern

```csharp
public interface IOrderService
{
    Task<List<Order>> GetOrdersForUserAsync(string userId);
    Task<Order> CreateAsync(CreateOrderViewModel model, string createdByUserId);
}

public class OrderService : IOrderService
{
    private readonly ApplicationDbContext _context;
    private readonly ILogger<OrderService> _logger;

    public OrderService(ApplicationDbContext context, ILogger<OrderService> logger)
    {
        _context = context;
        _logger = logger;
    }

    public async Task<List<Order>> GetOrdersForUserAsync(string userId)
    {
        return await _context.Orders
            .Where(o => o.CreatedByUserId == userId)
            .OrderByDescending(o => o.CreatedAt)
            .ToListAsync();
    }
}
```

## Dependency Injection Registration

```csharp
// Program.cs
builder.Services.AddScoped<IOrderService, OrderService>();
```

## Rules

- Constructor injection only — no service locator (`IServiceProvider.GetService()`)
- Services must be registered as `Scoped` (one per request) unless you have a specific reason for `Singleton` or `Transient`
- Every `[HttpPost]` action must have `[ValidateAntiForgeryToken]`
- Every controller class must have `[Authorize]` unless it intentionally serves anonymous users
- Never access `_context` directly in a controller — always go through a service
