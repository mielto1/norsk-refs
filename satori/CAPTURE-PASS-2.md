# Satori Reader capture pass 2

Date: 2026-08-02. Account: free. **All content read is free content** - episodes
1-2 of a series only, no paid episodes were opened.

Everything new in this pass was captured at **1920 x 889 CSS px,
devicePixelRatio 2**, in the **dark** theme unless a file says otherwise, matching
the existing `computed-styles-light.json` / `-dark.json`.

## What was added

```
satori/
  annotations/                      <- NEW: the four longest OTHER NOTE cards
    README.md                          index, popup DOM structure, caption bars
    kiki-mimi-rajio-ep1-hasamu-you-ni.{html,md,png}    4,433 chars, 2039px tall
    kitsune-no-yomeiri-ep2-e-to.{html,md,png}          3,859 chars, 1813px tall
    sakura-suzuki-ep2-yobisute.{html,md,png}           2,334 chars,  929px tall
    bartender-ep1-nigate-na-hou.{html,md,png}          1,894 chars, 1051px tall
  states/                           <- NEW: interaction states
    README.md                          words, sentence glyphs, audio bar, gaps
    underline-kinds.md                 dotted vs solid: selector + meaning
    computed-styles-states-dark.json
    computed-styles-states-light.json
    frontend-css-extract.css           relevant rule blocks from frontend.css
  CAPTURE-PASS-2.md                 <- this file
```

Untouched from pass 1: `shots/` (14 PNGs), `computed-styles-light.json`,
`computed-styles-dark.json`.

## The questions this pass was asked, and the answers

**How does a long note close?** It aims back at the article sentence and lands
on it - "Now, at last, we can analyze the sentence from the article...",
"Similarly, in the sentence from the episode, ...". It does not summarise itself
and it does not trail off. Three of the four captured notes do exactly this. The
fourth has no episode sentence to return to and closes on a bolded
one-sentence generalisation instead. Detail and full transcriptions in
`annotations/`.

**Which underline means what?** Dotted (`.word.additional-notes`,
`border-bottom: dotted 2px #cccccc`) = reference cards only: DICTIONARY ENTRY
and sometimes SENTENCE FORM. Solid (`.word.additional-notes-special`,
`border-bottom: solid 2px #cccccc`) = there is an OTHER NOTE. Exact across all 27
annotated spans checked. See `states/underline-kinds.md`.

**Word hover / selected, sentence glyphs, audio bar.** In `states/`. Highlights:
the resting word reserves a 5px transparent underline so nothing reflows; the
hover colour `#bee0f1` is shared by both themes while the selected state is
theme-specific; the sentence glyphs are sprite swaps with no dark variant and tap
targets of about 32 x 41 and 30 x 38.5 CSS px; the desktop audio bar is
`position: fixed` and pinned to the 870px article column, not full-bleed; the
playhead is a 14px dot with an invisible 36px `::before` drag target.

**Any caption bar besides DICTIONARY ENTRY / SENTENCE FORM / OTHER NOTE?** Yes.
The per-sentence translation glyph opens the same popup with a **MEANING**
caption, and any note can override its caption with free text - NAME, PLACE NAME,
ABBREVIATION and STRUCTURAL BREAKDOWN are all in use in the free content.
`annotations/README.md` has the full evidence.

## Explicit gaps - things not done or not determined

1. **Mobile (390 x 844) was deliberately skipped**, by instruction. Nothing in
   this pass is mobile. So `shots/mobile-*.png`,
   `computed-styles-mobile-light.json` and `-dark.json` do not exist yet, and the
   mobile questions (where the audio bar sits, whether the popup becomes a bottom
   sheet, mobile body text metrics, mobile tap sizes) are all still open. One
   thing that is already known from the stylesheet: below 1024px the desktop
   audio bar element is `display: none`, so mobile uses a different player,
   `#nav-mobile #nav-mobile-audio-player-container .audio-controls`.
2. **frontend.css was not saved verbatim.** Reasons and the workaround are in
   `states/README.md`. `states/frontend-css-extract.css` covers the selectors
   that matter.
3. **The four `.png` screenshots contain JPEG bytes.** The capture tool available
   in this pass only emits JPEG. Names are `.png` as requested; content is JPEG.
4. **Two tall popups are two-column composites.** A viewport is 889px and the
   popups are up to 2039px, and only the viewport can be screenshotted, so the
   panel was cloned into two side-by-side vertical bands. Nothing is cropped.
   Details and scale factors in `annotations/README.md`.
5. **The `.html` files have `id` / `data-id` attributes stripped** and are
   pretty-printed. Every file restates this in its own header comment.
6. **What gesture changes the audio speed** is not determined - the handlers are
   bound by script rather than inline.
7. **Where payload note `type: 10` is rendered** is not determined. It exists (20
   instances in the free content, one-word discussions like "it" / "they") but
   clicking every word and every translation glyph in the article that has 14 of
   them surfaced no card for it.

## Method notes worth keeping

- Every article page embeds its full content tree as JSON in an inline script:
  `var content = {...}`. Annotations live in there with `type` (1 = dictionary
  entry, 99 = free-form note, 3 = per-sentence note, 2 = sentence-form,
  10 = unknown), `heading` (caption override), `sentenceForm` (bitmask) and
  `discussion` (markdown, with `:::: slug ::::` placeholders that expand into the
  example-sentence blocks). Ranking notes by length across a whole series is a
  fetch-and-parse job, not a browsing job.
- Synthetic mouse clicks from the automation layer do **not** open the annotation
  popup on this site even though the events arrive correctly on the right
  element. `element.click()` does. Hover works normally. Anyone repeating this
  capture will lose an hour to that.
