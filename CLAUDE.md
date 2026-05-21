# CLAUDE.md — Valley Painting Pros (vpp-hub)

This file is the standing context Claude Code reads at the start of every session in this project. It contains the rules and identity that don't change. It is NOT a task list or build plan — those live in the prompts Ricardo gives Claude Code on demand. Deeper reference material lives in `/docs/`.

---

## TOP-LEVEL RULE — read this first, every session

**No assumptions. Verify everything before acting.**

This rule supersedes everything else in this file. Examples of what it means in practice:

- Before claiming a file does or does not exist, view the filesystem.
- Before claiming a package is or is not installed, check `package.json` and `node_modules/`.
- Before claiming the build succeeds, run the build.
- Before deploying anything, confirm Ricardo has explicitly approved the deploy.
- Before changing anything that touches the live domain `valleypaintingpros.com`, stop and ask. The live domain serves the legacy cabinet landing page from a separate project (`vpp-website`), not this one.
- If a request from Ricardo conflicts with a rule in this file, raise it before executing.
- If a task is ambiguous, ask one clear question rather than guessing.

Ricardo is not a developer. He cannot verify code-level claims himself; he relies on Claude Code's honesty. Never invent, never hand-wave, never describe work as done when it isn't.

---

## PROJECT IDENTITY

**Business:** Valley Painting Pros (VPP) — residential painting in the East Valley of Arizona.

**Owner-operator:** Ricardo Parra. Named on every estimate, every contract, every escalation. The site must feel like Ricardo's business, not "a painting company."

**Field operations partner:** Gereimy Hernandez (she/her). Mastered all three VPP service disciplines.

**Service area (six cities, Queen Creek is priority):** Chandler, Gilbert, Mesa, Queen Creek, Tempe, Scottsdale.

**Services (presented with equal visual weight on the hub):** Cabinet refinishing (anchor strength, highest margins), Interior painting, Exterior painting.

**Credentials:**
- AZ ROC License: #363664 — Class CR-34 Dual (Residential & Commercial Painting)
- Bonded
- Insured: $1M per occurrence / $2M aggregate general liability through ERGO NEXT Insurance
- A+ BBB Accredited

**Contact:**
- Phone: (480) 433-2680
- Current operational email: valleypaintingprosllc@gmail.com
- Planned email (to be provisioned via Google Workspace before launch): info@valleypaintingpros.com

**Tagline:** **Painting. Done right.** — never "Painters. Direct." or any earlier variant.

---

## INFRASTRUCTURE MAP

