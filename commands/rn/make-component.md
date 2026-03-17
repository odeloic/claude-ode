---
description: Create or extend a React Native/Expo component using best practices
arguments:
  - name: description
    description: Description of the component to create, or path/name of existing component to extend
    required: true
---

# Create React Native Component

Create a new React Native/Expo component or extend an existing one, following best practices from installed skills.

**Input**: $ARGUMENTS

---

## Phase 1: Analyze Context

### Check for Existing Component Reference

1. **If input contains a file path** (e.g., `./components/Button.tsx`, `src/ui/Card`):
   - Read the existing component file
   - Analyze its structure, props, styles, and patterns
   - Determine if extending, refactoring, or creating a variant

2. **If input mentions a component name** (e.g., "like UserCard", "extend Button", "based on ProfileHeader"):
   - Search the project for matching component files
   - Read and analyze the referenced component(s)
   - Extract patterns to follow or extend

3. **If creating a new component**:
   - Scan project structure to find component directories
   - Identify existing patterns (naming, file structure, exports)
   - Look for shared utilities, hooks, styles, and types

### Project Context Discovery

1. Run `find . -name "*.tsx" -path "*/components/*" | head -20` to discover component structure
2. Check for shared resources:
   - `hooks/` - Custom hooks to reuse
   - `utils/` or `lib/` - Utility functions
   - `types/` - Shared TypeScript types
   - `styles/` or `theme/` - Shared styles, colors, spacing
   - `constants/` - App constants
3. Read 1-2 similar existing components to match project conventions

---

## Phase 2: Plan Component Structure

Based on the description and project context, determine:
1. Component name and file location (following project conventions)
2. Props interface and types (reuse existing types where applicable)
3. State requirements
4. Existing components to compose with or extend
5. Existing hooks and utilities to leverage
6. Navigation integration (if applicable)

---

## Phase 2: Implement with Best Practices

Build the component following these skills:

### React Native Performance (vercel-react-native-skills)
Apply:
- Use FlashList for any lists (not FlatList)
- Memoize callbacks with useCallback
- Avoid inline objects in props
- Use GPU-friendly animation properties (transform, opacity)
- Use expo-image instead of Image
- Use Pressable instead of TouchableOpacity
- Handle safe areas properly
- Wrap text in Text components
- Use ternary instead of && for conditional rendering

### React Native Best Practices (react-native-best-practices)
Apply:
- Optimize for 60 FPS
- Minimize JS thread blocking
- Avoid bridge overhead where possible
- Prevent memory leaks (cleanup useEffect)
- Use React.memo for pure components
- Optimize re-renders with proper dependency arrays

### Expo UI Patterns (expo-app-design:building-ui)
Apply:
- Follow Expo Router patterns for navigation
- Use proper component structure
- Implement animations with Reanimated
- Follow styling conventions

### React Performance (vercel-react-best-practices)
Apply:
- Derive state instead of syncing
- Use transitions for non-urgent updates
- Avoid barrel imports
- Lazy load heavy components

---

## Phase 3: Create or Modify Component Files

### If Extending Existing Component
- Preserve existing functionality
- Add new props/features as requested
- Maintain backwards compatibility if needed
- Update types to reflect changes

### If Creating New Component
Generate the following files:

### Main Component File
- TypeScript with proper types (reuse existing project types)
- Props interface exported
- Import and compose with existing components where applicable
- Use existing hooks and utilities from the project
- Memoized if pure
- Proper cleanup in effects
- Follow project naming and export conventions

### Styles (if needed)
- Use existing theme/style system if present
- StyleSheet.create for static styles
- Responsive considerations

### Test File (optional)
- Basic render test
- Props validation test

---

## Phase 4: Refactor with Code Simplifier

Use the **code-simplifier** agent to review and refactor:
- Remove unnecessary complexity
- Eliminate redundant abstractions
- Improve code clarity
- Ensure consistency with project patterns
- Simplify nested ternaries or dense logic

---

## Phase 5: Final Output

Provide:
1. Complete component code (new or updated)
2. Usage example showing integration with existing project components
3. Props documentation (highlight new/changed props if extending)
4. Any additional setup needed (dependencies, navigation config)
5. If extending: summary of changes made to existing component
