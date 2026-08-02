# OTHER NOTE - へと (e to) and the "manner" particle と

Second-longest OTHER NOTE found in the free content surveyed: **3,859
characters** of rendered text, a 1,813 px tall popup at 1920x889. Notable
because it is the only long note captured that ends with an explicitly labelled
**Caveats** section, introduced by a `-----` rule.

| | |
| --- | --- |
| Article | <https://www.satorireader.com/articles/kitsune-no-yomeiri-episode-2-edition-n> |
| Series / episode | The Wedding of the Fox, episode 2 "Fallen Leaves" (free) |
| Tapped word | 男 (おとこ) in 男はやぶの方へと歩いて行き |
| Underline on the tapped word | **solid** (`.word.additional-notes-special`) |
| Caption bars in the popup, in order | DICTIONARY ENTRY, OTHER NOTE |
| Viewport | 1920x889 CSS px, devicePixelRatio 2 |
| Theme | dark (`body.theme-dark`) |
| Popup size | 870 x 1813 CSS px |
| Captured | 2026-08-02 |
| Companion files | `kitsune-no-yomeiri-ep2-e-to.html`, `kitsune-no-yomeiri-ep2-e-to.png` |

---

## Full visible copy

Bracketed labels are mine and mark structural elements. Furigana is written as
kanji(reading).

### [caption bar] DICTIONARY ENTRY

[expression] 男(おとこ)

[sense, class `sense context`] man; male [noun, noun used as prefix]

[button] Add to Your Studylist

### [caption bar] OTHER NOTE

[expression]

> 男(おとこ)はやぶの方(ほう)へと歩(ある)いて行(い)き

[discussion]

We have two instances of ***e to*** in close proximity here (in this and the following sentence), which makes it a nice opportunity to discuss it together.

Almost any place you see *e to* could simply be *e* without any significant change in meaning. For example, the sentence from the episode could take either of these forms:

[example block]

> 男(おとこ)はやぶの方(ほう)へ歩(ある)いて行(い)った。
>
> The man went walking toward the thicket.

[example block - immediately followed by a second one, no prose in between]

> 男(おとこ)はやぶの方(ほう)へと歩(ある)いて行(い)った。
>
> The man went walking toward the thicket.

If so, then how are we to understand the choice to use the combination *e to*?

The dictionary 明鏡国語辞典 says that "the directionality of the movement is stronger" with *e to* than with a simple *e*. But why?

Let's start by understanding what the *to* is doing. There are different ways of thinking about this *to*, but we (the Satori Reader team) usually recommend thinking of it as the same helper particle that we see with mimetic words. For example:

[example block]

> 男(おとこ)はゆっくりと道路(どうろ)を渡(わた)った。
>
> The man slowly crossed the street.

The *to* links in the mimetic word. (With some mimetic words, this *to* is optional, but we can safely ignore that for this discussion.) The important thing is to understand that the *to* serves a linking function, connecting in *yukkuri* to show us **how** the crossing the street happened.

This *to* can also turn up with other adverbial expressions that are not mimetic words. For example:

[example block - note the square brackets inserted into the Japanese line as `.nw` non-word spans]

> レミングたちが[次(つぎ)から次(つぎ)へ]と水(みず)の中(なか)に飛(と)び込(こ)んだ。
>
> The lemmings dove into the water [one after another].

The expression "from next to next" means "one after another; in unending succession; without letup." But notice that it is linked into the sentence using this same helper *to*. Once again, *to* is marking the preceding phrase as the **manner** in which the verb happens.

Now let's see if we can apply this way of thinking to an *e to* sentence. Here's a simple one, with brackets for clarity:

[example block]

> 女(おんな)の子(こ)は[台所(だいどころ)へ]と走(はし)って行(い)った。
>
> The girl went running in a "toward the kitchen" manner. = The girl went running for the kitchen.

The "toward the kitchen" part is marked with *to*. It is the **manner** in which she went running. But what could that mean? It means that when she set off running, there was a clear **directionality**, an **objective**, to the action. In English, we can sometimes capture a similar sense with "for": She went running **for** the kitchen. Say that out loud and really feel it. What does it mean when you set off running **for** the kitchen? Not "to" or "toward," but "for." There is this sense of a **goal** or **purpose** underlying the action, right? It is very similar with *e to*.

With *e to*, we are seeing the movement itself, plus the motivating goal, and **it tends to pull our focus into that movement and make us visualize it more vividly**. So when we hear about the girl running off for the kitchen, we see, not her arrival there, but rather her setting off with that objective in mind. And it is for this reason that that sense of directionality feels stronger. **It is that underlying arrow of a stated objective that makes the directionality feel stronger.**

Keep your eyes out for this *e to* as we proceed. We'll be seeing lots of it.

-----

Caveats:

1. "For" is a useful aid to understanding *e to* in some sentences, but it doesn't always fit seamlessly in English, so it's important not to expect to see it mechanistically applied to every *e to* sentence translation.

2. The sense of an objective is not always something the subject literally feels. It could be the interpretation of an observer who reports the action. For example, a narrator could say "The man set off walking for his car" if it looked like that was his objective (regardless of what the man himself thinks). Even inanimate objects can be spoken of as moving *as though* with some objective in mind: "The rocket headed deeper and deeper for the blackness of space."

---

## How this note closes

This one closes in two stages, and it is the most structurally interesting of
the four.

First the *body* ends with a bolded one-line restatement of the rule ("It is
that underlying arrow of a stated objective that makes the directionality feel
stronger.") followed by a forward-looking instruction to the reader: "Keep your
eyes out for this *e to* as we proceed. We'll be seeing lots of it."

Then comes a horizontal rule written as five hyphens in its own paragraph
(`<span class="p">-----</span>` - it is literal text in a paragraph, **not** an
`<hr>`), the word "Caveats:" on its own line, and two hand-numbered items
("1." and "2." as literal text at the start of a paragraph - there is no list
markup in the DOM). The final caveat ends mid-topic on an illustrative quote
("The rocket headed deeper and deeper for the blackness of space.") with no
closing sentence at all.

Two things to copy for the Norwegian app:

- The note has an above-the-line teaching part and a below-the-line
  small-print part. The divider and the numbering are plain text inside
  `.discussion .p` paragraphs; the renderer has no list or rule primitives.
- A long note is allowed to just stop once the small print is done. Only the
  main body gets a deliberate ending.
