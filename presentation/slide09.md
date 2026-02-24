# The Multi-Agent Reality

## VS Code as the orchestration hub

---

### VS Code 1.109 — the multi-agent hub

Since January 2026, VS Code natively hosts **multiple agent backends**
side by side:

```
┌─────────────────────────────────────────────────────┐
│                    VS Code 1.109                     │
│                                                      │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐          │
│   │  Copilot  │  │  Claude   │  │  Custom  │  ...    │
│   │  Agent    │  │  Agent    │  │  Agents  │          │
│  └─────┬────┘  └─────┬────┘  └─────┬────┘          │
│        │              │              │               │
│        └──────────────┴──────────────┘               │
│                       │                              │
│              Shared workspace                        │
│              Shared terminal                         │
│              Shared file system                      │
│              Shared instruction files                │
└─────────────────────────────────────────────────────┘
```

- **Local agents:** Available to all Copilot subscribers
- **Cloud agents (background):** Require Copilot Pro+ or Enterprise

---

### Cross-agent instruction files

VS Code auto-detects multiple instruction formats:

```
your-repo/
├── .github/
│   └── copilot-instructions.md     ← Copilot reads this
├── AGENTS.md                        ← VS Code agents read this
├── CLAUDE.md                        ← Claude agent reads this
└── src/...
```

Write once, get compliance across ecosystems.

---

### Claude Code — a quick look for context

Claude Code uses the same conceptual model with different file conventions:

```
your-repo/
├── CLAUDE.md                       ← instructions (like copilot-instructions.md)
├── .claude/
│   ├── rules/                      ← modular instruction files
│   │   ├── security.md
│   │   └── testing.md
│   ├── agents/                     ← sub-agent definitions
│   │   ├── implementer.md
│   │   └── reviewer.md
│   └── skills/                     ← skill definitions
│       └── deploy/
│           └── SKILL.md
└── src/...
```

| Concept        | Copilot (VS Code)            | Claude Code               |
|----------------|------------------------------|---------------------------|
| Instructions   | `copilot-instructions.md`    | `CLAUDE.md`               |
| Modular rules  | `*.instructions.md`          | `.claude/rules/*.md`      |
| Skills         | `.github/skills/`            | `.claude/skills/`         |
| Agents         | `.github/agents/*.agent.md`  | `.claude/agents/*.md`     |
| Sub-agents     | Same files, agent-spawned    | Same files, agent-spawned |
| Hooks          | `.github/hooks/*.json`       | `.claude/hooks/`          |
| Memory         | *(session-based)*            | `.claude/memory.json`     |

---

### The portable pattern

The *concepts* are identical. The *file paths* differ.

```
┌─────────────────────────────────────────────────┐
│              Conceptual Layer                    │
│                                                  │
│   Instructions → Skills → Agents → Sub-Agents   │
│                                                  │
├────────────────────┬────────────────────────────┤
│   Copilot          │   Claude Code              │
│                    │                             │
│  .github/          │  .claude/                   │
│  copilot-*.md      │  CLAUDE.md                  │
│  *.instructions.md │  rules/*.md                 │
│  *.agent.md        │  agents/*.md                │
│  SKILL.md          │  SKILL.md  (same!)          │
└────────────────────┴────────────────────────────┘
```

> **The mental model transfers.** Learn it once for Copilot,
> and you already know 90% of Claude Code's architecture.
> `SKILL.md` is even the same file format across both.
