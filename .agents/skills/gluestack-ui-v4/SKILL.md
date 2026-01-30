---
name: gluestack-ui-v4
description: Enforces constrained, opinionated styling patterns for gluestack-ui v4 that reduce decision fatigue, improve performance, enable consistent theming, and limit the solution space to canonical patterns. Covers semantic tokens, component usage, spacing scale, dark mode, and composable sub-components.
---

# Gluestack UI v4 Design Patterns

This skill enforces constrained, opinionated styling patterns for gluestack-ui v4 that reduce decision fatigue, improve performance, enable consistent theming, and limit the solution space to canonical patterns.

## Core Principles

1. **Gluestack components over React Native primitives** - Gluestack wraps RN with theming, accessibility, and cross-platform consistency
2. **Component props over className utilities** - Use built-in props (size, variant, space) instead of className when available
3. **Semantic tokens over arbitrary values** - Tokens encode intent, not just appearance
4. **className over inline styles** - Inline styles bypass optimization and consistency
5. **Spacing scale over pixel values** - Arbitrary values create unsustainable exceptions
6. **Copy-paste philosophy** - Components are copied into your codebase, not installed as dependencies
7. **Composable sub-components** - Use compound component patterns for flexibility
8. **Remove dead code** - Unused patterns mislead AI and increase cognitive load

## When to Use This Skill

- Creating new components with styling
- Reviewing existing component styles
- Refactoring styles to follow the design system
- Fixing styling inconsistencies
- Adding dark mode support
- Theming components
- Copying components from gluestack-ui into your project

**Before using any component, always verify the latest usage patterns at `https://v4.gluestack.io/ui/docs/components/${componentName}/`**

## Rule 1: Gluestack Components Over React Native Primitives

Always use Gluestack components instead of direct React Native imports:

| React Native                         | Gluestack Equivalent                           |
| ------------------------------------ | ---------------------------------------------- |
| View from "react-native"             | Box from "@/components/ui/box"                 |
| Text from "react-native"             | Text from "@/components/ui/text"               |
| TouchableOpacity from "react-native" | Pressable from "@/components/ui/pressable"     |
| ScrollView from "react-native"       | ScrollView from "@/components/ui/scroll-view"  |
| Image from "react-native"            | Image from "@/components/ui/image"             |
| TextInput from "react-native"        | Input, InputField from "@/components/ui/input" |
| FlatList from "react-native"         | FlatList from "@/components/ui/flat-list"      |

### Correct Pattern

```tsx
import { Box } from "@/components/ui/box";
import { Text } from "@/components/ui/text";
import { Pressable } from "@/components/ui/pressable";

const Component = () => (
  <Box className="p-4">
    <Text className="text-foreground">Hello</Text>
    <Pressable onPress={handlePress}>
      <Text>Press Me</Text>
    </Pressable>
  </Box>
);
```

### Incorrect Pattern

```tsx
import { View, Text, TouchableOpacity } from "react-native";

const Component = () => (
  <View style={{ padding: 16 }}>
    <Text style={{ color: "#333" }}>Hello</Text>
    <TouchableOpacity onPress={handlePress}>
      <Text>Press Me</Text>
    </TouchableOpacity>
  </View>
);
```

### Exceptions

- Platform-specific code where RN primitives are explicitly required
- Deep integration with native modules
- Performance-critical paths where wrapper overhead matters (rare, must document)

## Rule 2: Use Component Props Over className Utilities

Always prefer component props over className utilities when a component provides built-in props. This ensures type safety, better maintainability, and consistent styling.

### Component Props vs className

Many Gluestack components provide props that map to common styling needs. Use these props instead of className utilities:

| Component | Use Prop Instead of className | Available Values |
|-----------|------------------------------|-----------------|
| `VStack` / `HStack` | `space` instead of `gap-*` | `xs`, `sm`, `md`, `lg`, `xl`, `2xl`, `3xl`, `4xl` |
| `Button` | `variant` instead of `bg-*` classes | `default`, `destructive`, `outline`, `secondary`, `ghost`, `link` |
| `Button` | `size` instead of `px-* py-*` classes | `default`, `sm`, `lg`, `icon` |
| `Heading` | `size` instead of `text-*` classes | `xs`, `sm`, `md`, `lg`, `xl`, `2xl`, `3xl`, `4xl`, `5xl` |
| `Text` | `size` instead of `text-*` classes | `2xs`, `xs`, `sm`, `md`, `lg`, `xl`, `2xl`, `3xl`, `4xl`, `5xl`, `6xl` |
| `Heading` / `Text` | `bold` prop instead of `font-bold` | boolean |
| `Heading` / `Text` | `isTruncated` prop instead of `truncate` | boolean |
| `VStack` / `HStack` | `reversed` prop instead of `flex-*-reverse` | boolean |

### Correct Pattern: Using Component Props

