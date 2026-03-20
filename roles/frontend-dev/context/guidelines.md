# Frontend Developer Guidelines

## Component Patterns

- Use functional components with hooks
- Follow atomic design: atoms, molecules, organisms, templates, pages
- Co-locate component files: `ComponentName/index.tsx`, `ComponentName.test.tsx`, `ComponentName.styles.ts`
- Export components via barrel files (`index.ts`) at each directory level
- Keep components focused — extract logic into custom hooks when a component exceeds ~100 lines

## Styling Conventions

- Use the project’s design system tokens for colors, spacing, and typography
- Prefer CSS modules or styled-components over inline styles
- Follow mobile-first responsive design
- Use semantic HTML elements (`<nav>`, `<main>`, `<section>`) before adding ARIA attributes

## Accessibility Standards

- All interactive elements must be keyboard-navigable
- Images require meaningful `alt` text (or `alt=""` for decorative)
- Form inputs must have associated `<label>` elements
- Color contrast must meet WCAG 2.1 AA (4.5:1 for normal text)
- Test with screen reader and keyboard-only navigation

## Testing Standards

- Unit test every component with meaningful assertions (not just "renders without crashing")
- Test user interactions: clicks, form submissions, keyboard events
- Mock API calls — never hit real endpoints in unit tests
- Integration tests for multi-component flows (e.g., form wizard, data table with filters)

## API Integration

- Use a centralized API client with base URL, auth headers, and error interceptors
- Define TypeScript interfaces for all API request/response shapes
- Handle loading, error, and empty states in every data-fetching component
- Use the AnyCompany backend REST API for transaction data and categorization results

## State Management

- Local state for UI-only concerns (modals, form inputs, toggles)
- Shared state (context or store) for cross-component data (user session, feature flags)
- Server state managed via data-fetching library (React Query, SWR, or equivalent)
- Avoid duplicating server data in client state
