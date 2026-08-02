# Satori Reader - mobile layout (capture pass 2, section 2)

## Provenance

| | |
|---|---|
| Article page | https://www.satorireader.com/articles/bartender-episode-1-edition-n |
| Series page | https://www.satorireader.com/series |
| Viewport measured | **500 x 751 CSS px, devicePixelRatio 2** |
| Viewport requested | 390 x 844 |
| Captured | 2026-08-02 |
| Account | free account, episode 1 only (free content) |
| Themes | dark and light, both from the same page load |

### Stated gap: the viewport is 500 x 751, not 390 x 844

The window could not be resized in this session, and the tab's emulated device
metrics could not be changed either. What follows was measured at the mobile
layout the browser actually produced, 500 x 751 CSS px at dpr 2. Satori's mobile
layout is fully engaged at this width: the desktop nav and the desktop audio bar
are both `display: none`, and `#nav-mobile` is live. So the *structure*
below is the real mobile structure. The *numbers* are for a 500 px viewport and
should be re-taken at 390 before being trusted as pixel values.

One further distortion: `body` measures **485**, not 500, because the emulated
window paints a 15 px classic scrollbar. On a real touch device body would equal
the full viewport width and every x / width below gains 15 px of room.

---

## 1. Where does the audio bar sit on mobile?

**It stops being an audio bar and becomes part of the top nav.**

On desktop the player is `.article-viewer-audio-controls-large-container`, a block
870 px wide starting at x = 393 - constrained to the article column, not
full-bleed. At mobile width that element is `display: none`.

What replaces it is `#nav-mobile`: a `position: fixed`, `z-index: 100`,
full-bleed orange bar, 485 x 50 (dark) / 485 x 52 (light), laid out as a CSS
table with four cells:

| cell | width | contents |
|---|---|---|
| `#nav-mobile-menu-button` | 45 | hamburger, opens `#nav-mobile-menu` popdown |
| `#nav-mobile-settings-button` | 40 | gear, opens `#nav-mobile-settings` popdown |
| `#nav-mobile-audio-player-container` | 357 | `#audio-controls-mobile.audio-controls` |
| `#nav-mobile-home-button` | 43 | Satori logo |

The player inside that middle cell keeps the same internal parts as desktop, just
squeezed:

| part | desktop | mobile |
|---|---|---|
| auto-scroll button | 48 x 50 | 30 x 50 (padding 0 15px), opacity 0.45 when off |
| play / pause | 40 x 50 | 40 x 50 |
| speed stepper | 51.49 x 50 | 46.49 x 50, indicator 8 x 22, text 28.49 x 22 |
| timeline container | 730.51 x 50 | 240.51 x 50 |
| timeline track | 710.51 x 10, radius 15px | 230.51 x 10, radius 15px |
| playhead | 14 x 14, radius 50% | 14 x 14, radius 50% |

So the design decision to copy is: **on narrow screens the transport controls are
promoted into the persistent app chrome rather than kept with the article.** The
bar never scrolls away, and the article scrolls underneath it.

Note the series list uses the same `#nav-mobile` bar but with no gear cell and an
empty player cell (397 wide), so the bar is hamburger + empty space + logo.

---

## 2. Does the popup become a bottom sheet?

**No. It stays exactly what it is on desktop: an absolutely positioned panel in
the flow of the article, anchored directly below the line containing the tapped
word, spanning the full column.**

Measured with the long OTHER NOTE on 苦手な方 open:

| | |
|---|---|
| `.tooltip` | 465 wide, x = 10, `position: absolute`, `z-index: 1` |
| gutters | 10 px each side - the popup is exactly as wide as the article column |
| top edge | 36 px below the top of the tapped word's line box |
| height | 1437.5 px, i.e. nearly 2x the 751 px viewport |
| border | 2px solid rgb(51,51,51) dark / rgb(144,148,151) light |
| background | rgb(102,102,102) dark / rgb(249,249,249) light |
| caption bar | `.heading` 459 x 20, 13px, white on rgb(255,147,0), padding 2px 10px |
| close control | `.tooltip-close` 20.16 x 25, top right - the only way to dismiss it |

There is no sheet, no backdrop, no drag handle, no independent scroll container.
A long note simply makes the article taller and you scroll the page through it.
For a 465 px column that means a single note can be two screens tall, and the
close X can end up far above the fold.

---

## 3. Body text size, line-height, column width, gutters

