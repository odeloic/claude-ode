---
name: guide
description: Generate a beautiful visual HTML guide that helps the user understand a concept or solve a problem by pointing them to the right resources, mental models, and investigative steps — without writing the solution for them.
argument-hint: <what to understand or figure out>
user-invocable: true
metadata:
  author: loicishimwe
  version: "2.0.0"
---

# Guide — Visual Learning Guide

Generate a self-contained HTML page that guides the user toward understanding a concept or solving a problem, then open it in the browser.

## How It Works

1. **Clarify the question**: Restate what the user is trying to understand or accomplish. If ambiguous, ask one focused clarifying question before proceeding.
2. **Assess context**: Read relevant files or docs only to understand what the user is working with — language, framework, architecture, existing patterns.
3. **Generate the visual guide**: Build an HTML page with all the guide content and open it in the browser.

## Output Generation

After researching the codebase and understanding the problem, generate a **self-contained HTML file** with inline CSS and write it to a temp file using the Bash tool, then open it in the browser.

Use the following Bash command pattern:

```bash
GUIDE_FILE="/tmp/guide-$(date +%s).html"
cat > "$GUIDE_FILE" << 'HTMLEOF'
<!DOCTYPE html>
<html>...the full HTML guide...</html>
HTMLEOF
if command -v xdg-open &>/dev/null; then xdg-open "$GUIDE_FILE"
elif command -v open &>/dev/null; then open "$GUIDE_FILE"
fi
echo "Guide opened: $GUIDE_FILE"
```

### HTML Structure & Design

