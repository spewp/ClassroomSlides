---
name: classroom-slides
version: 2.1.0
description: >
  Design system for building distinctive, projection-ready classroom decks. Produces a
  designed HTML deck first (where all iteration happens), then exports to .pptx at the
  end. Use whenever the user asks for slides, a deck, a lesson presentation, or a .pptx
  for class — any subject, any level. Supersedes v1 and v2.0.0 entirely.
audience:
  # Default subjects. If the user's request doesn't name one, ask.
  subjects: [English, Religious Education (RE), CSPE, Literacy]
  levels: [Junior Cycle, Leaving Cert (HL/OL), Small-group Literacy (~age 12)]
  country: Ireland
---

# Classroom Slides — v2.1

This skill defines a visual and editorial system for classroom decks. Output is a
**PowerPoint file** for projection. The build happens in HTML because that is how this
environment works — HTML is fast to iterate, supports live preview and toggleable
variants, and exports cleanly to `.pptx`. Treat the HTML as the editing surface and the
`.pptx` as the deliverable.

### What changed from v2.0

- **Single direction only** — the `Editorial` direction is now the house style. Gallery
  and Bold are removed. The Tweaks panel no longer offers a direction toggle.
- **Running heads and slide numbers are off by default.** Do not add them unless the
  user asks. This was noise chrome; decks read better without it.
- **Em-dash absolute clarified** and moved into the content rules so it's visible while
  writing, not only in the absolutes list.
- **Parallelism exception** to the "no two consecutive layouts" rule: repeated
  structural slides (section headers, parallel dividers) are allowed and encouraged.
- **Type floors aligned to the validator** — everything on a slide is at least 24px.
- **Annotated-text anti-patterns** added.
- **Worked examples** of full title sequences appended for two lessons.

---

## The one idea that governs everything

> The slide is not the lesson. **The teacher is the lesson.** The slide anchors what the
> teacher is saying — it does not replicate it.

Every rule below follows from this. If a rule seems to be pulling the other way in a
specific case, follow the idea, not the rule.

The commonest failure mode of classroom decks — including those produced by earlier
versions of this skill — is slides that try to *contain* the teaching. Commentary
paragraphs, "why this matters" sub-text, explanatory footers. All of it should be said
aloud. The slide's only job is to sit on the wall and hold the anchor in place.

Where does teacher-script material live, then? In **speaker notes** (when the user asks
for them) or in the teacher's head. Never on the slide.

---

## Step 0 — What to ask before building

If the user's prompt doesn't give you all four, ask for the missing ones in a single
question. Do not build without them.

1. **Subject** — English, RE, CSPE, Literacy (or other if they say so).
2. **Level** — Junior Cycle / Leaving Cert HL / Leaving Cert OL / Literacy group.
3. **Topic** — as specific as possible ("Bishop's use of imagery", not "poetry").
4. **Duration** — minutes, or number of class periods.

Do **not** ask about:
- Whether the user wants activities (use judgement; default to one if it fits).
- Colour palette preference (you choose — see palette rules below).
- Layout preferences (the skill decides).
- Direction / style variants (there is only one direction in v2.1).

If the user explicitly names something — "end with a choice board", "use navy and
gold", "no activity", "add running heads" — do what they said and do not second-guess
it.

---

## Build order

The build happens in HTML; the deliverable is a `.pptx`. Never skip the HTML step and
go straight to PPTX — you lose live preview and the ability for the user to iterate on
wording before export.

1. **Confirm the four inputs** (subject, level, topic, duration).
2. **Plan the slide sequence** on paper before writing any code: how many slides, which
   are dark, which layout each uses, what anchors each slide.
3. **Build the HTML deck** using the layout library, type scale, and palette rules
   below. Put the slides in a `slides.js` file (or inline as sections), styles in
   `styles.css`, and the shell in a single root HTML file. Include the standard Tweaks
   panel (see Tweaks section).
4. **Show the HTML to the user.** Let them read it and ask for changes. Edits happen in
   `slides.js` / `styles.css` — fast, localised.
5. **Run visual QA** (see QA section). This is a gate, not optional.
6. **Export to `.pptx`** using the environment's PPTX export. Include speaker notes if
   the user has asked for them.
7. **Deliver both** the HTML file and the `.pptx` so the user can re-edit without
   starting from scratch.

Revisions after delivery: edit the HTML, re-export. Always keep the HTML as the source
of truth.

---

## Absolutes — do not break

These are hard constraints. No topic, level, or request justifies breaking them.

1. **No bullet points.** Content is prose, cards, quotes, numbered steps, or tables.
   Dashes pretending to be bullets are still bullets.
