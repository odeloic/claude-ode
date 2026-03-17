# claude-ode

Loic's personal Claude Code plugin — commit workflows, Linear integration, React Native scaffolding, Figma-to-code, and guided implementation plans.

## Install

```bash
claude --plugin-dir ~/workspace/ai/claude-ode
```

Or add to your Claude Code settings:

```json
{
  "plugins": ["~/workspace/ai/claude-ode"]
}
```

## Commands

| Command | Description |
|---------|-------------|
| `/claude-ode:docs-commit` | Stage changes, generate a conventional commit message, and optionally create a PR |
| `/claude-ode:update-claude` | Analyze the session and update the project's `CLAUDE.md` with discovered knowledge |
| `/claude-ode:linear:up-next` | Create a branch and implement a Linear task with planning, execution, and quality checks |
| `/claude-ode:linear:make-issue` | Fetch a Linear issue by ID and implement it end-to-end |
| `/claude-ode:rn:make-component` | Create or extend a React Native/Expo component using best practices |
| `/claude-ode:rn:review-component` | Review React Native/Expo component changes against best practices |

## Skills

| Skill | Description |
|-------|-------------|
| `/claude-ode:showme` | Generate a guided step-by-step implementation plan (advisory only — doesn't write code) |
| `/claude-ode:figma-design` | Extract a Figma design via MCP and implement it as production-grade frontend code |

## License

MIT