```tsx
// ✅ CORRECT: Using space prop instead of gap className
<VStack space="lg">
  <Box>Item 1</Box>
  <Box>Item 2</Box>
</VStack>

// ✅ CORRECT: Using Button variant and size props
<Button variant="outline" size="lg">
  <ButtonText>Click Me</ButtonText>
</Button>

// ✅ CORRECT: Using Heading size prop
<Heading size="2xl" bold>
  Title
</Heading>

// ✅ CORRECT: Using Text size and bold props
<Text size="sm" bold>
  Important text
</Text>

// ✅ CORRECT: Using HStack space prop
<HStack space="md" className="items-center">
  <Text>Label</Text>
  <Button size="sm">
    <ButtonText>Action</ButtonText>
  </Button>
</HStack>
```

### Incorrect Pattern: Using className Instead of Props

```tsx
// ❌ INCORRECT: Using gap className instead of space prop
<VStack space="lg">
  <Box>Item 1</Box>
  <Box>Item 2</Box>
</VStack>

// ❌ INCORRECT: Using className for button styling instead of variant/size props
<Button className="bg-primary px-8 py-2">
  <ButtonText>Click Me</ButtonText>
</Button>

// ❌ INCORRECT: Using text size className instead of size prop
<Heading className="text-2xl font-bold">
  Title
</Heading>

// ❌ INCORRECT: Using className for spacing instead of space prop
<HStack space="sm" className="items-center">
  <Text>Label</Text>
  <Button size="sm">
    <ButtonText>Action</ButtonText>
  </Button>
</HStack>
```

### When to Use className vs Props

**Use Props When:**
- Component provides a built-in prop for the styling (size, variant, space, etc.)
- You want type safety and autocomplete
- The styling is part of the component's design system

**Use className When:**
- Component doesn't provide a prop for the specific styling needed
- You need custom styling not covered by props
- Combining multiple utilities that don't have prop equivalents
- Layout utilities (flex, items-center, justify-between, etc.)

### Combining Props and className

You can combine props with className for additional styling:

```tsx
// ✅ CORRECT: Using space prop + className for additional styling
<VStack space="lg" className="p-4 bg-card rounded-lg">
  <Heading size="xl">Title</Heading>
  <Text size="sm">Description</Text>
</VStack>

// ✅ CORRECT: Using space prop in examples
<VStack space="lg">
  <Box>Item 1</Box>
  <Box>Item 2</Box>
</VStack>

// ✅ CORRECT: Using variant prop + className for custom adjustments
<Button variant="outline" size="lg" className="w-full">
  <ButtonText>Full Width Button</ButtonText>
</Button>
```

### Space Prop Mapping

The `space` prop on VStack/HStack maps to standard spacing:

| space prop | Gap Value | Equivalent className |
|------------|-----------|---------------------|
| `xs` | 4px | `gap-1` |
| `sm` | 8px | `gap-2` |
| `md` | 12px | `gap-3` |
| `lg` | 16px | `gap-4` |
| `xl` | 20px | `gap-5` |
| `2xl` | 24px | `gap-6` |
| `3xl` | 28px | `gap-7` |
| `4xl` | 32px | `gap-8` |

### Benefits of Using Props

1. **Type Safety** - TypeScript will catch invalid prop values
2. **Autocomplete** - IDE provides suggestions for valid values
3. **Consistency** - Enforces design system values
4. **Maintainability** - Easier to refactor and update
5. **Documentation** - Props are self-documenting
6. **Performance** - Props are optimized by the component system

### Quick Reference: Common Props

```tsx
// VStack/HStack spacing
<VStack space="lg">  // ✅ Use space prop
<VStack className="gap-4">  // ❌ Don't use gap className

// Button styling
<Button variant="outline" size="lg">  // ✅ Use variant and size props
<Button className="bg-primary px-8 py-2">  // ❌ Don't use className for variant/size

// Text/Heading sizing
<Heading size="2xl" bold>  // ✅ Use size and bold props
<Heading className="text-2xl font-bold">  // ❌ Don't use className for size/bold

<Text size="sm" bold>  // ✅ Use size and bold props
<Text className="text-sm font-bold">  // ❌ Don't use className for size/bold
```

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

## Rule 6: Gluestack Compound Component Pattern

Use Gluestack's composable compound component pattern for complex components. This is **REQUIRED** for proper rendering, styling, and functionality. Compound components provide proper context sharing, styling inheritance, and accessibility.

### Critical Rule: InputIcon MUST Be Wrapped in InputSlot

**ALL InputIcon components MUST be wrapped in InputSlot**, regardless of whether they're on the left or right side of the input. This is required for proper styling, positioning, and interaction handling.

### Input Component Patterns

#### Correct: InputIcon Wrapped in InputSlot (Required)

```tsx
// ✅ CORRECT: Left icon wrapped in InputSlot
<Input>
  <InputSlot>
    <InputIcon as={MailIcon} className="text-muted-foreground" />
  </InputSlot>
  <InputField placeholder="Enter email" />
</Input>

// ✅ CORRECT: Right icon (interactive) wrapped in InputSlot
<Input>
  <InputField placeholder="Enter password" secureTextEntry={!showPassword} />
  <InputSlot onPress={() => setShowPassword(!showPassword)}>
    <InputIcon as={showPassword ? EyeOffIcon : EyeIcon} className="text-muted-foreground" />
  </InputSlot>
</Input>

// ✅ CORRECT: Both left and right icons wrapped in InputSlot
<Input>
  <InputSlot>
    <InputIcon as={SearchIcon} className="text-muted-foreground" />
  </InputSlot>
  <InputField placeholder="Search..." />
  <InputSlot onPress={handleClear}>
    <InputIcon as={XIcon} className="text-muted-foreground" />
  </InputSlot>
</Input>
```

