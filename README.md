# Agents, Sub-Agents, Prompts, Instructions & Skills

Presentation materials for a February 2026 talk on customizing GitHub Copilot
(Agent Mode in VS Code, CLI, and SDK) — with cross-references to Claude Code
for conceptual continuity.

## Slide outline

| Slide | Topic |
|-------|-------|
| 0 | Title |
| 1 | The Problem — why these terms confuse people |
| 2 | The Mental Model — persistence × scope |
| 3 | Instructions |
| 4 | Prompt Files |
| 5 | Skills |
| 6 | Agents |
| 7 | Sub-Agents |
| 8 | The Spectrum — full comparison & decision flow |
| 9 | The Multi-Agent Reality — VS Code as hub |
| 10 | Practical Operating Model — five-step plan |
| 11 | Handoffs |
| 12 | Key Takeaways |
| 13 | Sources & Further Reading |

## Key concepts covered

- **Instructions** — `copilot-instructions.md`, `*.instructions.md`, `AGENTS.md`
- **Prompt Files** — `.prompt.md` with variables, tool/agent references
- **Skills** — `SKILL.md` bundles (GA as of VS Code 1.109)
- **Agents** — `.agent.md` personas with tools and model preferences
- **Sub-Agents** — isolated delegation between agents
- **Handoffs** — guided user-in-the-loop workflows

## Rendering

The slides use standard Markdown with Mermaid diagrams. They render well in:

- GitHub's markdown viewer (Mermaid supported natively)
- VS Code with a Markdown preview extension
- Any static site generator that supports Mermaid (Docusaurus, MkDocs, etc.)

## Accuracy

All citations were verified against live documentation as of **February 24, 2026**. AI work moves FAST - if you see something that looks incorrect, it may not be February 24, 2026 anymore :) Source links are collected in [`presentation/slide13.md`](presentation/slide13.md).

## License

[MIT](LICENSE)
