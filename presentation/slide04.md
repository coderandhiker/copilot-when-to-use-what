# Prompt Files

## Reusable macros you invoke on demand

---

### What they are

Prompt files are `.prompt.md` files that act like **saved recipes**. Unlike
instructions (which are always-on), prompt files are **invoked explicitly** via
`/` slash command in chat. They can wire in tools, reference agents, and define
multi-step workflows.

> **Status:** Public preview — VS Code, Visual Studio, and JetBrains

---

### Directory structure

```
your-repo/
├── .github/
│   ├── prompts/
│   │   ├── new-api-endpoint.prompt.md
│   │   ├── refactor-to-hooks.prompt.md
│   │   ├── write-migration.prompt.md
│   │   └── review-pr.prompt.md
│   └── copilot-instructions.md
└── src/
    └── ...
```

---

### Anatomy of a prompt file

```markdown
---
name: 'New API Endpoint'
description: 'Scaffold a complete REST endpoint with validation, handler, and test'
tools:
  - run_in_terminal
  - file_search
  - grep_search
---

# New API Endpoint

## Inputs
- **Resource name:** ${input:resource}  (e.g., "users", "orders")
- **HTTP method:** ${input:method}      (e.g., GET, POST, PUT, DELETE)

## Steps

1. Create `src/routes/${resource}.ts` with:
   - A Zod schema for request validation
   - An Express handler that validates input and calls the service layer
   - Proper error handling returning RFC 9457 Problem Details

2. Create `src/routes/${resource}.test.ts` with:
   - Happy-path test
   - Validation-failure test (400)
   - Not-found test (404) if applicable

3. Register the route in `src/routes/index.ts`

4. Run `npm run lint` and `npm test` to verify everything passes

## Template
Use the pattern established in `src/routes/health.ts` as a reference.
```

---

### Invoking a prompt file

In VS Code chat, type `/` and select the prompt:

```
User:  /new-api-endpoint

       resource: products
       method: POST
```

Copilot picks up the prompt file, fills in the variables, and executes the
multi-step plan — creating files, running tests, and fixing lint errors.

---

### Context variables

Prompt files support built-in context variables using `${variableName}` syntax:

| Category    | Variables                                                              |
|-------------|------------------------------------------------------------------------|
| Workspace   | `${workspaceFolder}`, `${workspaceFolderBasename}`                     |
| Selection   | `${selection}`, `${selectedText}`                                      |
| File        | `${file}`, `${fileBasename}`, `${fileDirname}`,                       |
|             | `${fileBasenameNoExtension}`                                           |
| User input  | `${input:varName}`, `${input:varName:placeholder}`                     |

```markdown
## Context
- Current file: ${file}
- Working directory: ${workspaceFolder}
- Selected code:
${selection}

## Task
Refactor the ${input:component} component to use ${input:pattern:"e.g. hooks"}
```

> `${input:x}` prompts the user for a value when the prompt file runs.
> The other variables are filled in automatically from editor state.

---

### Prompt files with agent references

```markdown
---
name: 'Full Feature Implementation'
description: 'End-to-end feature: design, implement, test, document'
agent: 'planner'
tools:
  - agent
---

# Full Feature Implementation

1. Ask @designer to review the requirements and propose a design
2. Ask @implementer to build it following the design
3. Ask @tester to write and run comprehensive tests
4. Summarize what was built and any open items
```

---

### Instructions vs. Prompt Files — at a glance

```
┌─────────────────────┬──────────────────────────────────┐
│                     │  Instructions   │  Prompt Files  │
├─────────────────────┼─────────────────┼────────────────┤
│ Activation          │  Always-on      │  On-demand (/) │
│ Purpose             │  Guardrails     │  Recipes       │
│ Variables           │  No             │  Yes (${x})    │
│ Tool references     │  No             │  Yes           │
│ Agent references    │  No             │  Yes           │
│ Shared with team    │  ✅ via repo    │  ✅ via repo  │
│ File extension      │  .instructions  │  .prompt.md    │
│                     │  .md            │                │
│ Location            │  .github/       │  .github/      │
│                     │ instructions/   │  prompts/      │
└─────────────────────┴─────────────────┴────────────────┘
```

> **Rule of thumb:** Instructions say "always do this."
> Prompt files say "when I ask, do *this sequence*."
