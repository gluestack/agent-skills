# Agent Skills

Agent skills for AI coding agents (Cursor, Claude Code, Cline, etc.). Skills are reusable instruction sets that help agents follow specific patterns and best practices.

## Available Skills

### gluestack-ui-v4

Enforces constrained, opinionated styling patterns for [gluestack-ui v4](https://v4.gluestack.io/): semantic tokens, component usage, spacing scale, dark mode, and composable sub-components. Use when building or refactoring UI with gluestack-ui v4.

## Installation

Install all skills from this repo:

```bash
npx skills add gluestack/agent-skills
```

Install only the gluestack-ui-v4 skill:

```bash
npx skills add gluestack/agent-skills --skill gluestack-ui-v4
```

After installation, skills are available to your agent and will be used when relevant tasks are detected.

## Showing on skills.sh

The [skills.sh](https://skills.sh/) directory lists skills and ranks them by **install telemetry**. For this repo’s skills to appear there:

1. **Push the repo to GitHub** and keep it **public** (e.g. `github.com/gluestack/agent-skills`).
2. **Install at least once** so the skill is registered and counted:
   ```bash
   npx skills add gluestack/agent-skills
   ```
3. New skills and repos can take a short time to show on the leaderboard after the first installs.

If the repo is only local or private, it will not be installable via `npx skills add` and will not appear on skills.sh.

## License

MIT
