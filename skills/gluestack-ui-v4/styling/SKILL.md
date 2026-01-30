---
name: gluestack-ui-v4:styling
description: Styling patterns for gluestack-ui v4 - covers semantic tokens, spacing, dark mode, variants with tva, and className merging.
---

# Gluestack UI v4 - Styling Patterns

This sub-skill focuses on styling patterns, theming, colors, spacing, dark mode, and variant management for gluestack-ui v4.

## Rule 3: Semantic Color Tokens Over Raw Values (v4)

Use Gluestack v4 semantic tokens instead of raw Tailwind colors or arbitrary values:

| Instead of       | Use                         |
| ---------------- | --------------------------- |
| text-red-500     | text-destructive            |
| text-green-500   | text-success (if available) |
| text-blue-500    | text-primary                |
| text-gray-500    | text-muted-foreground       |
| bg-blue-600      | bg-primary                  |
| border-gray-200  | border-border               |
| #DC2626 (inline) | text-destructive            |
| bg-white         | bg-background               |
| text-black       | text-foreground             |

### Available Semantic Token Categories (v4)

| Token                | Purpose                                  | Usage Example                        |
| -------------------- | ---------------------------------------- | ------------------------------------ |
| primary              | Brand identity, key interactive elements | `bg-primary`, `text-primary`         |
| primary-foreground   | Text on primary backgrounds              | `text-primary-foreground`            |
| secondary            | Secondary actions, supporting elements   | `bg-secondary`                       |
| secondary-foreground | Text on secondary backgrounds            | `text-secondary-foreground`          |
| background           | Main background color                    | `bg-background`                      |
| foreground           | Main text color                          | `text-foreground`                    |
| card                 | Card backgrounds                         | `bg-card`                            |
| card-foreground      | Text on card backgrounds                 | `text-card-foreground`               |
| popover              | Popover/modal backgrounds                | `bg-popover`                         |
| popover-foreground   | Text on popover backgrounds              | `text-popover-foreground`            |
| muted                | Muted backgrounds                        | `bg-muted`                           |
| muted-foreground     | Muted text color                         | `text-muted-foreground`              |
| destructive          | Error states, destructive actions        | `bg-destructive`, `text-destructive` |
| border               | Border colors                            | `border-border`                      |
| input                | Input border colors                      | `border-input`                       |
| ring                 | Focus ring colors                        | `ring-ring`                          |
| accent               | Accent highlights                        | `bg-accent`                          |
| accent-foreground    | Text on accent backgrounds               | `text-accent-foreground`             |

### Alpha Values

All color tokens support alpha values using the `/` syntax:

```tsx
// Correct: Using alpha values
<Box className="bg-primary/90" />
<Text className="text-foreground/70" />
<Box className="border-border/80" />
```

### Correct Pattern

```tsx
<Box className="bg-destructive">
  <Text className="text-white">Error message</Text>
</Box>

<Box className="border border-border bg-card">
  <Text className="text-card-foreground">Success!</Text>
</Box>

<Box className="bg-primary/90">
  <Text className="text-primary-foreground">Primary action</Text>
</Box>
```

### Incorrect Pattern

```tsx
<Box className="bg-red-500">
  <Text className="text-white">Error message</Text>
</Box>

<Box style={{ backgroundColor: '#DC2626' }}>
  <Text style={{ color: 'green' }}>Success!</Text>
</Box>

<Box className="bg-[#3b82f6]">
  <Text className="text-[#ffffff]">Primary action</Text>
</Box>
```

## Rule 3: No Inline Styles

Avoid inline `style` props when className can achieve the same result.

### Resolution Hierarchy (in order of preference)

1. **className utilities** - Use existing Tailwind/NativeWind classes
2. **Gluestack component variants** - Use built-in component variants
3. **tva (Tailwind Variant Authority)** - Create reusable variant patterns
4. **NativeWind interop** - Enable className on third-party components
5. **Inline styles** - Only as absolute last resort with documented justification

### Correct Pattern

```tsx
<Box className="w-20 h-20 rounded-full bg-background" />

<Text size="lg" bold className="text-foreground" />

<Button variant="outline" size="lg">
  <ButtonText>Click Me</ButtonText>
</Button>
```

### Incorrect Pattern

```tsx
<Box
  style={{
    width: 80,
    height: 80,
    borderRadius: 40,
    backgroundColor: "rgba(255, 255, 255, 0.1)",
  }}
/>

<Text style={{ fontSize: 18, fontWeight: "bold", color: "#000" }} />
```

### Acceptable Exceptions

Inline styles are acceptable for:

1. **Dynamic values** - Values computed at runtime (e.g., animation values, safe area insets)
2. **Third-party component requirements** - Components that don't support className
3. **Platform-specific overrides** - When Platform.select is needed

