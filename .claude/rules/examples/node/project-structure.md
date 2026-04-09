# Project Structure (Node.js)

A consistent folder structure makes it obvious where to add new code and where to find existing logic.

## Standard Structure

```
src/
  routes/          # Route definitions only — no business logic
    index.ts       # Root router — mounts all sub-routers
    users.ts       # /users routes
    orders.ts      # /orders routes
  controllers/     # Request handling — thin, delegates to services
    userController.ts
    orderController.ts
  services/        # Business logic — framework-agnostic
    userService.ts
    orderService.ts
  middleware/      # Express middleware (auth, logging, error handling)
    auth.ts
    errorHandler.ts
  models/          # Data models / ORM entities
    User.ts
    Order.ts
  utils/           # Pure utility functions (no side effects)
    dateUtils.ts
    hashUtils.ts
  types/           # Shared TypeScript interfaces and types
    index.ts
  config/          # Configuration loading and validation
    index.ts
tests/
  unit/
  integration/
```

## Rules

- Route handlers delegate immediately to a controller — no logic in route files
- Controllers delegate to services — no DB queries in controllers
- Services are framework-agnostic — they must not import `express`, `req`, or `res`
- Barrel exports (`index.ts`) per directory for clean imports
- No circular dependencies between modules

## Route → Controller → Service

```typescript
// routes/users.ts — routing only
router.post('/users', authenticate, userController.create);

// controllers/userController.ts — request handling only
export const create = async (req: Request, res: Response) => {
    const user = await userService.createUser(req.body);
    res.status(201).json(user);
};

// services/userService.ts — business logic only (no express imports)
export const createUser = async (data: CreateUserDto): Promise<User> => {
    // validate, hash password, save to DB, send welcome email
};
```

## Why

When business logic lives in route handlers, it cannot be tested without HTTP infrastructure. Services that are framework-agnostic can be unit tested directly.
