# Design Screen

You are helping the user create a screen design for a section of their product. The screen design will be a semantic HTML5 template styled with Tailwind CSS utility classes, ready to be exported and integrated into any web project or server-side template engine (e.g., Laravel Blade, Jinja2, Twig, ERB).

## Step 1: Check Prerequisites

First, identify the target section and verify that `spec.md` and `data.json` exist.

Read `/product/product-roadmap.md` to get the list of available sections.

If there's only one section, auto-select it. If there are multiple sections, use the AskUserQuestion tool to ask which section the user wants to create a screen design for.

Then verify all required files exist:

- `product/sections/[section-id]/spec.md`
- `product/sections/[section-id]/data.json`

If spec.md doesn't exist:

"I don't see a specification for **[Section Title]** yet. Please run `/shape-section` first to define the section's requirements."

If data.json doesn't exist:

"I don't see sample data for **[Section Title]** yet. Please run `/sample-data` first to create sample data for the screen designs."

Stop here if any file is missing.

## Step 2: Check for Design System and Shell

Check for optional enhancements:

**Design Tokens:**
- Check if `/product/design-system/colors.json` exists
- Check if `/product/design-system/typography.json` exists

If design tokens exist, read them and use them for styling. If they don't exist, show a warning:

"Note: Design tokens haven't been defined yet. I'll use default styling, but for consistent branding, consider running `/design-tokens` first."

**Shell:**
- Check if `src/shell/shell.html` exists

If shell exists, the screen design will render inside the shell in Design OS. If not, show a warning:

"Note: An application shell hasn't been designed yet. The screen design will render standalone. Consider running `/design-shell` first to see section screen designs in the full app context."

## Step 3: Analyze Requirements

Read and analyze the section files:

1. **spec.md** - Understand the user flows and UI requirements
2. **data.json** - Understand the data structure and sample content

Identify what views are needed based on the spec. Common patterns:

- List/dashboard view (showing multiple items)
- Detail view (showing a single item)
- Form/create view (for adding/editing)

## Step 4: Clarify the Screen Design Scope

If the spec implies multiple views, use the AskUserQuestion tool to confirm which view to build first:

"The specification suggests a few different views for **[Section Title]**:

1. **[View 1]** - [Brief description]
2. **[View 2]** - [Brief description]

Which view should I create first?"

If there's only one obvious view, proceed directly.

## Step 5: Invoke the Frontend Design Skill

Before creating the screen design, read the `frontend-design` skill to ensure high-quality design output.

Read the file at `.claude/skills/frontend-design/SKILL.md` and follow its guidance for creating distinctive, production-grade interfaces.

## Step 6: Create the HTML Screen Design

Create the main HTML file at `src/sections/[section-id]/[view-name].html`.

### HTML Structure

The HTML file MUST:

- Use semantic HTML5 elements (`<main>`, `<section>`, `<article>`, `<header>`, `<nav>`, `<aside>`, `<footer>`, etc.)
- Use Tailwind CSS utility classes directly on HTML elements for all styling
- Use template placeholders (`{{ variable }}`) for dynamic data — this syntax is compatible with most server-side template engines (Blade, Twig, Jinja2, etc.)
- Use `{{-- comment --}}` for template comments explaining sections
- Be fully self-contained and portable
- Be cleanly formatted and modular for easy copy/paste into template engines

Example:

