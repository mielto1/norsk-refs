# satori/states - interaction states

Capture pass 2, 2026-08-02. Article
<https://www.satorireader.com/articles/bartender-episode-1-edition-n> (episode 1,
free) at **1920 x 889 CSS px, devicePixelRatio 2**, in both themes. Same article
and viewport as the existing `satori/computed-styles-light.json` /
`-dark.json`, so the numbers here are directly comparable.

## Files

| file | what it holds |
| --- | --- |
| `underline-kinds.md` | the two article-body underline styles, their selectors, and which annotation kind each marks, with the evidence |
| `computed-styles-states-dark.json` | computed styles: word resting / hover / selected, per-sentence play + translation glyph resting and hover, whole audio bar, popup container |
| `computed-styles-states-light.json` | same, light theme |
| `frontend-css-extract.css` | the rule blocks from frontend.css that govern all of the above |

## Themes

Light is the default. `frontend.css` has **no `.theme-light` selector at all**;
there are 238 rule blocks prefixed `body.theme-dark`. Switching theme is
therefore adding or removing one class on `<body>`, which is how both JSON files
were produced.

## Words: resting, hover, selected

A resting word already carries `border-bottom: solid 5px transparent` with
`padding-bottom: 3px`, so hover and selection can colour a 5px underline without
moving any text. The two annotation underlines are thinner (2px) and sit in that
same reserved space.

| state | selector | dark | light |
| --- | --- | --- | --- |
| resting | `.paragraph.body .word` | `5px solid rgba(0,0,0,0)` | same |
| hover | `.paragraph.body .word:hover` | `5px solid rgb(190,224,241)` | same |
| tapped / selected | `.paragraph.body .word.word-selected` | border + text `rgb(30,144,255)`, background `rgb(44,46,51)` | border `rgb(190,224,241)`, text `rgb(49,112,143)`, background `rgb(255,255,255)` |

The hover colour `#bee0f1` is shared by both themes - there is no dark override
for `.word:hover`, only for `.word.word-selected`. In dark mode the selected
state also switches the border colour to match the text (`#1e90ff`) via
`border-color: ... !important`, so selected and hover look different; in light
mode they share the same pale border and differ only in text colour and
background.

The class applied on tap is `word-selected`, on the `.word` span itself. Only one
word carries it at a time.

One measurement caveat, recorded because it will bite anyone repeating this:
**synthetic mouse clicks delivered through the browser-automation layer do not
open the popup on this site.** The events arrive correctly (pointerdown,
mousedown, pointerup, mouseup, click, all on the inner `.wpt` span at the right
coordinates, `button: 0`, `detail: 1`) and the `.word` ancestor has an inline
`onclick`, but nothing happens and no error is logged. `element.click()` works.
Hover, by contrast, responds normally to real mouse movement, so every `:hover`
value in the JSON files was taken with a genuine hover held on the element.

## Per-sentence controls

Both glyphs are `background-image` sprites on an inline span with
`color: transparent` (the character inside is the fallback: 再 for play, 訳 for
translation). Hover swaps the image file. **No dark-theme override exists for
either** - the same PNGs are used in both themes.

| control | selector | resting image | hover image | box |
| --- | --- | --- | --- | --- |
| play (before the sentence) | `.sentence .play-button-container .play-button.play-button-standard` | `/images/play-sentence-off.png` | `/images/play-sentence-on.png` | 32 x 41 px, `font-size: 32px`, `opacity: 1` both states |
| translation (after the sentence) | `.sentence .notes-button-container .notes-button.notes-button-standard` | `/images/sentence-notes-off.png` | `/images/sentence-notes-on.png` | 30 x 38.5 px, `font-size: 30px`, `opacity: 0.9` resting -> `1` on hover |

Both are `cursor: pointer`, `display: inline`, `background-size: contain`. The
play glyph is centred (`background-position: center`); the translation glyph is
nudged down (`50% 60%`). Spacing comes from the container: play has
`padding: 0 8px 0 0` in body paragraphs (`0 10px 0 0` in the headline),
translation has `padding: 0 0 0 10px`.

So the tap targets in the reader are roughly **32 x 41** and **30 x 38.5** CSS
px. Note that is the glyph box only; there is no extra hit padding.

The translation glyph opens the same `.tooltip` panel used for annotations, with
a single card captioned **MEANING** - see `../annotations/README.md`.

There are colour variants of both glyphs in the stylesheet (`-yellow`, `-blue`,
`-green`, `-red`, plus `-not-accessible` and `-cloud` for play) following the
same off/on pattern. The reader uses `-standard`.

## Audio bar (desktop)

Confirmed: **on desktop the audio bar is constrained to the article column, not
full-bleed.** It is `position: fixed; bottom: 0` and its width is pinned to the
870 px article column:

