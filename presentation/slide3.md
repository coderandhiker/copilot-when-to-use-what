# Instructions

## Your repo's constitution — boring, stable, always-on

---

### What they are

Instructions are markdown files that Copilot **automatically injects into every
chat request**. You never have to remember to include them. They just apply.

---

### The three flavors

```
your-repo/
├── .github/
│   ├── copilot-instructions.md          ← repo-wide (always-on)
│   └── instructions/
│       ├── python-standards.instructions.md   ← path-specific
│       ├── react-components.instructions.md   ← path-specific
│       └── testing.instructions.md            ← path-specific
├── AGENTS.md                             ← cross-agent compatible
├── CLAUDE.md                             ← cross-agent compatible
└── src/
    └── ...
```

---

### 1. Repo-wide: `copilot-instructions.md`

Applied to **every** chat request in the workspace. No conditions.

```markdown
<!-- .github/copilot-instructions.md -->

# Project: Contoso API

## Tech stack
- Node.js 22 + TypeScript 5.7 (strict mode)
- Express 5 with Zod validation
- PostgreSQL 17 via Drizzle ORM
- Vitest for testing

## Build & test
- `npm run build` — compiles TypeScript
- `npm test` — runs Vitest in watch mode
- `npm run test:ci` — single run with coverage (must pass before PR)

## Conventions
- All API handlers go in `src/routes/`
- Use Zod schemas for request validation — never trust raw input
- Error responses follow RFC 9457 (Problem Details)
- No default exports. Use named exports everywhere.
```

---

### 2. Path-specific: `*.instructions.md`

Applied **only** when Copilot is working on files matching the `applyTo` glob.

```markdown
---
name: 'React Components'
description: 'Standards for React component files'
applyTo: 'src/components/**/*.tsx'
---

# React Component Standards

- Use function components with arrow syntax
- Props interface must be exported and named `{Component}Props`
- Use `clsx` for conditional classNames, never string concatenation
- Co-locate tests: `Button.tsx` → `Button.test.tsx`
- Prefer composition over prop drilling — use React context for 3+ levels
```

---

### 3. Cross-agent: `AGENTS.md` and `CLAUDE.md`

VS Code natively detects these files too. One instruction file, multiple agent
ecosystems.

```markdown
<!-- AGENTS.md (root of workspace) -->

# Contoso API — Agent Instructions

This is a Node.js REST API. When working in this repo:

1. Always run `npm run lint` after making changes
2. Never modify files in `src/generated/` — they are auto-generated
3. All database migrations go in `drizzle/migrations/`
4. Before committing, run `npm run test:ci` and ensure 0 failures
```

---

### When to use instructions

✅ Team coding standards and naming conventions
✅ Build/test/lint commands ("always run X before Y")
✅ Architectural constraints ("never put business logic in controllers")
✅ Security guardrails ("always validate input with Zod")
✅ Technology stack declarations

❌ NOT for one-off tasks
❌ NOT for personal preferences (use user-level settings instead)

> **Rule of thumb:** If violating it would cause a code review rejection,
> it belongs in instructions.