#### Incorrect: InputIcon Used Directly (Will Break)

```tsx
// ❌ INCORRECT: InputIcon used directly without InputSlot
<Input>
  <InputIcon as={MailIcon} />  {/* ❌ Missing InputSlot wrapper */}
  <InputField placeholder="Enter email" />
</Input>

// ❌ INCORRECT: InputIcon outside Input structure
<InputIcon as={MailIcon} />  {/* ❌ Must be inside Input > InputSlot */}
<Input>
  <InputField placeholder="Enter email" />
</Input>
```

### Button Component Patterns

#### Correct Pattern

```tsx
// ✅ CORRECT: Button with text and icon
<Button variant="default" size="md">
  <ButtonText>Click Me</ButtonText>
  <ButtonIcon as={ChevronRightIcon} />
</Button>

// ✅ CORRECT: Button with only text
<Button>
  <ButtonText>Submit</ButtonText>
</Button>

// ✅ CORRECT: Button with only icon
<Button size="icon">
  <ButtonIcon as={SearchIcon} />
</Button>

// ✅ CORRECT: Button with loading state
<Button isDisabled={isLoading}>
  {isLoading ? (
    <>
      <ButtonSpinner />
      <ButtonText>Loading...</ButtonText>
    </>
  ) : (
    <ButtonText>Submit</ButtonText>
  )}
</Button>
```

#### Incorrect Pattern

```tsx
// ❌ INCORRECT: Text not wrapped in ButtonText
<Button>Click Me</Button>

// ❌ INCORRECT: Direct text children
<Button>
  Click Me  {/* ❌ Must use ButtonText */}
</Button>

// ❌ INCORRECT: Icon not wrapped in ButtonIcon
<Button>
  <ButtonText>Next</ButtonText>
  <ChevronRightIcon />  {/* ❌ Must use ButtonIcon */}
</Button>
```

### FormControl Component Patterns

#### Correct Pattern

```tsx
// ✅ CORRECT: Complete FormControl with label, input, and error
<FormControl isInvalid={!!errors.email}>
  <FormControlLabel>
    <FormControlLabelText>Email Address</FormControlLabelText>
  </FormControlLabel>
  <Input>
    <InputSlot>
      <InputIcon as={MailIcon} />
    </InputSlot>
    <InputField
      placeholder="Enter email"
      value={email}
      onChangeText={setEmail}
    />
  </Input>
  {errors.email && (
    <FormControlError>
      <FormControlErrorIcon as={AlertCircleIcon} />
      <FormControlErrorText>{errors.email}</FormControlErrorText>
    </FormControlError>
  )}
  <FormControlHelper>
    <FormControlHelperText>We'll never share your email</FormControlHelperText>
  </FormControlHelper>
</FormControl>
```

#### Incorrect Pattern

```tsx
// ❌ INCORRECT: Missing sub-components
<FormControl>
  <Text>Email</Text>  {/* ❌ Must use FormControlLabel > FormControlLabelText */}
  <InputField />  {/* ❌ Must wrap in Input */}
</FormControl>

// ❌ INCORRECT: Error text not wrapped
<FormControl isInvalid={hasError}>
  <Input>
    <InputField />
  </Input>
  {hasError && <Text>Error message</Text>}  {/* ❌ Must use FormControlError > FormControlErrorText */}
</FormControl>
```

### Card Component Patterns

#### Correct Pattern

```tsx
// ✅ CORRECT: Complete Card structure
<Card>
  <CardHeader>
    <Heading size="lg">Card Title</Heading>
    <Text className="text-muted-foreground">Subtitle</Text>
  </CardHeader>
  <CardBody>
    <Text>Card content goes here</Text>
  </CardBody>
  <CardFooter>
    <Button variant="outline">
      <ButtonText>Cancel</ButtonText>
    </Button>
    <Button>
      <ButtonText>Confirm</ButtonText>
    </Button>
  </CardFooter>
</Card>
```

#### Incorrect Pattern

```tsx
// ❌ INCORRECT: Direct children without sub-components
<Card>
  <Heading>Title</Heading>  {/* ❌ Must use CardHeader */}
  <Text>Content</Text>  {/* ❌ Must use CardBody */}
</Card>
```

### Checkbox Component Patterns

#### Correct Pattern

```tsx
// ✅ CORRECT: Checkbox with label
<Checkbox
  value="terms"
  isChecked={accepted}
  onChange={(isChecked) => setAccepted(isChecked)}
>
  <CheckboxIndicator>
    <CheckboxIcon as={CheckIcon} />
  </CheckboxIndicator>
  <CheckboxLabel>I accept the terms and conditions</CheckboxLabel>
</Checkbox>
```

#### Incorrect Pattern

