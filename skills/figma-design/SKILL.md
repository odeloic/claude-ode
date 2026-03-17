---
name: figma-design
description: Extract a Figma design and implement it as production-grade frontend code. Use when the user provides a Figma URL and wants it implemented as web code.
argument-hint: <figma-url> [additional context]
user-invocable: true
metadata:
  author: loicishimwe
  version: "1.0.0"
---

# Figma Design — Extract & Implement

Takes one or more Figma URLs, extracts the full design context, and delegates to /frontend-design for a high-quality implementation.

## Step 1: Parse the Figma URL(s)

Extract `fileKey` and `nodeId` from each Figma URL provided:
- `figma.com/design/:fileKey/:fileName?node-id=:nodeId` → convert `-` to `:` in `nodeId`
- `figma.com/design/:fileKey/branch/:branchKey/...` → use `branchKey` as `fileKey`
- `figma.com/board/:fileKey/...` → FigJam file, use `get_figjam`
- `figma.com/make/:makeFileKey/...` → use `makeFileKey`

## Step 2: Extract Design Context

Use the Figma MCP tools to extract as much context as needed. Run these in parallel where possible:

### 2a. Primary design data
Call `get_design_context` with the `fileKey` and `nodeId`. This returns:
- Generated reference code (React + Tailwind)
- A screenshot of the node
- Code Connect snippets (if mapped)
- Design annotations and hints

### 2b. Visual variables
Call `get_variable_defs` with the `fileKey` to extract:
- Color tokens and palettes (hex, RGBA, opacity)
- Typography tokens (font families, sizes, weights, line heights, letter spacing)
- Spacing, radius, shadow, and effect tokens
- Theme/mode definitions (light/dark variants)

### 2c. Screenshot for visual validation
Call `get_screenshot` to capture the full visual appearance of the node. This is the ground truth — always refer to it when interpreting ambiguous layout or styling.

### 2d. Metadata (when needed)
Call `get_metadata` when you need to understand component hierarchy, layer structure, or resolve ambiguity about how parts of the design relate.

## Step 3: Interpret the Design

Carefully analyze all extracted data before writing any prompt. Map design decisions to code decisions:

**Colors:**
- Map Figma color variables to CSS custom properties: `--color-name: #hex`
- Identify the primary, secondary, accent, background, surface, and text colors
- Note opacity variants (e.g., `rgba(...)` vs solid)
- Distinguish semantic roles: brand colors, neutral grays, status colors (success, error, warning)

**Typography:**
- Extract every font family used — note exact names for Google Fonts / system font fallback selection
- Map font sizes to a scale (e.g., 12/14/16/20/24/32/48px)
- Note font weights, line heights, letter spacing, and text transforms
- Identify heading vs body vs caption vs label hierarchies

**Spacing & Layout:**
- Extract padding, margin, and gap values — look for a base unit (4px, 8px, etc.)
- Note border radii (sharp vs rounded vs pill)
- Identify grid structure: columns, gutters, breakpoints if visible
- Note any fixed widths/heights, aspect ratios, or overflow behaviors

**Visual Effects:**
- Extract box shadows (offset, blur, spread, color, inset)
- Note gradients (direction, stops, colors)
- Note blur/backdrop effects, overlays, and layering

**Interactions & States:**
- Look for hover, active, disabled, focused variants in the design
- Note any animation or transition cues in design annotations

**Components:**
- List all distinct UI components visible (buttons, cards, inputs, navbars, etc.)
- Note component variants (size, color, state) from Code Connect or layer names

## Step 4: Construct the Implementation Prompt

Synthesize all extracted context into a precise, rich prompt for the `/frontend-design` skill. Include:

```
Implement this design extracted from Figma.

## Visual Reference
[Screenshot description — layout, sections, key visual features]

## Design Tokens

### Colors
--color-primary: #...
--color-background: #...
[... all extracted tokens]

### Typography
Font families: [exact names]
Scale: [sizes and weights]

### Spacing
Base unit: [N]px
[key spacing values]

### Effects
[shadows, gradients, radii]

## Layout & Structure
[Describe the overall layout, sections, hierarchy]

## Components
[List each component with its variants and states]

## Design Intent
[Note the overall aesthetic — the tone, mood, and style visible in the design]
[Note any explicit annotations or designer instructions from the Figma file]

## Technical Constraints
[Any constraints from the user's context or the project]
```

## Step 5: Delegate to /frontend-design

Use the Skill tool to invoke `frontend-design` with the constructed prompt.

The `/frontend-design` skill will:
- Make bold aesthetic decisions **anchored to the extracted Figma tokens and style**
- Implement production-grade, visually faithful code
- Add motion, texture, and refinement on top of the design intent

**Important:** Pass the Figma design tokens and visual reference as ground truth. The frontend-design skill should honor the color system, typography, and spacing exactly — creative latitude applies only to implementation details (animations, micro-interactions, code structure) not to the core design fidelity.

## Guidelines

- Always fetch the screenshot — it is the ground truth for resolving ambiguity
- Extract variables before writing the prompt; never guess at colors or fonts
- If multiple Figma URLs are provided, extract all of them in parallel, then synthesize into a single coherent prompt
- When Code Connect snippets are returned, reference the mapped codebase components in the implementation prompt
- When designer annotations are present, treat them as hard requirements
- If `get_design_context` returns a link to component documentation, fetch it before writing the prompt
