---
name: gluestack-ui-v4:setup
description: Guide for initializing gluestack-ui v4 in a project and adding components - covers CLI usage, installation, configuration, and adding individual components.
---

# Gluestack UI v4 - Setup & Installation

This sub-skill helps you initialize gluestack-ui v4 in a project and add components using the official CLI tool.

## Prerequisites

Before initializing gluestack-ui, verify:

### Required

| Requirement | Minimum Version |
|-------------|-----------------|
| Node.js | ≥ 16 |
| React Native | ≥ 72.5 |
| Expo | ≥ 50 |
| Next.js | 13-15 |

### Project Requirements
- [ ] package.json exists in the project root
- [ ] Project is one of: Expo, React Native CLI, or Next.js
- [ ] Project directory name has no spaces (Expo limitation)

## Quick Start

### Step 1: Check if Already Initialized

Before initializing, check if gluestack-ui is already set up:

```bash
# Check for GluestackUIProvider
find . -name "gluestack-ui-provider" -type d 2>/dev/null | grep -v node_modules

# Check for components directory
ls -la components/ui/ 2>/dev/null
```

### Step 2: Initialize Gluestack UI

Run the initialization command at your project root:

```bash
npx gluestack-ui init
```

**What this does:**
- Detects your project type (Expo, Next.js, or React Native CLI)
- Installs required dependencies automatically
- Creates/updates configuration files:
  - `tailwind.config.js` (or `.ts`)
  - `metro.config.js` (Expo/RN)
  - `babel.config.js` (Expo/RN)
  - `postcss.config.js` (Next.js)
  - `next.config.js` (Next.js)
  - `global.css` or `globals.css`
  - `tsconfig.json`
  - `nativewind-env.d.ts`
- Creates `components/ui/gluestack-ui-provider/` directory
- Adds provider dependencies (overlay, toast, icon)
- Modifies layout files to add GluestackUIProvider wrapper

**CLI Options:**

```bash
# Specify package manager
npx gluestack-ui init --use-npm
npx gluestack-ui init --use-yarn
npx gluestack-ui init --use-pnpm
npx gluestack-ui init --use-bun

# Specify custom components path (default: components/ui)
npx gluestack-ui init --path src/components/ui

# Template only (skip dependency installation)
npx gluestack-ui init --template-only
```

### Step 3: Add Components

After initialization, add components as needed:

```bash
# Add a single component
npx gluestack-ui add button

# Add multiple components
npx gluestack-ui add button input card

# Add all available components
npx gluestack-ui add --all

# Add components with options
npx gluestack-ui add button --use-npm
npx gluestack-ui add button --path src/components/ui
```

## Dependencies Installed by CLI

The CLI automatically installs the correct dependencies based on your project type.

### Expo Projects

**Dependencies:**
```json
{
  "nativewind": "^4.1.23",
  "react-native-safe-area-context": "^5.6.1",
  "react-native-reanimated": "~4.1.0",
  "react-native-worklets": "^0.5.1",
  "react-aria": "^3.33.0",
  "@expo/html-elements": "^0.10.1",
  "tailwind-variants": "^0.1.20",
  "@legendapp/motion": "^2.3.0",
  "react-native-svg": "^15.13.0",
  "react-stately": "^3.39.0",
  "@gluestack-ui/core": "^3.0.10",
  "@gluestack-ui/utils": "^3.0.11"
}
```

**Dev Dependencies:**
```json
{
  "babel-plugin-module-resolver": "^5.0.0",
  "tailwindcss": "^3.4.17",
  "prettier-plugin-tailwindcss": "^0.5.11"
}
```

### React Native CLI Projects

**Dependencies:**
```json
{
  "nativewind": "^4.1.23",
  "react-native-safe-area-context": "^5.6.1",
  "react-aria": "^3.33.0",
  "@expo/html-elements": "^0.10.1",
  "tailwind-variants": "^0.1.20",
  "@legendapp/motion": "^2.3.0",
  "react-native-svg": "^15.13.0",
  "react-stately": "^3.39.0",
  "react-native-reanimated": "~4.1.0",
  "react-native-worklets": "^0.5.1",
  "@gluestack-ui/core": "^3.0.10",
  "@gluestack-ui/utils": "^3.0.11"
}
```