```tsx
// ❌ INCORRECT: Missing sub-components
<Checkbox isChecked={accepted}>
  <Text>Accept terms</Text>  {/* ❌ Must use CheckboxLabel */}
</Checkbox>

// ❌ INCORRECT: Icon not wrapped properly
<Checkbox>
  <CheckIcon />  {/* ❌ Must use CheckboxIndicator > CheckboxIcon */}
  <CheckboxLabel>Accept</CheckboxLabel>
</Checkbox>
```

### Select Component Patterns

#### Correct Pattern

```tsx
// ✅ CORRECT: Select with trigger and options
<Select>
  <SelectTrigger>
    <SelectInput placeholder="Select option" />
    <SelectIcon as={ChevronDownIcon} />
  </SelectTrigger>
  <SelectPortal>
    <SelectBackdrop />
    <SelectContent>
      <SelectDragIndicatorWrapper>
        <SelectDragIndicator />
      </SelectDragIndicatorWrapper>
      <SelectItem label="Option 1" value="1">
        <SelectItemText>Option 1</SelectItemText>
      </SelectItem>
      <SelectItem label="Option 2" value="2">
        <SelectItemText>Option 2</SelectItemText>
      </SelectItem>
    </SelectContent>
  </SelectPortal>
</Select>
```

### Compound Component Reference Table

| Component | Required Sub-Components | Optional Sub-Components | Notes |
|-----------|------------------------|------------------------|-------|
| **Input** | `InputField` | `InputSlot`, `InputIcon` | **InputIcon MUST be inside InputSlot** |
| **Button** | `ButtonText` | `ButtonIcon`, `ButtonSpinner` | Text content must use ButtonText |
| **Card** | None | `CardHeader`, `CardBody`, `CardFooter` | Structure for organization |
| **FormControl** | None | `FormControlLabel`, `FormControlError`, `FormControlHelper` | Wrapper for form fields |
| **Checkbox** | `CheckboxIndicator`, `CheckboxLabel` | `CheckboxIcon` | Icon goes inside Indicator |
| **Select** | `SelectTrigger`, `SelectInput` | `SelectIcon`, `SelectContent`, `SelectItem` | Complex structure required |
| **Alert** | `AlertText` | `AlertIcon`, `AlertTitle` | Text must use AlertText |
| **Toast** | `ToastTitle` | `ToastDescription`, `ToastCloseButton` | Title required for display |

### Key Principles

1. **Always use sub-components** - Never place raw text, icons, or elements directly as children
2. **InputIcon requires InputSlot** - This is mandatory, not optional
3. **Text content requires text sub-components** - ButtonText, AlertText, etc.
4. **Icons require icon sub-components** - ButtonIcon, InputIcon (inside InputSlot), etc.
5. **Check official docs** - Component structures may vary; always verify at `https://v4.gluestack.io/ui/docs/components/${componentName}/`

### Common Mistakes to Avoid

```tsx
// ❌ MISTAKE: InputIcon without InputSlot
<Input>
  <InputIcon as={MailIcon} />
  <InputField />
</Input>

// ❌ MISTAKE: Button text not wrapped
<Button>Submit</Button>

// ❌ MISTAKE: FormControl error not using sub-components
<FormControl isInvalid={true}>
  <InputField />
  <Text className="text-red-500">Error</Text>  {/* ❌ Use FormControlError */}
</FormControl>

// ❌ MISTAKE: Checkbox without proper structure
<Checkbox>
  <Text>Label</Text>  {/* ❌ Use CheckboxLabel */}
</Checkbox>
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

## Rule 9: Copy-Paste Philosophy

Gluestack-ui uses a copy-paste approach. Components are copied into your codebase, not installed as npm packages.

**IMPORTANT**: Before copying or using any component, verify the latest usage patterns, sub-components, and API at `https://v4.gluestack.io/ui/docs/components/${componentName}/`

### Correct Pattern

1. **Check official v4 docs** - Visit `https://v4.gluestack.io/ui/docs/components/${componentName}/` to verify latest API and patterns
2. Copy component files from gluestack-ui into your `components/ui/` directory
3. Import from your local components directory
4. Customize as needed

```tsx
// Import from your local components
import { Button, ButtonText } from "@/components/ui/button";
import { Box } from "@/components/ui/box";
```

### Incorrect Pattern

```tsx
// Don't try to import from a package
import { Button } from "@gluestack-ui/button"; // ❌ This doesn't exist
```

## Rule 10: Provider Setup

Always wrap your app with `GluestackUIProvider` to enable theming and component functionality.

### Correct Pattern

```tsx
import { GluestackUIProvider } from "@/components/ui/gluestack-ui-provider";

export default function App() {
  return (
    <GluestackUIProvider>
      <YourApp />
    </GluestackUIProvider>
  );
}
```

## Rule 11: Icon Usage

Use icons from `@/components/ui/icon` following this priority:

1. **Pre-built icons** - Use icons already exported from `components/ui/icon/index.tsx` (e.g., `ChevronRightIcon`, `SearchIcon`, `CheckIcon`)
2. **Lucide Icons (Recommended)** - If the icon is not available in `components/ui/icon/index.tsx`, use Lucide Icons if available
3. **Custom icons with createIcon** - If neither is available, create custom icons using the `createIcon` function

