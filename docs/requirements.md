# Requirements

## Running Design OS

Design OS runs locally on your machine. You'll need:

- **Node.js** (v18 or higher)
- **npm** (comes with Node.js)
- **An AI coding assistant** — Design OS uses slash commands to guide the design process. Claude Code is recommended, but you can invoke the Design OS commands from any AI coding tool that supports custom commands or prompts (Cursor, Windsurf, Codex, etc.)

## Installing Your Exported Templates

When you export your designs, you get production-ready HTML5 templates styled with Tailwind CSS. Your target codebase needs:

### Required

- **Tailwind CSS** (v4) — Templates use Tailwind utility classes for all styling
- **A server-side template engine** (optional) — Blade (Laravel), Twig (Symfony), Jinja2 (Python), ERB (Rails), or similar. Templates use `{{ placeholder }}` syntax that can be adapted to any engine.

### Backend

Your backend can be anything—Rails, Laravel, Next.js API routes, Python, Go, whatever. Design OS only handles the frontend design layer.
