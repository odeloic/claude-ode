---
name: guide
description: Guide the user toward understanding a concept or solving a problem by pointing them to the right resources, mental models, and investigative steps — without writing the solution for them. Use when the user wants to learn and build understanding rather than receive a ready-made implementation.
argument-hint: <what to understand or figure out>
user-invocable: true
metadata:
  author: loicishimwe
  version: "1.0.0"
---

# Help — Learn by Understanding

Guide the user toward solving a problem themselves by building the right mental model, pointing to authoritative resources, and suggesting investigative steps — not by handing them the answer.

## How It Works

1. **Clarify the question**: Restate what the user is trying to understand or accomplish. If ambiguous, ask one focused clarifying question before proceeding.
2. **Assess context**: Read relevant files or docs only to understand what the user is working with — language, framework, architecture, existing patterns.
3. **Produce the guide**: Output a structured learning path using the format below.

## Output Format

### What You're Solving

One or two sentences framing the core problem or concept. Name the domain clearly (e.g., "This is a routing problem", "This is about state management", "This is an authentication flow").

### Mental Model

Explain the underlying concept in plain language. Use analogies or diagrams where helpful. The goal is for the user to understand *why* the solution works, not just *what* to type. Cover:

- What is actually happening under the hood
- How the relevant parts connect to each other
- Common misconceptions or traps

### Where to Look

Point the user to the specific places they should investigate — both in their own codebase and in external resources.

**In your codebase:**
- Which files or modules are relevant and why
- What to look for in each (a pattern, a function signature, a config value)
- How to trace the flow (e.g., "start at the route handler and follow the call chain to the data layer")

**In the docs:**
- Direct links to the specific documentation sections that cover this topic (not just the homepage — link to the exact page/section)
- Which parts of the docs to focus on and which to skip
- Note if the docs have known gaps or if a GitHub issue / discussion thread is more useful

**In the tooling:**
- Relevant CLI commands, REPL experiments, or browser devtools techniques to explore the behavior hands-on
- How to read error messages or logs that relate to this problem

### Key Questions to Ask Yourself

A short list (3–5) of diagnostic questions the user should work through to arrive at their own solution. These should guide thinking, not lead to a single answer.

Example style:
- "What happens if you remove X — does Y still work? That tells you whether they're coupled."
- "Check the return type of `foo()` — is it what the next function in the chain expects?"
- "Look at how the existing `Bar` implementation handles this — can you follow the same pattern?"

### Try This

One small, concrete experiment the user can run right now to test their understanding. Not a full implementation — just enough to validate or invalidate their mental model. Examples:

- "Add a `console.log` / `print` / `dd()` at the entry point of the handler and confirm the data shape you're receiving."
- "Try calling the API endpoint directly with curl and compare the response to what the UI shows."
- "Comment out the middleware and see what changes — that will tell you what it's responsible for."

### When You're Ready

Brief description of what a correct solution looks like at a high level — enough for the user to know when they've got it, without spelling out the implementation. Think "acceptance criteria", not "code to write".

---

## Guidelines

- ALWAYS read relevant files first to ground advice in the user's actual codebase — never give generic guidance when specific guidance is possible.
- Prefer linking to official docs over explaining things that are already well-documented. Your job is to point, not to parrot.
- When linking to docs, link to the *specific section* — not the landing page. If you need to search to find the right page, do so.
- Be language and framework agnostic in structure. Adapt examples and tooling suggestions to whatever stack the user is working in.
- Keep it concise. If a section isn't relevant (e.g., no codebase to investigate), skip it.
- Do NOT write the solution. Do NOT use Edit/Write tools. This skill is advisory only.
- If the user is stuck on something that `/showme` would handle better (they understand the concept but need precise implementation steps), suggest they use `/showme` instead.
