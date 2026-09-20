# CLAUDE.md

Context for AI assistants working on this repo. Read this before changing anything.

## What this is

Kevin Ongaro's personal portfolio. Static HTML + CSS, no framework, no build step,
no package.json. **One page**, one stylesheet. Deployed on Vercel as a static site.

```
index.html            the whole site — hero, about, toolkit, work grid, write-ups, resume, contact
style.css             the stylesheet
Kevin_Ongaro_CV.pdf   served directly by the résumé button
README.md
```

Run it with `python3 -m http.server 5173`. That's the whole toolchain.

It used to be two pages (`index.html` + `projects.html`). The write-ups were folded
into `index.html` as the `#writeups` section and `projects.html` was removed. Do not
reintroduce a second page — nav and footer link to in-page anchors now.

### Anchors

`#top` `#about` `#work` (the card grid) `#writeups` (the long-form entries)
`#resume` `#contact`. The header nav's "work" goes to `#writeups`; the hero's
"peek at the work" goes to `#work`. That split is intentional.

## Design direction: "warm famicom"

Japanese interface design crossed with Famicom-era game UI. This is a deliberate
direction, not a default. Do not "modernise" it toward a generic SaaS look.

**Hard rules — breaking these breaks the design:**

- **No `border-radius`.** Anywhere. Every corner is square on purpose.
- **No blur.** No `box-shadow` with a blur radius, no `filter: blur()`, no soft
  gradients. Shadows are hard offsets only: `box-shadow: 8px 8px 0 <color>`.
- **Texture comes from dither**, never from noise images or gradients. Use the
  `.dither`, `.dither-shu`, `.dither-ai`, `.dither-gold` utilities — they are
  hard-edged checkerboards built from `linear-gradient` with 25%/75% stops.
- **Everything lands on a 4px grid.** Use the spacing tokens (`--u`, `--s1`…`--s8`).
  No arbitrary values like `13px` or `0.7rem` for spacing.
- **Two typefaces only.** `Zen Kaku Gothic New` for anything meant to be read;
  `Silkscreen` for bitmap labels, numbers and buttons. Do not add a third.
  Silkscreen is for short labels — never body text.

**Colour tokens** (defined in `:root`):

| Token | Value | Role |
|---|---|---|
| `--paper` | `#F5F0E3` | page ground, rice paper |
| `--paper-2` | `#FFFCF3` | cards and raised surfaces |
| `--ink` | `#1E1A16` | text, borders — warm near-black |
| `--ink-2` / `--ink-3` | | secondary / tertiary text |
| `--rule` | `#DCD2BE` | hairlines, default shadows |
| `--shu` | `#D9502E` | vermilion — the lead accent |
| `--ai` | `#2A4A8F` | indigo — support |
| `--gold` | `#E0A32E` | kincha gold — small doses only |

Vermilion leads, indigo supports, gold accents. Don't promote gold to a primary
or introduce a fourth hue.

**Motion budget:** one page-load fade sequence (`.enter`, `.enter-1`…`.enter-4`)
plus the pixel cursor trail. That's it. No scroll-reveal animations — an earlier
version had them and they were removed deliberately. Hover transitions use
`steps(2)` so movement snaps rather than glides, matching the bitmap feel.
`prefers-reduced-motion` is respected in several places — keep it that way.

The `.enter` classes belong to the hero only. Now that everything is on one page,
do not attach them to content further down — it would fade in things the visitor
cannot see yet, and the second time they scroll past, nothing happens.

## Content facts

Four projects, in this order in **both** the card grid and the write-ups:

| # | Project | Colour | Live | Source |
|---|---|---|---|---|
| 01 | The Exodus | shu | the-exodus-delta.vercel.app | github.com/Dapper434/ProjectGen |
| 02 | Clinvia | ai | clinvia-indol.vercel.app | github.com/Dapper434/Clinvia |
| 03 | SokoCredit | gold | soko-credit-client.vercel.app | github.com/Dapper434/SokoCredit_Client |
| 04 | Sokolink | ink-3 | sokolink-frontend.vercel.app | github.com/Dapper434/Sokolink-Client |

The number, the dither colour and the hover shadow must agree between a project's
card and its write-up. They disagreed while the two lived on separate pages; on one
page the mismatch is visible, so keep them in sync.

- **Clinvia was previously called TB Track.** If you find "TB Track" or any
  `tbtrack` URL anywhere, it's stale — update it. The name TB still appears
  correctly in prose describing tuberculosis patients; that's not a rename target.
- Placings are specific: **1st place at the Jitume Hackathon** (The Exodus),
  **3rd place at a Moringa-organised hackathon** (Clinvia). Don't round these up
  or blur them into "award-winning".
- SokoCredit was a **team** build; Kevin owned the Analytics & Reporting module
  and the credit-scoring logic. Sokolink was solo. Say so accurately.
- Contact: "say hi" goes to Instagram (`mysti_calmaestro`), not email. The hero
  CTA goes to email. GitHub is `Dapper434`.
- The résumé button must serve `Kevin_Ongaro_CV.pdf` from the repo root with a
  `download` attribute. This link was broken for a long time — don't reintroduce a
  placeholder filename.

## Voice

First person, lowercase headings, dry and self-deprecating. "no filler projects,
promise." / "built solo, because delegating is scary." Keep it. Do not rewrite copy
into corporate portfolio language ("passionate about leveraging cutting-edge...").
If you need new copy, match the existing register: plain verbs, short sentences,
no hype, willing to admit the unglamorous part.

## Quality floor — already met, keep it

- Responsive to mobile; the nav stays visible on small screens (it used to
  disappear entirely — don't regress this)
- Visible keyboard focus via `:focus-visible`
- `prefers-reduced-motion` honoured
- Meta description, Open Graph tags and an inline SVG favicon
- One `h1` on the page (the hero). Write-up entry titles are `h3`, their
  "The problem / What I built / What it taught me" labels are `h4`
- The cursor-trail `requestAnimationFrame` loop stops when idle rather than
  drawing empty frames forever

## Open items

- The about section says "two hackathon-winning platforms", which is vaguer than
  the CV's precise 1st and 3rd placings. Kevin is deciding whether to make it
  exact. Don't change it unilaterally.
- The "what it taught me" sections are drafted for Kevin and pending his review.
  They're claims about his own thinking — he edits those, not you.

## Things that are deliberate, not bugs

- The hero right column is a Famicom dialogue box with a blinking caret. It is the
  one intentionally loud element on the page. Everything else stays disciplined.
- The toolkit is a bordered "inventory" grid rather than pill chips, with the
  strongest four skills marked in vermilion.
- `h1 { max-width: 19ch }` — the headline is meant to break into two lines. An
  earlier `13ch` broke it into four stubs; don't go back below ~18ch.
- Project cards each throw a shadow in their own colour on hover (`.proj-1`
  through `.proj-4`). The variation is the point.
- Inside `#writeups`, the nested `.band` elements drop their divider rule
  (`#writeups .band { border-top: none }`). One section, four cards — the card
  borders already do the separating.
