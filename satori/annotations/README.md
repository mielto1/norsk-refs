# satori/annotations - long OTHER NOTE cards, captured whole

Capture pass 2, 2026-08-02. All content here is from **free episodes only**
(episodes 1-2 of a series), read on a free account.

Everything was captured at **1920 x 889 CSS px, devicePixelRatio 2, dark theme**
(`body.theme-dark`). Each `.md` file repeats its own article URL and viewport.

## The four notes

Picked for length, not representativeness. Lengths are characters of rendered
text inside the OTHER NOTE card.

| files | article | tapped word | note length | popup height |
| --- | --- | --- | --- | --- |
| `kiki-mimi-rajio-ep1-hasamu-you-ni.*` | kiki-mimi-rajio episode 1 | その (in その広場を挟むように) | 4,433 | 2,039 px |
| `kitsune-no-yomeiri-ep2-e-to.*` | kitsune-no-yomeiri episode 2 | 男 (in 男はやぶの方へと歩いて行き) | 3,859 | 1,813 px |
| `sakura-suzuki-ep2-yobisute.*` | sakura-suzuki episode 2 | 桜 (in 桜の言った通りだ) | 2,334 | 929 px |
| `bartender-ep1-nigate-na-hou.*` | bartender episode 1 | 苦手 (in 苦手な方) | 1,894 | 1,051 px |

The bartender one is deliberately included because it is **the note that
`../shots/tooltip-note-honorific.png` cut off** at "The narrator is using a
pattern in which the worl...".

Each note has three files: `.html` (the popup markup), `.md` (full plain-text
transcription plus provenance and a note on how it closes) and `.png` (a
full-height screenshot).

## How the longest notes were found

Not by browsing. Every article page embeds its whole content tree as a JSON
literal in an inline `<script>` (`var content = {...}`), and every annotation is
in there with a `type` and a `discussion` field. `type: 99` is the OTHER NOTE
kind. So: the two free episodes of all 42 series were fetched (50 distinct
`edition-n` articles), the JSON was parsed out of each, and the type-99
discussions were ranked by length. The top of that ranking is what is captured
here. The next candidates, if a pass 3 wants more, were
sakura-suzuki-2 (2,270 chars of source markdown), akiko-nikki-day-2 (1,967),
kona-adventure-2-episode-1 (1,910) and saigetsu-no-mon-episode-1 (1,782).

## Caveats on these files - please read

**1. The `.png` files contain JPEG bytes.** The browser capture tool used for
this pass emits JPEG; a real PNG was not obtainable. The file names are `.png`
because that is what the request asked for, and browsers and GitHub render them
fine, but `file(1)` will say JPEG. If byte-correct PNGs matter, they need
re-capturing with a different tool.

**2. The full-height screenshots are composites.** A single viewport is 889 px
tall and the popups are up to 2,039 px, and the automation layer can only
screenshot the viewport. So for the two tall notes the panel was cloned into two
side-by-side columns (top half left, bottom half right) and the pair was scaled
to fit one frame. Nothing is cropped or missing, and no text was re-flowed - each
column is the real 870 px-wide panel, just vertically clipped to its band - but
read them left column first, then right column. Scale factors: hasamu 2 columns
at 0.866, e-to 2 columns at 0.974, yobisute 1 column at 0.95, nigate-na-hou 1
column at 0.84.

**3. The `.html` files are normalised in two documented ways** (each file
restates this in its own header comment): every `id="..."` and `data-id="..."`
attribute was removed, and the single-line markup was pretty-printed one line
per `.tooltip-content` child and per `.discussion` child. Tags, classes, inline
event handlers, `style`, `lang`, `data-type` and all text are as read from the
live DOM.

## Popup structure, as it actually is

One reusable `.tooltip` element per page holds all cards; tapping a word
replaces its contents.

```
span.tooltip                       (position:absolute, left:0 right:0, top set inline)
  span.tooltip-close               "X", floated right, sits in the caption bar
  span.tooltip-content
    span.heading                   <- the caption bar (text-transform: uppercase)
    span.note-body
      span.expression              the covered span, repeated as a heading
        span.article[lang=ja] > span.paragraph.body > span.sentence > span[data-type=run] > span.word ...
      span.sense.context           dictionary gloss (DICTIONARY ENTRY cards only)
      span.tooltip-button ...      studylist buttons (DICTIONARY ENTRY cards only)
      span.discussion              (OTHER NOTE cards)
        span.p                     one per paragraph
        span.example-sentence
          span.japanese > span.article ...
          span.english
    span.footer                    5px grey spacer, closes the card
    ... heading / note-body / footer repeats for each further card
```

There is no list, heading or horizontal-rule primitive in the renderer. When a
note wants numbered items or a divider it writes them as literal text inside
`.p` paragraphs - see `kitsune-no-yomeiri-ep2-e-to`, whose "Caveats" section is
a `-----` paragraph followed by paragraphs starting "1." and "2.".

## Caption bars: the complete set found in free content

The request asked whether any caption bar exists other than DICTIONARY ENTRY,
SENTENCE FORM and OTHER NOTE. **Yes - two more mechanisms.**

The caption text comes from the note object in the page payload: type 1 renders
DICTIONARY ENTRY, a non-zero `sentenceForm` field renders SENTENCE FORM, type 99
renders OTHER NOTE - *unless* the note carries a non-empty `heading` field, in
which case that string is used verbatim as the caption. It is free text set by
the annotator.

Across all 50 free articles surveyed, the custom headings actually in use are:

| caption | occurrences | example |
| --- | --- | --- |
| NAME | 5 | sakura-suzuki episode 2, on 桜 - see `sakura-suzuki-ep2-yobisute.html` |
| PLACE NAME | 1 | |
| ABBREVIATION | 1 | |
| STRUCTURAL BREAKDOWN | 1 | |

And separately: the per-sentence translation glyph (訳, `.notes-button`) opens
**the same `.tooltip` component** with a single card captioned **MEANING**.
Checked on every sentence of three articles (13, 39 and 39 sentences): always
exactly one MEANING card, shape `heading > note-body > footer`. So MEANING is a
fifth caption bar that the earlier passes had not recorded.

Two note types in the payload have no caption of their own and remain
unexplained: `type: 2` (118 instances across the 50 articles - always an
expression with a non-zero `sentenceForm` and no discussion, so it is almost
certainly what produces a SENTENCE FORM card) and `type: 10` (20 instances, 14 of
them in human-japanese-extra-credit-107, with one-word discussions like "it" and
"they"). Clicking every word and every translation glyph in that article
surfaced only DICTIONARY ENTRY / SENTENCE FORM / OTHER NOTE, so **where type 10
is rendered is a stated gap, not a guess.**

## How long notes close - the pattern across all four

Three of the four close the same way: after the invented examples, a final
paragraph goes back to the sentence from the episode and says what it therefore
means. It is signposted ("Now, at last, we can analyze the sentence from the
article", "Similarly, in the sentence from the episode, ..."). No summary of the
note, no restatement of the rule, no sign-off.

The fourth (yobisute) has no episode sentence to return to - it is about social
convention rather than grammar - and closes instead on a one-sentence
generalisation with the two key phrases bolded.

The e-to note adds a second stage: the teaching part gets a proper ending, then a
plain-text divider and a "Caveats:" small-print section which just stops
mid-topic on an illustrative quote.

So: **long notes do not trail off, and they do not summarise themselves.** They
aim at the article sentence and land on it. Small print, if any, lives below a
divider and is allowed to end abruptly.
