---
description: Update CLAUDE.md with important project information discovered during this session
---

# Update CLAUDE.md

Analyze the current conversation and update the project's CLAUDE.md with valuable information discovered during this session.

---

## Step 1: Review Current Context

Examine the conversation to identify:

1. **Architectural patterns** discovered or established
2. **Code conventions** that weren't documented
3. **Gotchas or pitfalls** encountered and solved
4. **Important file locations** that were hard to find
5. **Dependencies or integrations** and how they work
6. **Testing approaches** that work for this project
7. **Build/deployment quirks** discovered
8. **Common commands** that are useful but not documented
9. **Design decisions** made and their rationale

---

## Step 2: Read Existing CLAUDE.md

1. Read the project's `CLAUDE.md` file (if it exists)
2. Understand what's already documented
3. Identify gaps between existing docs and discovered knowledge
4. Note any outdated information that should be updated

---

## Step 3: Evaluate What to Add

For each piece of information, ask:

- **Is it project-specific?** (Don't add generic knowledge)
- **Will it save future time?** (Worth documenting if it took effort to figure out)
- **Is it stable?** (Don't document things likely to change soon)
- **Is it non-obvious?** (Don't document what's clear from code)

Prioritize:
- Things that caused confusion or errors
- Patterns that differ from common conventions
- Integration details that aren't in READMEs
- Commands with non-obvious flags or options

---

## Step 4: Make Surgical Updates

Update CLAUDE.md with:

1. **Minimal additions** - Only add what's truly useful
2. **Correct placement** - Put info in the right section
3. **Concise writing** - Brief, scannable bullet points
4. **No duplication** - Don't repeat what's already there
5. **Preserve structure** - Match existing formatting style

If CLAUDE.md doesn't exist, create one with:
```markdown
# Project Name

Brief description.

## Architecture

Key architectural decisions and patterns.

## Development

### Commands

Common commands for development.

### Conventions

Code conventions specific to this project.

## Notes

Important gotchas and non-obvious behaviors.
```

---

## Step 5: Summary

After updating, report:

1. **What was added** - List new sections/items
2. **What was updated** - List modified sections
3. **What was skipped** - Information considered but not added (and why)
