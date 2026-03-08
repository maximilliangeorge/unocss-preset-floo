# unocss-preset-floo

A [UnoCSS](https://unocss.dev) preset for fluid responsive design. Define responsive values with simple expressions that generate CSS `calc()` functions for smooth, viewport-aware scaling — no complex media query math required.

[Try the playground](https://maximilliangeorge.github.io/uno-preset-floo/)

## Install

```bash
pnpm add -D unocss-preset-floo
```

## Setup

```ts
// uno.config.ts
import { defineConfig, presetUno } from 'unocss'
import { presetFloo } from 'unocss-preset-floo'

export default defineConfig({
  presets: [
    presetUno(),
    presetFloo(),
  ],
})
```

## Usage

Prefix any arbitrary value with `~` to create a fluid expression:

```html
<!-- Scale: value scales linearly with viewport -->
<h1 class="md:text-[~48px]">Fluid heading</h1>

<!-- Range: interpolate between two values across a breakpoint range -->
<p class="md:text-[~16px-24px]">Grows from 16px to 24px</p>

<!-- Dampened: scales at a reduced rate -->
<div class="md:p-[~32px/2]">Half-rate padding</div>
```

Works with any utility that accepts arbitrary values — `w-`, `h-`, `p-`, `m-`, `gap-`, `text-`, etc.

## Expressions

### Scale — `~{value}{unit}`

Scales linearly with viewport width relative to the ideal frame width.

```
md:text-[~48px]
→ calc(48px * 100vw / 768px)
```

### Range — `~{start}{unit}-{end}{unit}`

Interpolates between two values across the current breakpoint's range.

```
md:text-[~16px-24px]
→ calc(16px + 8px * (100vw - 768px) / 512px)
```

### Dampened — `~{value}{unit}/{factor}`

Scales at a reduced rate controlled by the dampening factor.

```
md:text-[~48px/2]
→ calc(48px * (1 + (100vw - 768px) / 768px / 2))
```

## Configuration

### Ideals

Each breakpoint has an **ideal** — the Figma frame width where that breakpoint's design looks pixel-perfect. Expressions scale relative to these ideals.

Defaults:

| Breakpoint | Ideal |
|------------|-------|
| `_` (default) | 375px |
| `sm` | 390px |
| `md` | 768px |
| `lg` | 1280px |
| `xl` | 1440px |

Override or extend with the `ideals` option:

```ts
presetFloo({
  ideals: {
    _: '360px',
    md: '800px',
    '2xl': '1920px',
  },
})
```

Ideal keys must match your theme's breakpoint keys (except `_`, which is the default/mobile breakpoint).

## Supported Units

`px`, `rem`, `em`

## Universal Selector Variant

Use `*:` to apply fluid values without a breakpoint prefix:

```html
<div class="*:text-[~16px]">Always fluid</div>
```

## Development

```bash
pnpm install        # Install dependencies
pnpm build          # Build the library
pnpm test           # Run tests
pnpm test:watch     # Run tests in watch mode
pnpm playground     # Launch interactive playground
pnpm typecheck      # Type-check
```

## License

MIT