2. **No em dashes in slide content.** Use commas, colons, full stops, or restructure.
   Fraunces and similar editorial serifs *want* em dashes — resist. You may use them
   in speaker notes and prose *about* the deck, just not *in* the slide text.
3. **No pure white backgrounds.** Also no pure black — ink tones read warmer on a
   projector.
4. **No two consecutive slides share the same layout — with one exception.** Parallel
   structural slides (section dividers, repeated template slides) are allowed and
   encouraged when the content is genuinely parallel. The rule is really: no two
   consecutive *content* slides share a layout. Don't accidentally make two adjacent
   concept cards look identical.
5. **Text floors are absolute and uniform: 24px minimum for anything on a slide.** See
   Type section. Auto-shrink below floor is not an option — cut content instead.
6. **Nothing the teacher will say aloud goes on the slide.** If a sentence on a slide
   explains, contextualises, justifies, or walks through a point, it belongs in the
   teacher's mouth, not on the wall. Anchor phrases and evidence only.
7. **No running heads or slide numbers by default.** The chrome adds noise. Only add
   if the user asks.

---

## Principles — the heuristics serve these

Principles describe the *effect* the deck should have. The heuristics under each are
ways to achieve the effect; if a heuristic works against its principle in a specific
case, follow the principle.

### 1. A deck should feel *curated*

Like a page in a well-made book. Each slide should look chosen. If a slide could be
swapped into a corporate marketing deck unchanged, it has failed this test.

*Heuristics:* generous negative space; one dominant element per slide; typography that
does the heavy lifting before colour does; imagery used sparingly and only when it
genuinely clarifies.

### 2. The deck has *rhythm*

Dark slides punctuate moments of emphasis; light slides carry content. The alternation
is felt when clicked through, not calculated slide-by-slide.

*Heuristics:* title and closing slides are almost always dark. Section dividers are
dark. Key quote slides are often dark. Everything else is a judgement call. A 6-slide
deck might have two dark slides; a 16-slide deck might have four or five.

### 3. The palette is *specific to the topic*

If the colours would still work if swapped into a completely unrelated deck, they're
not specific enough. A deck on nature poetry should not look like a deck on the Irish
Constitution.

*Heuristics:* start from the mood the room should have when the first slide goes up.
Then pick one dark tone, one warm accent, and one light base. Complete the six-role
palette below from there.

### 4. Type carries the design

Because these decks are typographically driven — no stock imagery, no icon mazes — the
type choices are what the students notice first. Pick a serif with personality for
headings and a clean humanist sans for everything else.

---

## Typography

These are HTML-first decks, so you are not restricted to Georgia/Calibri. Pick from the
recommended pairings below. Don't invent new ones.

**Recommended pairings** (heading / body — choose one pair per deck, no mixing):

| Mood | Heading (serif) | Body / UI (sans) |
|---|---|---|
| Editorial, warm, literary | Fraunces / EB Garamond / Cormorant Garamond / Source Serif 4 | Inter / Source Sans 3 |
| Sharp, modern, structured | Instrument Serif / Playfair Display / Spectral | Inter / Manrope |
| Classroom-friendly, approachable | Lora / Literata | Open Sans / Work Sans |
| Bold, graphic, confident | DM Serif Display / Bricolage Grotesque (serif) | DM Sans / Bricolage Grotesque |

Avoid over-used pairings (Times / Arial, Georgia / Calibri unless the user asks,
anything with Roboto).

**Size scale** — designed for 1920×1080 projection, read from the back of a room. All
floors are aligned to the validator (which rejects anything under 24px):

| Element | Size | Floor | Weight | Family |
|---|---|---|---|---|
| Title slide H1 | 180–240px | 160px | 500–700 | serif |
| Slide title (H2) | 120–160px | 96px | 500 | serif |
| Section divider roman | 96–140px | 72px | 500 | serif |
| Lead line / subtitle | 40–56px | 36px | italic 400 | serif |
| Card heading | 56–80px | 48px | 500 | serif |
| Body prose | 32–44px | 28px | 400 | serif or sans |
| Annotated quote text | 56–80px | 48px | italic 500 | serif |
| Small UI label (kickers, attributions) | 24–28px | **24px** | 500, tracked | sans, uppercase |
| Slide number / footer chrome | only if requested | 24px | 400 | sans |

**Floors are absolute.** If content won't fit at the floor size, cut content. Never
shrink below floor. **The universal floor for anything on a slide is 24px** — below
that, the validator rejects it and back-of-room readability drops.

---

## Colour — the six-role palette

