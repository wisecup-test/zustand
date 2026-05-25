# Adopt Shallow Comparison for State Change Detection in Primary Datastores: Implementations Optimize Shallow

Status: proposed
Date: 2024-01-15
Deciders: Detection Pipeline (automated)

## Activation

This ADR is ALWAYS ACTIVE for all state management and datastore implementations that require change detection and subscription notification mechanisms.

## Context

- State management systems require efficient change detection to notify subscribers only when relevant data has changed, avoiding unnecessary re-renders or recomputations
- Deep equality checks are computationally expensive for large state objects and can create performance bottlenecks in high-frequency update scenarios
- Shallow comparison provides a pragmatic balance between performance and correctness for immutable state patterns where object references change on mutation
- The pattern appears across multiple implementation contexts (React components, vanilla state stores, and developer tooling middleware) indicating a cross-cutting architectural concern
- Cache invalidation and subscription notification systems benefit from fast, predictable comparison operations that can be executed on every state transition

## Problem Statement

Primary datastores and state management systems need a consistent, performant mechanism to detect state changes and determine when to notify subscribers or invalidate caches. Without a standardized approach, implementations may suffer from either performance degradation (deep comparisons) or correctness issues (reference equality only), leading to inconsistent behavior across the codebase.

## Decision

1. MAY: Implementations MAY optimize shallow comparison by caching property keys or using fast-path checks for common object types

## Policy Block

- MAY Implementations MAY optimize shallow comparison by caching property keys or using fast-path checks for common object types

In scope:
- All primary datastore implementations (Redux-like stores, state managers, cache layers)
- Subscription and observer notification systems
- React component state selectors and hooks
- Middleware that intercepts state changes (devtools, logging, persistence)
- Cache invalidation logic based on state comparison

Out of scope:
- Deep nested state validation or schema checking
- Serialization and deserialization logic
- Initial state hydration from external sources
- State persistence to storage backends
- Business logic that requires semantic equality beyond structural comparison

Exceptions:
- EXC-001: Performance profiling demonstrates that deep comparison does not impact application performance for specific small, bounded state slices
- EXC-002: Legacy integration requires compatibility with external systems that mutate state in place

## Rationale

- Pattern detected across 3 files with 90.47% confidence, indicating consistent architectural approach to state change detection
- Shallow comparison provides O(n) complexity for object properties versus O(n*m) for deep comparison, critical for performance in high-frequency update scenarios
- Aligns with immutable state patterns prevalent in modern JavaScript frameworks (React, Redux, Zustand) where object references signal changes
- Enables predictable subscription notification behavior while maintaining acceptable performance characteristics for typical application state sizes

## Consequences

Positive:
- Consistent, predictable performance characteristics for state change detection across all datastores
- Reduced computational overhead compared to deep equality checks, improving application responsiveness
- Natural alignment with immutable state patterns encourages better state management practices
- Simplified debugging and reasoning about when subscribers will be notified of changes

Negative:
- Requires strict adherence to immutable update patterns; in-place mutations will not be detected
- May miss nested object changes if parent reference remains unchanged, requiring explicit nested immutability
- Developers must understand the difference between shallow and deep equality to avoid subtle bugs
- Additional complexity in scenarios requiring semantic equality beyond structural comparison

## Alternatives

- Use reference equality (===) only for change detection (rejected)
  Rejected because: Too coarse-grained; fails to detect changes in object properties when the object reference is reused, leading to missed updates and stale UI states
  When valid: Only valid for primitive values or when complete object replacement is guaranteed
- Implement deep equality checks for all state comparisons (rejected)
  Rejected because: Prohibitively expensive for large state objects and high-frequency updates; creates performance bottlenecks and unpredictable latency
  When valid: Acceptable for small, bounded state objects in low-frequency update scenarios with documented performance validation
- Use structural sharing with persistent data structures (Immutable.js, Immer) (deferred)
  Rejected because: Adds significant dependency weight and learning curve; may be considered for future optimization if performance profiling indicates need
  When valid: When state size and update frequency justify the overhead of persistent data structure libraries

## Risks

- Developers unfamiliar with shallow comparison semantics may introduce bugs by mutating nested objects in place
  Mitigation: Provide linting rules, code review guidelines, and training materials on immutable update patterns; consider Immer for safer nested updates
  Owner: Engineering team leads
- Performance degradation if shallow comparison is applied to objects with hundreds of properties
  Mitigation: Establish state normalization patterns to keep objects flat; implement performance monitoring for comparison operations; consider memoization for expensive selectors
  Owner: Performance engineering team
- Inconsistent implementation of shallow comparison across different datastore modules leading to subtle behavioral differences
  Mitigation: Extract shared shallow comparison utility function; enforce usage through code review and automated testing; document canonical implementation
  Owner: Platform architecture team

## Implementation Notes

- Create a shared utility module (e.g., `utils/shallowEqual.ts`) that implements the canonical shallow comparison algorithm with reference equality fast-path
- For React components, leverage built-in shallow comparison from React.memo or implement custom comparison functions for useSelector hooks
- Document immutable update patterns in team guidelines, including use of spread operators, Object.assign, or helper libraries like Immer for complex nested updates
- Add performance monitoring to track comparison operation duration in production; alert if p95 latency exceeds acceptable thresholds

## Continuation Context


Verify commands:
- grep -r "shallowEqual\|shallow" --include="*.ts" --include="*.tsx" --include="*.js" --include="*.jsx" src/
- npm test -- --testNamePattern="shallow.*comparison|state.*change.*detection"
- eslint src/ --rule 'no-param-reassign: error' --rule 'immutable/no-mutation: error'

Accept when:
- All datastore implementations use a shared shallow comparison utility or documented equivalent algorithm
- Unit tests verify that shallow comparison correctly detects property-level changes and ignores nested mutations
- Code review checklist includes verification of immutable update patterns in state mutations
- Performance tests demonstrate comparison operations complete within acceptable latency bounds (e.g., <1ms p95)

## Enforcement

- Verified by: Automated unit tests for shallow comparison behavior in all datastore modules
- Verified by: Code review checklist item requiring verification of immutable state patterns
- Verified by: Static analysis with ESLint rules enforcing immutability (no-param-reassign, immutable/no-mutation)
- Verified by: Performance regression tests monitoring comparison operation latency
- Violation handling: CI pipeline fails if shallow comparison tests are missing or failing
- Violation handling: ESLint violations for mutation patterns block PR merge
- Violation handling: Code review requires explicit justification and tech lead approval for any deep comparison usage
- Violation handling: Performance regression alerts trigger investigation and potential rollback
- Exception process: Developer documents exception request with performance data or technical justification
- Exception process: Tech lead reviews and approves/rejects based on documented criteria in policy exceptions
- Exception process: Approved exceptions are tracked in ADR amendments or inline code documentation
- Exception process: Exceptions are reviewed quarterly for potential remediation or pattern updates