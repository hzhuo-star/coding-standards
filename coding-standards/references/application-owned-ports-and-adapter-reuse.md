## Application-owned ports and Adapter reuse

Define ports beside the Application Service that needs them and in the application's language, not the provider's language. Depend on the smallest meaningful capability the operation uses; let a cohesive concrete Adapter be wider. Port inputs, outputs, and errors must be application/domain types rather than raw rows, SDK objects, or framework values.

Because TypeScript is structurally typed, this works well:

```ts
type UsersForPasswordReset = {
  findActiveByEmail(email: EmailAddress): Promise<Result<ActiveUser, UserLookupError>>;
};

export class PasswordReset {
  constructor(private readonly users: UsersForPasswordReset) {}
}
```

A wider adapter can satisfy it:

```ts
export class PostgresUsers {
  findActiveByEmail(...) { ... }
  findById(...) { ... }
  updateProfile(...) { ... }
}
```

This avoids both mega-repositories and one-method adapter sprawl.

### Adapter reuse audit

Before creating a new adapter or service, agents must audit existing adapters/services.

Prefer, in order:

1. Reuse an existing adapter as-is through a narrow dependency type.
2. Extend an existing adapter if the new method fits its existing cohesive capability and changes for the same reason.
3. Create a new adapter only when reuse/extension would create bad coupling or an accidental interface.

Do not require an ADR for a routine feature-level Adapter or Application Service. Create an ADR when the new module introduces a lasting architectural boundary, shared pattern, provider strategy, or deliberate exception to these standards. The ADR should explain:

- what existing Adapters or Application Services were checked
- why reuse or extension did not fit
- why the new boundary or pattern is a separate cohesive capability

### Repositories and persistence

Avoid repository-per-table by default.

Repository-like adapters are acceptable when they represent a cohesive domain persistence capability. They should expose meaningful domain operations and return parsed domain types / typed errors, not raw rows and ORM errors.

Treat raw database rows and ORM models as infrastructure DTOs. Parse them before application/core logic. Keep SQL/ORM details inside infrastructure adapters or persistence modules.
