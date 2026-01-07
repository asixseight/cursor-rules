---
globs: *.tsx,*.ts
alwaysApply: false
---

# Optional Properties
Apply the following rules based on the file context.


## For React Components (`*.tsx`)
PREFER the standard optional property syntax (prop?: T).

This is the idiomatic standard for React. It improves developer experience (DX) and aligns with all third-party libraries and documentation, which is critical for teams with mixed experience levels. The component's implementation is responsible for providing sensible defaults or handling the undefined case.

```tsx
// GOOD (in .tsx)
type ButtonProps = {
  /**
   * @default 'button'
   */
  type?: "button" | "submit" | "reset";
  onClick?: () => void;
};

const Button = (props: ButtonProps) => {
  const { type = "button" } = props;
  // ...
};
```

## For Data Models & Function Options (Primarily `.ts` files)
PREFER explicit `prop: T | undefined` over `prop?: T`.

Use `prop?: T` extremely sparingly. For critical data models or function options (e.g., `AuthOptions`), requiring an explicit `undefined` forces the developer to acknowledge the property, preventing bugs from "forgetting" to pass it. This is safer for data-centric logic and aligns with "Empathy-Driven Development" for junior developers.

```ts
// BAD (in .ts)
// A developer could forget to pass userId, resulting in an unauthenticated call.
type AuthOptions = {
  userId?: string;
};

const func = (options: AuthOptions) => {
  const userId = options.userId;
};
```
```ts
// GOOD (in .ts)
// Forces the caller to explicitly pass `userId: undefined` if it's not available.
type AuthOptions = {
  userId: string | undefined;
};

const func = (options: AuthOptions) => {
  const userId = options.userId;
};
```
