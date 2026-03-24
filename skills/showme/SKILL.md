---
name: showme
description: Generate a beautiful visual HTML tutorial with step-by-step implementation instructions the user can follow to write the code themselves. Opens in browser.
argument-hint: <what to implement>
user-invocable: true
metadata:
  author: loicishimwe
  version: "2.0.0"
---

# Show Me — Visual Implementation Tutorial

Generate a self-contained HTML page with a step-by-step visual implementation tutorial, then open it in the browser.

## How It Works

1. **Understand the request**: Parse the user's prompt to understand what they want to implement.
2. **Research the codebase**: Read relevant files to understand the current state — architecture, patterns, conventions, and dependencies.
3. **Generate the visual tutorial**: Build an HTML page with all implementation steps and open it in the browser.

## Output Generation

After researching the codebase and understanding the implementation needed, generate a **self-contained HTML file** with inline CSS and write it to a temp file using the Bash tool, then open it in the browser.

Use the following Bash command pattern:

```bash
TUTORIAL_FILE="/tmp/showme-$(date +%s).html"
cat > "$TUTORIAL_FILE" << 'HTMLEOF'
<!DOCTYPE html>
<html>...the full HTML tutorial...</html>
HTMLEOF
if command -v xdg-open &>/dev/null; then xdg-open "$TUTORIAL_FILE"
elif command -v open &>/dev/null; then open "$TUTORIAL_FILE"
fi
echo "Tutorial opened: $TUTORIAL_FILE"
```

### HTML Structure & Design