```tsx
// Acceptable: dynamic value from hook
<Box style={{ paddingBottom: bottomInset }} />

// Acceptable: animation value
<Animated.View style={{ transform: [{ translateX: animatedValue }] }} />

// Acceptable: platform-specific
<Box style={Platform.select({ ios: { paddingTop: 20 }, android: {} })} />
```

## Rule 4: Spacing Scale Adherence

Use only values from the standard spacing scale. Arbitrary values create maintenance burden.

### Allowed Spacing Values

| Class | Size  |
| ----- | ----- |
| 0     | 0px   |
| 0.5   | 2px   |
| 1     | 4px   |
| 1.5   | 6px   |
| 2     | 8px   |
| 2.5   | 10px  |
| 3     | 12px  |
| 3.5   | 14px  |
| 4     | 16px  |
| 5     | 20px  |
| 6     | 24px  |
| 7     | 28px  |
| 8     | 32px  |
| 9     | 36px  |
| 10    | 40px  |
| 11    | 44px  |
| 12    | 48px  |
| 14    | 56px  |
| 16    | 64px  |
| 20    | 80px  |
| 24    | 96px  |
| 28    | 112px |
| 32    | 128px |
| 36    | 144px |
| 40    | 160px |
| 44    | 176px |
| 48    | 192px |
| 52    | 208px |
| 56    | 224px |
| 60    | 240px |
| 64    | 256px |
| 72    | 288px |
| 80    | 320px |
| 96    | 384px |

### Prohibited Patterns

- Arbitrary values: `p-[13px]`, `m-[27px]`, `gap-[15px]`
- Non-scale decimals: `p-2.7`, `m-4.3`

### Correct Pattern

```tsx
<Box className="p-4 m-2 gap-3" />
<Box className="px-6 py-4 mt-8" />
```

### Incorrect Pattern

```tsx
<Box className="p-[13px] m-[27px]" />
<Box style={{ padding: 13, margin: 27 }} />
```

## Rule 5: Dark Mode Using CSS Variables

Use the `dark:` prefix for dark mode support. Gluestack v4 uses CSS variables that automatically adapt to light/dark themes.

### Correct Pattern

```tsx
<Box className="bg-background dark:bg-background" />
<Text className="text-foreground dark:text-foreground" />

<Box className="border border-border dark:border-border" />
```

### Component-Level Dark Mode

```tsx
const CardView = ({ isDark }: { readonly isDark: boolean }) => (
  <Box className={isDark ? "bg-card" : "bg-background"}>
    <Text className={isDark ? "text-card-foreground" : "text-foreground"}>
      Content
    </Text>
  </Box>
);
```

### Using Data Attributes for States

Gluestack components use data attributes for interactive states. These are automatically applied by the components based on user interaction:

| State Prop     | Data Attribute       | Usage in className                       |
| -------------- | -------------------- | ---------------------------------------- |
| `hover`        | `data-hover`         | `data-[hover=true]:bg-primary/90`        |
| `active`       | `data-active`        | `data-[active=true]:bg-primary/80`       |
| `disabled`     | `data-disabled`      | `data-[disabled=true]:opacity-50`        |
| `focusVisible` | `data-focus-visible` | `data-[focus-visible=true]:ring-2`       |
| `focus`        | `data-focus`         | `data-[focus=true]:border-ring`          |
| `invalid`      | `data-invalid`       | `data-[invalid=true]:border-destructive` |
| `checked`      | `data-checked`       | `data-[checked=true]:bg-primary`         |

```tsx
// Correct: Using data attributes for states in tva
const buttonStyle = tva({
  base: 'rounded-md',
  variants: {
    variant: {
      default: 'bg-primary data-[hover=true]:bg-primary/90 data-[active=true]:bg-primary/80',
      destructive: 'bg-destructive data-[hover=true]:bg-destructive/90',
    },
  },
});

// Correct: Data attributes work automatically with component props
<Button disabled={isLoading}>
  <ButtonText>Submit</ButtonText>
</Button>

<Input isInvalid={hasError}>
  <InputField placeholder="Email" />
</Input>
```

## Rule 7: Variant-Based Styling with tva

For components with multiple style variants, use `tva` (Tailwind Variant Authority).

### Correct Pattern

```tsx
import { tva } from "@gluestack-ui/utils/nativewind-utils";

const cardStyles = tva({
  base: "rounded-lg p-4",
  variants: {
    variant: {
      default: "bg-card border border-border shadow-sm",
      elevated: "bg-card shadow-hard-2",
      outlined: "bg-transparent border border-border",
      filled: "bg-muted",
    },
    size: {
      sm: "p-2",
      md: "p-4",
      lg: "p-6",
    },
  },
  defaultVariants: {
    variant: "default",
    size: "md",
  },
});

const Card = ({ variant, size, className }: CardProps) => (
  <Box className={cardStyles({ variant, size, class: className })} />
);
```

