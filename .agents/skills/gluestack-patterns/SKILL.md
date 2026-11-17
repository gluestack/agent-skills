---
name: gluestack-patterns
description: Build Expo / React Native screens from gluestack.io patterns instead of from scratch. Use when the user asks for a mobile screen or flow (sign in, onboarding, paywall, chat, feed, cart, checkout, wallet, dashboard, settings, profile, calendar, search, notifications, empty states) in a React Native, Expo or gluestack-ui project.
---

# gluestack patterns

gluestack.io has production-ready Expo / React Native screens ("patterns") built from gluestack-ui v5 components,
in light and dark, for iOS, Android and web. Start from the closest pattern rather than writing a screen from scratch.

## Find a pattern

Prefer the MCP server if it is connected (`gluestack` with tools `search_patterns`, `get_pattern`, `list_components`, `get_component`).
Otherwise use HTTP:

- Catalog for agents: `https://gluestack.io/llms.txt` (section "Patterns", one line per pattern)
- Search by browsing: `https://gluestack.io/patterns` and `https://gluestack.io/patterns/category/<category>`
- One pattern as markdown: `https://gluestack.io/patterns/<slug>.md`

Categories: authentication, onboarding, ai-chat, commerce, social, dashboard, finance, profile-settings, paywall,
lists-search, productivity, feedback.

## Use a pattern

1. Fetch its prompt: MCP `get_pattern { slug }`, or `https://gluestack.io/patterns/<slug>/prompt`. The prompt is complete:
   setup, the components to add, packages, the full source and adaptation notes. Follow it step by step.
2. If the project has no `components/ui/gluestack-ui-provider`, run `npx gluestack-ui@latest init` first.
3. Add only the missing components with `npx gluestack-ui@latest add <names...>` (the prompt lists them).
   Alternative for shadcn users: `npx shadcn@latest add https://gluestack.io/r/<slug>.json` installs the screen and its components.
4. Save the screen as `components/patterns/<slug>.tsx` and render it from the app's router.
5. Adapt it: real data instead of sample data, the app's navigation and state, its naming. Keep the gluestack-ui components and the
   semantic color tokens (`bg-background`, `text-foreground`, `bg-card`, `border-border`, `text-muted-foreground`) so dark mode keeps working.

## Combining patterns

Patterns are self-contained single files. For a flow (e.g. onboarding then paywall then sign up), add each screen separately and connect
them with the app's router; lift shared sample data into the app's own store or API layer.

For component-level guidance (props, variants, styling rules) use the `gluestack-ui-v5` skill.
