## Imports, exports, and files

Prefer direct imports from the file that owns the abstraction. Avoid barrel files / `index.ts` re-export layers by default.

For domain modules, namespace imports often preserve the module shape:

```ts
import * as EmailAddress from "./email-address";

EmailAddress.parse(input);
```

Use named imports for classes and focused shared helpers:

```ts
import { PasswordReset } from "./password-reset";
```

Use `import type` / `export type` for type-only imports and exports.

Export only what callers should use. Keep internal helpers unexported unless intentionally shared. Do not export internals just for tests.

Avoid TypeScript `namespace` unless there is a compelling interop reason.

Avoid vague files:

```txt
utils.ts
helpers.ts
common.ts
misc.ts
```

Use precise names:

```txt
email-address.ts
billing-period.ts
string-case.ts
array.ts
```

Tiny ubiquitous generic helpers/types may share one explicit module when no more precise owner exists. Appropriate contents include:

- `casesHandled`
- `shouldNeverHappen`
- `notYetImplemented`
- `Redacted`
- `Tags`, `ExtractTag`, and `ExcludeTag`
- common `Result` helpers when the project uses neither Effect nor `better-result`
- broad type utilities

Keep only helpers justified by the target project. Keep domain and application policy with their owning modules.

No arbitrary file-size limits. Prefer cohesion and discoverability over small files for their own sake. Split when a file has multiple unrelated reasons to change or callers must understand unrelated concepts.
