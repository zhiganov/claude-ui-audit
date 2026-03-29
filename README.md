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

## Roadmap

Potential integrations with existing community skills:

- **Automated accessibility scanning** — replace the manual accessibility check with real scanner results
  - [claude-a11y-skill](https://github.com/airowe/claude-a11y-skill) — axe-core + jsx-a11y
  - [skill-a11y-audit](https://github.com/snapsynapse/skill-a11y-audit) — WCAG 2.1 AA + Lighthouse
- **Visual regression / screenshot testing** — render components at mobile/desktop viewports and visually verify layout issues instead of inferring from CSS classes
  - [playwright-skill](https://github.com/lackeyjb/playwright-skill) — browser automation for screenshot comparison
  - [claude-code-frontend-dev](https://github.com/hemangjoshi37a/claude-code-frontend-dev) — multimodal vision (Claude sees the rendered UI)
- **Storybook integration** — for projects using Storybook: component-level visual regression, accessibility remediation, story coverage gaps
  - [storybook-assistant](https://github.com/flight505/storybook-assistant)
- **React/Next.js best practices** — React component composition, state management, rendering optimization, Server Components patterns
  - [vercel-react-best-practices](https://mcpservers.org/claude-skills/vercel/react-best-practices) — 176K installs, covers React and Next.js patterns
- **Formal UX heuristics** — Nielsen heuristics, WCAG, and Don Norman principles evaluation alongside the pattern and JTBD checks
  - [mastepanoski/claude-skills](https://github.com/mastepanoski/claude-skills)
