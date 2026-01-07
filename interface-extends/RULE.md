---
globs: *.tsx,*.ts
alwaysApply: false
---

# Interface vs. Type
ALWAYS prefer `interface extends` when modelling inheritance.

The `&` operator (type intersection) has terrible performance in TypeScript and should only be used where `interface extends` is not possible.

```ts
// BAD

type A = {
  a: string;
};

type B = {
  b: string;
};

type C = A & B;
```

```ts
// GOOD

interface A {
  a: string;
}

interface B {
  b: string;
}

interface C extends A, B {
  // Additional properties can be added here
}
```

## Exception: React Component Props
A primary case where `interface extends` is "not possible" is when extending React's built-in `type` aliases for component props (e.g., `ComponentPropsWithRef`).

In this situation, using the `&` operator is the correct and accepted pattern.

```tsx
// GOOD (for React Components)

import type { ComponentPropsWithRef } from "react";

// You CANNOT extend a 'type' with an 'interface'
// Therefore, using '&' is required and correct here.
type ButtonProps = ComponentPropsWithRef<"button"> & {
  variant?: "primary" | "secondary";
};
```