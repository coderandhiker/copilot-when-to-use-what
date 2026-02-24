---
name: 'presentation-viewer'
description: 'Generate a single-file HTML slideshow viewer from a folder of markdown slides. Produces a self-contained viewer.html with dark theme, Mermaid diagram support, keyboard navigation, and a slide picker.'
---

# Presentation Viewer

Build a **single self-contained `viewer.html`** from a folder of markdown slide files.

## When to use this skill

The user has a folder of numbered markdown files (e.g. `slide00.md`, `slide01.md`, …) and wants a browser-based slide deck generated from them.

## Inputs

1. **Slide folder** — path to the directory containing markdown slides (default: `presentation/`).
2. **Output path** — where to write `viewer.html` (default: project root).
3. **Presentation title** — used for the HTML `<title>` tag, extracted from the first `# heading` in slide 00 if not specified.

## Steps

### 1. Discover slides

- List the slide folder.
- Collect all `.md` files, sorted in natural order (slide00, slide01, … slide99, then any non-numbered files like `sources.md` last).
- Read every file's full content.

### 2. Extract titles for the slide picker

For each slide, derive a short title:

- Use the first `# heading` text in the file.
- If index is known, prefix with the number: `"0 — Title"`, `"1 — The Problem"`, etc.
- For non-numbered files, use the heading alone: `"Sources & Further Reading"`.

### 3. Generate `viewer.html`

Produce a single HTML file following the **exact template** in `template.html` (in this skill folder). The only things that change between presentations are:

| Placeholder | What to insert |
|---|---|
| `{{TITLE}}` | Presentation title for the `<title>` tag |
| `{{SLIDES_JS}}` | One `slides.push(\`…\`);` call per slide, with the raw markdown embedded as a JS template literal (backtick-delimited). Escape any backticks in the markdown as `\\\`` and any `${` as `\${`. |
| `{{TITLES_JS}}` | The `const titles = [...]` array of picker labels. |

**Do NOT modify the CSS, the JavaScript engine, or the CDN imports.** The template is the source of truth.

### 4. Verify

After creating the file, confirm:

- The file is valid HTML (no unclosed tags).
- Every slide's markdown is present in the JS array.
- The `titles` array length matches the `slides` array length.

## Rules

- **One file.** Everything goes in `viewer.html` — no external CSS, no build step.
- **CDN only.** Use `marked.js` and `mermaid.js` from jsDelivr (already in template).
- **Escape carefully.** Markdown content goes inside JS backtick strings. Any literal backticks or `${` sequences in the markdown must be escaped.
- **Preserve order.** Slides appear in filename sort order.
- **Dark theme.** The template uses a GitHub-dark palette. Do not change it.
- **Mermaid fix included.** The CSS forces `overflow: visible` on Mermaid `foreignObject` elements to prevent text clipping inside diagram nodes. Do not remove this.
