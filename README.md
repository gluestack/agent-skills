# Agent Skills

Agent skills for AI coding agents (Cursor, Claude Code, Cline, etc.). Skills are reusable instruction sets that help agents follow specific patterns and best practices.

## Available Skills

### gluestack-ui-v5

Enforces constrained, opinionated styling patterns for [gluestack-ui v5](https://gluestack.io/): semantic tokens, component props over className, copy-paste philosophy, compound sub-components, and Tailwind CSS v4 (NativeWind v5 / UniWind). Use when building or refactoring UI with gluestack-ui v5.

**Sub-skills structure for token efficiency:**
- `gluestack-ui-v5` - Main overview and core principles (249 lines)
- `gluestack-ui-v5:setup` - Installation, CLI init, NativeWind v5 & UniWind setup (485 lines)
- `gluestack-ui-v5:creating-components` - Component creation templates and recipes (878 lines)
- `gluestack-ui-v5:components` - Component patterns, props, compound components, icons (767 lines)
- `gluestack-ui-v5:styling` - Semantic tokens, dark mode, tva variants, style context (706 lines)
- `gluestack-ui-v5:variants` - tva variant creation, extending components, compound variants (887 lines)
- `gluestack-ui-v5:performance` - Cross-platform, memoization, reanimated, FlatList (569 lines)
- `gluestack-ui-v5:validation` - Validation checklist, anti-patterns, code review guidelines (574 lines)

### gluestack-ui-v4

Enforces constrained, opinionated styling patterns for [gluestack-ui v4](https://v4.gluestack.io/): semantic tokens, component usage, spacing scale, dark mode, and composable sub-components. Use when building or refactoring UI with gluestack-ui v4.

**Sub-skills structure for token efficiency:**
- `gluestack-ui-v4` - Main overview and core principles (162 lines)
- `gluestack-ui-v4:creating-components` - Component creation templates and recipes (624 lines)
- `gluestack-ui-v4:components` - Component patterns, props, compound components, icons (708 lines)
- `gluestack-ui-v4:styling` - Colors, spacing, dark mode, variants, tva (490 lines)
- `gluestack-ui-v4:performance` - Cross-platform, performance, best practices (552 lines)
- `gluestack-ui-v4:validation` - Validation checklist and anti-patterns (477 lines)

## Installation

Install all skills from this repo:

```bash
npx skills add gluestack/agent-skills
```

Install only a specific skill:

```bash
npx skills add gluestack/agent-skills --skill gluestack-ui-v5
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