| | mobile (500 vp) | desktop (1920 vp) |
|---|---|---|
| body paragraph font-size | **24px** | 24px |
| body line-height | **60px** (2.5x) | 60px |
| font-family | `"MS Mincho"` | same |
| column width | **465** | 870 |
| side gutters | **10 px** (`.content-safe { margin: 0 10px }`) | centred column |
| top / bottom padding | 20px 0 (`.content-with-control-panel`) | same |
| paragraph spacing | `margin: 0 0 24px` | same |
| sentence trailing gap | `margin-right: 35px` | same |
| headline paragraph | 36px / 80px, padding 21px 20px 15px, margin-bottom 40px | same type scale |

**The type does not shrink on mobile.** 24px Japanese body text at 60px leading is
carried over unchanged from desktop; only the measure narrows, from 870 to 465.
That is roughly 19 CJK characters per line instead of 36.

The reading surface is effectively edge to edge: 10 px of gutter on a 485 px body.
There is no max-width and no centring at this size.

---

## 4. Do the per-sentence controls stay inline, and at what tap size?

**They stay inline, in the text flow, at exactly their desktop size - which is
below the usual 44 px minimum touch target.**

| control | selector | box | opacity |
|---|---|---|---|
| play-before-sentence (再) | `.play-button.play-button-standard` | **32 x 41** | 1 |
| reveal-translation (訳) | `.notes-button.notes-button-standard` | **30 x 38.5** | 0.9 |

Both are `display: inline` glyph spans with transparent text colour and a PNG
sprite background (`play-sentence-off.png`, `sentence-notes-off.png`), sitting on the
80px line box, so vertically they get some slop from the leading but horizontally
they are only 30-32 px wide. Identical numbers to the desktop capture: there is
no mobile-specific enlargement, and no dark-theme override.

Worth flagging for the Norwegian app: if we copy this literally we inherit a
sub-44px inline tap target. Satori gets away with it because the targets sit in
generous 60-80px line boxes, but it is a deliberate choice to re-examine rather
than copy.

---

## Other things visible at this width

**Preferences is a popdown, not a page.** The gear opens `#nav-mobile-settings`,
`position: absolute` directly under the bar, 485 x 254, three levels deep:

- `.tab-set.primary` 475 x 35 - Display / My Status / Editions
- `.tab-set.secondary` 475 x 40 - Kanji / Furigana / Spaces
- `.leaf-set` 475 x 127 containing `.selection` rows 465 x 39, padding 10px 12px,
  font-weight 700, the active one carrying `.on`
- a footer line, "Make these settings my defaults."

The whole popdown is orange-on-orange: rgb(68,68,68) container in dark theme,
rgb(245,122,0) in light, with progressively darker oranges for the nested levels
(secondary rgb(143,71,0), leaf rgb(196,89,0) dark / rgb(143,71,0) light,
selection rgb(155,69,4) dark / rgb(169,84,0) light).

**Series list goes to two columns.** Tiles are `.tile-w.tile-u-1-1.tile-u-sm-1-2`
with `.tile-ar.ar-2-1`, i.e. a 2:1 aspect ratio box, 239 x 119 at this width, two
per row starting at x = 4. Episode counts sit as a chip in the top-right of each
tile. The "Recently added" carousel stays full width above the grid.

**`.article-controls` still exists in the DOM but is 0 x 0.** The desktop side
control panel is simply not laid out on mobile; its functions moved into
`#nav-mobile`.

---

## Files this produced

| file | what |
|---|---|
| `computed-styles-mobile-dark.json` | 48 selectors, getComputedStyle + rects, dark |
| `computed-styles-mobile-light.json` | same 48 selectors, light |
| `shots/mobile-reader.png` | article body scrolled, nav bar + player visible |
| `shots/mobile-reader-popup.png` | 苦手な方 OTHER NOTE open, showing anchoring and column span |
| `shots/mobile-series-list.png` | two-column series grid |
| `shots/mobile-preferences.png` | gear popdown, Display > Kanji |

### Stated gaps

1. **Viewport is 500 x 751, not 390 x 844.** See above. Structure is right,
   pixel values are for a 500 px viewport.
2. **The four `shots/mobile-*.png` files are JPEG data with a `.png` extension.**
   The capture tool returns JPEG; the extension was kept to match the naming in
   the request. Same caveat as the `annotations/*.png` files from section 1.
3. **No real touch device was used.** Everything is Chrome desktop with emulated
   metrics, so `:hover` still exists and no touch-specific media query
   (`pointer: coarse`) was exercised. Satori's breakpoints here are width-based,
   but a real device could still differ.
4. **Only one article was measured** (bartender episode 1) plus the series list.
   Series *detail* and the article footer were not re-shot at mobile width.
5. `#nav-mobile-menu` (the hamburger popdown) was measured while closed, so its
   box reads 0 x 0. Only the gear popdown was opened and measured.
