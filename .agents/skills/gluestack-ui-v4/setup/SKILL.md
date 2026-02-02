---
name: gluestack-ui-v4:setup
description: Guide for installing gluestack-ui v4 per the official Installation doc - CLI and Manual paths only. Follow https://v4.gluestack.io/ui/docs/home/getting-started/installation strictly.
---

# Gluestack UI v4 - Setup & Installation

This sub-skill follows the official **gluestack-ui v4** installation guide only. Nothing is followed from outside [Installation | gluestack-ui v4](https://v4.gluestack.io/ui/docs/home/getting-started/installation).

Use either **CLI** or **Manual** as defined on that page.

---

## CLI

### Step 1: Setup your project

- **New project:** Set up your project with [Next.js](https://nextjs.org/docs/getting-started/installation) or [Expo](https://docs.expo.dev/get-started/create-a-project/).
- **Existing project:** Skip this step.

### Prerequisites

Ensure these prerequisites are satisfied to use the gluestack-ui CLI:

| Package Name   | Supported Versions   | Upcoming Support |
|----------------|----------------------|------------------|
| next           | 13 <= versions <= 15 | -                |
| react-native   | versions >= 72.5      | -                |
| expo           | versions >= 50       | -                |
| node           | versions > 16        | -                |

### Step 2: Initialize

1. Go to your project root.
2. Run **init** to add **GluestackUIProvider** and essential components (**icon**, **overlay**, **toast**):

```bash
npx gluestack-ui init
```

3. Your project is then ready to use gluestack-ui components. To add components via CLI, use the **add** command or the [CLI guide](https://v4.gluestack.io/ui/docs/home/getting-started/cli). Example:

```bash
npx gluestack-ui add box
```

4. If you run into issues during CLI installation, use the **manual installation guide** (Manual tab on the same Installation page).

---

## Manual

For installation without the CLI, follow the **Manual** steps on the official Installation page:

**Manual installation:** [https://v4.gluestack.io/ui/docs/home/getting-started/installation](https://v4.gluestack.io/ui/docs/home/getting-started/installation) (switch to the **Manual** tab on that page).

Do not follow manual steps from any other source; only use what is defined there for v4.

---

## Common issues

**Expo app stuck in `tailwindcss(ios) rebuilding...` while running `expo start`**

- Cause: Project directory name contains spaces (e.g. `Expo App`).
- Fix: Rename the directory so it has no spaces (e.g. `Expo-App`).

---

## Reference

- **Installation (v4):** [https://v4.gluestack.io/ui/docs/home/getting-started/installation](https://v4.gluestack.io/ui/docs/home/getting-started/installation)
- **CLI guide:** [https://v4.gluestack.io/ui/docs/home/getting-started/cli](https://v4.gluestack.io/ui/docs/home/getting-started/cli)
