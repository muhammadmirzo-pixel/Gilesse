---
name: glisse
description: Build Apple Liquid Glass (iOS 26 style) interfaces in React with the `glisse` component library - glass buttons, switches, sliders, segmented controls, tab bars, menus, search fields, inputs and glass panels. Use whenever the user mentions glisse, Liquid Glass, glass/frosted-glass UI, glassmorphism, or iOS/Apple-style controls for a React web app (in any language, e.g. "liquid glass effekt", "iOS uslubidagi tugma", "стеклянная кнопка"), and whenever you build or restyle buttons, toggles, tabs, sliders, menus or search inputs in a project whose package.json already depends on `glisse`.
---

# glisse — Liquid Glass for React

`glisse` is a React (>= 18) component library that renders Apple's Liquid Glass material on the web: frosted, refracting glass with a lit rim, springy selection lenses and iOS 26 behaviour. Docs with live examples: https://glisse.kelvinzhang.ca

## 1. Check the setup first

1. Look in the app's `package.json`. If `glisse` is missing, install it in that app's folder (not the repo root):
   ```bash
   npm install glisse
   ```
2. `react` and `react-dom` >= 18 are peer dependencies.
3. **No CSS import is needed.** On first use the library injects its stylesheet as the *first* `<style>` in `<head>`, so the app's own CSS loads after it and wins ties. A static copy is also exported as `glisse/styles.css` if a setup really needs a stylesheet file.
4. The bundle starts with `"use client"`, so in Next.js App Router use it from client components.

## 2. Components

All are named exports: `import { Button, Switch, TabBar } from "glisse";`

| Component | Use for | Key props |
|---|---|---|
| `Button` | Actions | `variant="glass"` (default, clear) or `"filled"` (accent fill for the primary action), `icon`, `destructive`, `tint` (any CSS colour) |
| `Switch` | On/off settings | `checked` / `defaultChecked`, `onCheckedChange(checked)` |
| `Slider` | A number in a range | `value` / `defaultValue`, `onValueChange(n)`, `min`, `max`, `step` |
| `SegmentedControl` | Choosing one of 2–5 options in place | `items: {value, label}[]`, `value` / `defaultValue` (or `null`), `onValueChange(v)` |
| `TabBar` | Bottom navigation / top-level sections | `items: {value, label, icon?, href?}[]`, `value` / `defaultValue`, `onValueChange(v)`, `link` |
| `Menu` | An overflow "…" menu | `items: ({label, icon?, onSelect?, destructive?, disabled?} \| "separator")[]`, **required** `aria-label`, `icon`, `align="start" \| "end"` |
| `Input` | Text entry | `value` / `defaultValue`, `onValueChange(text)`, `icon` |
| `SearchField` | Search | Same as `Input`; comes with a magnifier, a "Search" placeholder and a clear button |
| `Glass` | A glass panel/card around your own content | Any `div` props |

Full prop reference with examples: [references/api.md](references/api.md).

## 3. Rules that are easy to get wrong

- **The change callbacks are not `onChange`.** Use `onValueChange` (Input, SearchField, Slider, SegmentedControl, TabBar) and `onCheckedChange` (Switch). They receive the value, not an event. Pair `value` with `onValueChange` for controlled use, or use `defaultValue` alone.
- **Accessible names.** A `Button` with only an `icon` becomes a circle: give it `aria-label`. `Menu` requires `aria-label`. A `Switch` without visible label text needs `aria-label`.
- **`destructive`** goes on actions that delete or can't be undone (Button, Menu item). It is shorthand for a red tint.
- **One `filled` button per view**, for the primary action. Everything else stays `glass`.
- **TabBar links pass `href`, not `to`.** With `link={Something}`, every tab that has `href` renders `<Something href=...>`. Next.js `Link` works as is. React Router's `Link` takes `to`, so wrap it:
  ```tsx
  import { forwardRef } from "react";
  import { Link } from "react-router";

  const RouterLink = forwardRef<HTMLAnchorElement, { href: string }>(
    ({ href, ...rest }, ref) => <Link ref={ref} to={href} {...rest} />,
  );

  <TabBar link={RouterLink} value={section} items={[
    { value: "home", label: "Home", icon: <HomeIcon />, href: "/" },
    { value: "profile", label: "Profile", icon: <UserIcon />, href: "/profile" },
  ]} />
  ```
  When every item has `href`, the bar becomes navigation. Derive `value` from the current route.
- **Glass needs something behind it.** Over a flat background it looks like a plain grey pill. Put it over imagery, gradients, maps or scrolling content (sticky headers, floating bottom bars).

## 4. Refraction in Safari, iOS and Telegram

Only Chromium can bend the *live* page behind an element. In WebKit (Safari, every iOS browser, Telegram/WebView on iOS) and Firefox, glass over unmarked content shows only the frost and the lit rim.

To get real refraction everywhere, mark the content that sits behind the glass with `data-glisse-backdrop`:

```tsx
<div style={{ position: "relative" }}>
  <img data-glisse-backdrop src="/hero.jpg" alt="" />
  <Glass style={{ position: "absolute", inset: "auto 16px 16px 16px", padding: 16 }}>
    Caption on glass
  </Glass>
</div>
```

The glass uses the closest marked element whose visible area it overlaps.

## 5. Theming

- **Light/dark:** the library follows `prefers-color-scheme`, and `data-theme="light" | "dark"` on `<html>` (or on any ancestor) overrides it. If the app already sets `data-theme`, glisse follows it automatically. Changes are picked up live.
- **Tokens:** everything is a CSS custom property declared with `:where()` (zero specificity), so plain overrides win. Set them on `:root` or scope them to a container:
  ```css
  :root {
    --lg-fill: #6c47ff;       /* accent: filled buttons, slider fill, selected tab */
    --lg-on: #34c759;         /* switch "on" colour */
    --lg-destructive: #ff383c;
  }
  [data-theme="dark"] { --lg-fill: #8b6cff; }
  ```
  Other useful tokens: `--lg-foreground`, `--lg-muted`, `--lg-glass` (glass tint), `--lg-surface` (menu surface), `--lg-separator`, `--lg-ring` (focus ring).
- **Glass strength:** `--lg-glass-bend`, `--lg-glass-dispersion` (colour split) and `--lg-rim-shine` tune the glass. They are read each time it renders, so you can set them from React on the element or an ancestor.
- Prefer the `tint` prop over CSS when you only need one coloured button.
- `prefers-reduced-motion` is respected out of the box. Don't add your own transitions on these components.

## 6. Fit it into the existing app

- Keep the app's own design tokens. Map the accent with `--lg-fill` instead of restyling component internals (`.lg-*` classes are implementation details and may change).
- Pass `className` / `style` for layout (margin, width, position). Don't override colours, radii or shadows there.
- Replace only what the user asked for. Don't swap every native control for glass on your own.
- After changes, run the app's typecheck/build. Types ship with the package (`ButtonProps`, `TabBarItem`, `MenuEntry`, ...).