```html
{{-- invoice-list.html - Invoice listing view --}}
<div class="max-w-4xl mx-auto px-4 py-8">
  <header class="flex items-center justify-between mb-6">
    <h1 class="text-2xl font-bold text-stone-900 dark:text-stone-100">Invoices</h1>
    <a href="{{ route_create }}" class="inline-flex items-center gap-2 rounded-lg bg-lime-600 px-4 py-2 text-sm font-medium text-white hover:bg-lime-700 transition-colors">
      Create Invoice
    </a>
  </header>

  {{-- Invoice list --}}
  <div class="space-y-2">
    {{-- Repeat for each invoice --}}
    <div class="flex items-center justify-between rounded-lg border border-stone-200 dark:border-stone-700 bg-white dark:bg-stone-800 p-4 hover:shadow-md transition-shadow">
      <div>
        <p class="font-medium text-stone-900 dark:text-stone-100">{{ invoice.clientName }}</p>
        <p class="text-sm text-stone-500 dark:text-stone-400">{{ invoice.invoiceNumber }}</p>
      </div>
      <div class="flex gap-2">
        <a href="{{ route_view }}" class="text-sm text-lime-600 hover:text-lime-700 dark:text-lime-400">View</a>
        <a href="{{ route_edit }}" class="text-sm text-stone-600 hover:text-stone-800 dark:text-stone-400">Edit</a>
        <button data-action="delete" data-id="{{ invoice.id }}" class="text-sm text-red-600 hover:text-red-700 dark:text-red-400">Delete</button>
      </div>
    </div>
    {{-- End repeat --}}
  </div>
</div>
```

### Design Requirements

- **Mobile responsive:** Use Tailwind responsive prefixes (`sm:`, `md:`, `lg:`) and ensure the design layout works gracefully on mobile, tablet and desktop screen sizes.
- **Light & dark mode:** Use `dark:` variants for all colors
- **Use design tokens:** If defined, apply the product's color palette and typography
- **Follow the frontend-design skill:** Create distinctive, memorable interfaces
- **Semantic HTML5:** Use appropriate semantic elements for accessibility and SEO
- **Template-engine friendly:** Use `{{ variable }}` placeholder syntax for dynamic content, making it easy to adapt to Blade, Twig, Jinja2, ERB, or similar engines

### Applying Design Tokens

**If `/product/design-system/colors.json` exists:**
- Use the primary color for buttons, links, and key accents
- Use the secondary color for tags, highlights, secondary elements
- Use the neutral color for backgrounds, text, and borders
- Example: If primary is `lime`, use `lime-500`, `lime-600`, etc. for primary actions

**If `/product/design-system/typography.json` exists:**
- Note the font choices for reference in comments
- The fonts will be applied at the page level via Google Fonts, but use appropriate font weights

**If design tokens don't exist:**
- Fall back to `stone` for neutrals and `lime` for accents (Design OS defaults)

### What to Include

- Implement ALL user flows and UI requirements from the spec
- Use template placeholders for dynamic data (not hardcoded values)
- Include realistic UI states via Tailwind (hover, focus, active, etc.)
- Use `data-action` attributes on interactive elements to document intended behaviors
- Use `<a href="{{ route_name }}">` for navigation actions and `<button data-action="...">` for in-page actions

### What NOT to Include

- No hardcoded data — use `{{ placeholder }}` syntax for all dynamic content
- No features not specified in the spec
- No JavaScript frameworks or libraries
- No navigation elements (shell handles navigation)
- No inline styles — all styling via Tailwind CSS utility classes

## Step 7: Create Partial HTML Files (If Needed)

For complex views, break down into modular partial HTML files. Each partial represents a reusable section of the page.

Create partials at `src/sections/[section-id]/partials/[partial-name].html`.

Example:

```html
{{-- _invoice-row.html - Single invoice row partial --}}
<div class="flex items-center justify-between rounded-lg border border-stone-200 dark:border-stone-700 bg-white dark:bg-stone-800 p-4 hover:shadow-md transition-shadow">
  <div>
    <p class="font-medium text-stone-900 dark:text-stone-100">{{ invoice.clientName }}</p>
    <p class="text-sm text-stone-500 dark:text-stone-400">{{ invoice.invoiceNumber }}</p>
  </div>
  <div class="flex gap-2">
    <a href="{{ route_view }}" class="text-sm text-lime-600 hover:text-lime-700 dark:text-lime-400">View</a>
    <a href="{{ route_edit }}" class="text-sm text-stone-600 hover:text-stone-800 dark:text-stone-400">Edit</a>
    <button data-action="delete" data-id="{{ invoice.id }}" class="text-sm text-red-600 hover:text-red-700 dark:text-red-400">Delete</button>
  </div>
</div>
```

