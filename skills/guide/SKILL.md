---
name: guide
description: Generate a visual HTML learning guide that helps the user understand a concept or solve a problem. Opens an interactive page in the browser with mental models, flow diagrams, codebase pointers, and diagnostic questions.
argument-hint: <what to understand or figure out>
user-invocable: true
allowed-tools: Bash(python *)
metadata:
  author: loicishimwe
  version: "2.0.0"
---

# Guide — Visual Learning Guide

Generate an interactive HTML learning guide and open it in the browser.

## Workflow

1. **Clarify the question**: Restate what the user is trying to understand. If ambiguous, ask one focused clarifying question.
2. **Research the codebase**: Read relevant files to ground advice in the user's actual code — never give generic guidance when specific guidance is possible.
3. **Build the JSON data**: Assemble a JSON object with all guide content (see schema below).
4. **Run the script**: Pass the JSON to the bundled generator script.

## Running the script

```bash
python ${CLAUDE_SKILL_DIR}/scripts/generate.py '<json-data>'
```

The script generates a self-contained HTML file in `/tmp/` and opens it in the browser.

## JSON schema

```json
{
  "title": "Topic title",
  "summary": "One-sentence framing of the core problem or concept",
  "mental_model": {
    "explanation": "Plain-language explanation of the underlying concept",
    "misconceptions": ["Common mistake 1", "Common mistake 2"],
    "code_example": "Optional code snippet illustrating the concept"
  },
  "flow_steps": ["Step A", "Step B", "Step C"],
  "where_to_look": {
    "codebase": ["File or module to investigate and why"],
    "docs": [
      {"url": "https://...", "text": "Specific doc section"},
      "Or plain text pointer"
    ],
    "tooling": ["CLI command or devtools technique"]
  },
  "questions": [
    "Diagnostic question to guide the user's thinking"
  ],
  "experiment": {
    "description": "Small concrete experiment to validate understanding",
    "command": "Command or code to run"
  },
  "when_ready": "What a correct solution looks like at a high level",
  "callouts": [
    {"type": "tip|warning|danger", "text": "Important note"}
  ]
}
```

All fields are optional — skip sections that aren't relevant. But always include `title`, `summary`, and at least `mental_model` or `where_to_look`.

## Content guidelines

- ALWAYS read relevant files first to ground advice in the user's actual codebase.
- Prefer linking to official docs over re-explaining well-documented topics. Link to the *specific section*, not the landing page.
- Use `flow_steps` to visualize how concepts connect, data flows, or request lifecycles.
- Use `callouts` for gotchas, common mistakes, and important caveats.
- Include `code_example` in the mental model when a snippet clarifies the concept.
- Be language and framework agnostic in structure. Adapt to whatever stack the user is working in.
- Do NOT write the solution. Do NOT use Edit/Write tools for the user's code. This skill is advisory only.
- If the user needs precise implementation steps rather than conceptual understanding, suggest `/showme` instead.
