# Enforce Input Validation and Sanitization in Security-Critical Code Paths: Validation Logic Use

These rules are ALWAYS ACTIVE for all code paths handling external input, user-provided data, or data crossing trust boundaries, including HTTP request handlers, configuration file parsers, build tool configurations, API endpoints, environment variable processing, and file upload handlers.

### Rules

- **R-VAL-001** SHOULD: Validation logic SHOULD use allowlists (permitted patterns) rather than denylists (blocked patterns) where feasible.

### Verify

```bash
# Check for unvalidated environment variable access
grep -r "process\.env" --include="*.ts" --include="*.js" | grep -v "validate\|sanitize\|check" | wc -l

# Check for unvalidated HTTP request handlers
grep -r "req\.body\|req\.query\|req\.params" --include="*.ts" | grep -v "validate\|schema\|check" | wc -l

# Check validation test coverage
npm run test -- --grep "validation|sanitization" --reporter json | jq '.stats.passes'
```

**Accept when:**
- All environment variable access is wrapped in validation functions that check type and format
- All HTTP request handlers include explicit validation of request parameters, body, and query strings before processing
- Validation test coverage exceeds 80% for all modules handling external input
- Security audit confirms no unvalidated input paths exist in production code

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis scanning for unvalidated input patterns is mandatory in CI pipeline. Code review must verify validation for all PRs touching input handling. Violations block merge until validation is added or approved exception is documented.
</enforcement>