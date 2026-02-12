---
globs: src/**/*.tsx, src/**/*.ts
applyTo: "src/**/*.tsx, src/**/*.ts"
paths: 
  - "src/**/*.{ts,tsx}"
description: Best practices for handling generated GraphQL types and partial data in components
---
# GraphQL Type Usage in Components

When passing data derived from GraphQL queries to UI components, match the prop types to the component's requirements, not the full generated schema.

## Problem

Generated GraphQL types define all possible properties, but specific queries often fetch only a subset (fragments). Additionally, query arrays often contain nullable items.
Strictly typing props as the full `GeneratedType` causes errors when passing partial data, leading to dangerous `as any` casts.

## Use `Pick` for Component Props

Define props based on exactly what the component uses. This ensures type safety without requiring the full unused schema.

```tsx
// ✅ Good
import { ArticleFragment } from '@/generated/graphql';

interface ArticleCardProps {
  // Only ask for what you need
  article: Pick<ArticleFragment, 'title' | 'slug' | 'publishedAt'>;
}

function ArticleCard({ article }: ArticleCardProps) {
  return <a href={article.slug}>{article.title}</a>;
}
```

## Filter Nulls with Type Guards

When dealing with CMS arrays, do not loosen component props to accept `null`. Instead, use a type predicate to strictly filter the array.

### ✅ Recommended Pattern

```ts
/**
 * Standard predicate for filtering nullable CMS data
 */
export function isNonNull<T>(value: T | null | undefined): value is T {
  return value != null;
}

// Usage in query components:
const items = data.collection.filter(isNonNull); 

// Now 'items' is strictly Type[] and matches the component props perfectly.
<Component items={items} />
```
