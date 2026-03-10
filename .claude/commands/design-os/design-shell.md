# Design Shell

You are helping the user design the application shell — the persistent navigation and layout that wraps all sections. This is a screen design, not implementation code.

## Step 1: Check Prerequisites

First, verify prerequisites exist:

1. Read `/product/product-overview.md` — Product name and description
2. Read `/product/product-roadmap.md` — Sections for navigation
3. Check if `/product/design-system/colors.json` and `/product/design-system/typography.json` exist

If overview or roadmap are missing:

"Before designing the shell, you need to define your product and sections. Please run:
1. `/product-vision` — Define your product
2. `/product-roadmap` — Define your sections"

Stop here if overview or roadmap are missing.

If design tokens are missing, show a warning but continue:

"Note: Design tokens haven't been defined yet. I'll proceed with default styling, but you may want to run `/design-tokens` first for consistent colors and typography."

## Step 2: Analyze Product Structure

Review the roadmap sections and present navigation options:

"I'm designing the shell for **[Product Name]**. Based on your roadmap, you have [N] sections:

1. **[Section 1]** — [Description]
2. **[Section 2]** — [Description]
3. **[Section 3]** — [Description]

Let's decide on the shell layout. Common patterns:

**A. Sidebar Navigation** — Vertical nav on the left, content on the right
   Best for: Apps with many sections, dashboard-style tools, admin panels

**B. Top Navigation** — Horizontal nav at top, content below
   Best for: Simpler apps, marketing-style products, fewer sections

**C. Minimal Header** — Just logo + user menu, sections accessed differently
   Best for: Single-purpose tools, wizard-style flows

Which pattern fits **[Product Name]** best?"

Wait for their response.

## Step 3: Gather Design Details

Use AskUserQuestion to clarify:

- "Where should the user menu (avatar, logout) appear?"
- "Do you want the sidebar collapsible on mobile, or should it become a hamburger menu?"
- "Any additional items in the navigation? (Settings, Help, etc.)"
- "What should the 'home' or default view be when the app loads?"

## Step 4: Present Shell Specification

Once you understand their preferences:

"Here's the shell design for **[Product Name]**:

**Layout Pattern:** [Sidebar/Top Nav/Minimal]

**Navigation Structure:**
- [Nav Item 1] → [Section]
- [Nav Item 2] → [Section]
- [Nav Item 3] → [Section]
- [Additional items like Settings, Help]

**User Menu:**
- Location: [Top right / Bottom of sidebar]
- Contents: Avatar, user name, logout

**Responsive Behavior:**
- Desktop: [How it looks]
- Mobile: [How it adapts]

Does this match what you had in mind?"

Iterate until approved.

## Step 5: Create the Shell Specification

Create `/product/shell/spec.md`:

```markdown
# Application Shell Specification

## Overview
[Description of the shell design and its purpose]

## Navigation Structure
- [Nav Item 1] → [Section 1]
- [Nav Item 2] → [Section 2]
- [Nav Item 3] → [Section 3]
- [Any additional nav items]

## User Menu
[Description of user menu location and contents]

## Layout Pattern
[Description of the layout — sidebar, top nav, etc.]

## Responsive Behavior
- **Desktop:** [Behavior]
- **Tablet:** [Behavior]
- **Mobile:** [Behavior]

## Design Notes
[Any additional design decisions or notes]
```

## Step 6: Create Shell HTML Templates

Create the shell HTML templates at `src/shell/`:

### shell.html
The main shell layout template that wraps section content. Use semantic HTML5 elements and Tailwind CSS classes.

```html
{{-- shell.html - Application shell layout --}}
<div class="min-h-screen bg-white dark:bg-stone-950">
  {{-- Sidebar / Top Navigation --}}
  <nav class="..." aria-label="Main navigation">
    {{-- Navigation items --}}
    <a href="{{ nav_item.href }}" class="...">{{ nav_item.label }}</a>
  </nav>

  {{-- User menu --}}
  <div class="...">
    <img src="{{ user.avatarUrl }}" alt="{{ user.name }}" class="h-8 w-8 rounded-full" />
    <span>{{ user.name }}</span>
    <a href="{{ route_logout }}">Logout</a>
  </div>

  {{-- Main content area --}}
  <main class="...">
    {{-- Section content is rendered here --}}
    {{ content }}
  </main>
</div>
```

