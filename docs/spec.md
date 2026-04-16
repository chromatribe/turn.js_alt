# turn.js — Specification

**Version:** `2026-0416-01.00`

---

## Overview

turn.js is a lightweight jQuery plugin that adds a page-flip effect to DOM elements.  
This fork modernises the original 2012 codebase for standards-compliant browsers (2026 and beyond).

---

## Versioning Policy

| Segment | Format | Meaning |
|---------|--------|---------|
| Date | `YYYY-mmdd` | Calendar date of the release series |
| Daily sequence | `XX` (01–99) | Incremented for each independent release on the same date |
| Revision | `.XX` (00–99) | Incremented for patch-level fixes within the same sequence |

**Examples**

```
2026-0416-01.00   First release on 2026-04-16
2026-0416-01.01   Patch to the above
2026-0416-02.00   Second independent release on the same day
```

Rules:
- Version string is stored **only** in this file (`docs/spec.md`) and in `docs/CHANGELOG.md`.
- `spec/history/` layout is **forbidden**; version history lives exclusively in `docs/CHANGELOG.md`.
- Both files **must** be updated atomically in the same commit whenever the version changes.

---

## Requirements

| Dependency | Minimum version |
|------------|----------------|
| jQuery | 1.7 (3.x recommended) |
| Browser | Any modern browser with CSS transforms (Chrome 36+, Firefox 16+, Safari 9+, Edge 79+) |

---

## Options

### `turn(options)`

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `page` | `number` | `1` | Initial page |
| `pages` | `number` | `0` | Total page count (for dynamic/lazy loading) |
| `display` | `'single'` \| `'double'` | `'double'` | Layout mode |
| `duration` | `number` | `600` | Flip animation duration in milliseconds |
| `gradients` | `boolean` | `true` | Enable page-shadow gradients |
| `acceleration` | `boolean` | `true` | Enable CSS `translate3d` hardware acceleration |
| `elevation` | `number` | `0` | Lift offset (px) at which the page corner starts the turn animation |
| `corners` | `object` | see below | Override active corner sets |
| `when` | `object` | `null` | Event handler map (keys are event names) |

#### Default corner sets

```js
{
  backward: ['bl', 'tl'],
  forward:  ['br', 'tr'],
  all:      ['tl', 'bl', 'tr', 'br']
}
```

---

## Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `turn('page', n)` | `$el` | Navigate to page `n` with animation |
| `turn('next')` | `$el` | Turn to the next view |
| `turn('previous')` | `$el` | Turn to the previous view |
| `turn('stop')` | `$el` | Stop any running animation |
| `turn('destroy')` | `$el` | Remove all listeners and plugin data; cleans up DOM |
| `turn('addPage', el, n)` | `$el` | Insert a page element at position `n` |
| `turn('removePage', n)` | `$el` | Remove the page at position `n` |
| `turn('hasPage', n)` | `boolean` | Check whether page `n` is in the DOM |
| `turn('pages', n)` | `$el` / `number` | Set or get the total page count |
| `turn('page')` | `number` | Get the current page number |
| `turn('view')` | `number[]` | Get the currently visible page numbers |
| `turn('range', n)` | `number[]` | Get the `[start, end]` range of pages kept in the DOM |
| `turn('size', w, h)` | `$el` | Set width and height |
| `turn('display', mode)` | `$el` | Switch between `'single'` and `'double'` |
| `turn('disable', bool)` | `$el` | Enable or disable page turning |
| `turn('animating')` | `boolean` | Whether a flip is currently in progress |

---

## Events

Events are bound via the `when` option or jQuery's `.on()`.

| Event | Arguments | Fired when |
|-------|-----------|-----------|
| `turning` | `(e, page, view)` | Before a page turn begins |
| `turned` | `(e, page, view)` | After a page turn completes |
| `start` | `(e, opts, corner)` | User begins interacting with a corner |
| `end` | `(e, turned)` | Interaction ends; `turned` is `true` if the page flipped |
| `first` | `(e)` | The first page is in view |
| `last` | `(e)` | The last page is in view |
| `turn` | `(e, page)` | During the flip animation |

---

## Implementation Notes

- Corner activation maps are **per-instance** — multiple `.turn()` books on one page are fully independent.
- Touch `touchmove` / `touchend` listeners are registered with `{ passive: false }` so `preventDefault()` works.
- `animatef` uses `requestAnimationFrame`; no `setInterval` timers remain.
- Gradients use the standard `linear-gradient()` syntax (the old `-webkit-gradient(linear)` path has been removed).
- `turn('destroy')` removes `fwrapper`, `wrapper`, and (when unreferenced) `fparent` DOM nodes.
