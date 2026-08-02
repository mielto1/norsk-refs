TEST LINE 1
TEST LINE 2
# Article-body underline styles: which annotation kind each one means

**Captured:** 2026-08-02 (Satori Reader capture pass 2)  
**Viewport:** 1920x889 CSS px, devicePixelRatio 2  
**Themes:** the values below are identical in light (no body class) and dark (`body.theme-dark`) -- frontend.css contains no dark override for either rule.  
**Stylesheet:** https://web.cdn.satorireader.com/css/v1.0.9699.26209/frontend.css  
**Articles used (all free content, episodes 1-2 only):**

- https://www.satorireader.com/articles/bartender-episode-1-edition-n
- https://www.satorireader.com/articles/kitsune-no-yomeiri-episode-2-edition-n
- https://www.satorireader.com/articles/kiki-mimi-rajio-episode-1-edition-n
- https://www.satorireader.com/articles/sakura-suzuki-2-edition-n

---

## 1. The selectors

Both underlines are a `border-bottom` on the individual `.word` span. Verbatim rule blocks from frontend.css:

```css
.article-standard > .article > .paragraph.body .word.additional-notes {
  border-bottom: dotted 2px #cccccc;
}
.article-standard > .article > .paragraph.body .word.additional-notes-special {
  border-bottom: solid 2px #cccccc;
}
```

The same pair is repeated verbatim for `.paragraph.headline` and `.paragraph.blurb`. One extra rule widens the dotted variant when the popup is anchored to it:

```css
.article-standard .paragraph.body .word.additional-notes.tooltip-alignment-border {
  border-width: 5px;
}
```

Computed values measured live on the page (dark theme, 1920x889):

| selector | computed border-bottom |
| --- | --- |
| `.word` (plain, resting) | `5px solid rgba(0, 0, 0, 0)` |
| `.word.additional-notes` | `2px dotted rgb(204, 204, 204)` |
| `.word.additional-notes-special` | `2px solid rgb(204, 204, 204)` |
| `.word:hover` | `5px solid rgb(190, 224, 241)` |
| `.word.word-selected` (dark) | `5px solid rgb(30, 144, 255)` |
| `.word.word-selected` (light) | `5px solid rgb(190, 224, 241)` |

The resting word already carries a **5px transparent** bottom border, so swapping in a 5px hover/selected colour causes no reflow. The annotation underlines are deliberately thinner (2px).

---

## 2. What each one means

**DOTTED = `.word.additional-notes`** -- the word belongs to a span that has *reference* cards only. Tapping it opens a popup whose caption bars are DICTIONARY ENTRY (usually two: one for the single word, one for the multi-word expression the word belongs to) and sometimes SENTENCE FORM. No editorial prose.

**SOLID = `.word.additional-notes-special`** -- the span carries an **OTHER NOTE** card: free-form editorial prose written by an annotator, in addition to whatever reference cards also apply.

So: dotted = "there is a dictionary / sentence-form entry for this multi-word chunk"; solid = "an editor has written a note about this chunk". The `-special` suffix effectively means "has a hand-written note".

Every word inside the covered span carries the class; that is what makes the underline look continuous across a phrase. There is no single wrapper element for the span.

### Evidence

Every annotated span in two of the articles, class vs. the caption bars that appear in the popup.

kitsune-no-yomeiri episode 2:

| span (tapped word) | class | popup caption bars |
| --- | --- | --- |
| しゃが | dotted | Dictionary Entry + Sentence Form + Dictionary Entry |
| 男 (男はやぶの方へと歩いて行き) | solid | Dictionary Entry + **Other Note** |
| 目 | dotted | Dictionary Entry + Dictionary Entry |
| 気 | dotted | Dictionary Entry + Dictionary Entry |
| 足 | solid | Dictionary Entry + Dictionary Entry + **Other Note** |
| こんな | solid | Dictionary Entry + **Other Note** |
| 捜し | solid | Dictionary Entry + Sentence Form + **Other Note** |

bartender episode 1:

| span (tapped word) | class | popup caption bars |
| --- | --- | --- |
| 気 (気になっている) | dotted | Dictionary Entry + Dictionary Entry |
| 品 (品があって) | dotted | Dictionary Entry + Dictionary Entry |
| 彼女 (彼女は1時間ぐらいで...) | solid | Dictionary Entry + **Other Note** |
| 比較的 | solid | Dictionary Entry + **Other Note** |
| こんな (こんな仕事をしている割に) | solid | Dictionary Entry + **Other Note** |
| 苦手 (苦手な方) | solid | Dictionary Entry + **Other Note** |

kiki-mimi-rajio episode 1 and sakura-suzuki episode 2 behave the same way; across the 27 annotated spans checked in the four articles the correlation was exact: solid <-> an OTHER NOTE card is present, dotted <-> no OTHER NOTE card.

---

## 3. How this was measured

Class names and computed values were read with `getComputedStyle` in the live page at 1920x889. Popup caption bars were enumerated by dispatching `element.click()` on the first word of each annotated span and reading the text of every `.tooltip-content > .heading`.

Caveat worth recording: synthetic mouse clicks delivered through the browser-automation layer (real CDP mousedown/mouseup at the word's coordinates) do **not** open the popup on this site, even though the events reach the `.wpt` span and the `.word` ancestor has an inline `onclick`. Only `element.click()` opens it. Hover states, by contrast, do respond to real mouse movement, so `:hover` values in this pass were captured with a genuine hover.
