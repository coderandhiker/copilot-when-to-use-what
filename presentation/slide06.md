# Agents

## Autonomous personas with their own tools and personality

---

### What they are

An Agent is a **full persona** — an identity with its own system prompt,
tool set, model preference, and behavioral constraints. You interact with
agents by `@mentioning` them in chat.

Unlike instructions (passive rules) or skills (invokable capabilities),
agents are **actors** that make decisions and choose which tools to use.

---

### Directory structure

```
your-repo/
├── .github/
│   ├── agents/
│   │   ├── planner.agent.md
│   │   ├── implementer.agent.md
│   │   ├── reviewer.agent.md
│   │   └── security-auditor.agent.md
│   ├── copilot-instructions.md
│   └── skills/
│       └── ...
└── src/
    └── ...
```

---

### Anatomy of an `.agent.md` file

```markdown
---
name: 'reviewer'
description: 'Senior code reviewer — catches bugs, style issues, and security gaps'
tools:
  - grep_search
  - read_file
  - get_errors
  - run_in_terminal
model: 'claude-sonnet-4'
---

# Code Reviewer

You are a senior engineer performing a thorough code review.

## Approach
1. Read the changed files to understand what was modified
2. Check for:
   - Logic errors and edge cases
   - Security vulnerabilities (injection, auth bypass, data exposure)
   - Performance issues (N+1 queries, unbounded loops)
   - Style violations per the project's `copilot-instructions.md`
3. Run the test suite to verify nothing is broken
4. Provide a structured review with severity ratings

## Output format
For each finding:
- **File:** path/to/file.ts
- **Line:** 42
- **Severity:** 🔴 Critical | 🟡 Warning | 🔵 Suggestion
- **Issue:** Clear description
- **Fix:** Suggested code change

## Constraints
- Never modify code directly — only suggest changes
- Always run tests before concluding "no issues found"
- Flag any TODO/FIXME comments that relate to the changed code
```

---

### Frontmatter fields

```yaml
---
name: 'agent-name'           # Required — the @mention handle
description: 'What I do'     # Required — shown in agent picker
tools:                        # Optional — restrict available tools
  - read_file
  - grep_search
  - run_in_terminal
model: 'claude-sonnet-4'     # Optional — override default model
agents:                       # Optional — restrict which subagents I can call
  - implementer
  - tester
---
```

---

### Invoking an agent

```
User:  @reviewer  Review the changes in my current branch
                  focusing on the auth module

User:  @planner   Break down this feature request into tasks:
                  "Add two-factor authentication via TOTP"

User:  @implementer  Build the TOTP verification endpoint
                     following the plan from @planner
```

---

### What separates agents from the rest

```
┌──────────────┬───────────┬──────────┬──────────┬──────────┐
│              │Instructions│ Prompts  │ Skills   │ Agents   │
├──────────────┼───────────┼──────────┼──────────┼──────────┤
│ Has identity │     ❌     │    ❌    │    ❌    │    ✅    │
│ Chooses tools│     ❌     │    ❌    │    ❌    │    ✅    │
│ Has persona  │     ❌     │    ❌    │    ❌    │    ✅    │
│ Autonomous   │     ❌     │    ❌    │  Partial │    ✅    │
│ Composable   │     ✅     │    ✅    │    ✅    │    ✅    │
│ Always-on    │     ✅     │    ❌    │    ❌    │    ❌    │
│ @mentionable │     ❌     │    ❌    │    ❌    │    ✅    │
└──────────────┴───────────┴──────────┴──────────┴──────────┘
```

---

### Real-world agent ideas

| Agent              | Purpose                                   | Key tools              |
|--------------------|-------------------------------------------|------------------------|
| `@planner`         | Break features into implementation tasks  | read_file, semantic    |
| `@implementer`     | Write code following a plan               | create_file, edit, run |
| `@reviewer`        | Review changes for bugs & style           | grep, read, get_errors |
| `@security`        | Audit for OWASP Top 10 vulnerabilities    | grep, read, run        |
| `@doc-writer`      | Generate API docs from source code        | read, semantic, create |
| `@migration-bot`   | Handle database schema changes            | run_in_terminal, read  |

> **Rule of thumb:** If you'd assign it to a specific person on your team,
> it's an agent.
