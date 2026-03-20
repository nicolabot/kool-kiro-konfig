---
name: component-scaffolding
description: Scaffold a new UI component with test file, styles, and barrel export following AnyCompany frontend conventions and atomic design patterns.
---

## Steps

1. Ask which component to create and its atomic design level (atom, molecule, organism, template, page)
2. Create the component directory under the appropriate level
3. Generate:
   - `index.tsx` — functional component with props interface and semantic HTML
   - `ComponentName.test.tsx` — unit test with render and interaction assertions
   - `ComponentName.styles.ts` — styled-components or CSS module file with design tokens
4. Add the component to the parent barrel export (`index.ts`)
5. Include accessibility attributes (ARIA labels, keyboard handlers) where applicable
6. Present the generated files for review
