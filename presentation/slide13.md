# Bonus: Hooks

## Deterministic automation at agent lifecycle points

---

### What they are

Hooks are **shell commands** that execute at key points during an agent session.
Unlike instructions (which guide the AI), hooks run **your code** with
guaranteed outcomes — no matter how the agent is prompted.

> **Status:** Preview — VS Code 1.109.3+

---

### Why hooks matter

```
Without hooks:
  "Hey Copilot, please run prettier after editing files"
  → Sometimes it does. Sometimes it forgets. Non-deterministic.

With hooks:
  PostToolUse hook → prettier runs EVERY time a file is edited.
  → Deterministic. Guaranteed. No prompt required.
```

Instructions *suggest*. Hooks *enforce*.

---

### The eight lifecycle events

```
┌──────────────────────────────────────────────────────────────┐
│                    Agent Session Lifecycle                    │
│                                                              │
│  SessionStart ──→ UserPromptSubmit ──→ PreToolUse ──→ Tool   │
│       │                                     │         runs   │
│       │                                     │          │     │
│       │                               PostToolUse ◄────┘     │
│       │                                     │                │
│       │              PreCompact ◄───────────┘                │
│       │                                                      │
│       │          SubagentStart ──→ SubagentStop              │
│       │                                                      │
│       └─────────────────────────────────────────→ Stop       │
└──────────────────────────────────────────────────────────────┘
```

| Event              | Fires when…                            | Use case                         |
|--------------------|----------------------------------------|----------------------------------|
| `SessionStart`     | New agent session begins               | Inject project context           |
| `UserPromptSubmit` | User sends a prompt                    | Audit requests                   |
| `PreToolUse`       | Before any tool runs                   | Block dangerous operations       |
| `PostToolUse`      | After a tool completes                 | Auto-format, lint, log           |
| `PreCompact`       | Before context is compacted            | Export state before truncation   |
| `SubagentStart`    | Subagent is spawned                    | Track nested agent usage         |
| `SubagentStop`     | Subagent completes                     | Aggregate results                |
| `Stop`             | Agent session ends                     | Generate reports, cleanup        |

---

### Where hooks live

```
Workspace hooks (shared with team):
  .github/hooks/*.json

Claude-compatible locations:
  .claude/settings.json
  .claude/settings.local.json       ← local only, not committed

Personal hooks (your machine only):
  ~/.claude/settings.json
```

Workspace hooks take precedence over user hooks for the same event.

---

### Configuration format

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "type": "command",
        "command": "./scripts/validate-tool.sh",
        "timeout": 15
      }
    ],
    "PostToolUse": [
      {
        "type": "command",
        "command": "npx prettier --write \"$TOOL_INPUT_FILE_PATH\""
      }
    ]
  }
}
```

Each hook entry needs `type: "command"` plus:

| Field     | Required | Description                                |
|-----------|----------|--------------------------------------------|
| `command` | Yes      | Default command to run (cross-platform)    |
| `windows` | No       | Windows-specific override                  |
| `linux`   | No       | Linux-specific override                    |
| `osx`     | No       | macOS-specific override                    |
| `timeout` | No       | Seconds before timeout (default: 30)       |

---

### How hooks communicate

Hooks receive JSON on **stdin** and return JSON on **stdout**:

```
┌─────────┐     stdin (JSON)      ┌─────────────┐     stdout (JSON)     ┌─────────┐
│ VS Code │ ──────────────────→  │  Your hook   │ ──────────────────→  │ VS Code │
│         │  tool_name, input,   │   script     │  continue, decision, │         │
│         │  session context     │              │  messages, context   │         │
└─────────┘                      └─────────────┘                       └─────────┘
```

**Exit codes matter:**

| Exit code | Meaning                                   |
|-----------|-------------------------------------------|
| `0`       | Success — parse stdout as JSON            |
| `2`       | Blocking error — stop and show error      |
| Other     | Warning — show warning, continue          |

---

### Real-world scenario: auto-format + auto-lint on every edit

Your team wants **every file the agent touches** to be formatted and linted
automatically — no prompt needed, no chance of forgetting.

**.github/hooks/code-quality.json**
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "type": "command",
        "command": "npx prettier --write \"$TOOL_INPUT_FILE_PATH\"",
        "windows": "npx prettier --write \"%TOOL_INPUT_FILE_PATH%\""
      },
      {
        "type": "command",
        "command": "npx eslint --fix \"$TOOL_INPUT_FILE_PATH\"",
        "windows": "npx eslint --fix \"%TOOL_INPUT_FILE_PATH%\"",
        "timeout": 15
      }
    ]
  }
}
```

That's it. Two hooks, one file. Every time the agent creates or edits a file:

1. Prettier formats it
2. ESLint fixes what it can

```
Agent edits src/routes/users.ts
  │
  ├─→ PostToolUse hook #1: prettier --write  ✅ formatted
  ├─→ PostToolUse hook #2: eslint --fix      ✅ lint-clean
  │
  └─→ Agent continues with clean code
```

No instructions needed. No "please remember to format." It just happens.

---

### Hooks vs. Instructions — complementary, not competing

```
┌────────────────────────────────┬──────────────────────────────────┐
│         Instructions           │            Hooks                 │
├────────────────────────────────┼──────────────────────────────────┤
│ Non-deterministic (guidance)   │ Deterministic (guaranteed)       │
│ Influence AI decisions         │ Execute your code                │
│ "Please run tests"             │ Tests run. Period.               │
│ Applied to AI context          │ Run as shell commands            │
│ Can be overridden by prompts   │ Cannot be bypassed by prompts   │
└────────────────────────────────┴──────────────────────────────────┘
```

> **Rule of thumb:** Instructions tell the agent what to *think*.
> Hooks control what *actually happens*.

---

### Troubleshooting

- **Diagnostics:** Right-click Chat view → Diagnostics → look for hooks section
- **Output logs:** Output panel → "GitHub Copilot Chat Hooks" channel
- **Common fix:** Ensure scripts have execute permissions (`chmod +x`)
- **Configure via chat:** Type `/hooks` to open the interactive hooks UI

> See [Sources](sources.md) #7 for full VS Code hooks documentation.
