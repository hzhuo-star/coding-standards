## Comments and JSDoc

Comments should explain invariants, trade-offs, non-obvious domain rules, and safety justifications. Avoid comments that narrate obvious code.

Every exported symbol from a JavaScript or TypeScript module requires JSDoc. Public methods and properties of an exported class also require JSDoc. Private and otherwise internal code requires documentation only when its complexity warrants it. Put documentation on the original declaration; re-exports do not need duplicate documentation.

Do not use `@inheritDoc`, `@inherit`, or similar inheritance tags. Write the required documentation explicitly on each symbol or member.

Use standard JSDoc syntax:

```ts
/**
 * Parse an email address from untrusted input.
 *
 * @param input - The untrusted string to parse.
 * @returns A parsed email address, or `InvalidEmailAddress` when the input is invalid.
 */
export function parse(input: string): Result<EmailAddress, InvalidEmailAddress>;
```

For generics:

```ts
/**
 * Map the success value of a result.
 *
 * @template T - The original success type.
 * @template U - The mapped success type.
 * @template E - The error type.
 * @param result - The result to map.
 * @param fn - The function applied to the success value.
 * @returns A result with the mapped success value, or the original error.
 */
export function map<T, U, E>(result: Result<T, E>, fn: (value: T) => U): Result<U, E>;
```

Use `@throws` only for unrecoverable defects, framework-required behavior, or temporary `notYetImplemented` paths. Do not document expected typed errors as throws.

For complex exported object types, document fields when helpful:

```ts
/** Input required to create a user. */
export type CreateUserInput = {
  /** The actor creating the user. */
  readonly actor: AdminUser;

  /** The parsed email address for the new user. */
  readonly email: EmailAddress;
};
```
