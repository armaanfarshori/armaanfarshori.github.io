# armaanfarshori.github.io - Design System

Personal site for Armaan Farshori: founder of Cyvire, security program operator.
One page, one theme, heavy but purposeful motion. This document is the contract
every section of the site follows.

## 1. Brand direction

**Concept: "Instrument panel."** Not a SaaS landing page, not a resume template.
The site should feel like the personal console of someone who runs security
programmes and is building a company: dark, warm, precise, kinetic. Motion
communicates state and hierarchy, never decoration for its own sake.

Voice: first person, short declarative sentences, no filler verbs
(no "elevate", "seamless", "unleash"). No em-dashes anywhere in copy.

What we deliberately avoid (AI-portfolio tells):

- Inter, purple gradients, cyan-on-navy, centered hero over mesh gradient
- Pulsing status dots, "scroll down" cues, section-number eyebrows
- Three equal feature cards, uniform fade-up on every element
- Fake-precise numbers, fake product screenshots, version badges

## 2. Color tokens

One theme (dark), locked for the whole page. Sections never invert.
One accent. Everything else is warm neutrals.

| Token          | Value                    | Use                             |
| -------------- | ------------------------ | ------------------------------- |
| `--bg`         | `#0B0A08`                | Page ground (warm near-black)   |
| `--bg-2`       | `#12100C`                | Raised bands, nav               |
| `--surface`    | `#171410`                | Cards, panels                   |
| `--line`       | `rgba(226,213,190,.12)`  | Hairlines, borders              |
| `--line-2`     | `rgba(226,213,190,.28)`  | Hover borders                   |
| `--text`       | `#EDE6D8`                | Primary type (warm bone)        |
| `--mid`        | `#A79E8C`                | Secondary type                  |
| `--faint`      | `#6B6355`                | Tertiary, labels                |
| `--accent`     | `#F0A63B`                | Amber. The only accent.         |
| `--accent-12`  | `rgba(240,166,59,.12)`   | Accent fills                    |
| `--accent-35`  | `rgba(240,166,59,.35)`   | Accent borders                  |
| `--ink`        | `#161006`                | Text on accent surfaces         |

Amber phosphor on warm black: instrument-panel reference, distinctive against
the teal/purple default space, and it holds WCAG AA for large type and labels.
Body copy is always `--text` or `--mid`, never accent.

## 3. Typography

| Role    | Face                          | Notes                                    |
| ------- | ----------------------------- | ---------------------------------------- |
| Display | Archivo (variable, wdth+wght) | 800-900, `font-stretch: 125%`, tight     |
| Body    | Geist                         | 400/500/600, 1.7 line-height             |
| Mono    | Geist Mono                    | Labels, dates, data, terminal moments    |

- Display scale: name `clamp(3.4rem, 11vw, 8.5rem)`, section titles
  `clamp(1.9rem, 3.6vw, 2.8rem)`, tracking `-0.03em`.
- Body: 16px base, `max-width: 62ch` for paragraphs.
- Mono labels: 11px, `letter-spacing: .18em`, uppercase. Max one labelled
  "eyebrow" per three sections.
- No Inter. No serif.

## 4. Spacing, layout, radii

- 8px base unit. Section rhythm: `clamp(96px, 12vh, 150px)` vertical.
- Content column: `max-width: 1120px`, 28px gutters (20px mobile).
- Nav: single line, 64px tall.
- Hero fits the initial viewport. Headline max 2 lines, subtext max 20 words,
  max 4 stacked elements.
- Layout families never repeat: hero (type-led), marquee (one, only one),
  feature split (Cyvire), timeline (record), asymmetric bento (expertise),
  index rows (writing), type-led band (contact).
- Radii: `--r-s: 4px` (chips, small ui), `--r-m: 10px` (cards). Sharp corners
  are part of the brand; nothing pill-shaped except nowhere.

## 5. Motion language

