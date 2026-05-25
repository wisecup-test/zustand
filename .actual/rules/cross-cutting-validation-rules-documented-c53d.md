# Enforce Input Validation and Sanitization in Security-Critical Code Paths: Validation Rules Documented

These rules are ALWAYS ACTIVE for all code paths handling external input, user-provided data, or data crossing trust boundaries, including HTTP request handlers, configuration file parsers, build tool configurations, API endpoints, environment variable processing, and file upload handlers.

### Rules

- **R-VAL-001** MUST: All HTTP request handlers and middleware processing user input SHALL include explicit validation of request parameters, body, and query strings before processing.
- **R-VAL-002** MUST: All configuration file parsers and loaders SHALL validate and sanitize external configuration data at the point of ingestion.
- **R-VAL-003** MUST: All build tool configurations processing external file paths or commands SHALL validate inputs to prevent injection attacks and path traversal vulnerabilities.
- **R-VAL-004** MUST: All API endpoints receiving data from external clients SHALL implement input validation before passing data to business logic.
- **R-VAL-005** MUST: All environment variable processing and validation SHALL check type, format, and allowed values before use in security-critical contexts.
- **R-VAL-006** MUST: All file upload handlers and file path processing SHALL validate file paths and names to prevent directory traversal and malicious file operations.
- **R-VAL-007** SHOULD: Validation rules SHOULD be documented and centralized in a shared validation module to ensure consistency across the codebase.
- **R-VAL-008** SHOULD: Schema validation libraries (Zod, Joi, Yup) SHOULD be used for declarative, type-safe validation with comprehensive error reporting.
- **R-VAL-009** MAY: Performance-critical code paths MAY bypass validation if input has been pre-validated by a security-reviewed upstream validator, with documented proof and exception approval.

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
- Validation utilities are centralized and reused across the codebase
- Configuration loaders validate all external configuration data before use
- File path processing includes validation to prevent directory traversal attacks

<enforcement>
Claude Code MUST NOT skip or defer verification of input validation rules. All code paths handling external input MUST be audited and validated before approval. Violations MUST be caught by automated static analysis in CI or flagged during code review.
</enforcement>