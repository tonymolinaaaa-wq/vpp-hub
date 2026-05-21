# Homepage Architecture — Valley Painting Pros Hub

This document specifies the section structure of the home page (`/src/pages/index.astro`). Every section is implemented as one component in `/src/components/`. Sections are built in the order listed below, one PR per section, after the combined Header + Hero PR.

Read this alongside `/CLAUDE.md`, `/docs/brand-brief-v2.md`, `/docs/design-research.md`, and `/docs/about-story.md`. Where this doc conflicts with those, the project files win.

## Background sequence (locked in CLAUDE.md)

Per CLAUDE.md: `Bone → Ink → Bone → Putty → Bone → Ink → Bone → Putty → Bone → Ink`. Adjacent sections never share a background. Never introduce a fifth background.

**Interpretive note (flagged for review):** the Header is treated as structural chrome — a sticky element above the content sections — and is not counted in the 10-element alternation sequence. The alternation applies to the 10 content sections starting with the Hero. The Header background is Bone so it dissolves visually into the Bone Hero below it. This interpretation is consistent with how every cross-category reference in `/docs/design-research.md` (Mercury, Stripe, Linear, The Hoxton, Aesop) treats their header chrome. If the strict reading of CLAUDE.md is preferred (Header counted as the first Bone, Hero forced to Ink), flag the override and stop.

## Section order, top to bottom

**Header (chrome)** — Bone background. Sticky. Wordmark left (visually dominant, fills available header height), hamburger menu toggle right. Hairline rule (`--vpp-rule`) below. No phone CTA at any breakpoint. No tagline. No inline nav links — nav lives behind the menu toggle (panel built in a later PR).

**1. Hero** — Bone background. IBM Plex Mono ALL CAPS metadata line, one-orange-word Archivo Black H1, Inter subhead, Ricardo's name + photo block + role line, two echoed VPP Promise callouts ($300/Day Finish Promise and Daily Photo Updates), primary CTA "Get your written estimate" (filled Signal Orange), secondary CTA click-to-call (480) 433-2680 (ghost / outlined).

**2. VPP Promise** — Ink background. Five-card row (stacked on mobile): $300/Day Finish Promise, HOA Champion Service, Daily Photo Updates, Lifetime Touch-Up Kit, 12-Month Wellness Check. Full descriptions per `/docs/brand-brief-v2.md` Section 07.

**3. Services** — Bone background. Three cards, equal visual weight, in this order: Cabinet Refinishing, Interior Painting, Exterior Painting. Each card: photo, service name (Archivo Black, one orange word), short Inter description, named price range with conditions (per Principle 6), "Learn more" link to that service's page.

**4. About** — Putty background. The ~480-word locked story from `/docs/about-story.md` used verbatim. Three insider quotes (one coat / power-washing / just finish it) styled as pull-quotes: indented, italic. Ricardo's photo. "Painting. Done right." tagline as sign-off.

**5. Recent Work / Gallery preview** — Bone background. 3–4 project tiles linking to individual project pages. Each tile: full-bleed photo, IBM Plex Mono metadata line (neighborhood · service · duration · paint product code), one-sentence description in Mid color. "See all projects" link to the gallery index.

**6. Testimonials** — Ink background. 3–5 static testimonials, no carousel, no auto-rotate. Each: quote in Inter, attribution in IBM Plex Mono (first name + last initial · neighborhood · service · date). At launch, this section may contain a single placeholder microcopy ("Reviews from East Valley homeowners — coming as we collect them") until review velocity supports 3+ named testimonials.

**7. Service Area** — Bone background. Six cities listed: Queen Creek, Chandler, Gilbert, Mesa, Tempe, Scottsdale. Queen Creek listed first per `/docs/brand-brief-v2.md` Section 01 priority. Each city links to its service-area page (to be built in subsequent PRs).

**8. FAQ** — Putty background. 6–8 question/answer pairs. Inter body. No bold for emphasis. IBM Plex Mono eyebrow above each question.

**9. Get Estimate CTA band** — Bone background. Section eyebrow, Archivo Black H2 with one orange word, Ricardo's name reinforcement, primary CTA "Get your written estimate" (filled orange), secondary click-to-call.

**10. Footer** — Ink background. Wordmark in knockout (use `/public/logos/VPP_Wordmark_Horizontal.svg` with the knockout variant if available, or `VPP_Mark_Knockout.svg` as fallback). License, address, phone, email. Social links. IBM Plex Mono ALL CAPS for credentials. Copyright.

## Build order

1. **Header + Hero** (this PR — combined for the first visible above-the-fold deliverable)
2. Footer (built early because every page references it)
3. VPP Promise section
4. Services section
5. About section
6. Recent Work / Gallery preview section
7. Testimonials section (initially with placeholder)
8. Service Area section
9. FAQ section
10. Get Estimate CTA band section
11. Final page assembly + full-page QA pass at all 6 breakpoints

## Component naming convention

PascalCase. Section components from PR #4 onward live in `/src/components/sections/`. The Header and Footer live in `/src/components/` directly.

## CTA destinations

- `Get your written estimate` → `/estimate` (page does not exist yet — will 404 until the estimate-page PR; this is acceptable for the wireframe build).
- `(480) 433-2680` → `tel:+14804332680`.

## Photography placeholders

Ricardo's photo asset is not yet in `/public/`. Every place a real photo is required, use a neutral placeholder block — `--vpp-rule` background, 1px solid `--vpp-mid` border, sized to the final aspect ratio, with centered IBM Plex Mono ALL CAPS Mid-color text reading `PHOTO PLACEHOLDER`. Real photos will be added in a later PR.

## Logo assets in `/public/logos/`

- `VPP_Wordmark_Horizontal.svg` — Header (primary use), Footer (knockout context)
- `VPP_Wordmark_Stacked.svg` — reserved for compact/square contexts (not used in v1)
- `VPP_Mark_Primary.svg` — favicon (already in use)
- `VPP_Mark_OnAccent.svg` — for use on Signal Orange backgrounds
- `VPP_Mark_Knockout.svg` — for use on Ink (dark) backgrounds
- `VPP_Mark_Mono.svg` — single-color reproduction contexts