Motion is the product here, so it is specified like one.

### Tokens

| Token        | Value                              | Use                          |
| ------------ | ---------------------------------- | ---------------------------- |
| `--t-fast`   | `150ms`                            | Hovers, presses              |
| `--t-base`   | `300ms`                            | State changes                |
| `--t-reveal` | `700ms`                            | Scroll reveals               |
| `--t-mask`   | `900ms`                            | Clip/mask reveals            |
| `--e-out`    | `cubic-bezier(0.16, 1, 0.3, 1)`    | Entrances (expo-out)         |
| `--e-inout`  | `cubic-bezier(0.65, 0, 0.35, 1)`   | Movement between states      |
| `--e-mask`   | `cubic-bezier(0.77, 0, 0.175, 1)`  | Masked/clip reveals          |

Stagger: 70-90ms between siblings. Stagger is used in exactly two places
(hero line reveal, expertise bento) so it stays a moment, not a texture.

### Inventory (each element animates differently by hierarchy)

- **Enter sequence**: 800ms boot bar + name scramble, then a full-page
  clip wipe. Runs once, never blocks input after 1.2s total.
- **Hero type**: per-line mask reveal (translateY 110% to 0 inside
  `overflow:hidden`, `--e-mask`). Descriptor word cycles with a
  monospace scramble/decrypt effect every 2.8s.
- **Living background**: canvas node mesh in the hero. Nodes drift, link
  under 130px, repel within 140px of the pointer. Amber lines at low alpha.
  Paused when offscreen, capped at devicePixelRatio 2.
- **Scroll reveals**: IntersectionObserver only (no scroll listeners for
  reveals). Headings use clip reveals, cards use 20px rise + fade,
  the timeline draws its spine (scaleY) and pops dots with a back-out curve.
- **Counters**: stats count up once when 50% visible, 1.2s ease-out cubic.
- **Cyvire card**: pointer tilt (max 6deg rotateX/Y, spring return), a
  radial glow that follows the pointer, border brightens on hover.
- **Magnetic buttons**: primary CTAs translate up to 6px toward the pointer
  inside a 60px field, spring back on leave. Applied to two elements total.
- **Index rows (writing)**: hover slides the row 6px, reveals an arrow,
  tints the background. 200ms, `--e-out`.
- **Marquee**: one, 30s linear loop, pauses on hover.
- **Scroll progress**: 2px top bar, transform-only writes in rAF.
- **Exits**: always subtler than enters (opacity only, 60% duration).

### Rules

- Animate `transform`, `opacity`, `clip-path`, `filter` only. Never layout
  properties. `will-change` only on the canvas and tilt card.
- Hover feedback everywhere interactive: 150-200ms. `:active` compresses
  `scale(0.98)`.
- Nothing loops visibly except the marquee and the ambient canvas.
  No pulsing dots or glowing badges.
- Interruptible: state motion uses transitions, not keyframes.

### Reduced motion

`prefers-reduced-motion: reduce` collapses everything: no boot sequence,
no canvas, no marquee autoplay (wraps to a static row), no tilt or magnetism,
reveals render in place instantly. The page is fully readable with zero motion.

## 6. Components

- `nav` fixed, blur backdrop, active-section highlight
- `.btn` primary (amber fill, ink text) and ghost (line border), 1-3 word labels
- `.chip` mono 12px capability tags
- `.card` surface + line border, hover lifts border not the box shadow
- `.row` writing/publication index rows
- `.stat` mono numerals with accent unit marks
- `.spine` timeline with drawn line and role entries
- Footer: live IST clock (mono), languages, copyright

## 7. Performance and a11y budget

- Static single page, system-hosted fonts via Google Fonts, no frameworks.
- One canvas, one IntersectionObserver set, no third-party JS.
- Semantic landmarks, skip link, `:focus-visible` rings, AA contrast,
  descriptive alt text, `aria-expanded` on the mobile menu.
