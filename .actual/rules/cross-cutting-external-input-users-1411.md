# Enforce Input Validation and Sanitization in Security-Critical Code Paths: External Input Users

These rules are ALWAYS ACTIVE for all code paths handling external input, user-provided data, or data crossing trust boundaries.

### Rules

- **R-EXT-001** MUST: All external input from users, configuration files, network requests, or environment variables MUST be validated before processing.
- **R-EXT-002** MUST: All HTTP request handlers and middleware processing user input MUST include explicit validation of request parameters, body, and query strings before processing.
- **R-EXT-003** MUST: Configuration file parsers and loaders MUST validate all parsed values against expected types and formats.
- **R-EXT-004** MUST: Build tool configurations that process external file paths or commands MUST sanitize and validate all path and command inputs.
- **R-EXT-005** MUST: Environment variable processing MUST validate type, format, and allowed values before use.
- **R-EXT-006** MUST: File upload handlers and file path processing MUST validate file paths to prevent directory traversal attacks.
- **R-EXT-007** SHOULD: Use schema validation libraries (Zod, Joi, Yup) for declarative, type-safe validation with runtime guarantees.
- **R-EXT-008** SHOULD: Create and maintain a centralized validation module with reusable validators for common input types (file paths, URLs, configuration values, user input).
- **R-EXT-009** SHOULD: Document validation requirements in API specifications and configuration schemas.
- **R-EXT-010** MAY: Bypass validation for data already validated at a previous trust boundary, provided validation is documented with proof of upstream validation.

### Verify

```bash
# Count unvalidated environment variable access
grep -r "process\.env" --include="*.ts" --include="*.js" | grep -v "validate\|sanitize\|check" | wc -l

# Count unvalidated HTTP request handlers
grep -r "req\.body\|req\.query\|req\.params" --include="*.ts" | grep -v "validate\|schema\|check" | wc -l

# Check validation test coverage
npm run test -- --grep "validation|sanitization" --reporter json | jq '.stats.passes'
```

**Accept when:**
- All environment variable access is wrapped in validation functions that check type and format
- All HTTP request handlers include explicit validation of request parameters, body, and query strings before processing
- Validation test coverage exceeds 80% for all modules handling external input
- Security audit confirms no unvalidated input paths exist in production code
- Centralized validation module exists with reusable validators for common input types
- All configuration file parsers validate parsed values against expected types and formats
- File path processing includes validation to prevent directory traversal attacks

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis scanning for unvalidated input patterns MUST be run in CI pipeline. Code review MUST verify validation for all PRs touching input handling. Quarterly security audits MUST review validation coverage. Unit test coverage for validation logic MUST meet minimum 80% threshold.
</enforcement>