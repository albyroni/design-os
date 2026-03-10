# Export

When your designs are complete, export everything your implementation agent (or team) needs to build the product.

## When to Export

You're ready to export when:

- Product vision and roadmap are defined
- At least one section has screen designs
- You're satisfied with the design direction

You can export at any point—it doesn't have to be "complete." Exporting generates a snapshot of your current designs. You can always export again later as you add more sections.

## Running the Export

```
/export-product
```

The export command:

1. **Checks prerequisites** — Verifies required files exist
2. **Gathers all design assets** — Components, types, data, tokens
3. **Generates implementation instructions** — Including ready-to-use prompts
4. **Generates test instructions** — TDD specs for each section
5. **Creates the export package** — A complete `product-plan/` directory
6. **Creates a zip file** — `product-plan.zip` for easy download

## What's Included

### Ready-to-Use Prompts

```
product-plan/prompts/
├── one-shot-prompt.md     # Prompt for full implementation
└── section-prompt.md      # Prompt template for section-by-section
```

These are pre-written prompts you copy/paste into your coding agent. They reference the instruction files and guide your agent to review the designs and ask clarifying questions before implementing.

### Instructions

```
product-plan/
├── product-overview.md              # Product summary (always provide)
└── instructions/
    ├── one-shot-instructions.md     # All milestones combined
    └── incremental/                 # Milestone-by-milestone implementation
        ├── 01-shell.md              # Design tokens + application shell
        ├── 02-[section-id].md        # One per section (e.g., 02-invoices.md)
        └── ...
```

**product-overview.md** provides context about the full product—always include it with any implementation session.

**one-shot-instructions.md** combines all milestones into a single document. Use this with `one-shot-prompt.md` for full implementation.

**Incremental instructions** break the work into milestones. Use these with `section-prompt.md` for step-by-step implementation.

### Design System

```
product-plan/design-system/
├── tokens.css           # CSS custom properties
├── tailwind-colors.md   # Tailwind configuration guide
└── fonts.md             # Google Fonts setup
```

### Data Shapes

```
product-plan/data-shapes/
├── README.md            # UI data contracts overview
└── overview.ts          # Combined type reference (all sections)
```

### Shell HTML Templates

```
product-plan/shell/
├── README.md            # Design intent
├── shell.html           # Main layout template
├── nav.html             # Navigation partial
├── user-menu.html       # User menu partial
└── screenshot.png       # Visual reference (if captured)
```

### Section HTML Templates

For each section:

```
product-plan/sections/[section-id]/
├── README.md            # Feature overview, user flows
├── tests.md             # UI behavior test specs
├── [view-name].html     # Exportable HTML templates
├── partials/            # Partial HTML files (if any)
│   └── [partial].html
├── types.ts             # Data shape interfaces
├── sample-data.json     # Test data
└── screenshot.png       # Visual reference (if captured)
```

### Test Instructions

Each section includes a `tests.md` file with framework-agnostic test-writing instructions:

- **User flow tests** — Success and failure paths for key interactions
- **Empty state tests** — Verifying UI when no records exist
- **Component interaction tests** — Specific UI elements and behaviors to verify

These instructions describe WHAT to test, not HOW—your coding agent adapts them to your test framework (Jest, Vitest, Playwright, Cypress, RSpec, Minitest, PHPUnit, etc.).

## About the Components

Exported HTML templates are:

- **Template-engine ready** — Use `{{ placeholder }}` syntax for dynamic data, compatible with Blade, Twig, Jinja2, ERB, and similar engines
- **Portable** — Work with any web stack, no Design OS dependencies
- **Complete** — Full styling, responsive design, dark mode support
- **Production-ready** — Not prototypes or mockups

```html
{{-- Templates use placeholder syntax for dynamic content --}}
<div class="max-w-4xl mx-auto">
  <div class="flex items-center justify-between p-4 border-b border-stone-200 dark:border-stone-700">
    <span class="font-medium text-stone-900 dark:text-stone-100">{{ invoice.clientName }}</span>
    <div class="flex gap-2">
      <a href="{{ route_view }}" class="text-sm text-lime-600 hover:text-lime-700">View</a>
      <a href="{{ route_edit }}" class="text-sm text-stone-600 hover:text-stone-800">Edit</a>
      <button data-action="delete" data-id="{{ invoice.id }}" class="text-sm text-red-600 hover:text-red-700">Delete</button>
    </div>
  </div>
</div>
```

Your implementation agent's job is to:
- Integrate the HTML templates into your template engine
- Wire up `{{ placeholder }}` syntax to your backend data
- Wire up `data-action` attributes and links to your routing and business logic
- Replace sample data with real data from your backend
- Implement proper error handling and loading states
- Implement empty states when no records exist (first-time users, after deletions)
- Build the backend APIs the templates need
- Write tests based on the provided test instructions (TDD approach)

## Using the Export

See [Codebase Implementation](codebase-implementation.md) for detailed guidance on implementing your design in your codebase.