**Dev Dependencies:**
```json
{
  "babel-plugin-module-resolver": "^5.0.0",
  "tailwindcss": "^3.4.17",
  "prettier-plugin-tailwindcss": "^0.5.11"
}
```

**Post-install for React Native CLI:**
```bash
cd ios && pod install && cd ..
```

### Next.js Projects

**Dependencies:**
```json
{
  "react-native-web": "^0.19.12",
  "nativewind": "^4.1.23",
  "react-aria": "^3.33.0",
  "@expo/html-elements": "^0.10.1",
  "tailwind-variants": "^0.1.20",
  "@legendapp/motion": "^2.3.0",
  "react-native-svg": "^15.13.0",
  "dom-helpers": "^5.2.1",
  "react-stately": "^3.39.0",
  "@gluestack-ui/core": "^3.0.10",
  "@gluestack-ui/utils": "^3.0.11",
  "@gluestack/ui-next-adapter": "^3.0.3",
  "react-native-safe-area-context": "^5.6.1",
  "react-native-reanimated": "~4.1.0",
  "react-native-worklets": "^0.5.1"
}
```

**Dev Dependencies:**
```json
{
  "@types/react-native": "0.72.8",
  "tailwindcss": "^3.4.17",
  "autoprefixer": "^10.4.21",
  "postcss": "^8.5.4",
  "@react-native/assets-registry": "^0.79.3"
}
```

## Configuration Files Created

### 1. tailwind.config.js

The CLI creates a complete tailwind configuration with:
- NativeWind preset
- Dark mode support (`class` strategy)
- Color system using CSS variables (primary, secondary, error, success, warning, info, typography, outline, background, indicator)
- Custom font families
- Custom shadows (hard and soft shadows)
- Extended spacing

**Example structure:**
```javascript
module.exports = {
  darkMode: 'class',
  content: [
    './app/**/*.{js,jsx,ts,tsx}',
    './components/**/*.{js,jsx,ts,tsx}',
  ],
  presets: [require('nativewind/preset')],
  theme: {
    extend: {
      colors: {
        primary: {
          0: 'rgb(var(--color-primary-0)/<alpha-value>)',
          50: 'rgb(var(--color-primary-50)/<alpha-value>)',
          // ... 100-950
        },
        // secondary, tertiary, error, success, warning, info, typography, outline, background, indicator
      },
      fontFamily: {
        heading: undefined,
        body: undefined,
        mono: undefined,
      },
      boxShadow: {
        'hard-1': '-2px 2px 8px 0px rgba(38, 38, 38, 0.20)',
        'hard-2': '0px 3px 10px 0px rgba(38, 38, 38, 0.20)',
        // ... more shadows
      },
    },
  },
};
```

### 2. metro.config.js (Expo/RN)

```javascript
const { getDefaultConfig } = require('expo/metro-config');
const { withNativeWind } = require('nativewind/metro');

const config = getDefaultConfig(__dirname);

module.exports = withNativeWind(config, { input: './global.css' });
```

### 3. babel.config.js (Expo/RN)

```javascript
module.exports = function (api) {
  api.cache(true);

  return {
    presets: [['babel-preset-expo'], 'nativewind/babel'],

    plugins: [
      [
        'module-resolver',
        {
          root: ['./'],
          alias: {
            '@': './',
            'tailwind.config': './tailwind.config.js',
          },
        },
      ],
      'react-native-worklets/plugin',
    ],
  };
};
```

### 4. global.css

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

### 5. nativewind-env.d.ts

```typescript
/// <reference types="nativewind/types" />
```

### 6. GluestackUIProvider

Created at `components/ui/gluestack-ui-provider/index.tsx`:

```tsx
import React, { useEffect } from 'react';
import { config } from './config';
import { View, ViewProps } from 'react-native';
import { OverlayProvider } from '@gluestack-ui/core/overlay/creator';
import { ToastProvider } from '@gluestack-ui/core/toast/creator';
import { useColorScheme } from 'nativewind';

export type ModeType = 'light' | 'dark' | 'system';

export function GluestackUIProvider({
  mode = 'light',
  ...props
}: {
  mode?: ModeType;
  children?: React.ReactNode;
  style?: ViewProps['style'];
}) {
  const { colorScheme, setColorScheme } = useColorScheme();

  useEffect(() => {
    setColorScheme(mode);
  }, [mode]);

  return (
    <View
      style={[
        config[colorScheme!],
        { flex: 1, height: '100%', width: '100%' },
        props.style,
      ]}
    >
      <OverlayProvider>
        <ToastProvider>{props.children}</ToastProvider>
      </OverlayProvider>
    </View>
  );
}
```

