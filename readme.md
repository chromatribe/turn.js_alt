# turn.js

> A jQuery plugin that adds a **page-flip effect** to any set of DOM elements.  
> This fork modernises the original 2012 codebase for standards-compliant browsers.

**Current version:** `2026-0416-01.00`  
**Spec & versioning policy:** [`docs/spec.md`](docs/spec.md)  
**Change history:** [`docs/CHANGELOG.md`](docs/CHANGELOG.md)

---

## Quick start

```html
<!-- jQuery 3.x (or 1.7+) -->
<script src="https://code.jquery.com/jquery-3.7.1.min.js"
        integrity="sha256-/JqT3SQfawRcv/BIHPThkBvs0OEvtFFmqPF/lYI/Cxo="
        crossorigin="anonymous"></script>

<!-- turn.js -->
<script src="turn.min.js"></script>
```

```html
<div id="book">
  <div>Page 1</div>
  <div>Page 2</div>
  <div>Page 3</div>
  <div>Page 4</div>
</div>

<script>
$('#book').turn({
  display: 'double',
  duration: 600,
  gradients: true,
  when: {
    turned: function(e, page) {
      console.log('Now on page', page);
    }
  }
});

// Keyboard navigation
$(window).on('keydown', function(e) {
  if (e.key === 'ArrowRight') $('#book').turn('next');
  if (e.key === 'ArrowLeft')  $('#book').turn('previous');
});
</script>
```

---

## Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `page` | `number` | `1` | Initial page |
| `pages` | `number` | `0` | Total page count (use for lazy/dynamic loading) |
| `display` | `'single'` \| `'double'` | `'double'` | Single-page or two-page spread |
| `duration` | `number` | `600` | Flip duration in milliseconds |
| `gradients` | `boolean` | `true` | Page-shadow gradients |
| `acceleration` | `boolean` | `true` | CSS `translate3d` hardware acceleration |
| `elevation` | `number` | `0` | Corner lift offset (px) at turn start |
| `corners` | `object` | — | Override which corners are active |
| `when` | `object` | `null` | Event handler map |

---

## Methods

```js
$('#book').turn('page', 3);       // go to page 3
$('#book').turn('next');          // next view
$('#book').turn('previous');      // previous view
$('#book').turn('stop');          // cancel animation
$('#book').turn('destroy');       // full teardown
$('#book').turn('size', 800, 600);// resize
$('#book').turn('display', 'single');

// Getters
var page  = $('#book').turn('page');   // current page number
var view  = $('#book').turn('view');   // [left, right] visible pages
var range = $('#book').turn('range');  // [start, end] pages in DOM
var total = $('#book').turn('pages');  // total page count
var busy  = $('#book').turn('animating');
```

### Dynamic / lazy pages

```js
$('#book').turn({
  pages: 500,
  when: {
    turning: function(e, page) {
      var range = $(this).turn('range', page);
      for (var p = range[0]; p <= range[1]; p++) {
        if (!$(this).turn('hasPage', p)) {
          var el = $('<div/>').text('Page ' + p);
          $(this).turn('addPage', el, p);
        }
      }
    }
  }
});
```

---

## Events

| Event | Arguments | Description |
|-------|-----------|-------------|
| `turning` | `(e, page, view)` | Fired before a turn begins |
| `turned` | `(e, page, view)` | Fired after a turn completes |
| `start` | `(e, opts, corner)` | User touches / clicks a corner |
| `end` | `(e, turned)` | Interaction ends |
| `first` | `(e)` | First page is visible |
| `last` | `(e)` | Last page is visible |

---

## Demos

Open **[`demo.html`](demo.html)** for a single-page interactive showcase that covers all three modes below.

| Demo | Description |
|------|-------------|
| [`demos/magazine/`](demos/magazine/index.html) | Double-page magazine spread |
| [`demos/magazine_single/`](demos/magazine_single/index.html) | Single-page mode |
| [`demos/bible/`](demos/bible/index.html) | 1,000-page lazy-loaded book |

---

## Browser support

Any modern browser with CSS transforms: Chrome 36+, Firefox 16+, Safari 9+, Edge 79+.  
Touch devices (iOS Safari, Android Chrome) are fully supported.

---

## License

Original library © 2012 Emmanuel Garcia — see [`license.txt`](license.txt).  
Fork modifications released under the same terms.
