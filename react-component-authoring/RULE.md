---
globs: *.tsx
alwaysApply: false
---

# React Component Authoring Rules

## Core Principle
Build components by extending native HTML primitives, not abstracting them. The goal is maximum flexibility and composability, ensuring all native HTML attributes are accessible to the developer. This directly supports the core principle of Accessibility (a11y) by encouraging semantic HTML.

## Rules for Component Authoring
1. Typing and Props Inheritance
  - DO NOT manually define individual props for native attributes (e.g., `onClick`, `type`, `className`). This is brittle and limiting.
  - DO inherit all native element props using `React.ComponentPropsWithRef<'element'>`. This provides full type-safety for all HTML attributes and automatically handles `ref` forwarding.
    - Example: `type ButtonProps = React.ComponentPropsWithRef<'button'>;`
2. Prop Spreading
  - DO spread the `...props` object onto the underlying native HTML element. This passes all native props (like `disabled`, `aria-label`, etc.) to the DOM.
  - SPREAD PROPS FIRST if you need to override or extend specific props (like `className` or `onClick`). This allows your component's custom logic to take precedence while still accepting user overrides.
    - Example: `<button {...props} type={type} className={mergedClasses} onClick={handleClick}>`
3. Content and Children
  - DO NOT create custom props for content (e.g., `text: string`).
  - DO use the `children` prop (which is included in `ComponentPropsWithRef`). This allows developers to pass strings, JSX, or other React components as content.
    - Example: `<Button>Click Me</Button>` or `<Button><span>Icon</span> Click</Button>`
4. Customizing Native Props
  - `className`: To add default classes, merge them with `props.className`. Use a utility like `clsx` or `tailwind-merge` for robust merging.
  - Event Handlers (`onClick`, `onChange`, etc.): To add logic (like analytics) without overriding the user's handler, create a wrapper function. In the wrapper, call your internal logic, then call the user's prop-based handler if it exists.
5. Sensible Defaults
  - DO provide sensible defaults for native props to create safer primitives.
  - Crucial Example: Default a `<button>` component's `type` to `"button"`. The HTML default is `"submit"`, which causes unintended form submissions.
    - Example: `const { type = "button", ...rest } = props;`
6. Adding New (Non-Native) Props
  - DO make all new custom props optional using the `prop?: T` syntax. This aligns with the React ecosystem, improves developer experience, and ensures new features are non-breaking.
  - The component's implementation is responsible for safely handling the `undefined` case (e.g., `const { variant = "default" } = props;`).
  - (See optional-properties.mdc for the distinction between this rule and rules for data models).
7. Composition Over Configuration
  - DO NOT create monolithic components with dozens of boolean props (e.g., `withLabel`, `withError`, `withButton`).
  - DO favor composition by accepting other components or prop objects as props. This aligns with the "Discriminated Unions" rule to prevent a "bag of optionals."
    ```typescript
    // Example: Composing an Input component
    type InputProps = React.ComponentPropsWithRef<'input'> & {
      labelProps?: React.ComponentPropsWithRef<'label'>;
      errorProps?: React.ComponentPropsWithRef<'p'>;
    };
    ```