The HTML page MUST be a polished, visually rich document. Use this structure and adapt content to the specific topic:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Guide: [Topic Title]</title>
  <style>
    /* Modern, clean design system */
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    :root {
      --bg: #0f1117;
      --surface: #1a1d27;
      --surface-hover: #232733;
      --border: #2a2e3a;
      --text: #e4e4e7;
      --text-muted: #9ca3af;
      --accent: #818cf8;
      --accent-dim: rgba(129, 140, 248, 0.12);
      --success: #34d399;
      --success-dim: rgba(52, 211, 153, 0.12);
      --warning: #fbbf24;
      --warning-dim: rgba(251, 191, 36, 0.12);
      --danger: #f87171;
      --danger-dim: rgba(248, 113, 113, 0.12);
      --code-bg: #141620;
      --radius: 12px;
      --font-mono: 'SF Mono', 'Fira Code', 'JetBrains Mono', monospace;
      --font-sans: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
    }

    body {
      font-family: var(--font-sans);
      background: var(--bg);
      color: var(--text);
      line-height: 1.7;
      padding: 2rem;
      max-width: 900px;
      margin: 0 auto;
    }

    /* Header with gradient accent bar */
    .header {
      position: relative;
      padding: 2.5rem 2rem 2rem;
      margin-bottom: 2rem;
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      overflow: hidden;
    }
    .header::before {
      content: '';
      position: absolute;
      top: 0; left: 0; right: 0;
      height: 4px;
      background: linear-gradient(90deg, var(--accent), #c084fc, var(--accent));
    }
    .header .badge {
      display: inline-block;
      font-size: 0.7rem;
      font-weight: 600;
      text-transform: uppercase;
      letter-spacing: 0.1em;
      color: var(--accent);
      background: var(--accent-dim);
      padding: 0.25rem 0.75rem;
      border-radius: 999px;
      margin-bottom: 1rem;
    }
    .header h1 {
      font-size: 1.75rem;
      font-weight: 700;
      line-height: 1.3;
      margin-bottom: 0.5rem;
    }
    .header p {
      color: var(--text-muted);
      font-size: 0.95rem;
    }

    /* Section cards */
    .section {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 1.75rem 2rem;
      margin-bottom: 1.25rem;
      transition: border-color 0.2s;
    }
    .section:hover { border-color: var(--accent); }

    .section-icon {
      display: inline-flex;
      align-items: center;
      justify-content: center;
      width: 36px; height: 36px;
      border-radius: 10px;
      font-size: 1.1rem;
      margin-bottom: 0.75rem;
    }
    .section-icon.purple { background: var(--accent-dim); }
    .section-icon.green { background: var(--success-dim); }
    .section-icon.yellow { background: var(--warning-dim); }
    .section-icon.red { background: var(--danger-dim); }

    .section h2 {
      font-size: 1.15rem;
      font-weight: 600;
      margin-bottom: 0.75rem;
    }
    .section p, .section li {
      color: var(--text-muted);
      font-size: 0.9rem;
      line-height: 1.75;
    }
    .section ul, .section ol {
      padding-left: 1.25rem;
      margin-top: 0.5rem;
    }
    .section li { margin-bottom: 0.4rem; }
    .section li strong { color: var(--text); }

    /* Code blocks */
    pre {
      background: var(--code-bg);
      border: 1px solid var(--border);
      border-radius: 8px;
      padding: 1rem 1.25rem;
      overflow-x: auto;
      margin: 0.75rem 0;
      font-family: var(--font-mono);
      font-size: 0.82rem;
      line-height: 1.7;
      color: var(--text-muted);
    }
    code {
      font-family: var(--font-mono);
      font-size: 0.85em;
      background: var(--code-bg);
      padding: 0.15em 0.4em;
      border-radius: 4px;
      color: var(--accent);
    }
    pre code { background: none; padding: 0; color: inherit; }

    /* Callout boxes */
    .callout {
      display: flex;
      gap: 0.75rem;
      padding: 1rem 1.25rem;
      border-radius: 8px;
      margin: 0.75rem 0;
      font-size: 0.88rem;
      line-height: 1.6;
    }
    .callout.tip { background: var(--success-dim); border-left: 3px solid var(--success); }
    .callout.warning { background: var(--warning-dim); border-left: 3px solid var(--warning); }
    .callout.danger { background: var(--danger-dim); border-left: 3px solid var(--danger); }
    .callout-icon { flex-shrink: 0; font-size: 1.1rem; }

    /* Diagram / flow visualization */
    .flow {
      display: flex;
      align-items: center;
      gap: 0.5rem;
      flex-wrap: wrap;
      margin: 1rem 0;
    }
    .flow-step {
      background: var(--accent-dim);
      color: var(--accent);
      padding: 0.5rem 1rem;
      border-radius: 8px;
      font-size: 0.82rem;
      font-weight: 500;
      white-space: nowrap;
    }
    .flow-arrow { color: var(--text-muted); font-size: 0.8rem; }

    /* Links */
    a { color: var(--accent); text-decoration: none; }
    a:hover { text-decoration: underline; }

    /* Footer */
    .footer {
      text-align: center;
      color: var(--text-muted);
      font-size: 0.75rem;
      margin-top: 2rem;
      padding-top: 1.5rem;
      border-top: 1px solid var(--border);
    }
  </style>
</head>
<body>
  <div class="header">
    <div class="badge">Learning Guide</div>
    <h1>[Topic Title]</h1>
    <p>[One-sentence framing of the core problem or concept]</p>
  </div>

  <!-- MENTAL MODEL section -->
  <div class="section">
    <div class="section-icon purple">[icon]</div>
    <h2>Mental Model</h2>
    <p>[Explain the underlying concept in plain language]</p>
    <!-- Use .flow for visual flow diagrams when helpful -->
    <!-- Use .callout for important notes -->
    <!-- Use <pre><code> for code examples -->
  </div>

  <!-- WHERE TO LOOK section -->
  <div class="section">
    <div class="section-icon green">[icon]</div>
    <h2>Where to Look</h2>
    <!-- Codebase, docs, tooling subsections -->
  </div>

  <!-- KEY QUESTIONS section -->
  <div class="section">
    <div class="section-icon yellow">[icon]</div>
    <h2>Key Questions to Ask Yourself</h2>
    <ol>...</ol>
  </div>

  <!-- TRY THIS section -->
  <div class="section">
    <div class="section-icon red">[icon]</div>
    <h2>Try This</h2>
    <p>[Concrete experiment to run]</p>
    <pre><code>[command or code snippet]</code></pre>
  </div>

  <!-- WHEN YOU'RE READY section -->
  <div class="section">
    <div class="section-icon purple">[icon]</div>
    <h2>When You're Ready</h2>
    <p>[What a correct solution looks like at a high level]</p>
  </div>

  <div class="footer">Generated by /guide &mdash; claude-ode</div>
</body>
</html>
```

### Design Guidelines

- **Dark theme** with accent colors for visual hierarchy.
- Use **flow diagrams** (`.flow` with `.flow-step` and `.flow-arrow`) to visualize how concepts connect, data flows, or request lifecycles. Build these with HTML/CSS — no external dependencies.
- Use **callout boxes** (`.callout.tip`, `.callout.warning`, `.callout.danger`) for important notes, common mistakes, and gotchas.
- Use **code blocks** with syntax-appropriate content for file paths, commands, and code snippets.
- Use **icons/emoji** in `.section-icon` elements to make sections visually scannable.
- Keep the page **self-contained** — no external stylesheets, scripts, or fonts. Everything inline.
- Make the content **specific to the user's codebase** — reference actual file paths, function names, and patterns found during research.
- When a concept benefits from it, build **simple ASCII or CSS-based diagrams** to illustrate architecture, data flow, or component relationships.

### Content Guidelines

- ALWAYS read relevant files first to ground advice in the user's actual codebase — never give generic guidance when specific guidance is possible.
- Prefer linking to official docs over explaining things that are already well-documented. Your job is to point, not to parrot.
- When linking to docs, link to the *specific section* — not the landing page.
- Be language and framework agnostic in structure. Adapt examples and tooling suggestions to whatever stack the user is working in.
- Keep it concise. If a section isn't relevant (e.g., no codebase to investigate), skip it.
- Do NOT write the solution. Do NOT use Edit/Write tools for the user's code. This skill is advisory only — it generates a visual guide.
- If the user is stuck on something that `/showme` would handle better (they understand the concept but need precise implementation steps), suggest they use `/showme` instead.

## Terminal Output

After generating and opening the HTML file, output a brief confirmation to the terminal:

```
Visual guide generated and opened in browser.
File: /tmp/guide-XXXXX.html
```
