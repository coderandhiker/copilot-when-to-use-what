# Key Takeaways

## Five concepts, five sentences

---

### The one-liner for each

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│  Instructions   "Always do this"         → your repo's laws      │
│                                                                  │
│  Prompt Files   "When I ask, do this"    → saved recipes         │
│                                                                  │
│  Skills         "Here's how to do this"  → teachable abilities   │
│                                                                  │
│  Agents         "You are this person"    → autonomous personas   │
│                                                                  │
│  Sub-Agents     "Delegate this part"     → isolated delegation   │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

### The three things to remember

**1. They're layers, not alternatives**

```
Instructions  ⊂  Prompt Files  ⊂  Skills  ⊂  Agents  ⊂  Sub-Agents
     ↑                                                        ↑
  foundation                                             orchestration
```

You use all five — each at the right level.

---

**2. Start simple, add complexity only when needed**

```
Week 1:  copilot-instructions.md                ← start here
Week 2:  + path-specific *.instructions.md
Week 3:  + .prompt.md files for common tasks
Month 2: + SKILL.md for repeatable capabilities
Month 3: + .agent.md for specialist personas
         + sub-agents for orchestration
```

---

**3. The mental model is portable**

```
Same concepts, different file paths:

  Copilot:      .github/copilot-instructions.md
  Claude Code:  CLAUDE.md
  VS Code:      AGENTS.md

  Copilot:      .github/skills/X/SKILL.md
  Claude Code:  .claude/skills/X/SKILL.md

  Copilot:      .github/agents/X.agent.md
  Claude Code:  .claude/agents/X.md
```

Learn the concepts once → apply them everywhere.

---

### Go build something

```
 ┌──────────────────────────────────────────────┐
 │                                               │
 │   mkdir -p .github/instructions               │
 │   touch .github/copilot-instructions.md       │
 │                                               │
 │   # That's it. You've started.                │
 │                                               │
 └──────────────────────────────────────────────┘
```

> **The best AI customization is the one your team actually commits to the repo.**

---

*Thank you*
