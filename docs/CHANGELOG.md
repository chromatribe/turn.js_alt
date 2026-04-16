# CHANGELOG

All notable changes to this fork are recorded here.  
Version format: `YYYY-mmdd-XX.XX` — see `docs/spec.md` for the full versioning policy.

---

## [2026-0416-01.00] — 2026-04-16

### Added
- `turn('destroy')` method: removes namespaced document listeners and cleans up `fwrapper`/`wrapper`/`fparent` DOM nodes.
- `getPointerFromEvent` helper to unify mouse and touch coordinate extraction.
- `installPassiveTouchListeners` / `removePassiveTouchListeners`: registers `touchmove`/`touchend`/`touchcancel` with `{ passive: false }` so `preventDefault()` works during drags.
- `elevation` documented in the default options object.
- `turnUid` counter for per-instance event namespacing.
- `data.corners` per-instance corner-set map (eliminates cross-instance mutation).
- `docs/spec.md` — canonical specification and versioning policy.
- `docs/CHANGELOG.md` — this file.

### Changed
- **Corner maps** are now stored per-instance in `data.corners`; the global `corners` object is no longer mutated on `init`.
- **`animatef`**: replaced `setInterval` with `requestAnimationFrame`. Easing function argument renamed from `data` (shadowing outer variable) to `d`.
- **`gradient`**: removed the dead `-webkit-gradient(linear, …)` branch; all browsers now receive standard `linear-gradient()`. Width/height DOM reads reduced to one per call.
- **`getPrefix`**: works before `document.body` is ready; tries unprefixed `transform` first.
- **`opts.when`**: switched deprecated `.bind()` to `.on()`.
- **`resize`**: `offset()` calls cached to avoid double reflow in the same frame.
- **`has3d` detection**: guards against missing `document.body` with `!!()`.
- Demos updated to **jQuery 3.7.1** loaded via HTTPS with SRI integrity attribute.

### Removed
- Obsolete `-webkit-gradient(linear)` gradient code path.
- Global `corners` mutation from `turnMethods.init`.

---

## Legacy (upstream turn.js — pre-fork)

### Release 3 — 2012-03-01
- Added `range`, `addPage`, `removePage`, `hasPage`, `pages`, `display`
- Added `when`, `pages`, `inclination` options
- Added `first` / `last` events
- Added gradients for non-webkit browsers

### Release 2 — 2012-02-15
- Added `size`
- Fixed background-image loss in Chrome 17–18 Beta

### Release 1 — 2012-02-05
- First alpha release
