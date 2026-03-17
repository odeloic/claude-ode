---
name: showme
description: Generate a guided technical implementation plan for a code change the user wants to implement themselves. Use when the user wants to understand how to implement something hands-on rather than having code written for them.
argument-hint: <what to implement>
user-invocable: true
metadata:
  author: loicishimwe
  version: "1.0.0"
---

# Show Me — Guided Technical Implementation

Generate a clear, step-by-step technical implementation guide so the user can write the code themselves.

## How It Works

1. **Understand the request**: Parse the user's prompt to understand what they want to implement.
2. **Research the codebase**: Read relevant files to understand the current state — architecture, patterns, conventions, and dependencies.
3. **Produce the guide**: Output a structured implementation walkthrough.

## Output Format

For each step in the implementation, provide:

### Step N: [Brief title]

**File:** `path/to/file.py` (new file | edit existing)
**Location:** Line XX / after function `foo()` / inside class `Bar` / end of file

**What to do:** Brief explanation of the change and why.

**Code to write:**
```language
// The exact code the user should add or modify
```

**Key details:**
- Any imports needed
- Gotchas or edge cases to watch for
- How this connects to the next step

---

## Guidelines

- ALWAYS read the relevant files first — never guess at file contents, line numbers, or existing code.
- Be precise about WHERE code goes: reference line numbers, surrounding functions, class names, or other anchors the user can search for.
- Show the minimal diff — only the code that needs to change, with enough surrounding context (2-3 lines) so the user can locate the insertion point.
- Respect existing patterns: match the project's naming conventions, import style, error handling, and structure.
- Order steps logically — dependencies first (e.g., models before services, services before routes).
- After all steps, include a **Verification** section: how to test/run/check that the implementation works.
- Keep explanations concise but don't skip the "why" — the user is learning by doing.
- Do NOT write the code for the user using Edit/Write tools. This skill is advisory only.
