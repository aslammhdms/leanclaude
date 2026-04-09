# Environment Configuration (Node.js)

Configuration must be explicit, validated, and never hardcoded. Following 12-factor app principles.

## Rules

- All config comes from environment variables — never hardcode URLs, ports, secrets, or feature flags
- `.env` is in `.gitignore` — never committed
- `.env.example` is committed — contains all keys with placeholder values
- Validate required environment variables at startup — fail fast with a clear error if missing
- Access config through a typed config module, not `process.env` scattered throughout the code

## Config Module Pattern

```typescript
// config/index.ts
function requireEnv(key: string): string {
    const value = process.env[key];
    if (!value) {
        throw new Error(`Missing required environment variable: ${key}`);
    }
    return value;
}

export const config = {
    port: parseInt(process.env.PORT || '3000', 10),
    database: {
        url: requireEnv('DATABASE_URL'),
    },
    auth: {
        jwtSecret: requireEnv('JWT_SECRET'),
        jwtExpiresIn: process.env.JWT_EXPIRES_IN || '7d',
    },
    email: {
        apiKey: requireEnv('EMAIL_API_KEY'),
        from: process.env.EMAIL_FROM || 'noreply@example.com',
    },
    nodeEnv: process.env.NODE_ENV || 'development',
    isProduction: process.env.NODE_ENV === 'production',
};
```

```typescript
// Usage throughout the app — import config, not process.env
import { config } from '../config';

const server = app.listen(config.port);
const client = new EmailClient(config.email.apiKey);
```

## .env.example (committed)

```bash
# Application
PORT=3000
NODE_ENV=development

# Database
DATABASE_URL=postgresql://user:password@localhost:5432/myapp

# Auth
JWT_SECRET=your-secret-here
JWT_EXPIRES_IN=7d

# Email
EMAIL_API_KEY=your-api-key-here
EMAIL_FROM=noreply@example.com
```

## .gitignore entries

```
.env
.env.local
.env.production
```

## Why

Scattered `process.env.X` calls make it impossible to know which variables are required without reading the entire codebase. A typed config module with startup validation catches missing config immediately on deploy — not hours later when a specific code path is hit.
