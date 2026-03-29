---
name: UI Audit
description: Audit a feature area against shadcn patterns, mobile UX, component best practices, and JTBD job mapping. Outputs actionable issues.
---

# UI Audit

Audit a UI feature area for pattern consistency, mobile UX, and component best practices.

**Trigger:** `/ui-audit <feature-area>` (e.g., `/ui-audit projects`, `/ui-audit session creation`, `/ui-audit settings`)

## Process

### Step 1: Identify components

Find all components, pages, and dialogs in the feature area. List them with file paths.

### Step 1b: Check Figma designs (if Figma MCP is available)

**If Figma MCP is available** (`mcp__figma__*` tools):

1. **Find the design file** — search for frames/pages matching the feature area name
2. **Pull design tokens** — colors, spacing, typography, border radius from the design system. Use these as the source of truth instead of hardcoded values in later steps.
3. **Compare against implementation** — for each component found in Step 1:
   - Does a corresponding Figma component exist?
   - Do spacing, colors, typography match between design and code?
   - Are there mobile variants in Figma that weren't implemented?
   - Are there designed components that were never built?
4. **Note drift** — list where implementation diverges from design. This is separate from pattern/mobile issues — it's about design intent vs reality.

**If no Figma MCP:** Skip this step. The code is the source of truth.

### Step 2: Component library check

Check the project's component library for patterns and best practices.

**If shadcn MCP is available** (`mcp__shadcn__*` tools):
- `mcp__shadcn__view_items_in_registries` — check component API and variants for each component type used (Dialog, Card, DropdownMenu, etc.)
- `mcp__shadcn__get_item_examples_from_registries` — check recommended patterns (e.g., `drawer-dialog` for responsive modals, `alert-dialog-demo` for destructive confirmations)
- Check what's installed (`src/components/ui/`) vs what's available — are there newer patterns the feature should use? (Drawer, Command, Popover, etc.)

**If no component library MCP:** Check installed UI components manually via file listing, and reference the library's documentation via web search.

### Step 3: Mobile audit

For each component, check:
- **Touch targets** — buttons/links at least 44px tap target on mobile
- **Hover-only interactions** — anything using `group-hover:opacity-*` or hover-dependent UI that breaks on touch
- **Text sizing** — `text-4xl` or larger without responsive variants (`text-2xl md:text-4xl`)
- **Padding/spacing** — `p-8` or larger without responsive variants (`p-4 md:p-8`)
- **Fixed heights** — `min-h-[200px]` or similar that wastes mobile viewport space
- **Dialog scrolling** — forms that overflow on small screens without `max-h-[90vh] overflow-y-auto` or Drawer/Sheet pattern
- **Layout** — multi-column grids that don't collapse (`grid-cols-2` without `grid-cols-1 md:grid-cols-2`)

### Step 4: Pattern audit

Check for anti-patterns:
- **`window.confirm()`** — should use AlertDialog or equivalent
- **`window.alert()`** — should use Toast or inline error
- **Hand-rolled button layouts in dialogs** — should use DialogFooter/DrawerFooter
- **Manual close handlers** (`setIsOpen(false)`) — should use DialogClose
- **Inline styles** — should use Tailwind classes or CSS variables
- **Hardcoded colors** — should use CSS variables / theme tokens
- **Missing loading states** — buttons should show loading during async actions
- **Missing error states** — forms should show validation errors inline
- **Destructive actions in same footer as save** — should be separated or use AlertDialog

### Step 5: Accessibility check

- **Labels** — form inputs have associated Label components
- **sr-only text** — icon-only buttons have screen reader text
- **Focus management** — dialogs trap focus, close on Escape
- **Color contrast** — text on gradient/image backgrounds has sufficient contrast

### Step 6: Job Map check

Evaluate the feature area against the job the user is hiring it to do. This adds product context to technical findings.

**If jtbd-knowledge MCP is available** (`mcp__jtbd-knowledge__*` tools):
1. **Identify the job** — what is the user trying to accomplish in this feature area?
2. **Map to Universal Job Map stages** — use `mcp__jtbd-knowledge__get_framework` with `universal-job-map`. Check which of the 8 stages (Define -> Locate -> Prepare -> Confirm -> Execute -> Monitor -> Modify -> Conclude) the feature supports and where there are gaps.
3. **Check Four Forces** — use `mcp__jtbd-knowledge__get_framework` with `four-forces-kalbach`. For each finding, identify which force it relates to:
   - **Push** (away from status quo) — does the UI make the problem visible?
   - **Pull** (toward new solution) — does the UI show the value of the feature?
   - **Anxiety** (about new solution) — does the UI create uncertainty? (e.g., ugly `confirm()` dialogs, missing loading states, unclear destructive actions)
   - **Habit** (of status quo) — does the UI require too much new learning?

**If no JTBD MCP:** Apply the same thinking manually. Ask: what job is this feature area doing? What stages of that job does the UI support? Where does the UI create anxiety or fail to show value?

A missing loading state is both a pattern issue (Step 4) AND an Anxiety force. Session cards without status are both an information density issue (Step 3) AND a Monitor stage gap.

### Step 7: Output

Present findings as a table:

```
| Severity | Component | Issue | Pattern/Framework | File:line |
|----------|-----------|-------|-------------------|-----------|
| High     | ...       | ...   | ...               | ...       |
```

The Pattern/Framework column should reference both the technical fix (e.g., "use AlertDialog") and the JTBD context (e.g., "Anxiety — destructive action without confirmation").

Then ask: "Create issues for these findings?"

If yes, create issues with:
- Specific file:line references
- Component library pattern references (component names, example names)
- JTBD context where applicable
- Before/after code snippets where helpful

### Step 8: Design review (optional)

If the audit reveals issues that go beyond pattern swaps — e.g., information density, visual hierarchy, layout composition, or the overall feel of a feature area — invoke the `frontend-design` skill to propose a visual redesign. This applies when:

- Components are functionally correct but visually sparse or cluttered
- The feature area lacks cohesion (mixed card sizes, inconsistent spacing)
- Mobile layout works but feels like a shrunken desktop rather than a native mobile experience
- The user explicitly asks for design improvements beyond pattern fixes

The audit identifies *what's wrong*; `frontend-design` proposes *how it should look*.
