---
description: Create a branch and implement a Linear task with planning, execution, and quality checks
argument-hint: <branch-name> <task-description>
---

# Linear Task Implementation

**Branch:** `$1`
**Task:** $ARGUMENTS

---

## Phase 0: Branch Setup

Before doing anything else, ensure the correct branch is checked out and up to date:

1. Run `git rev-parse --abbrev-ref HEAD` to check current branch
2. If not on `$1`:
   - If branch `$1` exists: `git checkout $1`
   - If branch doesn't exist: `git checkout -b $1`
3. Update branch with latest from main: `git pull origin main`
4. Confirm you're on the correct branch before proceeding

---

## Phase 1: Planning (Before Writing Any Code)

### 1.1 Read Task Details
Review and understand the task description provided:

> $ARGUMENTS

### 1.2 Understand the Project
Examine the codebase to understand:
- Project structure and architecture
- Existing patterns and conventions
- Relevant configuration files (package.json, tsconfig.json, etc.)
- Any project-specific documentation (README.md, CLAUDE.md)

### 1.3 Check Related Tasks/Dependencies
Review any related Linear tasks or dependencies mentioned in the task description. Check if there are blocking tasks or related features that need coordination.

### 1.4 Create Detailed Implementation Plan

Generate a comprehensive plan with these sections:

#### Overview
- One paragraph summary of what will be implemented
- Key user-facing functionality

#### Acceptance Criteria
- Specific, testable requirements from the task
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

## Phase 2: Implementation

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

## Phase 3: Refactoring

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

## Phase 4: Quality Checks

Run project-specific quality checks. Look for these in the project's `CLAUDE.md` or configuration files:

1. **Lint** - Run linter (ESLint, Prettier, etc.)
2. **Type Check** - Validate types (`tsc --noEmit` for TypeScript)
3. **Tests** - Run test suite
4. **Build** - Ensure project builds successfully

If any check fails, fix the issues before considering the task complete.

---

## Phase 5: Summary

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