### nav.html
The navigation partial (sidebar or top nav based on the chosen pattern).

### user-menu.html
The user menu partial with avatar and dropdown.

**Template Requirements:**
- Use `{{ placeholder }}` syntax for all dynamic data (template-engine compatible)
- Apply design tokens if they exist (colors, fonts)
- Support light and dark mode with `dark:` variants
- Be mobile responsive
- Use Tailwind CSS for all styling
- Use SVG icons inline or reference an icon set (no framework-specific icon libraries)
- Use semantic HTML5 elements (`<nav>`, `<main>`, `<header>`, etc.)

## Step 7: Create Shell Preview

Create `src/shell/shell-preview.html` — a full HTML page for previewing the shell in Design OS:

```html
<!DOCTYPE html>
<html lang="en" class="h-full">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Shell Preview</title>
  <link href="/src/index.css" rel="stylesheet" />
</head>
<body class="min-h-full bg-white dark:bg-stone-950 text-stone-900 dark:text-stone-100">

  <div class="min-h-screen flex">
    {{-- Navigation --}}
    <nav class="w-64 border-r border-stone-200 dark:border-stone-800 bg-stone-50 dark:bg-stone-900 p-4" aria-label="Main navigation">
      <div class="font-bold text-lg mb-6">[Product Name]</div>
      <ul class="space-y-1">
        <li><a href="#" class="block px-3 py-2 rounded-lg bg-lime-50 text-lime-700 dark:bg-lime-900/30 dark:text-lime-400 font-medium">[Section 1]</a></li>
        <li><a href="#" class="block px-3 py-2 rounded-lg text-stone-600 hover:bg-stone-100 dark:text-stone-400 dark:hover:bg-stone-800">[Section 2]</a></li>
        <li><a href="#" class="block px-3 py-2 rounded-lg text-stone-600 hover:bg-stone-100 dark:text-stone-400 dark:hover:bg-stone-800">[Section 3]</a></li>
      </ul>
    </nav>

    {{-- Main content area --}}
    <main class="flex-1 p-8">
      <h1 class="text-2xl font-bold mb-4">Content Area</h1>
      <p class="text-stone-600 dark:text-stone-400">
        Section content will render here.
      </p>
    </main>
  </div>

</body>
</html>
```

## Step 8: Apply Design Tokens

If design tokens exist, apply them to the shell templates:

**Colors:**
- Read `/product/design-system/colors.json`
- Use primary color for active nav items, key accents
- Use secondary color for hover states, subtle highlights
- Use neutral color for backgrounds, borders, text

**Typography:**
- Read `/product/design-system/typography.json`
- Apply heading font to nav items and titles
- Apply body font to other text
- Include Google Fonts link in the preview page

## Step 9: Confirm Completion

Let the user know:

"I've designed the application shell for **[Product Name]**:

**Created files:**
- `/product/shell/spec.md` — Shell specification
- `src/shell/shell.html` — Main shell layout template
- `src/shell/nav.html` — Navigation partial
- `src/shell/user-menu.html` — User menu partial
- `src/shell/shell-preview.html` — Preview page

**Shell features:**
- [Layout pattern] layout
- Navigation for all [N] sections
- User menu with avatar and logout
- Mobile responsive design
- Light/dark mode support

**Important:** Restart your dev server to see the changes.

When you design section screens with `/design-screen`, they will render inside this shell, showing the full app experience.

Next: Run `/shape-section` to start designing your first section."

## Important Notes

- The shell is a screen design — it demonstrates the navigation and layout design
- HTML templates use `{{ placeholder }}` syntax for dynamic data, compatible with server-side template engines
- The preview page is for Design OS only — not exported
- Apply design tokens when available for consistent styling
- Keep the shell focused on navigation chrome — no authentication UI
- Section screen designs will render inside the shell's content area
- Use semantic HTML5 elements and Tailwind CSS for all styling
