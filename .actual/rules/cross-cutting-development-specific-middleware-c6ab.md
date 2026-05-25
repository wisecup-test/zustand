# Standardize Middleware Logging for Development and Persistence Operations: Development Specific Middleware

These rules are ALWAYS ACTIVE for all middleware components that handle development tooling or state persistence operations in the src/middleware directory.

### Rules

- **R-DEVMW-001** SHOULD: Development-specific middleware SHOULD use appropriate log levels (DEBUG/INFO) to avoid noise in production

### Verify

```bash
# Count logging statements in middleware directory
grep -r "logger\|console\.log\|console\.error" src/middleware/ | wc -l

# Verify structured logging with request context in devtools and persist
grep -r "request_id\|timestamp" src/middleware/*.ts | grep -E "(devtools|persist)" | wc -l

# Run middleware logging tests
npm test -- --grep "middleware.*logging" --reporter json | jq '.tests | length'
```

**Accept when:**
- All middleware files in src/middleware/ contain at least one logging statement per major operation
- Devtools.ts and persist.ts middleware include structured logging with request context (request_id, timestamp)
- Middleware logging tests pass with coverage >80% for logging code paths

<enforcement>
Claude Code MUST NOT skip or defer verification. Pull requests adding new middleware without logging are blocked by automated checks. Existing middleware without logging is flagged in technical debt backlog for remediation.
</enforcement>