The config file (`components/ui/gluestack-ui-provider/config.ts`) defines color variables using NativeWind's `vars()` function for light and dark modes.

## Manual Verification Steps

After running `npx gluestack-ui init`, verify:

### 1. Check Files Created

```bash
# Configuration files
ls -la tailwind.config.* babel.config.js metro.config.js global.css nativewind-env.d.ts

# Provider created
ls -la components/ui/gluestack-ui-provider/

# Provider dependencies (icon, overlay, toast)
ls -la components/ui/icon components/ui/overlay components/ui/toast
```

### 2. Check Dependencies Installed

```bash
# Verify core dependencies
npm ls nativewind react-native-reanimated react-aria @gluestack-ui/core @gluestack-ui/utils

# For Expo
npx expo install --check

# For React Native CLI (iOS)
cd ios && pod install --repo-update && cd ..
```

### 3. Verify Provider Setup

The CLI automatically modifies your layout files to wrap your app with GluestackUIProvider:

**Expo Router** (`app/_layout.tsx`):
```tsx
import { GluestackUIProvider } from '@/components/ui/gluestack-ui-provider';
import { Slot } from 'expo-router';

export default function RootLayout() {
  return (
    <GluestackUIProvider mode="light">
      <Slot />
    </GluestackUIProvider>
  );
}
```

**Standard Expo/RN** (`App.tsx`):
```tsx
import { GluestackUIProvider } from '@/components/ui/gluestack-ui-provider';

export default function App() {
  return (
    <GluestackUIProvider mode="light">
      {/* Your app content */}
    </GluestackUIProvider>
  );
}
```

**Next.js** (`app/layout.tsx`):
```tsx
import { GluestackUIProvider } from '@/components/ui/gluestack-ui-provider';

export default function RootLayout({ children }) {
  return (
    <html>
      <body>
        <GluestackUIProvider mode="light">
          {children}
        </GluestackUIProvider>
      </body>
    </html>
  );
}
```

### 4. Test the Setup

Create a test component to verify everything works:

```tsx
// test-setup.tsx
import React from 'react';
import { Box } from '@/components/ui/box';
import { Text } from '@/components/ui/text';

export function TestSetup() {
  return (
    <Box className="flex-1 items-center justify-center p-4">
      <Text className="text-typography-900">
        Gluestack UI v4 is working! ✓
      </Text>
    </Box>
  );
}
```

Run the app:
```bash
# Expo
npx expo start

# React Native CLI
npx react-native run-android
npx react-native run-ios

# Next.js
npm run dev
```

## Adding Components

### Using the CLI

The recommended way to add components is using the CLI:

```bash
# Add button component
npx gluestack-ui add button

# This will:
# 1. Clone the gluestack-ui repository (cached in ~/.gluestack-ui/)
# 2. Check component dependencies
# 3. Install required dependencies
# 4. Copy component files to components/ui/button/
# 5. Add any additional components the button depends on
```

### Component Dependencies

Some components depend on others. The CLI automatically handles dependencies:

| Component | Dependencies |
|-----------|-------------|
| Button | icon (optional) |
| Input | icon (optional) |
| FormControl | input, text, icon |
| Card | box, text |
| Alert | icon, text |
| Checkbox | icon |
| Select | input, icon, overlay |
| Toast | overlay, icon |

### Available Components

Common components you can add:

- **Layout**: box, vstack, hstack, center, grid
- **Typography**: text, heading
- **Forms**: button, input, textarea, checkbox, radio, select, switch, slider, form-control
- **Feedback**: alert, toast, spinner, progress, skeleton
- **Overlay**: modal, actionsheet, popover, tooltip, menu
- **Data Display**: card, divider, badge, avatar, image
- **Navigation**: tabs, link
- **Icons**: icon

To see all available components:
```bash
npx gluestack-ui help
```

## Troubleshooting

### Issue: "gluestack is not initialized"

**Solution:**
Run `npx gluestack-ui init` first before adding components.

### Issue: "No package.json found"

**Solution:**
Ensure you're in your project root directory where package.json exists.

