# Skills

## Portable, composable abilities any agent can learn

---

### What they are

A Skill is a **self-contained capability** — a bundle of instructions, example
code, and supporting scripts that teaches an agent how to do one thing well.
Skills are portable across agents and repos. Think of them as plugins for
your AI.

> **Status:** GA (General Availability) — shipped with VS Code 1.109 (Jan 2026)

---

### Anatomy of a skill

Every skill lives in its own folder with a `SKILL.md` file at the root:

```
.github/skills/
├── deploy-preview/
│   ├── SKILL.md                  ← required: entry point
│   ├── deploy.sh                 ← supporting script
│   └── examples/
│       └── preview-config.yaml   ← reference files
├── create-migration/
│   ├── SKILL.md
│   ├── migrate.ts
│   └── templates/
│       ├── create-table.sql
│       └── alter-table.sql
└── generate-openapi/
    ├── SKILL.md
    └── openapi-template.yaml
```

---

### The `SKILL.md` file

```markdown
---
name: 'deploy-preview'
description: >
  Deploy a preview environment for the current branch.
  Creates a short-lived environment with a unique URL
  for testing before merge.
---

# Deploy Preview

## When to use
Invoke this skill when a user wants to preview changes in a
live environment before merging a pull request.

## Prerequisites
- The branch must have all CI checks passing
- Docker must be available in the terminal

## Steps
1. Read the current branch name via `git branch --show-current`
2. Build the Docker image: `docker build -t preview-$BRANCH .`
3. Run the deploy script: `bash deploy.sh $BRANCH`
4. Return the preview URL to the user

## Example invocation
"Deploy a preview of my current branch so I can share it with QA"
```

---

### YAML frontmatter — what's required

```yaml
---
name: 'skill-name'           # Required — unique identifier
description: 'What it does'  # Required — used for skill discovery
---
```

That's it. Two fields. Copilot uses the description to decide when to
suggest the skill.

---

### Where skills live

```
Project skills (shared with team):
  .github/skills/{skill-name}/SKILL.md

Personal skills (your machine only):
  ~/.copilot/skills/{skill-name}/SKILL.md
```

Project skills travel with the repo → everyone gets them.
Personal skills are yours → portable across all repos.

---

### Skills in the CLI

```bash
# List available skills
copilot skills list

# Get details on a specific skill
copilot skills info deploy-preview

# Invoke directly
copilot /deploy-preview
```

---

### What makes a good skill

```mermaid
graph LR
    A[Self-contained] --> B[One clear purpose]
    B --> C[Declarative steps]
    C --> D[Includes examples]
    D --> E[Portable across repos]

    style A fill:#1a73e8,color:#fff
    style B fill:#1a73e8,color:#fff
    style C fill:#1a73e8,color:#fff
    style D fill:#1a73e8,color:#fff
    style E fill:#1a73e8,color:#fff
```

| Good skill                          | Bad skill                              |
|-------------------------------------|----------------------------------------|
| "Deploy a preview environment"      | "Do everything for deployment"         |
| "Generate an OpenAPI spec from code"| "Handle all API documentation"         |
| "Create a DB migration from diff"   | "Manage the database"                  |
| Focused, testable, composable       | Vague, sprawling, monolithic           |

---

### Skills vs. Instructions — the crucial difference

```
Instructions:  "Always use Zod for validation"
               → passive, always-on, no action

Skills:        "Here's HOW to create a Zod schema for a new endpoint,
                including the template, the test, and the registration"
               → active, invokable, produces output
```

> **Rule of thumb:** Instructions set the rules.
> Skills teach the moves.
