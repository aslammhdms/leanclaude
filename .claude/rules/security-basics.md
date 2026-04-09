# Security Basics

Every endpoint and data operation must follow these baseline security practices. This is the minimum bar — not the ceiling.

## OWASP Top 10 Reference

### A01 — Broken Access Control
- Every route/endpoint must check authentication **and** authorization
- Never rely on UI hiding alone — validate permissions server-side on every state-changing request
- Check object ownership before returning or modifying data (prevent IDOR)
- Apply tenant/branch/org filtering on every data query for multi-tenant apps

### A02 — Cryptographic Failures
- Never store passwords in plain text — use bcrypt, Argon2, or equivalent
- Never put secrets, tokens, or credentials in source code
- Use HTTPS for all production traffic — never transmit sensitive data over HTTP
- Sensitive data (tokens, session IDs) must not appear in URLs or logs

### A03 — Injection
- **SQL**: always use parameterized queries or ORMs — never concatenate user input into SQL
- **HTML**: never render user input as raw HTML — escape output (XSS prevention)
- **Shell**: never pass user input to shell commands

```javascript
// WRONG — SQL injection risk
const query = `SELECT * FROM users WHERE email = '${req.body.email}'`;

// CORRECT — parameterized
const query = 'SELECT * FROM users WHERE email = $1';
db.query(query, [req.body.email]);
```

### A04 — Insecure Design
- Business logic belongs in the service layer, not in route handlers
- Permission checks must be consistent — never duplicated inconsistently across handlers
- Validate input at system boundaries (user input, external APIs) — not deep in business logic

### A05 — Security Misconfiguration
- Never commit `.env` files, `appsettings.Development.json`, or any secrets file
- Disable detailed error pages / stack traces in production
- Remove unused endpoints, debug routes, and admin tools before deploying

### A06 — Vulnerable Components
- Regularly audit dependencies: `npm audit`, `dotnet list package --vulnerable`, `pip-audit`
- Keep dependencies updated — especially security patches
- Pin dependency versions for reproducible builds

### A07 — Authentication Failures
- Invalidate sessions on logout — do not just clear the cookie client-side
- Enforce strong password policies (minimum length, complexity)
- Rate-limit login attempts to prevent brute force

### A08 — Integrity Failures
- Pin dependency versions in lock files (`package-lock.json`, `poetry.lock`, etc.)
- Verify integrity of downloaded artifacts in CI (checksums)

### A09 — Logging Failures
- Log all authentication events (login, logout, failed attempts) with user identity
- Log all sensitive operations (delete, role change, permission change) with actor
- Never log passwords, tokens, or PII

### A10 — SSRF
- Never construct URLs from user input without strict allowlisting
- Never redirect to user-supplied URLs without validation
- Allowlist external services your app is permitted to contact

## Secrets Management

```
NEVER hardcode:               USE instead:
API keys                  →   Environment variables
Database passwords        →   Secrets manager (Vault, AWS Secrets Manager, Azure KV)
JWT signing keys          →   Environment variables
Service account tokens    →   Environment variables
```

`.env` files:
- `.env` → in `.gitignore`, never committed
- `.env.example` → committed with placeholder values only

## Input Validation

- Validate at the **boundary** — where data enters your system (API endpoint, form submit)
- Validate **type, length, format, and range**
- Reject invalid input early — do not sanitize and hope

```javascript
// Validate before processing — don't trust client data
if (!req.body.email || !isValidEmail(req.body.email)) {
    return res.status(400).json({ error: 'Valid email required' });
}
```

## CSRF Protection

- All state-changing requests (POST, PUT, DELETE, PATCH) must include CSRF token validation
- GET requests must never cause state changes