The HTML page MUST be a polished, visually rich step-by-step tutorial. Use this structure:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Show Me: [Implementation Title]</title>
  <style>
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

    /* Header */
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
      background: linear-gradient(90deg, var(--success), #34d3e0, var(--accent));
    }
    .header .badge {
      display: inline-block;
      font-size: 0.7rem;
      font-weight: 600;
      text-transform: uppercase;
      letter-spacing: 0.1em;
      color: var(--success);
      background: var(--success-dim);
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
    .header p { color: var(--text-muted); font-size: 0.95rem; }
    .header .file-list {
      display: flex;
      flex-wrap: wrap;
      gap: 0.4rem;
      margin-top: 1rem;
    }
    .header .file-tag {
      font-family: var(--font-mono);
      font-size: 0.75rem;
      background: var(--code-bg);
      border: 1px solid var(--border);
      padding: 0.2rem 0.6rem;
      border-radius: 6px;
      color: var(--text-muted);
    }

    /* Step cards */
    .step {
      position: relative;
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 1.75rem 2rem;
      margin-bottom: 1.25rem;
      transition: border-color 0.2s;
    }
    .step:hover { border-color: var(--accent); }

    .step-number {
      position: absolute;
      top: -0.75rem;
      left: 1.5rem;
      display: inline-flex;
      align-items: center;
      justify-content: center;
      width: 28px; height: 28px;
      background: var(--accent);
      color: var(--bg);
      font-size: 0.8rem;
      font-weight: 700;
      border-radius: 50%;
    }

    .step-header {
      display: flex;
      justify-content: space-between;
      align-items: flex-start;
      margin-bottom: 0.75rem;
      gap: 1rem;
    }
    .step h2 {
      font-size: 1.1rem;
      font-weight: 600;
    }
    .step .file-badge {
      flex-shrink: 0;
      font-family: var(--font-mono);
      font-size: 0.75rem;
      background: var(--accent-dim);
      color: var(--accent);
      padding: 0.25rem 0.75rem;
      border-radius: 6px;
    }
    .step .location {
      font-size: 0.82rem;
      color: var(--text-muted);
      margin-bottom: 0.75rem;
      font-family: var(--font-mono);
    }
    .step p, .step li {
      color: var(--text-muted);
      font-size: 0.9rem;
      line-height: 1.75;
    }
    .step ul, .step ol {
      padding-left: 1.25rem;
      margin-top: 0.5rem;
    }
    .step li { margin-bottom: 0.4rem; }
    .step li strong { color: var(--text); }

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
      position: relative;
    }
    pre .lang-label {
      position: absolute;
      top: 0.5rem;
      right: 0.75rem;
      font-size: 0.65rem;
      text-transform: uppercase;
      letter-spacing: 0.05em;
      color: var(--text-muted);
      opacity: 0.5;
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

    /* Diff-style highlights */
    .line-add {
      background: rgba(52, 211, 153, 0.1);
      display: block;
      margin: 0 -1.25rem;
      padding: 0 1.25rem;
      border-left: 3px solid var(--success);
    }
    .line-remove {
      background: rgba(248, 113, 113, 0.1);
      display: block;
      margin: 0 -1.25rem;
      padding: 0 1.25rem;
      border-left: 3px solid var(--danger);
      text-decoration: line-through;
      opacity: 0.6;
    }
    .line-context {
      display: block;
      margin: 0 -1.25rem;
      padding: 0 1.25rem;
      opacity: 0.5;
    }

    /* Callout boxes */
    .callout {
      display: flex;
      gap: 0.75rem;
      padding: 1rem 1.25rem;
      border-radius: 8px;
      margin: 0.75rem 0;
      font-size: 0.85rem;
      line-height: 1.6;
    }
    .callout.tip { background: var(--success-dim); border-left: 3px solid var(--success); }
    .callout.warning { background: var(--warning-dim); border-left: 3px solid var(--warning); }
    .callout.danger { background: var(--danger-dim); border-left: 3px solid var(--danger); }
    .callout-icon { flex-shrink: 0; font-size: 1.1rem; }

    /* Connection lines between steps */
    .connector {
      display: flex;
      justify-content: center;
      padding: 0.25rem 0;
    }
    .connector .line {
      width: 2px;
      height: 20px;
      background: var(--border);
      border-radius: 1px;
    }

    /* Verification section */
    .verification {
      background: var(--surface);
      border: 2px solid var(--success);
      border-radius: var(--radius);
      padding: 1.75rem 2rem;
      margin-top: 1.25rem;
    }
    .verification h2 {
      color: var(--success);
      font-size: 1.1rem;
      font-weight: 600;
      margin-bottom: 0.75rem;
      display: flex;
      align-items: center;
      gap: 0.5rem;
    }
    .verification p, .verification li {
      color: var(--text-muted);
      font-size: 0.9rem;
      line-height: 1.75;
    }
    .verification ul, .verification ol {
      padding-left: 1.25rem;
      margin-top: 0.5rem;
    }
    .verification li { margin-bottom: 0.4rem; }

    /* Progress bar */
    .progress {
      display: flex;
      gap: 0.35rem;
      margin-top: 1rem;
    }
    .progress-dot {
      width: 8px; height: 8px;
      border-radius: 50%;
      background: var(--border);
    }
    .progress-dot.active { background: var(--accent); }

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
    <div class="badge">Implementation Tutorial</div>
    <h1>[Implementation Title]</h1>
    <p>[Brief description of what we're building and why]</p>
    <div class="file-list">
      <span class="file-tag">path/to/file1.ts</span>
      <span class="file-tag">path/to/file2.ts</span>
      <!-- List all files that will be touched -->
    </div>
  </div>

  <!-- Repeat for each step -->
  <div class="step">
    <div class="step-number">1</div>
    <div class="step-header">
      <h2>[Step Title]</h2>
      <span class="file-badge">path/to/file.ts</span>
    </div>
    <div class="location">Line XX &middot; after function foo() &middot; edit existing</div>
    <p>[What to do and why]</p>
    <pre><code><span class="line-context">// existing surrounding code</span>
<span class="line-add">+ new code to add</span>
<span class="line-context">// existing surrounding code</span></code></pre>
    <div class="callout tip">
      <span class="callout-icon">tip icon</span>
      <span>[Gotcha, import needed, or how this connects to the next step]</span>
    </div>
  </div>

  <div class="connector"><div class="line"></div></div>

  <!-- More steps... -->

  <div class="verification">
    <h2>checkmark icon Verification</h2>
    <p>How to test that the implementation works:</p>
    <ol>
      <li>[Test step 1]</li>
      <li>[Test step 2]</li>
    </ol>
    <pre><code>[test commands to run]</code></pre>
  </div>

  <div class="footer">Generated by /showme &mdash; claude-ode</div>
</body>
</html>
```

### Design Guidelines

- **Dark theme** with step-numbered cards for clear visual progression.
- Use **diff-style highlighting** (`.line-add`, `.line-remove`, `.line-context`) in code blocks to show exactly what changes — green for additions, red for removals, dimmed for context.
- Use **connector lines** between steps to show the implementation flow.
- Show a **file badge** on each step so the user always knows which file they're editing.
- Include **location hints** (line number, surrounding function, class) in monospace below each step title.
- Use **callout boxes** for gotchas, required imports, and edge cases.
- The **header** should list all files that will be touched as tags for a quick overview.
- The **verification section** at the end should have a distinct green border to stand out.
- Keep everything **self-contained** — no external stylesheets, scripts, or fonts.
- Use **emoji/unicode** for icons (e.g., checkmark for verification, lightbulb for tips, warning sign for warnings).

### Content Guidelines

- ALWAYS read the relevant files first — never guess at file contents, line numbers, or existing code.
- Be precise about WHERE code goes: reference line numbers, surrounding functions, class names, or other anchors.
- Show the minimal diff — only the code that needs to change, with enough surrounding context (2-3 lines) so the user can locate the insertion point.
- Respect existing patterns: match the project's naming conventions, import style, error handling, and structure.
- Order steps logically — dependencies first (e.g., models before services, services before routes).
- Always include a **Verification** section with concrete test commands or checks.
- Keep explanations concise but don't skip the "why" — the user is learning by doing.
- Do NOT write the code for the user using Edit/Write tools for their project. This skill generates a visual tutorial only.

## Terminal Output

After generating and opening the HTML file, output a brief confirmation to the terminal:

```
Visual tutorial generated and opened in browser.
File: /tmp/showme-XXXXX.html
```
