## Errors and failures

### Expected failures are values

Every known failure mode should appear in the return type as a custom tagged error, even when the immediate caller cannot recover. A caller must handle the error or return it upward. At the outermost boundary, translate it into a valid outcome such as an HTTP response, CLI exit code, retry decision, dead letter, or startup error message.

Known failures include domain, parsing, authorization, integration, I/O, persistence, configuration, and workflow failures.

Preferred order:

1. Effect, when the codebase already uses Effect.
2. `better-result`, when available and appropriate.
3. A small local tagged union:

```ts
type Result<T, E extends Error> =
  | { readonly _tag: "ok"; readonly value: T }
  | { readonly _tag: "err"; readonly error: E };
```

Prefer:

```ts
Promise<Result<User, UserLookupError>>
```

not:

```ts
Promise<User> // rejects for ordinary lookup/storage failures
```

Promise rejection is equivalent to throwing. Catch unclassified third-party rejection inside the owning Adapter and translate it into a known tagged error before it crosses the Adapter boundary. Rejection may escape application code only for a defect.

### Defects may throw or panic

Throw or panic only when a defect makes correct execution impossible, not merely because the current caller has no recovery strategy. Defects include:

- violated internal invariants
- impossible branches
- temporary `notYetImplemented` paths
- catastrophic runtime conditions

Known configuration failures are values; the composition root reports them safely and terminates startup.

Use established shared defect helpers where available, or the panic helper from the project's result library:

```ts
export function casesHandled(unexpectedCase: never): never;
export function shouldNeverHappen(msg?: string): never;
export function notYetImplemented(msg?: string): never;
```

Use `casesHandled` for exhaustive union handling. Avoid names like `absurd` or one-off `assertNever` helpers when the project already has these helpers.

### Custom errors

Expected failures should use custom tagged errors, generally extending:

- `Error`
- `TaggedError` from `better-result`
- `Schema.TaggedErrorClass` in Effect codebases

Custom errors should include:

- stable tag using 'as const'
- useful message
- structured contextual fields
- safe telemetry fields
- optional `cause: unknown`

Example:

```ts
export class UserStoreUnavailable extends Error {
  readonly _tag = "UserStoreUnavailable" as const;

  constructor(
    readonly operation: "findActiveByEmail",
    readonly provider: "postgres",
    readonly cause: unknown,
  ) {
    super(`User store unavailable during ${operation}`);
  }
}
```

Keep error unions precise at module boundaries:

```ts
Result<User, UserNotFound | UserStoreUnavailable>
```

Avoid broad `AppError`-style types except near entrypoints, orchestration, logging, and rendering layers.