### Using Parent Variants

For sub-components that inherit parent styles:

```tsx
const buttonTextStyle = tva({
  base: "font-sans",
  parentVariants: {
    variant: {
      default: "text-primary-foreground",
      destructive: "text-white",
      outline: "text-foreground",
    },
    size: {
      sm: "text-xs",
      md: "text-sm",
      lg: "text-base",
    },
  },
});
```

## Rule 8: className Merging for Custom Components

Allow className override in custom components using the `class` parameter in tva.

### Correct Pattern

```tsx
interface BoxCardProps {
  readonly className?: string;
  readonly children: React.ReactNode;
}

const BoxCard = ({ className, children }: BoxCardProps) => {
  const cardStyles = tva({
    base: "rounded-lg bg-card p-4",
  });

  return <Box className={cardStyles({ class: className })}>{children}</Box>;
};
```

### Using withStyleContext for Parent-Child Communication

For components that need to share context with children:

```tsx
import {
  withStyleContext,
  useStyleContext,
} from "@gluestack-ui/utils/nativewind-utils";

const SCOPE = "CUSTOM_COMPONENT";
const Root = withStyleContext(View, SCOPE);

const Parent = ({ variant, children }) => (
  <Root context={{ variant }} className={parentStyles({ variant })}>
    {children}
  </Root>
);

const Child = () => {
  const { variant } = useStyleContext(SCOPE);
  return <Box className={childStyles({ parentVariants: { variant } })} />;
};
```

## Layout Components Pattern

```tsx
// ✅ CORRECT: Using space prop instead of gap className
<VStack space="lg" className="p-4">
  <HStack space="md" className="items-center justify-between">
    <Heading size="xl">Title</Heading>
    <Button variant="default" size="sm">
      <ButtonText>Action</ButtonText>
    </Button>
  </HStack>
  <Box className="bg-card rounded-lg p-4">
    <Text size="md">Content</Text>
  </Box>
</VStack>
```

## Anti-Patterns to Avoid

### ❌ Don't: Use Raw Color Values

```tsx
// ❌ Incorrect
<Box className="bg-[#3b82f6]" />
<Text className="text-red-500" />

// ✅ Correct
<Box className="bg-primary" />
<Text className="text-destructive" />
```

### ❌ Don't: Use Inline Styles When className Works

```tsx
// ❌ Incorrect
<Box style={{ padding: 16, backgroundColor: '#fff' }} />

// ✅ Correct
<Box className="p-4 bg-background" />
```

### ❌ Don't: Use Arbitrary Spacing Values

```tsx
// ❌ Incorrect
<Box className="p-[13px] m-[27px]" />

// ✅ Correct
<Box className="p-3 m-6" />
```

## Common Styling Patterns

### Card with Semantic Tokens

```tsx
<Box className="bg-card rounded-lg border border-border p-4 shadow-sm">
  <Heading size="lg" className="text-card-foreground">
    Title
  </Heading>
  <Text size="sm" className="text-muted-foreground">
    Description
  </Text>
</Box>
```

### Button Variants with tva

```tsx
const buttonStyles = tva({
  base: "rounded-md px-4 py-2",
  variants: {
    variant: {
      default: "bg-primary text-primary-foreground",
      destructive: "bg-destructive text-white",
      outline: "border border-border bg-transparent text-foreground",
      ghost: "bg-transparent text-foreground",
    },
  },
});

<Button className={buttonStyles({ variant: "outline" })}>
  <ButtonText>Click Me</ButtonText>
</Button>
```

### Dark Mode Support

```tsx
<Box className="bg-background dark:bg-background">
  <Text className="text-foreground dark:text-foreground">
    Adaptive content
  </Text>
  <Box className="border border-border dark:border-border">
    <Text className="text-muted-foreground dark:text-muted-foreground">
      Muted text
    </Text>
  </Box>
</Box>
```

## Escalation Guidance

When a design request cannot be satisfied with existing patterns:

1. **Push back early** - Explain performance and maintenance implications
2. **Propose alternatives** - Map to existing tokens or suggest new semantic tokens
3. **Add to design system** - If truly needed, add token to `gluestack-ui-provider/config.ts`
4. **Document exception** - If inline style is unavoidable, add JSDoc explaining why

## Reference

- **Theme Configuration**: `@/components/ui/gluestack-ui-provider/config.ts`
- **Tailwind Config**: `./tailwind.config.js`
- **Documentation**: https://v4.gluestack.io/ui/docs
