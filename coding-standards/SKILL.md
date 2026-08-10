---
name: coding-standards
description: Correct-by-construction TypeScript standards. Use for TypeScript engineering or when another skill needs the user's coding standards.
---
These standards describe how to design and write TypeScript code in this codebase. They are especially intended for agents: inspect existing code before adding patterns, libraries, Adapters, or abstractions, but apply these standards to all new and refactored behavior. Follow existing conventions only when they are compatible with these standards.

## Decision priority

When rules pull in different directions, use this order:

1. Preserve correctness, safety, and debuggability.
2. Apply these standards to all new code and to the full behavior being refactored.
3. Follow compatible project architecture and conventions.
4. Contain incompatible existing patterns at the nearest boundary rather than copying them into new code.
5. Avoid changing unrelated old code unless a broader migration is explicitly requested.
6. Document meaningful trade-offs with comments or ADRs.

## Core principles

- Prefer **errors as values** over `throw` / rejected promises for expected failures.
- **Parse don't validate**. Parse early and as close to composition or application roots as possible. Do not merely validate and throw away the information learned.
- Make **illegal states unrepresentable** where practical.
- Prefer **correct-by-construction** APIs over convention-based invariants.
- Use branded/refined/domain types when they prevent a realistic mistake, such as mixing identifiers or units, bypassing parsing, or constructing an invalid value.
- Prefer **composition over inheritance**.
- Prefer **imperative shell / functional core**.
- Design **deep, cohesive modules** with **low caller burden**.
- Test behavior through real seams; **avoid** module mocks and spy-driven tests.
- Keep code discoverable for humans and agents.

## Adapting to existing codebases

Before adding a new pattern or library, inspect the repo for existing choices around:

- error handling
- schema parsing
- dependency injection
- testing
- observability
- adapters/services
- module layout

Apply these standards to all new code and to the full behavior being refactored. Do not preserve weaker patterns merely for consistency. Keep unrelated old code unchanged and translate incompatible patterns at the nearest boundary.

For example, if existing code uses exception-style errors, do not rewrite the whole system for an unrelated change. Represent known failures as typed values in new or refactored code, then translate them at the boundary into the outcome required by the existing framework. Preserve existing logging, tracing, metrics, and error-reporting hooks.

## References

Read every reference relevant to the behavior being designed, changed, tested, or reviewed:

- **Expected failures, defects, custom errors, Results, or error translation**: read [errors-and-failures.md](references/errors-and-failures.md).
- **Secrets, credentials, redaction, telemetry, logs, traces, or debugging data**: read [sensitive-data-telemetry-debugging.md](references/sensitive-data-telemetry-debugging.md).
- **Boundary parsing, schemas, protocol projections, DTO naming, or operation inputs**: read [parse-dont-validate.md](references/parse-dont-validate.md).
- **Brands, refined types, smart constructors, or construction invariants**: read [branded-types-and-correct-construction.md](references/branded-types-and-correct-construction.md).
- **Lifecycle states, discriminated unions, optionality, state transitions, or boolean parameters**: read [state-machines-and-boolean-blindness.md](references/state-machines-and-boolean-blindness.md).
- **Module depth, abstractions, Domain Modules, Application Services, Adapters, or composition roots**: read [modules-and-abstractions.md](references/modules-and-abstractions.md).
- **Application-owned ports, Adapter reuse, repositories, persistence, or authorization ownership**: read [application-owned-ports-and-adapter-reuse.md](references/application-owned-ports-and-adapter-reuse.md).
- **Pure domain logic, imperative shells, or entrypoints**: read [functional-core-imperative-shell-entrypoints.md](references/functional-core-imperative-shell-entrypoints.md).
- **Multi-step workflows, transactions, retries, or idempotency**: read [workflows-transactions-idempotency.md](references/workflows-transactions-idempotency.md).
- **Test strategy, real seams, module mocks, persistence tests, property tests, or arbitraries**: read [testing.md](references/testing.md).
- **Compiler settings, immutability, casts, `any`, or non-null assertions**: read [typescript-style-and-safety.md](references/typescript-style-and-safety.md).
- **Imports, exports, barrels, namespaces, file names, or module organization**: read [imports-exports-files.md](references/imports-exports-files.md).
- **Comments, JSDoc, exported APIs, or `@throws` documentation**: read [comments-and-jsdoc.md](references/comments-and-jsdoc.md).
- **Environment configuration, resources, side effects, global state, time, or randomness**: read [configuration-and-resources.md](references/configuration-and-resources.md).
- **Before coding and before declaring work complete**: read [quick-agent-checklist.md](references/quick-agent-checklist.md).