### Icon Resolution Hierarchy

1. Check if icon exists in `@/components/ui/icon` (e.g., `ChevronRightIcon`, `SearchIcon`)
2. Use Lucide Icons if available (recommended for missing icons)
3. Create custom icon using `createIcon` function

### Using Pre-built Icons

```tsx
import { ChevronRightIcon, SearchIcon } from '@/components/ui/icon';
import { Icon } from '@/components/ui/icon';
import { Button, ButtonIcon } from '@/components/ui/button';

<Button>
  <ButtonText>Next</ButtonText>
  <ButtonIcon as={ChevronRightIcon} />
</Button>

<Icon as={SearchIcon} size="md" className="text-foreground" />
```

### Using Lucide Icons (Recommended)

When an icon is not available in `components/ui/icon/index.tsx`, use Lucide Icons:

```tsx
import { Icon } from "@/components/ui/icon";
import { Heart } from "lucide-react-native";

<Icon as={Heart} size="md" className="text-foreground" />;
```

### Creating Custom Icons with createIcon

If an icon is not available in `components/ui/icon/index.tsx` and not available in Lucide Icons, create a custom icon using the `createIcon` function:

```tsx
import { Icon, createIcon } from "@/components/ui/icon";
import { Path } from "react-native-svg";

function App() {
  const CustomIcon = createIcon({
    viewBox: "0 0 32 32",
    path: (
      <>
        <Path
          d="M9.5 14.6642L15.9999 9.87633V12.1358L9.5 16.9236V14.6642Z"
          fill="grey"
        />
        <Path
          d="M22.5 14.6642L16.0001 9.87639V12.1359L22.5 16.9237V14.6642Z"
          fill="grey"
        />
        <Path
          d="M9.5 19.8641L15.9999 15.0763V17.3358L9.5 22.1236V19.8641Z"
          fill="grey"
        />
        <Path
          d="M22.5 19.8642L16.0001 15.0764V17.3358L22.5 22.1237V19.8642Z"
          fill="grey"
        />
      </>
    ),
  });

  return <Icon as={CustomIcon} size="xl" className="text-foreground" />;
}
```

### Correct Pattern

```tsx
// Using pre-built icon
import { ChevronRightIcon } from "@/components/ui/icon";
import { Button, ButtonIcon } from "@/components/ui/button";

<Button>
  <ButtonText>Continue</ButtonText>
  <ButtonIcon as={ChevronRightIcon} />
</Button>;

// Using Lucide icon (when not in components/ui/icon)
import { Icon } from "@/components/ui/icon";
import { Heart } from "lucide-react-native";

<Icon as={Heart} size="md" className="text-foreground" />;

// Creating custom icon
import { Icon, createIcon } from "@/components/ui/icon";
import { Path } from "react-native-svg";

const CustomIcon = createIcon({
  viewBox: "0 0 24 24",
  path: (
    <Path
      d="M12 2L2 7L12 12L22 7L12 2Z"
      strokeWidth="2"
      strokeLinecap="round"
      strokeLinejoin="round"
    />
  ),
});

<Icon as={CustomIcon} size="md" className="text-foreground" />;
```

### Incorrect Pattern

```tsx
// ❌ Don't import icons from external packages directly
import { Heart } from "@some-icon-package";

// ❌ Don't use raw SVG components without createIcon
import Svg, { Path } from "react-native-svg";

<Svg>
  <Path d="..." />
</Svg>;
```

## Common Patterns

### Form Components

```tsx
// Complete form with InputIcon properly wrapped in InputSlot
<FormControl isInvalid={hasError}>
  <FormControlLabel>
    <FormControlLabelText>Email</FormControlLabelText>
  </FormControlLabel>
  <Input>
    <InputSlot>
      <InputIcon as={MailIcon} className="text-muted-foreground" />
    </InputSlot>
    <InputField
      placeholder="Enter email"
      value={email}
      onChangeText={setEmail}
    />
  </Input>
  {hasError && (
    <FormControlError>
      <FormControlErrorIcon as={AlertCircleIcon} />
      <FormControlErrorText>Invalid email</FormControlErrorText>
    </FormControlError>
  )}
</FormControl>

// Password input with show/hide toggle
<Input>
  <InputSlot>
    <InputIcon as={LockIcon} className="text-muted-foreground" />
  </InputSlot>
  <InputField
    placeholder="Enter password"
    secureTextEntry={!showPassword}
    value={password}
    onChangeText={setPassword}
  />
  <InputSlot onPress={() => setShowPassword(!showPassword)}>
    <InputIcon
      as={showPassword ? EyeOffIcon : EyeIcon}
      className="text-muted-foreground"
    />
  </InputSlot>
</Input>
```

### Layout Components

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

### ❌ Don't: Mix React Native and Gluestack Components

```tsx
// ❌ Incorrect
import { View, Text } from "react-native";
import { Button } from "@/components/ui/button";

<View>
  <Text>Mixed usage</Text>
  <Button>Click</Button>
</View>;
```

### ❌ Don't: Use Raw Color Values

```tsx
// ❌ Incorrect
<Box className="bg-[#3b82f6]" />
<Text className="text-red-500" />
```

