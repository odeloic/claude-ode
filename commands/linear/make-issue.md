---
description: Fetch a Linear issue by ID and implement it with planning, execution, and quality checks
argument-hint: <issue-id>
---

# Linear Issue Implementation

**Issue ID:** `$ARGUMENTS`

---

## Pre-Implementation Checklist

Before starting work:
- ✅ Requirements are clear and unambiguous
- ✅ Acceptance criteria are defined (or inferrable from description)
- ✅ No blocking dependencies or related issues
- ✅ No conflicts with ongoing work by others

**If requirements are unclear:** Post a clarifying question in the Linear issue comments before proceeding. Do not guess or proceed with assumptions.

---

## Phase 0: Fetch Issue Details

Use the Linear MCP to retrieve the issue details:

1. Call `mcp__linear__get_issue` with issue ID `$ARGUMENTS`
2. Extract:
   - **Title** - Issue title
   - **Description** - Full issue description
   - **Priority** - Issue priority level
   - **Labels** - Any attached labels
   - **Assignee** - Who it's assigned to
   - **Parent/Sub-issues** - Related issues
3. Fetch issue comments using `mcp__linear__list_comments` with issue ID `$ARGUMENTS`
   - Review all comments for additional context, clarifications, or requirements
   - Note any decisions or discussions that affect implementation
4. Display the issue details and key comment highlights to confirm correct issue
5. **Pre-flight check**: Verify the issue is not already in progress by another team member or marked as blocked

---

## Phase 1: Branch Setup

Create a feature branch based on the issue:

1. Run `git rev-parse --abbrev-ref HEAD` to check current branch
2. Generate branch name from issue ID and title (e.g., `feature/ABC-123-issue-title`)
3. If branch exists: `git checkout <branch-name>`
4. If branch doesn't exist: `git checkout -b <branch-name>`
5. Update branch with latest from main: `git pull origin main`
6. Confirm you're on the correct branch before proceeding

---

## Phase 2: Planning (Before Writing Any Code)

### 2.1 Analyze Issue Requirements
Based on the fetched issue details:
- Identify acceptance criteria from description
- Note any specific requirements or constraints
- Check for linked issues or dependencies

### 2.2 Understand the Project
Examine the codebase to understand:
- Project structure and architecture
- Existing patterns and conventions
- Relevant configuration files (package.json, tsconfig.json, etc.)
- Any project-specific documentation (README.md, CLAUDE.md)

### 2.3 Check Related Tasks/Dependencies
Review any related Linear issues linked to this one. Check if there are blocking tasks or related features that need coordination.

### 2.4 Create Detailed Implementation Plan

For complex issues (multiple files, architectural decisions, or unclear requirements), use `EnterPlanMode` to create a structured plan that can be reviewed and approved before implementation begins.

Generate a comprehensive plan with these sections:

#### Overview
- One paragraph summary of what will be implemented
- Key user-facing functionality

#### Acceptance Criteria
- Specific, testable requirements from the issue
- Edge cases and error states to handle
- Success metrics

#### Technical Approach
- Architecture decisions
- File structure changes
- State management approach (if applicable)
- Database/API changes (if applicable)

#### Component/Module Breakdown
```
- [ ] path/to/file - Description of changes
- [ ] path/to/file - Description of changes
```

#### Data Models
- TypeScript interfaces / types needed
- Schema changes (if applicable)
- Migration strategy if needed

#### Testing Strategy
- Unit tests for business logic
- Integration/component tests
- Edge cases to cover

---

## Phase 3: Implementation

### Code Standards

- Write clean, well-documented code
- Use proper typing throughout (TypeScript preferred)
- Add inline comments for complex logic only
- Create reusable components/modules where appropriate
- Follow existing patterns in the codebase

### Implementation Order

1. **Data layer first** - Define data structures, schemas, types
2. **State/business logic second** - Set up state management, services
3. **UI/presentation last** - Build the interface
4. **Write tests alongside each layer**

### Workflow During Implementation

- Use `TodoWrite` to break complex implementation into discrete steps—update task status as you complete each
- Commit changes periodically with meaningful messages (e.g., after each logical component)
- Use the `/docs-commit` skill to generate commit messages that reference the Linear issue
- Push intermediate commits if working on a long task (enables recovery and team visibility)

---

## Phase 4: Refactoring

After initial implementation, use the **/simplify** skill to:

- Review for unnecessary complexity
- Suggest refactoring opportunities
- Ensure consistent patterns across the codebase
- Optimize performance where needed

Run:
```
/simplify
```

---

## Phase 5: Quality Checks

Run all project quality checks. **Always check the project's configuration first** to see what tools are actually configured:

### Find Configured Tools

- **TypeScript/Node**: Check `package.json` scripts, `.eslintrc`, `tsconfig.json`, `prettier.config.js`
- **Python**: Check `pyproject.toml`, `setup.cfg`, `ruff.toml`, `.flake8`, `tox.ini`
- **Any project**: Check `CLAUDE.md` for project-specific guidance

### Run Configured Commands

Once you know what's configured:

```bash
# TypeScript projects (example - check your package.json for actual commands)
npm run lint
npm run type-check
npm test
npm run build
```

```bash
# Python projects (example - check your configuration for actual commands)
python -m pytest
python -m ruff check .  # or whatever linter is configured
python manage.py test   # if Django
python manage.py runserver
```

### Quality Checklist

1. ✅ **Lint** - No linting errors or warnings (fix or suppress with explanation)
2. ✅ **Type Check** - No type errors (if applicable)
3. ✅ **Tests** - All existing tests pass + new tests added for features
4. ✅ **Build** - Project builds/runs without errors
5. ✅ **Manual smoke test** - Basic functionality works as expected

If any check fails, fix the issues and re-run before proceeding. Do not skip checks.

---

## Phase 6: Summary & Linear Update

After all quality checks pass, create a summary and update the Linear issue:

### Summary Content

1. **What was implemented**
   - Features added
   - User-facing changes
   - Any limitations or known issues

2. **Files created or modified**
   - List all touched files with brief descriptions

3. **Key technical decisions made**
   - Architecture choices
   - Trade-offs considered
   - Why certain approaches were chosen

4. **Any deviations from the original plan and why**
   - What changed during implementation
   - Reasons for changes

5. **Manual testing steps for verification**
   - How to verify the feature works
   - Edge cases to test
   - Expected behavior

### Update Linear Issue

Use `mcp__linear__save_issue` to:
- Change status to "In Review" or "Done" (depending on your workflow)
- Add a comment with the summary and any relevant details
- Link the PR (once created) if applicable
- Update any related sub-issues or blocking relationships

---

## Phase 7: Push & Create Pull Request

### Push to Remote

```bash
git push -u origin <branch-name>
```

This sets the upstream branch on first push, making future pushes simpler.

### Create Pull Request

1. Open the repository URL and navigate to create a new PR
2. Set **base branch** to `main` (or the default branch)
3. Set **head branch** to your feature branch
4. Use the issue title as the PR title (or a more descriptive variation)
5. In the PR description:
   - Reference the Linear issue: `Closes LIN-XXX` (or `Fixes`, `Resolves`)
   - Include key implementation details from Phase 6 summary
   - Mention any manual testing steps
   - Tag reviewers if applicable
6. Create the PR and request review

### PR Checklist

- ✅ Base branch is `main` (correct target)
- ✅ All commits have meaningful messages
- ✅ All CI checks pass (linting, tests, build)
- ✅ Description references the Linear issue
- ✅ Reviewers are assigned (if required by your workflow)

Once the PR is merged, the Linear issue can be closed by the reviewer or automated workflows.
