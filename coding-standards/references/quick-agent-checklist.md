## Quick agent checklist

Before coding:

- Read existing conventions for errors, schemas, tests, adapters, telemetry, and module layout.
- Classify each changed concern as Domain Module, Application Service Module, Adapter Module, or composition-root wiring.
- Reuse existing Domain Modules, Application Services, and Adapters before creating new ones.
- Define effect dependencies as narrow, application-owned ports; keep raw external types in Adapters or the composition root.
- Parse inputs at the edge and use domain types internally.
- Avoid raw DTOs, raw IDs, nullable bags, and `Partial<T>` in core/application logic.
- Prefer typed errors as values for new expected failures.
- Preserve existing observability/error mechanics.
- Test through public interfaces and real seams.
- Use `fast-check` arbitraries for generated test data when practical.
- Add JSDoc for exported symbols.
- Add an ADR only for a lasting architectural boundary, shared pattern, provider strategy, or deliberate exception discovered through the Adapter/Application Service reuse audit.
