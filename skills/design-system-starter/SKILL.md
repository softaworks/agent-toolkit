---
name: design-system-starter
description: "Create and evolve design systems with design tokens, component architecture, theming, and WCAG-compliant accessibility patterns for front-end UI projects. Use when the user asks to build a design system, UI kit, style guide, component library, theme system, brand guidelines, or set up design tokens, dark mode, or consistent UI across products."
license: MIT
metadata:
  version: 1.0.0
  tags: "design-system, ui, components, design-tokens, accessibility, frontend"
---

# Design System Starter

Build robust, scalable design systems that ensure visual consistency and exceptional user experiences.

## Triggers

| Trigger | Example |
|---------|---------|
| Create design system / UI kit / style guide | "Create a design system for my app" |
| Design tokens / theme setup | "Set up design tokens for colors and spacing" |
| Component library / component architecture | "Design component structure using atomic design" |
| Accessibility / WCAG compliance | "Ensure WCAG 2.1 compliance for my components" |
| Dark mode / theming | "Implement theming with dark mode support" |
| Brand guidelines / style consistency | "Set up brand guidelines for consistent UI" |

## Quick Reference

| Task | Output |
|------|--------|
| Design tokens | Color, typography, spacing, shadows in W3C token format |
| Component structure | Atomic design hierarchy (atoms, molecules, organisms) |
| Theming | CSS variables, Tailwind dark mode, or ThemeProvider setup |
| Accessibility | WCAG 2.1 AA compliant patterns with keyboard and ARIA support |
| Documentation | Component docs with props, examples, a11y notes |

## Bundled Resources

- `references/component-examples.md` — Complete component implementations (Button, FormField, Card, Modal, polymorphic patterns)
- `templates/design-tokens-template.json` — W3C design token format with color, typography, spacing, shadows
- `templates/component-template.tsx` — React component template with TypeScript props and a11y
- `checklists/design-system-checklist.md` — Design system audit checklist

## Workflow

### 1. Define Design Tokens

Establish foundational tokens for colors, typography, spacing, border-radius, and shadows. Use a three-tier token architecture:

- **Primitive tokens**: Raw values (e.g., `blue.600: "#2563eb"`)
- **Semantic tokens**: Contextual meaning referencing primitives (e.g., `brand.primary: "{color.primitive.blue.600}"`)
- **Component tokens**: Component-specific values referencing semantic tokens (e.g., `button.padding-x: "{spacing.4}"`)

See `templates/design-tokens-template.json` for the complete W3C token format with all categories.

**Validation**: Run a contrast checker on all text/background color combinations. WCAG 2.1 AA requires 4.5:1 for normal text, 3:1 for large text and UI components.

### 2. Build Components (Atomic Design)

Follow the atomic design hierarchy: **Atoms** → **Molecules** → **Organisms** → **Templates** → **Pages**

Start with atoms (Button, Input, Label, Icon, Badge, Avatar), then compose into molecules (SearchBar, FormField, Card) and organisms (Navigation, Modal Dialog).

Key API design principles:
- Use consistent prop names across components (`variant`, `size`)
- Provide sensible defaults (variant defaults to `primary`, size to `md`)
- Prefer composition over configuration (use `<Card.Header>` not `header="..."` props)
- Support polymorphic rendering (`<Button as="a" href="/login">`)

See `references/component-examples.md` for complete implementations with TypeScript interfaces.

### 3. Implement Theming

Choose a theming approach based on the project stack:
- **CSS Variables**: Set `:root` tokens, override with `[data-theme="dark"]`
- **Tailwind CSS**: Use `dark:` prefix utilities
- **Styled Components**: Use `ThemeProvider` with light/dark theme objects

See `references/component-examples.md` for implementation examples of each approach.

### 4. Ensure Accessibility

Every component must meet WCAG 2.1 Level AA:
- Use semantic HTML (`<button>`, `<nav>`, `<main>`) — no div/span for interactive elements
- Build in keyboard navigation and focus management (focus traps for modals)
- Apply ARIA attributes: `aria-label`, `aria-expanded`, `aria-controls`, `aria-live`

See `references/component-examples.md` for accessibility patterns including Skip Links and focus traps.

### 5. Document and Ship

Each component needs: purpose, usage example, variants, props table, accessibility notes, and code examples. Use Storybook or Docusaurus for interactive documentation.

See `templates/component-template.tsx` for the standard component documentation structure.

**Validation**: Verify every component has keyboard support, screen reader labels, and documented props before shipping.

## Checklist

- [ ] Define design token structure (primitives → semantic → component)
- [ ] Validate all color contrast ratios meet WCAG 2.1 AA
- [ ] Build atomic components with consistent prop APIs
- [ ] Implement theming system with dark mode support
- [ ] Add keyboard navigation and ARIA attributes to all interactive components
- [ ] Document each component with props, examples, and a11y notes
- [ ] Establish versioning and release strategy
