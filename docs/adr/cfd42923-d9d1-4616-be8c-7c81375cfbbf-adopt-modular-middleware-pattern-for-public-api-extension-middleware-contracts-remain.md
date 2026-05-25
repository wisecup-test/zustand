# Adopt Modular Middleware Pattern for Public API Extension: Middleware Contracts Remain

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The codebase implements a state management library with extensible middleware architecture, as evidenced by multiple middleware implementations (redux.ts, immer.ts, persist.ts, devtools.ts, subscribeWithSelector.ts)
- Public API consumers require standardized extension points to augment core functionality without modifying library internals
- The pattern appears across 16 files with 89.37% confidence, indicating a consistent architectural approach to middleware composition
- Examples demonstrate real-world usage patterns including demo applications and utility functions that consume the public middleware API
- The middleware pattern enables separation of concerns between core state management and optional features like persistence, dev tools, and framework integrations

## Problem Statement

Public APIs must provide extensibility mechanisms that allow consumers to add functionality without forking or modifying the core library, while maintaining type safety, composability, and backward compatibility. Without a standardized middleware pattern, consumers resort to wrapper functions, monkey-patching, or maintaining separate forks, leading to fragmentation and maintenance burden.

## Decision

1. MUST: Middleware API contracts MUST remain stable across minor versions to prevent breaking consumer code

## Policy Block

- MUST Middleware API contracts MUST remain stable across minor versions to prevent breaking consumer code

In scope:
- All public API extension points intended for consumer customization
- Middleware modules distributed as part of the official library package
- Third-party middleware that follows the established middleware contract
- Framework-specific integrations (Redux, Immer) implemented as middleware

Out of scope:
- Internal implementation details not exposed through public API
- Private utility functions used within core library
- Build configuration and tooling (rollup.config.mjs)
- Demo applications and examples (though they demonstrate usage patterns)

Exceptions:
- EXC-001: Breaking changes to middleware API are required for critical security fixes or fundamental architectural improvements
- EXC-002: Experimental middleware features are being tested before stabilization

## Rationale

- Pattern detected across 16 files with 89.37% confidence indicates this is a deliberate, well-established architectural decision rather than accidental code organization
- Multiple middleware implementations (redux, immer, persist, devtools, subscribeWithSelector, combine, ssrSafe) demonstrate the pattern's flexibility and real-world applicability
- Separation of middleware into distinct modules enables tree-shaking and reduces bundle size for consumers who don't need all features
- The middleware pattern aligns with industry best practices for extensible library design, as seen in Express.js, Redux, and other successful frameworks

## Consequences

Positive:
- Consumers can extend library functionality without forking or waiting for upstream changes
- Core library remains lean while supporting rich ecosystem of optional features
- Type safety is preserved through TypeScript generics, reducing runtime errors
- Middleware can be independently versioned, tested, and maintained
- Clear separation of concerns improves code maintainability and testability

Negative:
- Middleware API contract becomes a long-term maintenance burden requiring backward compatibility
- Complexity increases for library maintainers who must ensure middleware composition works correctly
- Documentation overhead increases as each middleware requires usage examples and API reference
- Potential for middleware conflicts or unexpected interactions when multiple middleware are composed
- Learning curve for consumers who must understand middleware composition patterns

## Alternatives

- Monolithic API with all features built into core library (rejected)
  Rejected because: Would result in large bundle sizes for consumers who only need basic functionality, and would require core library changes for any new feature
  When valid: Only appropriate for very small libraries with minimal feature set
- Plugin system with registration and lifecycle hooks (rejected)
  Rejected because: More complex than needed for this use case; middleware pattern provides sufficient flexibility with simpler mental model
  When valid: Better suited for applications with complex plugin lifecycles and inter-plugin communication
- Inheritance-based extension through subclassing (rejected)
  Rejected because: Violates composition-over-inheritance principle, makes type safety harder to maintain, and limits flexibility in combining multiple extensions
  When valid: May be appropriate for class-based APIs with clear hierarchical relationships

## Risks

- Middleware API changes could break existing consumer code and third-party middleware
  Mitigation: Enforce semantic versioning, maintain comprehensive test suite covering middleware composition, provide deprecation warnings before removing features
  Owner: Core library maintainers
- Poorly designed middleware could introduce performance bottlenecks or memory leaks
  Mitigation: Provide performance guidelines in documentation, include performance tests for official middleware, offer profiling utilities
  Owner: Engineering team
- Middleware composition order may produce unexpected behavior or conflicts
  Mitigation: Document composition order semantics clearly, provide combine utility for explicit composition control, test common middleware combinations
  Owner: Engineering team and documentation team

## Implementation Notes

- Follow the established pattern in src/middleware/ directory: each middleware should be a separate TypeScript module with clear type definitions
- Use TypeScript generics to preserve type information through middleware composition, as demonstrated in existing middleware implementations
- Provide a combine utility (src/middleware/combine.ts) to help consumers compose multiple middleware safely
- Include SSR-safe implementations (src/middleware/ssrSafe.ts) for middleware that may run in server environments
- Document middleware signature patterns and provide reference implementations for common use cases (persistence, dev tools, framework integration)

## Continuation Context


Verify commands:
- grep -r "export.*middleware" src/middleware/ | wc -l
- find src/middleware -name '*.ts' -type f | xargs grep -l 'export' | wc -l
- npm run type-check && npm test -- --grep middleware

Accept when:
- All middleware modules export functions with consistent type signatures that accept configuration and return enhanced API
- Type checking passes without errors for middleware composition scenarios
- Test suite includes integration tests for common middleware combinations
- Documentation includes middleware authoring guide and API reference for each official middleware

## Enforcement

- Verified by: TypeScript compiler enforces type safety for middleware signatures
- Verified by: CI pipeline runs integration tests covering middleware composition
- Verified by: Code review process checks for middleware API contract compliance
- Verified by: Automated bundle size checks ensure middleware remains independently tree-shakeable
- Violation handling: Type errors in middleware implementations fail CI build
- Violation handling: Breaking changes to middleware API trigger major version bump requirement
- Violation handling: Code review rejects PRs that violate middleware composition patterns
- Violation handling: Bundle size regressions trigger investigation and potential refactoring
- Exception process: Propose exception through RFC process with clear justification
- Exception process: Require approval from at least two core maintainers
- Exception process: Document exception in ADR updates section with rationale
- Exception process: Include migration guide if exception affects public API