### ❌ Don't: Skip Sub-Components

```tsx
// ❌ Incorrect - ButtonText is required
<Button>Click Me</Button>

// ✅ Correct
<Button>
  <ButtonText>Click Me</ButtonText>
</Button>
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

## Rule 12: Cross-Platform Rendering (Native & Web)

Gluestack UI v4 components are designed to work seamlessly on both React Native (iOS/Android) and Web platforms. Always use Gluestack wrapper components instead of direct React Native imports to ensure cross-platform compatibility.

### Critical Rule: Always Use Gluestack Wrappers

**NEVER import components directly from `react-native`** when a Gluestack wrapper exists. Gluestack wrappers handle platform-specific differences automatically.

### Platform-Specific Component Mapping

| React Native Import | Gluestack Wrapper | Notes |
|---------------------|-------------------|-------|
| `KeyboardAvoidingView` from `react-native` | `KeyboardAvoidingView` from `@/components/ui/keyboard-avoiding-view` | Required for web compatibility |
| `Platform` from `react-native` | Use only when absolutely necessary | Prefer Gluestack's built-in platform handling |
| `View`, `Text`, etc. | `Box`, `Text` from `@/components/ui/*` | Always use Gluestack components |

### Correct Pattern: Cross-Platform Components

```tsx
// ✅ CORRECT: Using Gluestack KeyboardAvoidingView wrapper
import { KeyboardAvoidingView } from '@/components/ui/keyboard-avoiding-view';
import { Platform } from 'react-native'; // Only when needed for platform-specific logic

<KeyboardAvoidingView
  behavior={Platform.OS === 'ios' ? 'padding' : 'height'}
  className="flex-1"
>
  <ScrollView>
    {/* Content */}
  </ScrollView>
</KeyboardAvoidingView>
```

### Incorrect Pattern: Direct React Native Imports

```tsx
// ❌ INCORRECT: Direct import from react-native
import { KeyboardAvoidingView, Platform } from 'react-native';

<KeyboardAvoidingView behavior={Platform.OS === 'ios' ? 'padding' : 'height'}>
  {/* This may not work correctly on web */}
</KeyboardAvoidingView>
```

### Web-Specific Considerations

1. **KeyboardAvoidingView**: The Gluestack wrapper handles web gracefully (web doesn't need keyboard avoidance)
2. **SafeAreaView**: Works on both native and web (web treats it as a regular View)
3. **ScrollView**: Works identically on both platforms
4. **Platform.select**: Only use when absolutely necessary; prefer Gluestack's built-in handling

### Testing Cross-Platform Compatibility

Always test components on both platforms:

1. **Native**: Run `npm run ios` or `npm run android`
2. **Web**: Run `npm run web` and verify in browser
3. **Verify**: Check that all components render correctly and interactions work on both platforms

### Platform-Specific Code (When Necessary)

If you must use platform-specific code, use it sparingly and document why:

```tsx
// Acceptable: Platform-specific behavior when Gluestack doesn't cover it
import { Platform } from 'react-native';

const keyboardBehavior = Platform.OS === 'ios' ? 'padding' : 'height';

<KeyboardAvoidingView behavior={keyboardBehavior} className="flex-1">
  {/* Content */}
