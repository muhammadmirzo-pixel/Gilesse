# glisse API reference (v0.6)

Every component forwards unknown props to its root element (except `Menu`, which takes only the props listed). Types are exported under the same names, for example `import type { TabBarItem } from "glisse"`.

## Button

Extends `<button>` attributes.

| Prop | Type | Notes |
|---|---|---|
| `variant` | `"glass" \| "filled"` | `glass` is clear. `filled` uses the accent colour (`--lg-fill`), for the primary action. |
| `icon` | `ReactNode` | Shown before the label. Icon only means a circle button, so add `aria-label`. |
| `destructive` | `boolean` | Red tint, for delete or irreversible actions. |
| `tint` | `string` | Any CSS colour. Tints a glass button's label, or a filled button's fill. |

```tsx
<Button variant="filled" onClick={save}>Save</Button>
<Button icon={<Trash2 />} aria-label="Delete" destructive onClick={remove} />
<Button tint="#ff9500">Snooze</Button>
```

## Switch

Extends `<button>` attributes, except `value`, `defaultValue`, `defaultChecked` and `onChange`.

| Prop | Type |
|---|---|
| `checked` | `boolean` |
| `defaultChecked` | `boolean` |
| `onCheckedChange` | `(checked: boolean) => void` |

```tsx
const [on, setOn] = useState(true);
<Switch aria-label="Notifications" checked={on} onCheckedChange={setOn} />
```

## Slider

Extends `<input>` attributes, except `type`, `value`, `defaultValue`, `onChange`, `min`, `max` and `step`, which are redefined:

| Prop | Type |
|---|---|
| `value` / `defaultValue` | `number` |
| `onValueChange` | `(value: number) => void` |
| `min` / `max` / `step` | `number` |

```tsx
<Slider aria-label="Volume" min={0} max={100} defaultValue={40} onValueChange={setVolume} />
```

## SegmentedControl

Extends `<div>` attributes, except `defaultValue` and `onChange`.

| Prop | Type | Notes |
|---|---|---|
| `items` | `{ value: string; label: ReactNode }[]` | |
| `value` / `defaultValue` | `string \| null` | `null` means nothing is selected. |
| `onValueChange` | `(value: string) => void` | |

Holding or dragging the thumb lifts it into a lens over the labels, as in iOS 26.

```tsx
<SegmentedControl
  aria-label="Period"
  items={[{ value: "day", label: "Day" }, { value: "week", label: "Week" }, { value: "month", label: "Month" }]}
  value={period}
  onValueChange={setPeriod}
/>
```

## TabBar

Extends `<div>` attributes, except `defaultValue` and `onChange`.

| Prop | Type | Notes |
|---|---|---|
| `items` | `{ value: string; label: ReactNode; icon?: ReactNode; href?: string }[]` | `icon` is drawn above the label. |
| `value` / `defaultValue` | `string \| null` | |
| `onValueChange` | `(value: string) => void` | |
| `link` | `ElementType` | Rendered for tabs with `href`, receiving `href`. Defaults to `"a"`. |

When every item has `href`, the bar is navigation. The selection springs between tabs, follows a drag and lifts into a lens while held.

```tsx
<TabBar
  aria-label="Sections"
  value={tab}
  onValueChange={setTab}
  items={[
    { value: "home", label: "Home", icon: <House /> },
    { value: "history", label: "History", icon: <Clock /> },
    { value: "settings", label: "Settings", icon: <Settings /> },
  ]}
/>
```

For a React Router adapter that maps `href` to `to`, see SKILL.md section 3.

## Menu

| Prop | Type | Notes |
|---|---|---|
| `items` | `(MenuItem \| "separator")[]` | `MenuItem = { label: ReactNode; icon?: ReactNode; onSelect?: () => void; destructive?: boolean; disabled?: boolean }` |
| `aria-label` | `string` | **Required.** Names the trigger and the menu. |
| `icon` | `ReactNode` | Trigger icon. Defaults to an ellipsis. |
| `align` | `"start" \| "end"` | The trigger edge the menu lines up with. |
| `className`, `style` | | Applied to the root. |

```tsx
<Menu
  aria-label="More actions"
  align="end"
  items={[
    { label: "Edit", icon: <Pencil />, onSelect: edit },
    { label: "Share", icon: <Share />, onSelect: share },
    "separator",
    { label: "Delete", icon: <Trash2 />, destructive: true, onSelect: remove },
  ]}
/>
```

## Input

Extends `<input>` attributes, except `value`, `defaultValue` and `onChange`.

| Prop | Type |
|---|---|
| `value` / `defaultValue` | `string` |
| `onValueChange` | `(value: string) => void` |
| `icon` | `ReactNode`, shown before the text |

```tsx
<Input icon={<Mail />} type="email" placeholder="Email" value={email} onValueChange={setEmail} />
```

## SearchField

`Input` without `type`. Adds a magnifier icon, a "Search" placeholder and a clear button.

```tsx
<SearchField value={query} onValueChange={setQuery} />
```

## Glass

A `<div>` with the glass material (`HTMLAttributes<HTMLDivElement>`). Use it for cards, floating bars, sheets and toolbars that hold your own content.

```tsx
<Glass style={{ position: "sticky", top: 8, padding: 12, borderRadius: 24 }}>
  <h2>Balance</h2>
</Glass>
```

## CSS custom properties

Declared with `:where()`, so any rule of yours overrides them. Light values apply on `:root` and `[data-theme="light"]`. Dark values apply on `[data-theme="dark"]`, and under `prefers-color-scheme: dark` unless `data-theme="light"` is set.

| Token | Meaning |
|---|---|
| `--lg-fill` | Accent: filled buttons, slider fill, selected tab in a TabBar |
| `--lg-on` | Switch "on" colour |
| `--lg-destructive` | Destructive red |
| `--lg-foreground`, `--lg-muted` | Text colours |
| `--lg-glass`, `--lg-glass-active` | Glass tint, at rest and pressed |
| `--lg-surface`, `--lg-surface-edge` | Menu surface |
| `--lg-separator`, `--lg-highlight` | Menu separators, hovered row |
| `--lg-track` | Switch track (off) |
| `--lg-rail` | Slider track |
| `--lg-segment-track`, `--lg-segment-thumb`, `--lg-segment-label` | Segmented control |
| `--lg-ring` | Focus ring |
| `--lg-shadow`, `--lg-edge`, `--lg-drop` | Shadows and edges |
| `--lg-glass-bend`, `--lg-glass-dispersion`, `--lg-rim-shine`, `--lg-lens-shine` | Glass optics |