Choose a palette appropriate to the topic. The six-role structure is the default
because it scales predictably across layouts.

| Role | Purpose | Typical values |
|---|---|---|
| Primary dark | Dark-slide backgrounds | Deep navy, charcoal, forest, oxblood |
| Secondary dark | Cards on dark slides, slightly lifted | 6–10% lighter than primary dark |
| Accent warm | The signature colour; rules, marks, highlights | Coral, amber, gold, terracotta, rust |
| Accent secondary | Used sparingly for second marks, variants | Teal, sage, deep plum |
| Light base | Light-slide backgrounds (never pure white) | Warm cream, bone, parchment |
| Light variant | Cards on light slides | A tinted variant of light base |

**Example palettes (for reference, don't re-use mechanically):**

| Topic | Primary dark | Accent warm | Accent secondary | Light base |
|---|---|---|---|---|
| Leaving Cert poetry (Bishop/Yeats) | `#1E1A17` ink | `#B74A3A` terracotta | `#3C5A4A` moss | `#F2EBDE` cream |
| Othello / Iago | `#0F1B2D` navy | `#D4A843` gold | `#2A7B88` teal | `#FAF6F0` cream |
| Inclusivity / CSPE | `#1E2D4A` navy | `#E8734A` coral | `#1A6B5C` sage | `#FFF8F0` cream |

When generating a new palette, think about the mood first, then use OKLCH to derive
harmonious variants rather than eyeballing hex. Keep contrast ratios WCAG-AA at
minimum.

---

## Layout library

Use a different layout for each content slide. Never repeat consecutively except for
parallel structural slides (see Absolute 4). Mix liberally.

1. **Title** — one enormous phrase, one thin accent rule, an italic subtitle. Nothing
   else.
2. **Editorial hero** — a large H2 statement, a single pullquote with a decorative "
   mark.
3. **Map / overview** — a small grid (2–3 cells) listing what's to come. Roman
   numerals for camps, one-line glosses, no paragraphs.
4. **Section divider** — dark slide with a roman numeral, a one-word title, one anchor
   quote. Used to separate parts of the lesson. Parallel dividers may appear adjacent
   if the structure genuinely warrants it.
5. **Concept card (single)** — one idea per slide: title, one-line definition, one
   example (quote / image / diagram). No commentary.
6. **Concept card (compare two)** — two evidence blocks side-by-side for the same
   concept. No explanation text.
7. **Annotated evidence** — a quote, passage, equation, or source with *spans inside
   it* marked by colour to draw the eye to the technique/feature being taught. See
   Annotated Text below — this is the signature device for content teaching.
8. **Numbered steps** — 3–5 cells with large numerals; one short label each, no body
   text.
9. **Before / after (table)** — two columns, "instead of" / "try". Short phrases only.
10. **Task** — one concrete, pen-and-paper activity connected to the slides before it.
    Two to three numbered steps, each a single sentence.
11. **Closing** — one sentence or one quote. Big. Nothing else.

Not every deck uses every layout. Pick the ones that serve the content.

---

## Annotated text — the signature content device

Long passages are where classroom decks usually fail. The trick is to present the
whole quote at projection size and *colour-mark the fragment being taught*. The rest
of the quote is ink; the marked fragment is accent-coloured (and usually
weight-shifted).

```html
<div class="eg__quote">
  "faint <span class="a-f">f</span>orked
  <span class="a-l">l</span>ightnings, catching
  <span class="a-l">l</span>ight."
</div>
```

CSS:
```css
.a-f, .a-l, .a-s, .a-h, .a-b, .a-u, .a-accent {
  color: var(--accent);
  font-weight: 700;
  text-decoration: underline wavy var(--accent);
  text-underline-offset: 0.15em;
}
```

Apply the same pattern to other subjects:
- **English** — mark sounds, syllables, figurative pivots, volta turns.
- **RE** — highlight key verbs, names, covenant language in scripture passages.
- **CSPE** — highlight rights-bearing language in articles; mark duty/right pairs.
- **Literacy** — mark morphemes, digraphs, rhymes, syllable boundaries.

This is the most powerful single technique the system has. Use it whenever evidence is
on the slide.

### Annotated-text anti-patterns

The device works because the eye catches a *sparse* mark inside ordinary text. When
used poorly it becomes noise.

- **Over-marking.** More than two or three spans in a single quote collapses the
  effect. The marks stop pointing at anything because everything is a mark.
- **Marking whole sentences.** If you find yourself marking four or more words in a
  row, you are using bold, not annotation. Restructure — put the sentence in a
  pullquote, or split the point into two slides.
- **Inconsistent semantics.** If `.a-underline` means "this sound" on slide 3 and "this
  theme" on slide 7, students lose the thread. Pick one meaning per deck; introduce
  more only with a legend slide.
- **Decoration marking.** Never mark a span just because the line would look bare. The
  mark must earn its presence by pointing at something you'll talk about.

---

## Content rules (rewritten from v1)

The old rules were right in spirit but still produced dense slides because the mental
model was wrong. New rules:

### Anchor, don't narrate

Every slide should answer **one** of these questions, and only one:
- What is this thing called?
- What does it look like on the page? (evidence)
- What do we do next? (task)

If a slide is answering two of them, split it. If it's answering none of them, cut it.

### Punctuation on slides

- **No em dashes on slides.** Even though the serifs you are using (Fraunces,
  Cormorant, Spectral) were designed around them, the em dash is the single most
  common giveaway that a deck was AI-generated. Use commas, colons, full stops, or
  restructure. This rule applies to every visible word on every slide, including
  sub-labels.
- **Smart quotes only.** `&rsquo;` for apostrophes, `&ldquo;` / `&rdquo;` for quotes.
  Straight quotes are a typography smell.
- **No exclamation marks.** They almost always sit where a colon or full stop would do
  more.

### Word budgets — tighter than v1

Budgets are per slide for *all non-chrome text combined* (title + subtitle + body +
card text).

| Slide type | Hard maximum | Aim for |
|---|---|---|
| Title | 12 words | 4–8 |
| Section divider | 16 words | 8–12 |
| Concept card (single) | 30 words | 15–22 |
| Concept card (compare two) | 40 words | 24–32 |
| Annotated evidence | Evidence wordcount + 12 words chrome | — |
| Numbered steps | 4 words × n steps | — |
| Before/after table | 10 words per cell | 5–7 |
| Task | 30 words | 18–22 |
| Closing | 20 words | 8–14 |

If you're over, cut. Don't shrink type.

### No teacher-scripting

If a line on the slide *explains*, *contextualises*, *justifies*, or *walks through* a
point, delete it. That line is teacher dialogue. The teacher has a mouth.

Tests that a line is teacher-scripting:
- It begins with a transition word ("Now,", "So,", "Here we see").
- It says why something is interesting or important.
- It rephrases something already on the slide in other words.
- It ends with "which means…".

Script material belongs in speaker notes, never on the slide.

### Quotes are evidence, not decoration

Every quote has an attribution (speaker, source, act/scene/line if literary). Quotes
are never truncated into soundbites — if the passage needs the full shape to land,
give it the full shape. If it doesn't, cut to the phrase that matters.

---

## Tweaks panel (standard, every deck)

Every deck ships with a small floating panel in the bottom-right corner, activated by
the toolbar toggle in the environment. The panel lives on all decks for consistency
even when it only has one or two controls.

**Standard control (always include):**

- **Dark mode toggle** — for decks projected in bright vs dim rooms. Swaps the primary
  dark and light base roles while keeping accent colours.

**Optional controls** — include only if the lesson genuinely benefits:

- A content toggle specific to the lesson (e.g. "Show / hide example answers"), when
  the teacher might want to reveal content live.

Do **not** add a direction picker. There is one direction in v2.1 — the house style
described above. Earlier versions shipped `Editorial / Gallery / Bold` variants; this
is removed. Decks should be opinionated about their own look.

Implementation: use the standard Tweaks protocol (register `__activate_edit_mode` /
`__deactivate_edit_mode` listener, post `__edit_mode_available`, persist with
`__edit_mode_set_keys` inside `/*EDITMODE-BEGIN*/ ... /*EDITMODE-END*/` markers in the
root HTML).

**Visual spec.** Panel sits bottom-right, ~260px wide, white card on cream decks (or
cream card on dark decks), 10px radius, subtle shadow. Controls are labelled in small
caps at 11–13px (the panel is chrome, not slide content, so the 24px floor does not
apply). Keep it visually quiet so it doesn't compete with the deck behind it.

---

## Activities

If the deck runs more than 15 minutes of content, it almost always benefits from an
activity. Default to including one unless the user said otherwise.

**Single task slide** — one concrete, clearly-described task, pen-and-paper unless the
user says devices are available. Must connect to the slides immediately before it —
generic "reflect on the themes" tasks fail. A good test: if the task could be pasted
into a different deck and still make sense, it's too generic.

**Choice board** — only if the user asks. Four to six options in a grid, each offering
a *different kind of thinking* (empathetic / analytical / creative / persuasive), not
the same task in different lengths.

---

## QA — a gate, not a suggestion

Do not export to PPTX or deliver until visual QA has passed.

Run through each slide in the HTML preview and check, in priority order:

1. **Text overflow or clipping** in any container.
2. **Any text below 24px** (the universal floor). If so, cut content or restructure.
3. **Two consecutive slides with the same layout** that are not parallel structural
   slides.
4. **Bullet points, including disguised dashes.**
5. **Em dashes in slide content.** Search the source for `—`, `&mdash;`, and `--`
   before declaring the deck done.
6. **Teacher-scripting sentences** still on slides (delete them).
7. **Low-contrast text** (light grey on cream, dark on dark).
8. **Running heads or slide numbers** present when the user did not ask for them.
9. **Annotated spans not landing visibly** — the mark has to register from the back
   of a classroom. And: not more than two or three marks per quote.

Skip sub-pixel nitpicks a student wouldn't notice. QA is about real user-visible
defects.

If a first pass surfaces defects, fork a verifier subagent to sweep once more with
fresh eyes before export.

---

## Level calibration

| Level | Tone | Density | Accent application |
|---|---|---|---|
| Junior Cycle (12–15) | Warmer, more inviting | A bit more visual space, slightly larger card headings | More colour, more accent marks |
| Leaving Cert HL (16–18) | Restrained, analytical, literary | Denser allowed, more annotated evidence | Discreet — colour does less heavy lifting |
| Leaving Cert OL | Clearer scaffolds, shorter sentences | As HL but with tighter word budgets | As HL |
| Literacy (small group, ~12) | Warmest, most playful | Fewer words per slide, more breathing room | Heavy use of annotated spans (morphemes, digraphs, syllables) |

---

## Slide count guidance

| Slot | Target range |
|---|---|
| 10-minute starter / plenary | 6–10 slides |
| 60-minute class | 12–18 slides |
| Revision resource (student-facing) | Up to 22 slides, denser content acceptable |

Don't pad to hit these numbers. Under is fine; over is a smell.

---

## Worked examples — full title sequences

These are the titles only, in order. Read them top to bottom; the shape of the lesson
should be visible from titles alone. One grammatical style per deck.

### Example 1 — "Show Your Working" (First Year English, exam revision, 45 min)

Short topic noun-phrases with occasional declarative phrases. Twelve slides.

1. Show Your Working
2. Today at a glance
3. Same question. Two answers.
4. Two answers on the board
5. What made B stronger
6. A simple habit *(section divider, dark)*
7. Point. Evidence. Explain.
8. One answer, modelled together
9. Your turn on the page
10. As you write, check in
11. Reading each other's work
12. Point to the page *(closing, dark)*

Notes on this sequence:
- The rhythm alternates: light content, light content, dark divider, light teaching,
  light task, light peer work, dark close. One dark divider is enough for a
  twelve-slide deck; two darks total (divider + closing) gives it shape.
- The titles "Same question. Two answers." and "Point. Evidence. Explain." both use
  parallelism with full stops — repeat the shape once per deck, not more.
- No title is a verdict. No title ends in "…" or begins with "The magic of".

### Example 2 — "The Craft of the Poem" (Leaving Cert HL, Bishop & Yeats, 40 min)

Sixteen slides. Section-heading nouns for dividers, topic noun-phrases for content.

1. The Craft of the Poem
2. What the question is really asking
3. The map of today
4. Sound *(section divider, dark)*
5. Alliteration
6. Sibilance
7. Assonance
8. Figurative language *(section divider, dark)*
9. Simile
10. Metaphor
11. Imagery across the senses
12. Writing it up *(section divider, dark)*
13. Point, Evidence, Explain, Link
14. From device to effect
15. Ten minutes on the page
16. The question is always craft *(closing, dark)*

Notes on this sequence:
- Three section dividers give the deck clear three-act structure. The dividers are
  one-word nouns ("Sound", "Figurative language", "Writing it up") — this is a valid
  case where consecutive *structural* slides (3→4, 7→8, 11→12) share a layout, because
  they are genuinely parallel.
- Titles 5, 6, 7 are single-word technique names; 9, 10 follow the same pattern.
  Parallel naming across a section is a feature, not a bug.
- The closing title is a phrase from the teacher's framing in slide 2 — closing loops
  are good when they're not laboured.

---

## What this skill does NOT cover

- Worksheet / handout design (different format, different rules).
- Interactive prototypes (different skill).
- Non-classroom decks (corporate, conference, etc.) — this skill is
  classroom-specific.

If the user asks for something outside the scope, say so and ask what they'd like
instead.
