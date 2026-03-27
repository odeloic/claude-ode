# claude-ode

Loic's personal Claude Code plugin — commit workflows, Linear integration, React Native scaffolding, Figma-to-code, and guided implementation plans.

## Install

```shell
/plugin marketplace add odeloic/claude-ode
/plugin install claude-ode@claude-ode
```

Then run `/reload-plugins` to activate.

**Local development** — load without installing:

```bash
claude --plugin-dir ~/workspace/ai/claude-ode
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
| `/claude-ode:guide` | Generate a visual HTML learning guide with mental models, resources, and investigative steps — opens in browser |
| `/claude-ode:showme` | Generate a visual HTML step-by-step implementation tutorial with diff-highlighted code — opens in browser |
| `/claude-ode:figma-design` | Extract a Figma design via MCP and implement it as production-grade frontend code |

## License

MIT
