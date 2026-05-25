# Adopt Message Queue Pattern for Middleware Communication: Middleware Components Not

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Activation

This ADR is ACTIVE for all integration patterns involving middleware communication and state management systems.

## Context

- The codebase demonstrates a consistent pattern of using message queue boundaries for middleware communication, particularly in devtools and persistence layers
- Message queues provide decoupling between components, allowing middleware to communicate asynchronously without tight coupling
- The pattern appears in test infrastructure (basic.test.ts), development tooling (devtools.ts), and persistence middleware (persist.ts), indicating a cross-cutting architectural concern
- This pattern enables better separation of concerns and allows middleware components to operate independently while maintaining communication channels
- The detection confidence of 91.37% across 3 files suggests this is an established architectural pattern rather than an isolated implementation

## Problem Statement

Middleware components in the system need to communicate and coordinate without creating tight coupling or synchronous dependencies. Direct method calls between middleware layers create rigid dependencies that make testing difficult, reduce modularity, and limit the ability to add or modify middleware behavior independently. A standardized integration pattern is needed to enable loose coupling while maintaining reliable communication between middleware components.

## Decision

1. MUST_NOT: Middleware components MUST NOT maintain direct references to other middleware instances for communication purposes

## Policy Block

- MUST_NOT Middleware components MUST NOT maintain direct references to other middleware instances for communication purposes

In scope:
- All middleware components requiring inter-component communication
- Devtools middleware for debugging and state inspection
- Persistence middleware for state synchronization
- Test infrastructure requiring middleware interaction verification
- Integration patterns between API layers and middleware

Out of scope:
- Internal implementation details within a single middleware component
- Direct API responses to external clients
- Synchronous validation logic within a single request handler
- Database query execution patterns
- Pure utility functions with no side effects

Exceptions:
- EXC-001: Performance-critical paths where message queue overhead is demonstrably unacceptable (>10ms latency impact)
- EXC-002: Legacy middleware components during migration period

## Rationale

- The pattern signature a6f195c041c1fbe8bf1d99d36e62263b was detected with 91.37% confidence across 3 critical files, indicating consistent architectural intent
- Message queue boundaries enable independent evolution of middleware components, supporting the Open/Closed Principle
- Asynchronous communication through message queues prevents blocking operations and improves system responsiveness
- The pattern's presence in both production middleware (devtools, persist) and test infrastructure demonstrates its value for testability and modularity

## Consequences

Positive:
- Middleware components can be developed, tested, and deployed independently without tight coupling
- System becomes more resilient as failures in one middleware component don't directly cascade to others
- Testing becomes easier as message queue boundaries provide natural seams for mocking and isolation
- New middleware can be added without modifying existing components, improving extensibility

Negative:
- Introduces additional complexity in understanding message flow through the system
- Debugging can be more challenging as communication is indirect and asynchronous
- Potential performance overhead from message serialization and queue management
- Requires additional infrastructure and monitoring for message queue health

## Alternatives

- Direct method calls between middleware components (rejected)
  Rejected because: Creates tight coupling, makes testing difficult, and reduces modularity. Changes to one middleware component would require changes to all dependent components.
  When valid: Only appropriate for internal operations within a single middleware component
- Event emitter pattern with synchronous callbacks (rejected)
  Rejected because: While providing some decoupling, synchronous callbacks can still create blocking behavior and make it harder to reason about execution order and error handling
  When valid: May be suitable for simple notification scenarios where order and synchronicity are required
- Shared state object with polling (rejected)
  Rejected because: Inefficient due to polling overhead, creates race conditions, and doesn't provide clear communication semantics
  When valid: Never recommended for middleware communication

## Risks

- Message queue failures could cause silent communication breakdowns between middleware components
  Mitigation: Implement health checks, monitoring, and dead letter queues for failed messages. Add timeout mechanisms and fallback behaviors.
  Owner: Engineering team
- Performance degradation from message serialization overhead in high-throughput scenarios
  Mitigation: Benchmark critical paths, implement message batching where appropriate, and use efficient serialization formats. Monitor queue latency metrics.
  Owner: Engineering team
- Increased complexity in debugging and tracing message flows across middleware boundaries
  Mitigation: Implement correlation IDs for message tracing, add comprehensive logging at queue boundaries, and provide devtools visualization of message flows
  Owner: Engineering team

## Implementation Notes

- Start by identifying all existing direct middleware-to-middleware communication and create a migration plan
- Implement message queue abstraction layer to allow different queue implementations (in-memory for tests, production queue for deployment)
- Add correlation IDs to all messages for tracing and debugging purposes
- Create clear message schemas and documentation for each message type used in middleware communication
- Implement monitoring and alerting for queue depth, message processing latency, and failure rates

## Continuation Context


Verify commands:
- grep -r "message.*queue\|queue.*message" src/middleware/ --include="*.ts" | wc -l
- grep -r "direct.*call\|invoke.*middleware" src/middleware/ --include="*.ts" | wc -l
- npm test -- --grep "message queue" 2>&1 | grep -c "passing"

Accept when:
- Message queue pattern is detected in middleware communication code with grep showing positive matches
- Direct middleware invocation patterns are minimized or eliminated (grep count approaches zero)
- Tests for message queue boundaries pass successfully, confirming isolation and proper communication

## Enforcement

- Verified by: Automated code review checks scanning for direct middleware-to-middleware method calls
- Verified by: CI pipeline verification commands checking for message queue pattern usage
- Verified by: Architecture review during pull request process for new middleware components
- Violation handling: CI build fails if direct middleware coupling is detected without documented exception
- Violation handling: Pull requests are blocked until message queue pattern is properly implemented
- Violation handling: Quarterly architecture audits identify and prioritize remediation of violations
- Exception process: Submit exception request to architecture review board with performance benchmarks or technical justification
- Exception process: Document exception in code with ADR reference and approval details
- Exception process: Review exceptions quarterly to determine if they can be eliminated with improved patterns or infrastructure