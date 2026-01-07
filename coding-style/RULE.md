---
globs: *.tsx,*.ts,*.js,*.json
alwaysApply: false
---
# Core Principles
These items are to be adhered to for all implementations.

## Maintainability Over Premature Optimization
The code should be performant but not over-engineered. Write clean, straightforward solutions first and only optimize critical bottlenecks when performance issues are actually identified.

## Empathy-Driven Development
A key consideration is that a junior developer might inherit the code. This value pushes for simplicity and clarity, avoiding overly complex solutions.

## Judicious Typing
While leveraging TypeScript's safety, avoid unnecessary type assertions. An exception is made in situations where the developer has more context than the compiler, such as when parsing JSON of a known structure. 
The any type is a dangerous "escape hatch" from the type system. Using any disables many type checking rules and is generally best used only as a last resort or when prototyping code.
Do not disable ESLint or TypeScript checks unless absolutely necessary, consider it the last resort.

## Accessibility (a11y) is Non-Negotiable
Building for the web means building for everyone. Accessibility is a fundamental requirement, involving semantic HTML, focus management, and keyboard navigability.

## Separation of Concerns (SoC)
Structure code so that different parts of the application handle different responsibilities. Keep UI components separate from business logic and data fetching to make them easier to understand, test, and maintain independently.

# Things to consider
These items are considered on a case-by-case basis. Would any of these principles improve the current implementation?

## Negative Space Programming
Define a program by what it should not do. Use invariants and assertions to prevent invalid states and guarantee correct behavior. Instead of numerous defensive if checks, assert constraints directly into the code. This makes programs more robust and secure by clearly specifying invalid conditions.