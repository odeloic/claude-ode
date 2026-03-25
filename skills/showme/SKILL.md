---
name: showme
description: Generate a visual HTML step-by-step implementation tutorial with diff-highlighted code. Opens an interactive page in the browser showing exactly what to change and where.
argument-hint: <what to implement>
user-invocable: true
allowed-tools: Bash(python3 *), Bash(python *)
metadata:
  author: loicishimwe
  version: "2.0.0"
---

# Show Me — Visual Implementation Tutorial

Generate an interactive HTML tutorial with step-by-step implementation instructions and open it in the browser.

## Workflow

1. **Understand the request**: Parse what the user wants to implement.
2. **Research the codebase**: Read relevant files to understand architecture, patterns, conventions, and dependencies. Never guess at file contents or line numbers.
3. **Build the JSON data**: Assemble a JSON object with all tutorial steps (see schema below).
4. **Run the script**: Pass the JSON to the bundled generator script.

## Running the script

Pass JSON via a heredoc to stdin in a **single** `python3` command. This avoids shell escaping issues with multiline strings and matches the `allowed-tools` pattern for auto-approval.

```bash
python3 ${CLAUDE_SKILL_DIR}/scripts/generate.py <<'SHOWME_EOF'
<json-data>
SHOWME_EOF
```

The script accepts JSON from stdin or as a CLI argument. It generates a self-contained HTML file in `/tmp/` and opens it in the browser.

## JSON schema

```json
{
  "title": "What we're building",
  "summary": "Brief description of the implementation and why",
  "files": ["path/to/file1.ts", "path/to/file2.ts"],
  "steps": [
    {
      "title": "Step title",
      "file": "path/to/file.ts",
      "location": "Line XX · after function foo() · edit existing",
      "description": "What to do and why",
      "code": "The exact code to add or modify",
      "lang": "typescript",
      "diff": [
        {"type": "context", "text": "existing surrounding code"},
        {"type": "add", "text": "new code to add"},
        {"type": "remove", "text": "old code to remove"},
        {"type": "context", "text": "existing surrounding code"}
      ],
      "notes": [
        {"type": "tip", "text": "Import needed or gotcha to watch for"},
        {"type": "warning", "text": "Edge case to handle"}
      ]
    }
  ],
  "verification": {
    "description": "How to test that the implementation works",
    "steps": ["Run the test suite", "Check the output"],
    "command": "npm test"
  }
}
```

For each step, use either `code` (plain code block) or `diff` (diff-style with add/remove/context lines). Prefer `diff` when modifying existing code — it shows exactly what changes.

## Content guidelines

- ALWAYS read the relevant files first — never guess at file contents, line numbers, or existing code.
- Be precise about WHERE code goes: reference line numbers, surrounding functions, class names, or other anchors.
- Show the minimal diff — only the code that needs to change, with enough surrounding context (2-3 lines).
- Respect existing patterns: match the project's naming conventions, import style, error handling, and structure.
- Order steps logically — dependencies first (e.g., models before services, services before routes).
- Always include a `verification` section with concrete test commands or checks.
- Keep explanations concise but don't skip the "why" — the user is learning by doing.
- List all files that will be touched in the top-level `files` array for a quick overview.
- Do NOT write the code for the user using Edit/Write tools. This skill generates a visual tutorial only.