Then reference in the main template:

```html
{{-- invoice-list.html --}}
<div class="space-y-2">
  {{-- Include partial for each invoice: @include('partials._invoice-row') --}}
</div>
```

## Step 8: Create the Preview Page

Create a preview page at `src/sections/[section-id]/[view-name]-preview.html` that renders the screen design with sample data inlined. This is what Design OS uses to display the design.

Example:

```html
<!DOCTYPE html>
<html lang="en" class="h-full">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>[View Name] — Preview</title>
  <link href="/src/index.css" rel="stylesheet" />
</head>
<body class="min-h-full bg-white dark:bg-stone-950 text-stone-900 dark:text-stone-100">

  {{-- The screen design with sample data populated --}}
  <div class="max-w-4xl mx-auto px-4 py-8">
    <header class="flex items-center justify-between mb-6">
      <h1 class="text-2xl font-bold">Invoices</h1>
      <a href="#" class="inline-flex items-center gap-2 rounded-lg bg-lime-600 px-4 py-2 text-sm font-medium text-white hover:bg-lime-700 transition-colors">
        Create Invoice
      </a>
    </header>

    <div class="space-y-2">
      <!-- Sample data from data.json rendered here -->
      <div class="flex items-center justify-between rounded-lg border border-stone-200 dark:border-stone-700 bg-white dark:bg-stone-800 p-4">
        <div>
          <p class="font-medium">Acme Corp</p>
          <p class="text-sm text-stone-500">INV-2024-001</p>
        </div>
        <div class="flex gap-2">
          <a href="#" class="text-sm text-lime-600 hover:text-lime-700">View</a>
          <a href="#" class="text-sm text-stone-600 hover:text-stone-800">Edit</a>
          <button class="text-sm text-red-600 hover:text-red-700">Delete</button>
        </div>
      </div>
      <!-- Repeat with more sample data records -->
    </div>
  </div>

</body>
</html>
```

The preview page:

- Is a complete HTML document that can be opened directly in a browser
- Uses sample data from data.json rendered as real content (not placeholders)
- Is NOT exported to the user's codebase — it's only for Design OS previewing
- **Will render inside the shell** if one has been designed

## Step 9: Confirm and Next Steps

Let the user know:

"I've created the screen design for **[Section Title]**:

**Exportable HTML templates** (portable, template-engine ready):

- `src/sections/[section-id]/[view-name].html`
- `src/sections/[section-id]/partials/[partial-name].html` (if created)

**Preview page** (for Design OS only):

- `src/sections/[section-id]/[view-name]-preview.html`

**Important:** Restart your dev server to see the changes.

[If shell exists]: The screen design will render inside your application shell, showing the full app experience.

[If design tokens exist]: I've applied your color palette ([primary], [secondary], [neutral]) and typography choices.

**Next steps:**

- Run `/screenshot-design` to capture a screenshot of this screen design for documentation
- If the spec calls for additional views, run `/design-screen` again to create them
- When all sections are complete, run `/export-product` to generate the complete export package"

If the spec indicates additional views are needed:

"The specification also calls for [other view(s)]. Run `/design-screen` again to create those, then `/screenshot-design` to capture each one."

## Important Notes

- ALWAYS read the `frontend-design` skill before creating screen designs
- HTML templates MUST use `{{ placeholder }}` syntax for all dynamic content — never hardcode data in exportable templates
- The preview page is the ONLY file that contains sample data inlined
- Use semantic HTML5 elements throughout
- Use `data-action` attributes to document interactive behaviors
- Always remind the user to restart the dev server after creating files
- Partials should be modular and reusable across views
- Apply design tokens when available for consistent branding
- Screen designs render inside the shell when viewed in Design OS (if shell exists)
- Format HTML cleanly with consistent indentation for easy integration into server-side template engines
