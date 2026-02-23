# Practical Operating Model

## The five-step plan you can use tomorrow

---

### Step 1: Write your repo's constitution

Create `.github/copilot-instructions.md` with:

```markdown
# Project: [Your Project Name]

## Stack
- [Language, framework, database, etc.]

## Build & test
- `[build command]`
- `[test command]`
- `[lint command]`

## Non-negotiable rules
- [Your team's top 5-10 coding standards]
- [Security requirements]
- [Architectural constraints]
```

**Time: 15 minutes. Impact: Every Copilot interaction improves immediately.**

---

### Step 2: Add path-specific instructions

Identify your repo's distinct zones and add targeted rules:

```
.github/instructions/
├── api-routes.instructions.md       → applyTo: 'src/routes/**'
├── react-components.instructions.md → applyTo: 'src/components/**'
├── database.instructions.md         → applyTo: 'src/db/**'
└── tests.instructions.md            → applyTo: '**/*.test.ts'
```

**Time: 30 minutes. Impact: Context-aware guidance without noise.**

---

### Step 3: Create your first prompt files

Start with your team's most common tasks:

```
.github/prompts/
├── new-endpoint.prompt.md      ← "scaffold a REST endpoint"
├── add-test-coverage.prompt.md ← "add tests for uncovered code"
└── fix-ci.prompt.md            ← "diagnose and fix the CI failure"
```

**Time: 1 hour. Impact: Repeatable, high-quality task execution.**

---

### Step 4: Build your first skill

Pick something your team does repeatedly that has a clear recipe:

```
.github/skills/
└── create-migration/
    ├── SKILL.md
    ├── templates/
    │   ├── create-table.sql
    │   └── add-column.sql
    └── examples/
        └── 001-add-users-table.sql
```

**Time: 1-2 hours. Impact: Encode tribal knowledge permanently.**

---

### Step 5: Define specialist agents

Start with two or three agents that match your workflow:

```
.github/agents/
├── implementer.agent.md   ← writes code following specs
├── reviewer.agent.md      ← reviews for bugs, style, security
└── planner.agent.md       ← breaks features into tasks, delegates
```

**Time: 1-2 hours. Impact: Autonomous multi-step workflows.**

---

### The maturity curve

```
Week 1          Week 2-3          Month 2           Month 3+
  │                │                 │                  │
  ▼                ▼                 ▼                  ▼

┌────────┐    ┌──────────┐    ┌──────────┐    ┌────────────────┐
│ Instruc-│    │ + Prompt │    │ + Skills │    │ + Agents &     │
│ tions   │ →  │   Files  │ →  │          │ →  │   Sub-Agents   │
│ only    │    │          │    │          │    │                │
└────────┘    └──────────┘    └──────────┘    └────────────────┘

"Copilot        "Copilot         "Copilot         "Copilot runs
 follows our     runs our         has our           our development
 standards"      playbook"        expertise"        workflow"
```

---

### Common pitfalls

| Pitfall                              | Fix                                      |
|--------------------------------------|------------------------------------------|
| Giant `copilot-instructions.md`      | Split into `*.instructions.md` by area   |
| Skill that does too many things      | One skill = one verb ("deploy", "migrate")|
| Agent without tool restrictions      | Always specify `tools:` in frontmatter   |
| Prompt file with no examples         | Include at least one example invocation   |
| Skipping straight to sub-agents      | Master single agents first, then compose  |

---

### Your filesystem after this talk

```
your-repo/
├── .github/
│   ├── copilot-instructions.md
│   ├── instructions/
│   │   ├── api.instructions.md
│   │   ├── frontend.instructions.md
│   │   └── tests.instructions.md
│   ├── prompts/
│   │   ├── new-endpoint.prompt.md
│   │   └── fix-ci.prompt.md
│   ├── skills/
│   │   └── create-migration/
│   │       ├── SKILL.md
│   │       └── templates/
│   │           └── migration.sql
│   └── agents/
│       ├── planner.agent.md
│       ├── implementer.agent.md
│       └── reviewer.agent.md
├── AGENTS.md                          ← optional: cross-agent compat
├── package.json
└── src/
    └── ...
```

> **You don't need all five on day one.**
> Start with instructions. Add layers as your team's AI fluency grows.