</KeyboardAvoidingView>
```

### Common Cross-Platform Issues to Avoid

1. **Direct React Native imports** - Always use Gluestack wrappers
2. **Platform-specific styling without fallbacks** - Ensure web has equivalent styles
3. **Native-only APIs** - Check if web alternatives exist
4. **Missing web polyfills** - Gluestack handles most of these automatically

### Verification Checklist for Cross-Platform

- [ ] All components imported from `@/components/ui/*` wrappers
- [ ] No direct imports from `react-native` for wrapped components
- [ ] KeyboardAvoidingView uses Gluestack wrapper
- [ ] Tested on both native (iOS/Android) and web platforms
- [ ] All interactions work on both platforms
- [ ] Styling renders correctly on both platforms
- [ ] No platform-specific code without documentation

## Rule 13: Performance & Best Practices

Follow these best practices to ensure optimal performance, type safety, and maintainability in React Native/Expo applications.

### Use TypeScript

Define navigation and prop types for type safety. This catches errors at compile time and improves developer experience.

#### Correct Pattern

```tsx
// ✅ CORRECT: Typed component props
interface LoginFormProps {
  readonly onSubmit: (email: string, password: string) => void;
  readonly isLoading?: boolean;
}

const LoginForm = ({ onSubmit, isLoading = false }: LoginFormProps) => {
  // Component implementation
};

// ✅ CORRECT: Typed navigation
import { useRouter } from 'expo-router';

const router = useRouter();
router.push('/login' as any); // Type-safe navigation
```

#### Incorrect Pattern

```tsx
// ❌ INCORRECT: No type definitions
const LoginForm = ({ onSubmit, isLoading }) => {
  // No type safety
};
```

### Memoize Components

Use `React.memo` and `useCallback` to prevent unnecessary rerenders, especially for expensive components or frequently re-rendered parent components.

#### Correct Pattern

```tsx
// ✅ CORRECT: Memoized component
import React, { useCallback, useState } from 'react';

const ExpensiveComponent = React.memo(({ data, onUpdate }: Props) => {
  // Expensive rendering logic
});

const ParentComponent = () => {
  const [count, setCount] = useState(0);
  
  // Memoized callback prevents child rerenders
  const handleUpdate = useCallback((value: string) => {
    // Update logic
  }, []);

  return (
    <>
      <Button onPress={() => setCount(count + 1)}>
        <ButtonText>Count: {count}</ButtonText>
      </Button>
      <ExpensiveComponent data={data} onUpdate={handleUpdate} />
    </>
  );
};
```

#### When to Memoize

- Components that receive stable props but parent rerenders frequently
- Callbacks passed to child components
- Expensive computations (use `useMemo`)

### Run Animations on UI Thread

Use Reanimated worklets for 60fps animations. This keeps animations smooth by running on the native UI thread instead of the JavaScript thread.

#### Correct Pattern

```tsx
// ✅ CORRECT: Using Reanimated worklets
import { useSharedValue, withTiming } from 'react-native-reanimated';
import Animated from 'react-native-reanimated';

const AnimatedBox = Animated.createAnimatedComponent(Box);

const Component = () => {
  const translateX = useSharedValue(0);

  const handlePress = () => {
    // Animation runs on UI thread
    translateX.value = withTiming(100, { duration: 300 });
  };

  return (
    <AnimatedBox
      style={{
        transform: [{ translateX }],
      }}
    >
      <Pressable onPress={handlePress}>
        <Text>Animate</Text>
      </Pressable>
    </AnimatedBox>
  );
};
```

#### Incorrect Pattern

```tsx
// ❌ INCORRECT: Using Animated API (runs on JS thread)
import { Animated } from 'react-native';

const Component = () => {
  const translateX = new Animated.Value(0);
  // This runs on JavaScript thread, can cause jank
};
```

### Handle Safe Areas

Use `SafeAreaView` or `useSafeAreaInsets` to handle device notches, status bars, and home indicators properly.

#### Correct Pattern

```tsx
// ✅ CORRECT: Using SafeAreaView
import { SafeAreaView } from '@/components/ui/safe-area-view';

const Screen = () => (
  <SafeAreaView className="flex-1 bg-background">
    <VStack className="p-4">
      {/* Content */}
    </VStack>
  </SafeAreaView>
);

// ✅ CORRECT: Using useSafeAreaInsets for custom layouts
import { useSafeAreaInsets } from 'react-native-safe-area-context';

const CustomLayout = () => {
  const insets = useSafeAreaInsets();
  
  return (
    <Box style={{ paddingTop: insets.top }}>
      {/* Content */}
    </Box>
  );
};
```

### Test on Real Devices

Simulator/emulator performance differs from real devices. Always test on physical devices before releasing.

#### Testing Checklist

- [ ] Test on real iOS device (iPhone/iPad)
- [ ] Test on real Android device
- [ ] Test on different screen sizes
- [ ] Test with different OS versions
- [ ] Test performance under load
- [ ] Test with slow network conditions

### Use FlatList for Lists

Never use `ScrollView` with `map` for long lists. `FlatList` provides virtualization, which only renders visible items.

#### Correct Pattern

```tsx
// ✅ CORRECT: Using FlatList
import { FlatList } from '@/components/ui/flat-list';

const ItemList = ({ items }: { items: Item[] }) => (
  <FlatList
    data={items}
    renderItem={({ item }) => (
      <Box className="p-4 border-b border-border">
        <Text>{item.name}</Text>
      </Box>
    )}
    keyExtractor={(item) => item.id}
    ListEmptyComponent={<Text>No items found</Text>}
  />
);
```

#### Incorrect Pattern

```tsx
// ❌ INCORRECT: Using ScrollView with map (no virtualization)
import { ScrollView } from '@/components/ui/scroll-view';

const ItemList = ({ items }: { items: Item[] }) => (
  <ScrollView>
    {items.map((item) => (
      <Box key={item.id} className="p-4">
        <Text>{item.name}</Text>
      </Box>
    ))}
  </ScrollView>
);
```

**Why this is bad**: All items are rendered at once, causing performance issues with long lists.

### Platform-Specific Code

Use `Platform.select` for iOS/Android differences. This provides a clean, declarative way to handle platform-specific code.

#### Correct Pattern

```tsx
// ✅ CORRECT: Using Platform.select
import { Platform } from 'react-native';

const styles = Platform.select({
  ios: {
    paddingTop: 20,
  },
  android: {
    paddingTop: 0,
  },
  default: {
    paddingTop: 0,
  },
});

<Box style={styles}>
  {/* Content */}
</Box>

// ✅ CORRECT: Platform.select for values
const keyboardBehavior = Platform.select({
  ios: 'padding',
  android: 'height',
  default: 'padding',
});
```

#### Incorrect Pattern

```tsx
// ❌ INCORRECT: Using if/else for platform checks
import { Platform } from 'react-native';

