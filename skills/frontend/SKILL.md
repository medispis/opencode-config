---
name: frontend
description: Builds UI components, pages, and frontend architecture — React, Vue, Svelte, Angular. Use when creating components, designing layouts, implementing state management, or optimizing frontend performance.
---

# Frontend

Builds UI components, pages, and frontend architecture following modern best practices.

## How it works

1. Detect the framework (React, Vue, Svelte, Angular) and existing patterns
2. Identify component structure, state management, and styling approach
3. Build components following project conventions
4. Implement responsive, accessible UI
5. Add proper error boundaries and loading states

## Component patterns

### React
- Functional components with hooks
- Custom hooks for shared logic
- Error boundaries for fault tolerance
- React.memo for expensive renders
- Proper key usage in lists

### Vue
- Composition API with `<script setup>`
- Composables for shared logic
- Provide/inject for deep state
- Async components for code splitting

### Svelte
- Component-level reactivity
- Stores for shared state
- Transitions and animations built-in
- Context API for deep state

## State management

| Complexity | Approach |
|------------|----------|
| Simple | Component state (useState, ref) |
| Medium | Context + useReducer, Pinia, Svelte stores |
| Complex | Redux, Zustand, Vuex, NgRx |
| Server state | React Query, SWR, RTK Query |

## Styling approaches

Detect and follow the project's approach:
- **CSS Modules**: Scoped styles, no conflicts
- **Tailwind**: Utility-first, rapid development
- **Styled Components**: CSS-in-JS, dynamic styles
- **Sass/SCSS**: Preprocessor, nesting, variables
- **Vanilla CSS**: Simple, no build step

## Accessibility (a11y)

Always implement:
- Semantic HTML elements
- ARIA labels where needed
- Keyboard navigation support
- Focus management
- Color contrast compliance (WCAG AA)
- Screen reader compatibility
- Alt text for images

## Performance

- Lazy load routes and heavy components
- Memoize expensive computations
- Debounce/throttle user inputs
- Optimize images (WebP, lazy loading, srcset)
- Code split by route
- Minimize bundle size
- Use virtual lists for large datasets

## Rules

- Detect the framework and follow existing patterns
- Make components reusable but not over-engineered
- Always handle loading and error states
- Write accessible components by default
- Use TypeScript for props/state when available
- Don't introduce new UI libraries without justification
- Keep components focused (single responsibility)

## Output format

```markdown
## Component: [Name]

**Type:** Presentational / Container / Page
**Props:** [list with types]
**State:** [local state description]

### Files created
- `components/[Name].tsx` — Component implementation
- `components/[Name].styles.ts` — Styles (if applicable)
- `components/[Name].test.tsx` — Tests

### Usage
```tsx
<[Name] prop1="value" prop2={data} />
```
```
