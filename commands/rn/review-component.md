---
description: Review React Native/Expo component changes against best practices and suggest refactoring
---

# Review Component Changes

Review current git changes (staged and unstaged) for React Native/Expo components against installed skills and best practices.

---

## Phase 1: Detect Component Changes

1. Run `git diff --name-only HEAD` to get all modified files (staged + unstaged)
2. Filter for files matching:
   - `*.tsx` (component files)
   - `*.test.tsx` or `*.spec.tsx` (corresponding test files)
3. If no matching files found, report "No component changes detected" and exit
4. Display the list of component files to be reviewed

---

## Phase 2: Review Against Skills

For each changed component file, analyze against these skills:

### React Native Performance (vercel-react-native-skills)
Check for:
- List performance issues (FlashList usage, memoization, inline objects)
- Animation patterns (GPU properties, derived values)
- UI patterns (expo-image, Pressable, safe areas)
- State management (minimize subscriptions, dispatcher pattern)
- Rendering issues (text in Text components, falsy && conditionals)

### React Native Best Practices (react-native-best-practices)
Check for:
- FPS and frame drops
- Time to interactive (TTI) concerns
- Bundle size impact
- Memory leak patterns
- Bridge overhead issues
- Re-render optimizations

### Expo UI Patterns (expo-app-design:building-ui)
Check for:
- Expo Router navigation patterns
- Component structure and styling
- Animation implementations
- Native tab usage

### React Performance (vercel-react-best-practices)
Check for:
- Re-render patterns (memo, derived state, transitions)
- Rendering optimizations (conditional render, content visibility)
- Bundle concerns (barrel imports, dynamic imports)

---

## Phase 3: Refactoring Suggestions

Use the **code-simplifier** agent to analyze the changed components for:
- Unnecessary complexity
- Redundant code or abstractions
- Consistency with project patterns
- Clarity improvements (avoiding nested ternaries, dense one-liners)

---

## Phase 4: Summary Report

Provide a categorized summary:

### Critical Issues
- Performance problems that cause frame drops or jank
- Memory leaks or bridge blocking

### High Priority
- Missing memoization in lists
- Suboptimal animation patterns
- Incorrect component usage

### Medium Priority
- Style optimization opportunities
- State management improvements

### Low Priority
- Code style suggestions
- Minor refactoring opportunities

For each issue, provide:
- File and line reference
- Description of the issue
- Suggested fix with code example
