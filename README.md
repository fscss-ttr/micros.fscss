# micros.fscss

> Shared micro-helpers for FSCSS modules — small defines everyone was rewriting by hand.

**Requires FSCSS ≥ 1.2.1** (`fscss:name` registry imports).

| Helper | Role |
|--------|------|
| `mirror` | Index array sized to another array’s length |
| `scoped-reset` | Zero margin/padding + `border-box` on a selector and descendants |
| `stagger` | `index * step` → delay value (e.g. `0.75s`) |
| `stagger-items` | Apply staggered `animation-delay` using a caller-owned index array |
| `alpha` | `color-mix` fade toward transparent |

---

## Install / import

**Registry name** (no quotes) - because it has been published into the FSCSS module space:

```css
@import((mirror, stagger, alpha) from fscss:micros)
```
Or initialize all
```
@import(exec(_init fscss:micros))
```

**URL** (quotes required):

```css
@import((mirror) from "https://cdn.jsdelivr.net/gh/fscss-ttr/FSCSS@main/xf/styles/fscss/micros.fscss")
```

**Local / monorepo:**

```css
@import((*) from "./micros.fscss")
```

Runtime example:

```html
<script src="https://cdn.jsdelivr.net/npm/fscss@1.2.1/runtime.min.js" async></script>
```

CLI:

```bash
fscss page.fscss page.css
```

---

## Helpers

### `mirror(data, idx)`

Builds an index array `1..N` matching `data.length`.

```css
@arr colors[#1E2783, #8C29B2, #C41348]
@mirror(colors, index)

exec(_log, "@arr.index")
/* → index list sized to colors */
```

Use the index with loops, `rpt`, or `stagger-items`.

> If parameterized array names fail in your build, log first:  
> `exec(_log, "@arr.index!.list")`

---

### `scoped-reset(st)`

```css
@scoped-reset(.siri-wave)
@scoped-reset(.flux-root)
```

Expands roughly to:

```css
.siri-wave,
.siri-wave * {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
```

---

### `stagger(idx, step?)`

Value helper for a single delay (default step `0.2`):

```css
animation-delay: @stagger(1, 0.2);   /* 0.2s */
animation-delay: @stagger(3, 0.25);  /* 0.75s */
animation-delay: @stagger(@arr.index[], 0.15);
```

---

### `stagger-items(sel, idxArr, step?)`

Caller passes a **pre-sized** index array (from `mirror` or hand-built). No internal `@arr`, so multiple calls on one page are safe.

```css
@arr bands[a, b, c, d]
@mirror(bands, bi)
@stagger-items(.blob-, bi, 0.15)

@arr panels[x, y, z]
@mirror(panels, pi)
@stagger-items(.panel-, pi, 0.3)
```

---

### `alpha(color, amt?)`

```css
background: @alpha(#8f6bff, 35%);
background: @alpha(var(--st-accent), 40%);
```

→ `color-mix(in srgb, … amt, transparent)`.

---

## Why these five

Pulled from real module code, not aliases of builtins:

1. **mirror** — same `count(length)` index line in every loop mixin  
2. **scoped-reset** — copy-pasted `-base()` blocks  
3. **stagger / stagger-items** — hand-written delay lists that were just `i * step`  
4. **alpha** — repeated `color-mix(..., transparent)` fills/glows  

---

## Repo layout

```text
micros/
├── micros.fscss
├── package.json
├── README.md
└── samples/
    └── test-log.html
```

Publish under [fscss-ttr/FSCSS](https://github.com/fscss-ttr/FSCSS).

---

## package.json surface

Helpers listed for tooling:

`mirror` · `scoped-reset` · `stagger` · `stagger-items` · `alpha`

```json
"fscss_version": "^1.2.1"
```

---

## License

MIT

