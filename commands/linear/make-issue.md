---
description: Fetch a Linear issue by ID and implement it with planning, execution, and quality checks
argument-hint: <issue-id>
---

# Linear Issue Implementation

**Issue ID:** `$ARGUMENTS`

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
3. Fetch issue comments using `mcp__linear__get_issue_comments` with issue ID `$ARGUMENTS`
   - Review all comments for additional context, clarifications, or requirements
   - Note any decisions or discussions that affect implementation
4. Display the issue details and key comment highlights to confirm correct issue

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

---

## Phase 4: Refactoring

After initial implementation, use the **code-simplifier** sub-agent to:

- Review for unnecessary complexity
- Suggest refactoring opportunities
- Ensure consistent patterns across the codebase
- Optimize performance where needed

Run:
```
Use the code-simplifier agent to review recent changes
```

---

## Phase 5: Quality Checks

Run project-specific quality checks. Look for these in the project's `CLAUDE.md` or configuration files:

1. **Lint** - Run linter (ESLint, Prettier, etc.)
2. **Type Check** - Validate types (`tsc --noEmit` for TypeScript)
3. **Tests** - Run test suite
4. **Build** - Ensure project builds successfully

If any check fails, fix the issues before considering the task complete.

---

## Phase 6: Summary

After implementation, provide a summary that includes:

1. **What was implemented**
   - Features added
   - User-facing changes

2. **Files created or modified**
   - List all touched files with brief descriptions

3. **Key technical decisions made**
   - Architecture choices
   - Trade-offs considered

4. **Any deviations from the original plan and why**
   - What changed during implementation
   - Reasons for changes

5. **Suggested manual testing steps**
   - How to verify the feature works
   - Edge cases to test
   - Expected behavior

6. **Linear Issue Update**
   - Suggest status update for the issue
   - Any comments to add to the issue
