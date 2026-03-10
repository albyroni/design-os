# Designing Sections

After completing [Product Planning](product-planning.md), you're ready to design individual sections. Work through each section in your roadmap, completing these steps for each one.

## 1. Shape the Section

```
/shape-section
```

Define what the section does and generate its sample data — all in one step. If you have multiple sections, you'll be asked which one to work on.

This is a conversational process to establish:

- **Overview** — What this section is for (2-3 sentences)
- **User flows** — The main actions and step-by-step interactions
- **UI requirements** — Specific layouts, patterns, or components needed
- **Scope boundaries** — What's intentionally excluded

Share any notes or ideas you have. The AI will ask clarifying questions about user actions, information to display, and UI patterns. Focus on experience and interface requirements—no backend or database details.

You'll also be asked whether this section should display inside the application shell (most sections do) or as a standalone page (for things like landing pages or embedded widgets).

Once it has enough information, the AI writes the spec and generates sample data + TypeScript types automatically:

- **Sample data** — 5-10 realistic records with varied content, edge cases, and a `_meta` section describing each entity
- **TypeScript types** — Data interfaces for each entity, plus a Props interface with callbacks for actions

**Creates:**
- `product/sections/[section-id]/spec.md` — Section specification
- `product/sections/[section-id]/data.json` — Sample data with `_meta` descriptions
- `product/sections/[section-id]/types.ts` — TypeScript interfaces

**To update sample data later:** Run `/sample-data` to modify the data structure or sample records.

## 2. Design the Screen

```
/design-screen
```

Build the actual HTML templates for the section. This is where the spec and sample data become a working UI.

### What Gets Created

**Exportable HTML templates** (portable, template-engine ready):

The main template and any partials, using semantic HTML5 and Tailwind CSS with `{{ placeholder }}` syntax for dynamic content. These are what get exported to your codebase.

```html
{{-- Example: HTML templates use placeholder syntax for dynamic data --}}
<div class="max-w-4xl mx-auto px-4 py-8">
  <header class="flex items-center justify-between mb-6">
    <h1 class="text-2xl font-bold text-stone-900 dark:text-stone-100">Invoices</h1>
    <a href="{{ route_create }}" class="rounded-lg bg-lime-600 px-4 py-2 text-sm font-medium text-white hover:bg-lime-700">
      Create Invoice
    </a>
  </header>
  {{-- Invoice list items rendered here --}}
</div>
```

**Preview page** (for Design OS only):

A full HTML page with sample data inlined, so you can see it running in Design OS.

### Design Requirements

All screen designs include:

- **Mobile responsive** — Tailwind responsive prefixes (`sm:`, `md:`, `lg:`)
- **Light & dark mode** — Using `dark:` variants
- **Design tokens applied** — Your color palette and typography choices
- **All spec requirements** — Every user flow and UI requirement implemented

### Multiple Views

If the spec implies multiple views (list view, detail view, form, etc.), you'll be asked which to build first. Run `/design-screen` again for additional views.

**Creates:**
- `src/sections/[section-id]/[view-name].html` — Main HTML template
- `src/sections/[section-id]/partials/[partial-name].html` — Partial templates as needed
- `src/sections/[section-id]/[view-name]-preview.html` — Preview page

**Important:** Restart your dev server after creating screen designs to see the changes.

## 3. Capture Screenshots (Optional)

```
/screenshot-design
```

Take screenshots of your screen designs for documentation. Screenshots are saved alongside the spec and data files.

This command:
1. Starts the dev server automatically
2. Navigates to your screen design
3. Hides the Design OS navigation bar
4. Captures a full-page screenshot

Screenshots are useful for:
- Visual reference during implementation
- Documentation and handoff materials
- Comparing designs across sections

**Requires:** Playwright MCP server. If not installed, you'll be prompted with setup instructions.

**Creates:** `product/sections/[section-id]/[screen-name].png`

## Repeat for Each Section

Work through your roadmap sections in order. Each section builds on the foundation you established and benefits from the consistency of your global data shape and design tokens.

## What's Next

When all sections are designed, you're ready to export. See [Export](export.md) for generating the complete handoff package.
