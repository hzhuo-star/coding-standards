## Parse, don't validate

Boundary code should turn unknown or less-structured input into application or domain types before it enters inner code.

Use a separate protocol projection only when its shape or meaning differs enough to be useful. `DTO` describes a boundary role in prose; never use `DTO` or `Dto` in a symbol name. Name the symbol after its actual protocol or persistence meaning, such as `CreateUserRequest`, `StripeCustomerResponse`, or `UserRecord`:

```ts
unknown -> CreateUserRequest -> CreateUserInput -> EmailAddress/UserId/etc.
```

Otherwise, parse directly into the application input:

```ts
unknown -> CreateUserInput
```

Do not pass a schema-inferred transport shape throughout the application:

```ts
unknown -> z.infer<typeof CreateUserSchema>
```

Use names that preserve meaning:

- `parseX(input): Result<X, ParseXError>` for untrusted or less-structured input
- `makeX(...)` / `createX(...)` for smart constructors from already-typed pieces
- `isX(value): value is X` for true predicates
- `assertX(...)` rarely, mostly at tests/framework boundaries

Avoid `validateX` when the function returns a refined value. It parsed something.

### Schemas

Use schema libraries as boundary parsers, not as ad-hoc validators sprinkled through core logic.

Preference:

- use the repo's established schema library if one exists
- use Effect Schema in Effect codebases
- prefer Standard Schema compatibility for generic helpers
- otherwise prefer Zod 4
- use hand-written smart constructors/parsers for small domain types when clearer

Schema parsing should produce refined/domain types and typed custom errors where practical.
