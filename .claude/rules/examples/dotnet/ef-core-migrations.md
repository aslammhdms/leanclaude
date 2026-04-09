# EF Core Migrations (.NET)

Migrations are the source of truth for the database schema. Treat them like committed code — never edit an applied migration.

## Rules

- **Never modify an already-applied migration** — create a new one instead
- Migration names must be descriptive: `AddUserEmailIndex`, `CreateOrdersTable`, `RenameProductSkuColumn`
- Always review the generated SQL before applying: `dotnet ef migrations script`
- `Down()` method must be implemented — never leave it empty
- Seeding data goes in a separate migration or a seeder class, not in `Up()` alongside schema changes
- Schema prefix all tables if your project uses multiple schemas (e.g. `Assets.ServiceTickets`)

## Commands

```bash
# Create a migration
dotnet ef migrations add AddUserEmailIndex --context ApplicationDbContext

# Review the generated SQL before applying
dotnet ef migrations script --idempotent

# Apply migrations
dotnet ef database update --context ApplicationDbContext

# Revert the last migration (before it's applied to prod)
dotnet ef migrations remove
```

## DbContext Configuration

```csharp
// Always use GETDATE() (local server time), not GETUTCDATE()
modelBuilder.Entity<User>()
    .Property(u => u.CreatedAt)
    .HasDefaultValueSql("GETDATE()");

// Always add indexes explicitly
modelBuilder.Entity<User>()
    .HasIndex(u => u.Email)
    .IsUnique();

// Schema prefix
modelBuilder.Entity<ServiceTicket>()
    .ToTable("ServiceTickets", schema: "Assets");
```

## Why

Editing an applied migration causes your migration history to diverge from the database state. The next `dotnet ef database update` will either fail or silently skip the change — both are dangerous.

## What NOT to Do

```csharp
// WRONG — editing an existing migration after it's been applied
public partial class AddUserEmailIndex : Migration
{
    protected override void Up(MigrationBuilder migrationBuilder)
    {
        // Don't add new statements here after this migration has run
        migrationBuilder.AddColumn<string>(...); // added later — DANGEROUS
    }
}
```
