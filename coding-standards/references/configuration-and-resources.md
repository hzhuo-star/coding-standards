## Configuration and resources

Parse environment/config at startup or the earliest boundary into typed config with branded/redacted values where appropriate. Return known configuration failures as tagged error values. The composition root should report a safe startup message and terminate rather than treating invalid configuration as an internal defect.

Do not read `process.env` throughout the app. Missing or invalid config is a startup failure with useful, safe context.

Avoid top-level side effects except in true entrypoint/bootstrap files. Modules should not start servers, open connections, read env, register handlers, or perform I/O at import time.

Resource creation and cleanup should be explicit and owned by bootstrap/imperative shell code or Effect layers when using Effect.

Avoid mutable singletons/global state. Constants and pure lookup tables are fine. If a singleton is required by a framework/runtime, isolate it at the boundary.

Inject `Clock` / `Random` services into dependency-bearing modules. Pure domain functions may accept explicit `now` / random values.