let styles;
if (Platform.OS === 'ios') {
  styles = { paddingTop: 20 };
} else {
  styles = { paddingTop: 0 };
}
```

### Best Practices Summary

| Practice | Why It Matters | When to Use |
|----------|---------------|-------------|
| **TypeScript** | Type safety, catch errors early | Always |
| **React.memo** | Prevent unnecessary rerenders | Components with stable props |
| **useCallback** | Stable function references | Callbacks passed to children |
| **Reanimated worklets** | 60fps animations | All animations |
| **SafeAreaView** | Handle device notches/bars | All screens |
| **FlatList** | Virtualization for performance | Lists with 10+ items |
| **Platform.select** | Clean platform-specific code | iOS/Android differences |
| **Real device testing** | Accurate performance metrics | Before release |

### Performance Checklist

- [ ] TypeScript types defined for all components and props
- [ ] Expensive components wrapped with `React.memo`
- [ ] Callbacks memoized with `useCallback`
- [ ] Animations use Reanimated worklets
- [ ] Safe areas handled with `SafeAreaView` or `useSafeAreaInsets`
- [ ] Long lists use `FlatList` instead of `ScrollView` + `map`
- [ ] Platform-specific code uses `Platform.select`
- [ ] Tested on real devices (not just simulators)
- [ ] Performance profiled and optimized

## Validation Checklist

When reviewing code, check for:

- [ ] Component usage verified against official v4 docs at `https://v4.gluestack.io/ui/docs/components/${componentName}/`
- [ ] All React Native primitives replaced with Gluestack components
- [ ] **Component props used instead of className when available:**
  - [ ] VStack/HStack use `space` prop instead of `gap-*` className
  - [ ] Button uses `variant` and `size` props instead of className
  - [ ] Heading/Text use `size` prop instead of `text-*` className
  - [ ] Heading/Text use `bold` prop instead of `font-bold` className
  - [ ] Other component props used where applicable
- [ ] All color values use semantic tokens (no raw colors)
- [ ] No inline styles where className can be used
- [ ] All spacing values use the standard scale
- [ ] Dark mode support using `dark:` prefix
- [ ] **Compound components used correctly:**
  - [ ] InputIcon always wrapped in InputSlot (CRITICAL)
  - [ ] ButtonText used for all button text content
  - [ ] FormControl sub-components used (FormControlLabel, FormControlError, etc.)
  - [ ] Card sub-components used (CardHeader, CardBody, CardFooter)
  - [ ] Checkbox sub-components used (CheckboxIndicator, CheckboxIcon, CheckboxLabel)
  - [ ] All other component sub-components as per official docs
- [ ] Variants defined using `tva` when needed
- [ ] className properly merged in custom components
- [ ] Components imported from local `@/components/ui/` directory
- [ ] GluestackUIProvider wraps the app
- [ ] Icons follow priority: pre-built icons → Lucide Icons → createIcon for custom icons
- [ ] **Cross-platform compatibility verified:**
  - [ ] All components use Gluestack wrappers (no direct react-native imports)
  - [ ] KeyboardAvoidingView uses Gluestack wrapper
  - [ ] Tested on both native and web platforms
  - [ ] All interactions work on both platforms
- [ ] **Performance & best practices:**
  - [ ] TypeScript types defined for components and props
  - [ ] Expensive components memoized with React.memo
  - [ ] Callbacks memoized with useCallback when needed
  - [ ] Animations use Reanimated worklets (not Animated API)
  - [ ] Safe areas handled with SafeAreaView or useSafeAreaInsets
  - [ ] Long lists use FlatList (not ScrollView + map)
  - [ ] Platform-specific code uses Platform.select
  - [ ] Tested on real devices (not just simulators)

## Escalation Guidance

When a design request cannot be satisfied with existing patterns:

1. **Push back early** - Explain performance and maintenance implications
2. **Propose alternatives** - Map to existing tokens or suggest new semantic tokens
3. **Add to design system** - If truly needed, add token to `gluestack-ui-provider/config.ts`
4. **Document exception** - If inline style is unavoidable, add JSDoc explaining why

## Reference Documentation

**IMPORTANT: Always verify component usage and patterns in the official v4 documentation before using components.**

The latest v4 components documentation and usage patterns are available at:

- **Component Docs**: `https://v4.gluestack.io/ui/docs/components/${componentName}/`
  - Example: `https://v4.gluestack.io/ui/docs/components/accordion/` for Accordion component
  - Always check the official docs for the latest API, props, sub-components, and usage patterns

For detailed information:

- [Gluestack UI v4 Documentation](https://v4.gluestack.io/ui/docs) - Complete component reference
- [Component Examples](./src/components/ui/) - Source code examples
- [Tailwind Config](./src/tailwind.config.js) - Available tokens and utilities
- [Provider Config](./src/components/ui/gluestack-ui-provider/config.ts) - Theme configuration

### Before Using Any Component

1. **Check official v4 docs** - Visit `https://v4.gluestack.io/ui/docs/components/${componentName}/` to verify:
   - Available sub-components and their correct usage
   - Required props and their types
   - Latest API changes and patterns
   - Accessibility features
   - Examples and best practices

2. **Verify import paths** - Ensure components are imported from `@/components/ui/${componentName}`

3. **Follow sub-component patterns** - Use the exact sub-component structure shown in the official docs