```css
@media (min-width: 1024px) {
  .article-viewer-audio-controls-large-container.stuck {
    position: fixed; left: 40px; right: 290px; bottom: 0;
  }
}
@media (min-width: 1200px) {
  .article-viewer-audio-controls-large-container.stuck {
    position: fixed; left: initial; right: initial; bottom: 0;
    margin: 0 auto; width: 870px;
  }
}
```

Between 1024 and 1199 px it is not centred - it is inset 40 px on the left and
290 px on the right, i.e. it stops short of the right-hand settings sidebar. At
1200 px and up it becomes a fixed 870 px centred block. Below 1024 px the base
rule `display: none` applies and this bar does not exist at all (the mobile
player is a different element in the nav; not captured in this pass).

Measured at 1920 x 889: container `left: 392.5px, right: 642.5px, top: 839px,
bottom: 0`, 870 x 50 px. The `.stuck` class is added by script when the bar
would otherwise scroll out of view.

### Structure

```
div.article-viewer-audio-controls-large-container.stuck   870 x 50
  div.audio-controls                                      870 x 50   bg #444 dark / #888 light
    div.primary-controls.noselect                          display:table
      div.auto-scroll-button.off                           48 x 50   autoscroll-icon.png, opacity .45 (.on -> 1)
      div.play-pause-button.play                           40 x 50   audio-player-play.png (.pause -> audio-player-pause.png)
      div.speed-control-container                           51.5 x 50
        div.speed-control                                   36.5 x 22
          div.indicator                                     8 x 22
            div.bar.off.speed-indicator-5                   4 x 2
            div.bar.off.speed-indicator-4                   4 x 2
            div.bar.on.speed-indicator-3                    8 x 2   <- current speed
            div.bar.off.speed-indicator-2                   4 x 2
            div.bar.off.speed-indicator-1                   4 x 2
          div.text.noselect                                 28.5 x 22   "1.0x"
      div.timeline-container                                730.5 x 50   padding 20px 20px 20px 0
        div.timeline                                        710.5 x 10   radius 15px
          div.playhead                                      14 x 14   radius 50%, white
  div.audio-controls-spacer                                 height: env(safe-area-inset-bottom)
```

### The speed "stepper" is not a stepper

There are no + / - buttons. `.speed-control-container` is a single
`cursor: pointer` cell containing a five-bar ladder plus the numeric label. The
bars are `speed-indicator-1` (bottom) to `speed-indicator-5` (top); exactly one
carries `.on` (8 px wide, opacity 1) and the rest carry `.off` (4 px wide,
2 px side margins, opacity 0.8). All bars are 2 px tall, white, 3 px apart. At
1.0x the active bar is `speed-indicator-3`, i.e. the middle of five, so the
ladder is a five-position speed scale with 1.0x in the centre. The handlers are
bound by script, not inline, so which gesture changes the speed (click to cycle
vs. drag) was not determined - a stated gap.

### Timeline and playhead

`.timeline` is 10 px tall with `border-radius: 15px`, `position: relative`, and
a translucent track: `rgba(0,0,0,0.25)` in dark, `rgba(255,255,255,0.3)` in
light. `.playhead` is an absolutely positioned 14 x 14 white circle with
`margin-top: -2px`, so it overhangs the track by 2 px top and bottom. It has a
`::before` pseudo-element that is a 36 x 36 transparent circle centred on it -
an invisible 36 px drag target around a 14 px dot. Worth copying.

## Gap: frontend.css was not saved verbatim

The request asked for <https://web.cdn.satorireader.com/css/v1.0.9699.26209/frontend.css>
saved verbatim. **It is not in this repo.** Stating that rather than pretending:

- The file reads as **155,130 characters / 6,357 lines**.
- It cannot be read from a page on `www.satorireader.com`: the CDN sends no CORS
  headers, so both `fetch()` and `document.styleSheets[n].cssRules` fail. It has
  to be opened as its own document to read at all.
- Getting 155 kB of text out of the browser and back in as a committed file has
  to pass through this agent, and doing that in ~1 kB chunks was not a good use
  of the pass. Worse, the plain-text view of the file mangles the Japanese
  font-family names in `.japanese-mincho` / `.japanese-gothic` into mojibake, so
  a "verbatim" copy would have been quietly wrong in at least two places.

`frontend-css-extract.css` is what was done instead: every rule block whose
selector touches the article words, the two annotation underlines, the
per-sentence glyphs, the annotation popup or the audio bar, plus an appendix of
the `@media` blocks that affect them. Selectors, properties and values are as
read; indentation inside blocks was re-applied as two spaces because it was lost
in transit. The two mojibake blocks are omitted and flagged in the file header.

If a verbatim copy is wanted, the least painful route is `curl` outside the
browser - the URL is public and needs no session.
