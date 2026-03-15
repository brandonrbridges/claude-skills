---
name: shad
description: Use when building UI with shadcn/ui components, implementing forms, theming, dark mode, or composing layouts. Enforces Tailwind v4, OKLCH theming, correct composition patterns, and accessibility best practices.
---

# Shad

Build UI with shadcn/ui the right way. Tailwind v4, OKLCH colors, proper composition, accessible by default.

## Non-Negotiable Rules

1. **Tailwind v4 only.** No `tailwind.config.ts`. No `@layer base`. Use `@theme inline` for custom tokens. Use `@import "tailwindcss"` not `@tailwind base/components/utilities`.

2. **OKLCH color format.** Not HSL. CSS variables use raw OKLCH values. The `@theme inline` block maps them to Tailwind utilities.
   ```css
   :root {
     --primary: oklch(0.21 0.006 285.88);
   }
   @theme inline {
     --color-primary: var(--primary);
   }
   ```

3. **CSS variables for all colors.** Every semantic color needs a `--foreground` pair. Use `bg-primary text-primary-foreground`, never raw color values like `bg-blue-500`.

4. **`data-slot` on all component roots.** Every component's outermost element gets a `data-slot` attribute matching its purpose. This is how shadcn/ui v2 components work.

5. **No `forwardRef`.** Use `React.ComponentProps<"element">` directly. React 19 handles ref forwarding natively.

6. **`cn()` for all className merging.** Import from your utils. Never concatenate class strings manually.

7. **Radix primitives for behavior.** Dialogs, popovers, dropdowns, tooltips, selects — always use the Radix-based shadcn component. Never build these from scratch.

8. **`size-*` over `h-* w-*`.** Use `size-4` not `h-4 w-4`. Tailwind v4 (and v3.4+) supports this.

## Theming

### CSS Variable Set

Every project needs these defined in both `:root` and `.dark`:

```
--background / --foreground
--card / --card-foreground
--popover / --popover-foreground
--primary / --primary-foreground
--secondary / --secondary-foreground
--muted / --muted-foreground
--accent / --accent-foreground
--destructive / --destructive-foreground
--border
--input
--ring
--radius
--chart-1 through --chart-5
```

Sidebar-heavy apps also need:
```
--sidebar / --sidebar-foreground
--sidebar-primary / --sidebar-primary-foreground
--sidebar-accent / --sidebar-accent-foreground
--sidebar-border / --sidebar-ring
```

### Dark Mode

Use `next-themes` with `attribute="class"`. Wrap app in `ThemeProvider`. Add `suppressHydrationWarning` to `<html>`. Define `.dark` overrides for all CSS variables.

### Custom Colors

To add a new semantic color (e.g. `warning`):
1. Define `--warning` and `--warning-foreground` in `:root` and `.dark`
2. Register in `@theme inline`: `--color-warning: var(--warning);`
3. Use as `bg-warning text-warning-foreground`

## Component Patterns

### Forms

Always use shadcn `Form` + react-hook-form + Zod:

```tsx
<Form {...form}>
  <FormField
    control={form.control}
    name="email"
    render={({ field }) => (
      <FormItem>
        <FormLabel>Email</FormLabel>
        <FormControl>
          <Input {...field} />
        </FormControl>
        <FormMessage />
      </FormItem>
    )}
  />
</Form>
```

Never use uncontrolled forms or raw `<input>` elements when shadcn Form is available.

### Responsive Dialogs

Use `Dialog` on desktop, `Drawer` on mobile:

```tsx
const isDesktop = useMediaQuery("(min-width: 768px)")
return isDesktop ? <Dialog>...</Dialog> : <Drawer>...</Drawer>
```

### Loading States

Use `Skeleton` components that mirror the loaded UI shape. Never use spinners where skeleton previews are possible.

### Toasts

Use `sonner` (shadcn's default toast). Call `toast()` directly — don't build custom toast UIs.

## Installing Components

```bash
npx shadcn@latest add button card dialog    # specific components
npx shadcn@latest add --all                  # everything
```

Always check that required shadcn dependencies exist before using a component. If a component imports from `@/components/ui/button` and button isn't installed, install it first.

## Common Mistakes

| Mistake | Fix |
|:--------|:----|
| Using `hsl()` color format | Use OKLCH — shadcn v4 default |
| Writing `tailwind.config.ts` | Use `@theme inline` in CSS for Tailwind v4 |
| Building custom modals/dropdowns | Use shadcn Dialog/DropdownMenu (Radix-based) |
| `h-4 w-4` on icons | `size-4` |
| Raw color classes (`bg-blue-500`) | Semantic tokens (`bg-primary`) |
| String concatenation for classes | `cn()` utility |
| `React.forwardRef` | `React.ComponentProps<"div">` — React 19 |
| Spinner for loading | `Skeleton` matching the content shape |
| `useToast` hook | `sonner` toast — simpler, shadcn default |

## Reference

For component API details, check the project's installed components in `components/ui/` — they're your source of truth since you own the code. For new components: https://ui.shadcn.com/docs/components
