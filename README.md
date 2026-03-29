# UI Audit

A Claude Code skill that audits UI feature areas for pattern consistency, mobile UX, component best practices, and product-job alignment.

## Install

```bash
npx skillsadd zhiganov/ui-audit
```

## Usage

```
/ui-audit <feature-area>
```

Examples:
- `/ui-audit projects` — audit the projects/workspace feature
- `/ui-audit session creation` — audit the session creation flow
- `/ui-audit settings` — audit the settings page

## What it checks

1. **Component library patterns** — are you using the recommended patterns from your UI library (shadcn, etc.)? Responsive Dialog+Drawer, AlertDialog for destructive actions, DialogFooter for button layout.
2. **Mobile UX** — touch targets, hover-only interactions, responsive text/spacing, dialog scrolling, grid collapse.
3. **Anti-patterns** — `window.confirm()`, hand-rolled layouts, missing loading/error states, hardcoded colors.
4. **Accessibility** — labels, sr-only text, focus management, color contrast.
5. **Job Map (JTBD)** — maps the feature against the Universal Job Map (8 stages) and Four Forces of Progress. Technical findings get product context.
6. **Design review** — optionally invokes `frontend-design` skill for visual redesign when issues go beyond pattern swaps.

## Optional MCP integrations

The skill works standalone but is enhanced by:
- **shadcn MCP** — queries component patterns and examples directly
- **Figma MCP** — pulls design tokens, compares implementation against designs, finds unimplemented mobile variants and design drift
- **jtbd-knowledge MCP** — queries JTBD frameworks (Universal Job Map, Four Forces) for product context

## Output

Findings table with severity, component, issue, pattern reference, and file:line. Optionally creates Linear/GitHub issues.