### Issue: Expo "rebuilding..." loop

**Problem:** Directory name contains spaces (e.g., "My App")

**Solution:**
```bash
# Rename directory to remove spaces
mv "My App" my-app
cd my-app
```

### Issue: Tailwind classes not applying

**Solution:**
1. Check that `global.css` is imported in your provider
2. Verify `tailwind.config.js` content paths include your files
3. Clear Metro bundler cache:
   ```bash
   npx expo start -c
   ```

### Issue: Module resolution errors for @/components

**Solution:**
Check `babel.config.js` or `tsconfig.json` has path aliases:

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["./*"]
    }
  }
}
```

### Issue: React Native CLI - native modules not found

**Solution:**
```bash
# iOS
cd ios && pod install --repo-update && cd ..
npx react-native run-ios

# Android
npx react-native run-android
```

### Issue: TypeScript errors with nativewind

**Solution:**
Ensure `nativewind-env.d.ts` exists and is included in your tsconfig:
```json
{
  "include": ["**/*.ts", "**/*.tsx", "nativewind-env.d.ts"]
}
```

### Issue: Dependencies conflict

**Solution:**
The CLI tries to match versions to your project. If conflicts occur:
```bash
# Use your preferred package manager's force flag
npm install --force
yarn install --force
pnpm install --force
```

## CLI Commands Reference

### init

Initialize gluestack-ui in your project:

```bash
npx gluestack-ui init [options]

Options:
  --use-npm          Use npm to install dependencies
  --use-yarn         Use yarn to install dependencies
  --use-pnpm         Use pnpm to install dependencies
  --use-bun          Use bun to install dependencies
  --path <path>      Path to components directory (default: components/ui)
  --template-only    Only install template without dependencies
  --projectType <type> Project type (default: auto-detect)
```

### add

Add components to your project:

```bash
npx gluestack-ui add [components...] [options]

Arguments:
  components         Component names to add (space-separated)

Options:
  --all              Add all available components
  --use-npm          Use npm to install dependencies
  --use-yarn         Use yarn to install dependencies
  --use-pnpm         Use pnpm to install dependencies
  --use-bun          Use bun to install dependencies
  --path <path>      Path to components directory
  --template-only    Only copy files without installing dependencies
```

### help

Show help information:

```bash
npx gluestack-ui help
```

### upgrade

Upgrade components to latest version:

```bash
npx gluestack-ui upgrade
```

## Best Practices

1. **Always use the CLI** for initialization and adding components
2. **Commit before init** - The CLI modifies several files
3. **Use consistent package manager** - Don't mix npm, yarn, pnpm
4. **Check documentation** - Visit https://v4.gluestack.io for latest updates
5. **Keep components updated** - Run `npx gluestack-ui upgrade` periodically
6. **Don't modify CLI-generated files** directly - They may be overwritten
7. **Use semantic tokens** - Follow the color system defined in config
8. **Test on real devices** - Especially after initialization

## Component File Structure

After adding components, your project will look like:

```
project-root/
├── components/
│   └── ui/
│       ├── gluestack-ui-provider/
│       │   ├── index.tsx
│       │   ├── config.ts
│       │   └── script.ts
│       ├── box/
│       │   └── index.tsx
│       ├── button/
│       │   └── index.tsx
│       ├── text/
│       │   └── index.tsx
│       └── icon/
│           └── index.tsx
├── tailwind.config.js
├── metro.config.js (Expo/RN)
├── babel.config.js (Expo/RN)
├── global.css
├── nativewind-env.d.ts
└── package.json
```

## Quick Setup Checklist

- [ ] Node.js ≥ 16 installed
- [ ] Project has package.json
- [ ] No spaces in directory names (Expo)
- [ ] Run `npx gluestack-ui init`
- [ ] Verify configuration files created
- [ ] Verify GluestackUIProvider added to layout
- [ ] Verify dependencies installed
- [ ] Add components with `npx gluestack-ui add <component>`
- [ ] Test with simple component
- [ ] Run app and verify setup

## Reference

- **Official Documentation**: https://v4.gluestack.io/ui/docs
- **Component Documentation**: https://v4.gluestack.io/ui/docs/components/
- **CLI Repository**: https://github.com/gluestack/gluestack-ui
- **Getting Started**: https://v4.gluestack.io/ui/docs/getting-started/installation