**Laptop project location:** `C:\Users\tonym\projects\vpp-hub\` — Windows, PowerShell, Astro 5 + TypeScript strict mode.

**GitHub repository:** https://github.com/tonymolinaaaa-wq/vpp-hub — public, branch `main`, owned by `tonymolinaaaa-wq`.

**Vercel project:** `vpp-hub` — auto-deploys from GitHub `main` to https://vpp-hub.vercel.app on every push. This is the working preview URL.

**Live production domain (DO NOT TOUCH from this project):** valleypaintingpros.com and www.valleypaintingpros.com both point to a separate Vercel project named `vpp-website`, which serves the legacy Next.js cabinet landing page. That project is not part of this codebase. Never push code there, never modify its DNS, never assume it should be replaced as part of this build's deploy pipeline.

**Cutover constraint:** The new hub (this project) and the legacy cabinet page (separate project) coexist until Ricardo explicitly approves cutover. Until then, all deploys go to vpp-hub.vercel.app. No exceptions.

---

## TECH STACK — LOCKED

- **Framework:** Astro 5 (currently 6.3.5)
- **TypeScript:** strict mode
- **Styling:** Astro scoped component styles + shared design tokens in `/src/styles/tokens.css` and `/src/styles/fonts.css`. No Tailwind. No UI component library (no shadcn/ui, no Radix, etc.). No CSS-in-JS.
- **Fonts:** Archivo Black (display), Inter (body), IBM Plex Mono (metadata). Delivered via `@fontsource/*` packages installed as dependencies, NOT via Google Fonts CDN.
- **Hosting:** Vercel, auto-deploy from GitHub main.
- **Forms / dynamic features:** TBD per feature. Default to static; add interactivity only when necessary.

**Why this stack:** Astro ships zero JavaScript by default, which is the right baseline for an SEO-first marketing site competing with Freedom Painting on local search. Tailwind was rejected because VPP has a strict locked design system (six colors, three fonts, specific spacing) and utility classes would constantly tempt drift toward generic values when the right answer is a custom value from `tokens.css`.

---

## BRAND SYSTEM — VISUAL

### Colors (use the named tokens from `/src/styles/tokens.css`, never raw hex)

| Token | Hex | Role |
|---|---|---|
| `--vpp-signal` | `#D65A3A` | Signal Orange — accent only. CTAs, one-word headline highlights, section numerals. **CAP AT ~10% OF ANY SURFACE.** |
| `--vpp-ink` | `#1A1A1A` | Ink — text, primary neutral, dark backgrounds. |
| `--vpp-bone` | `#FBF8F1` | Bone — page background, paper surface, reverse text on dark backgrounds. |
| `--vpp-putty` | `#E8E2D3` | Putty — secondary backgrounds for visual rhythm between sections. |
| `--vpp-mid` | `#5A4A3A` | Mid — secondary text, metadata. |
| `--vpp-rule` | `#D9D2C0` | Rule — hairlines, dividers, subtle borders. |

### Typography (three faces, defined in `/src/styles/fonts.css`)

| Role | Family | Weights | Treatment |
|---|---|---|---|
| Display | Archivo Black | 900 | ALL CAPS, tight tracking (~-0.01em). Headlines, section titles, wordmark. **DISPLAY ONLY.** |
| Body | Inter | 400 / 500 / 600 / 700 | Sentence case, normal tracking. Paragraphs, UI labels, descriptions, links. |
| Mono | IBM Plex Mono | 400 / 500 / 600 | ALL CAPS, wide tracking (~0.18-0.22em). Eyebrows, phone numbers, license number, metadata. |

Every text element maps to one of these three roles. No off-system fonts. No defaulting to Inter for everything.

### Border radius

| Token | Value | Use |
|---|---|---|
| `--vpp-radius` | `8px` | All boxes, cards, chips, buttons, photo placeholders. Single value applied universally. |

The Header strip and full-width section frames stay edge-to-edge — radius applies to bounded objects within sections, not the section containers themselves.

### Design grammar (locked rules)

1. **One-orange-word rule.** Every headline contains exactly ONE keyword in Signal Orange. The rest stays in Ink (on light backgrounds) or Bone (on dark).
2. **Signal Orange ~10% cap.** No section's total orange ink (text + fills + accents) exceeds approximately 10% of the surface. Restraint is the design.
3. **Section background alternation.** Sections alternate backgrounds for visual rhythm. Adjacent sections never share a background. Sequence: Bone → Ink → Bone → Putty → Bone → Ink → Bone → Putty → Bone → Ink. Never introduce a fifth background.
4. **8px spacing scale.** All margins and padding are multiples of 8px. Section verticals use 64 / 96 / 128. No one-off pixel values.
5. **SVG over PNG.** Always prefer SVG for logos, icons, and any graphic that can be vector. PNG only for photographs.

---

## BRAND SYSTEM — VOICE

### Patterns that earn trust on this site

- **Specific numbers beat adjectives.** "$300/day late credit" beats "on time." "238 Chandler homes painted" beats "many satisfied customers." "5 days" beats "fast."
- **Named human with a face.** "Ricardo runs every estimate himself" beats "our team of experts." Ricardo's name + photo appear on every estimate-related surface.
- **Plain English about prep.** "We pressure-wash, scrape, prime, caulk, then paint" beats "premium preparation process." Trade verbs, never trade jargon.
- **Risk-reversal in writing.** "If we miss the date, we credit you $300/day. In writing on the contract." Never a vague guarantee.
- **Geographic specificity.** "Chandler / Ocotillo / Power Ranch / San Tan Heights" beats "the Valley."
- **Headlines are statements, never questions.** "Painting. Done right." not "Looking for a painter you can trust?" Every H1 and H2 ends with a period.

### Banned phrases — never use these

- "World-class craftsmanship," "exceptional quality," "unparalleled service"
- "Family-owned and operated" alone (it's the dominant cliché in the painting category)
- "We're not happy until you're happy"
- "Transform your space"
- "Solutions" as a noun
- "Affordable" without a number
- "Partner with us," "discover the difference," "elevate"
- "Seamless," "unlock," "leverage" (generic AI clichés)
- Listing certifications without context (e.g., bare "PCA Accredited" with no explanation)

### When in doubt, ask "what would Ricardo say to a homeowner at her kitchen table?"

---

## DESIGN PRINCIPLES — FROM CROSS-CATEGORY RESEARCH

These 11 principles distill the cross-category design research (full version in `/docs/design-research.md`). Every build decision should be testable against them.

1. **Headlines are statements, never questions.** Period at the end of every H1 and section H2.
2. **Proof signals appear above the fold as primary metadata, not as footer disclosure.** AZ ROC #363664 · CLASS CR-34 DUAL · A+ BBB · $1M/$2M INSURED displays in IBM Plex Mono ALL CAPS between the wordmark and the H1 on the home hero.
3. **Body copy never uses bold for emphasis.** Emphasis comes from an IBM Plex Mono eyebrow line above the Archivo Black headline. Inter Medium is reserved for attribute labels in spec lists.
4. **Photography is owned, not licensed.** No stock paint cans, no stock smiling homeowners, no generic suburb hero photos. Use only real VPP work with the photo guidelines from the brand brief.
5. **Signal Orange cap enforced at the component level.** Section ink total stays at ~10% orange or less. Filled-orange CTAs are the only fully-saturated orange elements per section. One orange word per headline.
6. **Every service page publishes a named price range with conditions.** No "request a quote for pricing." Honest range plus an Everlane-style cost breakdown link to a "How we price" page.
7. **Named human appears on every estimate-related surface.** Ricardo's name and photo on home hero proof block, every service page, About, and estimate confirmation. Gereimy gets her own named block on About and on field-ops-related copy. Never "our team" or "our experts" where a named human is the truthful substitute.
8. **Galleries are project pages with metadata, not card grids with lightboxes.** No auto-rotating sliders anywhere — not for galleries, testimonials, or hero images. Each project gets its own URL with full-bleed cover, neighborhood + service + duration + paint code in Mid metadata, 4-6 stacked before/after pairs, a 60-120 word note from Ricardo.
9. **Testimonials are static, named, and neighborhood-tagged.** 3-5 per page. First name + last initial + neighborhood ("Sarah K. · Power Ranch · exterior repaint, May 2025"). No carousels.
10. **About uses the locked story verbatim; services close in named accountability.** The About section uses the ~480-word locked story in `/docs/about-story.md` as-is. The story opens with the homeowner's fear, reaches Ricardo's first-person voice at paragraph three (intentional — never rearrange), and closes with the five Promises and the tagline as sign-off. The three quoted insider lines (one coat / power-washing / just finish it) get pull-quote treatment — indented, italic. Every service page closes with a one-line signed accountability statement.
11. **The 8px spacing scale is the only spacing system.** Section backgrounds follow the locked alternation. Never introduce a fifth background color.

---

## FILE & FOLDER MAP

```
/                          project root
├── CLAUDE.md              this file — standing context
├── astro.config.mjs       Astro configuration
├── package.json           dependencies and scripts
├── tsconfig.json          TypeScript config (strict mode)
├── public/                static assets, served at root URL
│   ├── favicon.svg        VPP branded favicon (V monogram on Signal Orange)
│   ├── favicon-16.png, favicon-32.png, apple-touch-icon.png, icon-192.png, icon-512.png
│   ├── snippet.html       reference for the favicon link tags
│   └── logos/             6 brand SVGs
│       ├── VPP_Mark_Primary.svg
│       ├── VPP_Mark_Knockout.svg
│       ├── VPP_Mark_Mono.svg
│       ├── VPP_Mark_OnAccent.svg
│       ├── VPP_Wordmark_Horizontal.svg
│       └── VPP_Wordmark_Stacked.svg
├── src/
│   ├── pages/
│   │   └── index.astro    home / hub page (currently default Astro template)
│   ├── styles/
│   │   ├── tokens.css     CSS custom properties for colors + spacing tokens
│   │   └── fonts.css      @font-face declarations and utility classes
│   └── (layouts/, components/ to be created as needed)
└── docs/
    ├── brand-brief-v2.md      consolidated brand brief — read when needing deep brand context
    ├── design-research.md     cross-category precedents and the 11 design principles in full
    └── about-story.md         the locked About story, ~480 words, ready to use verbatim
```

When a task touches deep brand context (the VPP Promise details, the competitive landscape, the priority customer segments), read `/docs/brand-brief-v2.md` first.

When a task touches visual or voice design decisions, read `/docs/design-research.md` first.

When building the About section, use `/docs/about-story.md` verbatim or as the foundation.

---

## INSTALLED SKILLS — WHEN TO LEAN ON EACH

Three Claude Code skills are installed in `C:\Users\tonym\.claude\skills\`:

- **`frontend-design`** (Anthropic, official) — use for component design, visual hierarchy, and any time the default tendency might be generic AI styling. Forces a brand-honoring approach before code is written.
- **`publishing-astro-websites`** (SpillwaveSolutions) — use for Astro framework patterns: content collections, partial hydration, Markdown/MDX, deployment, performance. Astro has its own idioms that aren't standard frontend knowledge.
- **`claude-seo`** (AgriciDaniel) — family of sub-skills. Use the relevant one for each SEO task:
  - `seo-local` — local business SEO foundations
  - `seo-maps` — Google Business Profile and Maps optimization
  - `seo-schema` — LocalBusiness JSON-LD structured data
  - `seo-technical` — Core Web Vitals, mobile usability, indexability
  - `seo-content` — content quality and E-E-A-T
  - `seo-sitemap` — sitemap.xml generation

---

## WHAT REQUIRES RICARDO'S EXPLICIT APPROVAL

Never act on any of these without confirmation:

- Deploy to production (any environment Ricardo would consider "live")
- DNS changes of any kind
- Anything touching valleypaintingpros.com or www.valleypaintingpros.com
- Anything touching the separate `vpp-website` project on Vercel or GitHub
- Anything that costs money (paid integrations, premium fonts, plugins, services)
- Adding any new framework, library, or major dependency that wasn't already in `package.json`
- Adding Tailwind, UI component libraries, or any styling system beyond the locked stack
- Business decisions: pricing, copy that makes claims about VPP's services, financial guarantees beyond what's in the locked VPP Promise
- Anything that flags VPP's current crew/vetted-subcontractor operating model in customer-facing copy (it stays under the radar — never disclosed unless asked directly by a homeowner). Note: this rule applies to the CURRENT operating model only. Ricardo's personal history as a subcontractor under bigger painting companies — referenced openly in the locked About story — is a separate, intentional credential disclosure and stays as written.

---

## WORKING WITH RICARDO

- Ricardo is not a developer. He does not write code, read code, or debug code directly. He shares Claude Code's output with another Claude instance (via Claude.ai chat) for translation, evaluation, and strategy.
- When introducing technical concepts, explain them plainly before using them. Avoid jargon. When jargon is unavoidable, define it the first time.
- When tasks involve multiple steps, walk through them in order. Do not assume Ricardo knows the next step.
- Trust Ricardo's domain expertise on the painting business — he runs it, you don't. He trusts yours on the technical side. Both sides are real expertise.
- Don't bury decisions in tool calls. When a real choice exists (e.g., "which library for X?"), surface it to Ricardo with a recommendation and the trade-offs.
- Mobile-first. Most VPP visitors arrive on a phone. Test breakpoints early and often.
- Performance budget: target Lighthouse 95+ across all four categories. Astro's zero-JS default is the baseline — don't add JavaScript without a specific reason.

---

## SUCCESS METRICS — what we're optimizing for

This site is a conversion machine for HOA-pressed homeowners and kitchen refreshers. The success conditions:

- Lighthouse 95+ across Performance, Accessibility, Best Practices, SEO
- Mobile-first rendering with no horizontal scroll on any standard breakpoint
- Conversion form completion: 3% by month 3, 5% by month 6
- Top 3 Google Maps placement for "painter [city] AZ" in 3+ of the 6 service-area cities by month 12
- 100+ Google reviews aggregated by month 12

Every build decision should be testable against the question: does this make the site faster, more trustworthy, more visibly local, or more clearly differentiated from Freedom Painting and the other East Valley competitors? If not, reconsider.

---

*This file is a living document. When a rule materially changes, update it. CLAUDE.md and the docs in `/docs/` should always agree.*
