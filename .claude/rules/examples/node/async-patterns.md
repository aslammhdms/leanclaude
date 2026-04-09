# Async Patterns (Node.js)

Async bugs are among the hardest to debug. Follow these patterns to avoid silent failures and unhandled rejections.

## Rules

- Always `await` async calls — never leave floating promises
- Never mix callbacks and promises in the same flow
- Use `async/await` over `.then().catch()` chains for readability
- Wrap Express route handlers in an async error wrapper — unhandled rejections crash the process

## Async Route Handler Wrapper

Express does not catch errors thrown inside `async` route handlers by default. Always wrap them:

```typescript
// utils/asyncHandler.ts
export const asyncHandler = (fn: Function) => (req: Request, res: Response, next: NextFunction) => {
    Promise.resolve(fn(req, res, next)).catch(next);
};

// Usage in routes
router.get('/users', asyncHandler(async (req, res) => {
    const users = await userService.getAll();
    res.json(users);
}));
```

## Error Handling

Never swallow errors silently:

```typescript
// WRONG — error is swallowed
try {
    await sendEmail(user.email);
} catch (err) {
    // silence
}

// CORRECT — log it, decide whether to rethrow
try {
    await sendEmail(user.email);
} catch (err) {
    logger.warn('Failed to send welcome email', { userId: user.id, error: err });
    // Only swallow if this is truly non-critical — document why
}
```

## Parallel Execution

When multiple async operations are independent, run them in parallel:

```typescript
// WRONG — sequential when they could be parallel
const user = await userService.getById(id);
const orders = await orderService.getByUserId(id);

// CORRECT — parallel
const [user, orders] = await Promise.all([
    userService.getById(id),
    orderService.getByUserId(id),
]);
```

## Avoiding Floating Promises

```typescript
// WRONG — fire and forget, errors are lost
sendAnalyticsEvent(data);

// CORRECT — if truly fire-and-forget, handle the rejection explicitly
sendAnalyticsEvent(data).catch(err =>
    logger.warn('Analytics event failed', { error: err })
);
```

## Why

Unhandled promise rejections in Node.js cause process crashes in production (Node 15+). A single missing `await` or uncaught `.catch()` can take down the entire server.
