# Guide to migrate from LumexUI v1 to v2

This guide covers **only breaking changes** between v1 and v2.
For a full list of changes, see the [v2.0 release notes](https://github.com/LumexUI/lumexui/releases).

## ✅ Migration checklist (TL;DR)

- Upgrade to Tailwind CSS v4 (v3 is no longer supported).
- Migrate from C# theme config → CSS variables for theming.
- Replace usage of `LumexIcon` and built-in Google Material Icons with your own icon library (e.g. Lucide, Boxicons).
- Update slot customizations: rename `Root` → `Base`.

## Overview

### Tailwind CSS v3 support dropped

LumexUI v2 requires Tailwind CSS v4. Only features supported in Tailwind v4 are compatible.

👉 Follow the official [Tailwind upgrade guide](https://tailwindcss.com/docs/upgrade-guide) to migrate your project.


### Theming system migration

In v2, the old C# theme configuration has been removed.
LumexUI now uses a CSS-first approach with design tokens exposed as CSS variables.

**What to do:**

1. **Remove the old C# theme config**

```diff
- record Theme : LumexTheme
- {
-     public Theme()
-     {
-         Light = new ThemeConfigLight()
-         {
-             Colors = new ThemeColorsLight()
-             {
-                 Primary = new ColorScale( Colors.Orange ),
-                 Focus = new ColorScale( Colors.Orange["500"] )
-             }
-         };
-     }
- }
```

2. **Override theme via `--lumex-*` CSS variables in your project’s stylesheet:**

```css

/* `:root` = light theme, `.dark` = dark theme */

:root {
    /* Primary color */
    --lumex-primary-50: var(--color-orange-50);
    --lumex-primary-100: var(--color-orange-100);
    --lumex-primary-200: var(--color-orange-200);
    --lumex-primary-300: var(--color-orange-300);
    --lumex-primary-400: var(--color-orange-400);
    --lumex-primary-500: var(--color-orange-500);
    --lumex-primary-600: var(--color-orange-600);
    --lumex-primary-700: var(--color-orange-700);
    --lumex-primary-800: var(--color-orange-800);
    --lumex-primary-900: var(--color-orange-900);

    /* Focus */
    --lumex-focus: var(--color-orange-500);
}
```

You can find more documentation on [design tokens](https://lumexui.org/docs/theming/design-tokens) and [components customization](https://lumexui.org/docs/theming/customization).

> [!NOTE]  
> The `--color-*` CSS variables are [defined by Tailwind](https://tailwindcss.com/docs/colors#default-color-palette-reference).

### Icons

In v2, [Google Material Icons](https://fonts.google.com/icons) and the `LumexIcon` component were removed, allowing consumers to decide which icon library to use.
This change also has significantly reduced the bundle size.

If you were using the LumexIcon component and the previously built-in Google Material Icons, 
you will now need to handle icons yourself using your preferred icon library, such as [Lucide](https://lucide.dev/) or [Boxicons](https://boxicons.com/):

```diff
- <LumexIcon Icon="@Icons.Rounded.Favorite" />

+ <svg xmlns="http://www.w3.org/2000/svg"
+      width="24"
+      height="24"
+      viewBox="0 0 24 24"
+      focusable="false"
+      fill="none"
+      stroke="currentColor"
+      stroke-width="2"
+      stroke-linecap="round"
+      stroke-linejoin="round">
+     <path d="M19 14c1.49-1.46 3-3.21 3-5.5A5.5 5.5 0 0 0 16.5 3c-1.76 0-3 .5-4.5 2-1.5-1.5-2.74-2-4.5-2A5.5 5.5 0 0 0 2 8.5c0 2.3 1.5 4.05 3 5.5l7 7Z" />
+ </svg>
```

### Slot Renames (`Root` -> `Base`)

In v2, the `Root` slot was renamed to `Base`. If you were customizing slots of any components, make sure to rename `Root` to `Base`:

```diff
private ComponentSlots _slots = new()
{
-     Root = " ... "
+     Base = " ... "
};
```
