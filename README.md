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

- **Design system as source of truth** — if the project uses [interface-design](https://github.com/Dammyjay93/interface-design) (4.3K stars), the audit reads `.interface-design/system.md` and checks code against the project's actual design tokens (spacing grid, colors, depth, component patterns) instead of generic rules. Different tools for different moments: `interface-design` enforces consistency while *building*; `ui-audit` finds problems when *reviewing*
- **Visual regression / screenshot testing** — render components at mobile/desktop viewports and visually verify layout issues instead of inferring from CSS classes
  - [playwright-skill](https://github.com/lackeyjb/playwright-skill) (2.2K stars) — browser automation for screenshot comparison
- **React/Next.js best practices** — React component composition, state management, rendering optimization, Server Components patterns
  - [vercel-react-best-practices](https://mcpservers.org/claude-skills/vercel/react-best-practices) (176K installs) — covers React and Next.js patterns
- **Automated accessibility scanning** — replace the manual accessibility check with axe-core / Lighthouse scanner results. No proven skill exists yet — candidates: [claude-a11y-skill](https://github.com/airowe/claude-a11y-skill), [skill-a11y-audit](https://github.com/snapsynapse/skill-a11y-audit)
- **Shaping for deep UX issues** — when the audit finds problems that need solution design, not just pattern fixes (e.g., "the dashboard doesn't support the Monitor job stage"), use shaping methodology before jumping to implementation
  - [shaping-skills](https://github.com/rjs/shaping-skills) — Ryan Singer's Shape Up methodology: problem definition, breadboarding, fat marker sketches
- **Formal UX heuristics** — Nielsen heuristics, WCAG, Don Norman principles evaluation. No proven skill exists yet — candidate: [mastepanoski/claude-skills](https://github.com/mastepanoski/claude-skills)
- **Storybook integration** — component-level visual regression and story coverage. No proven skill exists yet — candidate: [storybook-assistant](https://github.com/flight505/storybook-assistant)
