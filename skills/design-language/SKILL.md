---
name: design-language
description: The reference library of crafted, non-generic, fully responsive design that the design-engineer agent studies and applies. Captures concrete rules for typography, color, layout, motion, and the "AI-look" tells to avoid — plus six curated Design Kits (complete, ready-to-commit visual systems) and Screen Blueprints for common page types, and a place to grow the user's own references over time. Use whenever designing or reviewing UI that must look deliberately crafted, not AI-generated.
origin: custom
---

# Design Language

The point of this file: make designs look **deliberately crafted** — so people
react to the work, not to "this was made by AI" — and make them work flawlessly
on desktop, tablet, and mobile. The `design-engineer` agent should read and
apply this before building UI.

> Provenance note: the seed principles below are general, widely-accepted design
> best practices, restated in our own words. They are not copied verbatim from
> any third-party or leaked document. Add your own references freely.

## Core Principles

### Typography
- The typeface carries the design. **Avoid overused defaults** (Inter, Roboto,
  Arial, system stacks); choose a distinctive, intentional face that fits the tone.
- Clear type scale and hierarchy; don't let everything sit at one weight/size.
- Use `text-wrap: pretty` / `balance`; generous minimum sizes; ≥44px touch targets.

### Color
- Source palette from the brand or existing system **first**.
- If choosing freely, build harmonious palettes (e.g. OKLCH for perceptual
  consistency). Commit to a **restrained** set — 1-2 backgrounds, a clear accent.
- Avoid aggressive gradients as the primary visual; use color with intent.

### Layout & Spacing
- CSS Grid + a real spacing system (tokens / scale), never magic pixel values.
- Visual rhythm and intentional whitespace; every element earns its place.
- Establish patterns up front (headers, sections, cards) and reuse them.

### Motion
- Subtle, purposeful transitions (state, focus, entrance) — not decoration.
- CSS transitions or simple state for prototypes; reserve heavy animation for
  real motion deliverables. Respect `prefers-reduced-motion`.

### Responsiveness (non-negotiable)
- Mobile-first; verify at ~360px, ~768px, ≥1280px.
- Fluid type (`clamp()`), `aspect-ratio` images, container/media queries.
- No horizontal scroll at any width; controls/toolbars don't shift on hover/label.
- Complex interactions must stay usable (and reachable) on small screens.

### Accessibility
- Semantic HTML, labeled controls, visible focus, logical tab order.
- WCAG AA contrast; never rely on color alone.

## "AI-look" Tells to Avoid
- Generic purple/blue gradients, glowing blobs, floating 3D shapes for decoration.
- **Rounded-corner cards with a left-border accent stripe** (classic giveaway).
- Cards inside cards inside cards; uniform equal-weight boxes.
- Decorative SVG "illustrations" as stand-in imagery; emoji as visual design.
- Vague hero copy ("Empower your workflow"); random stock imagery.
- Uniform spacing with no hierarchy.

## Design Kits

The design-engineer picks **ONE kit per product and commits to it** — every token,
face, and motion rule comes from that kit, never a blend. Kits exist so quality
never depends on improvisation. If the project has an existing brand or design
system, **the brand wins** and the kit only fills the gaps it leaves open.

### 1. Editorial SaaS

Confident magazine voice for software — reads like a publication, sells like a product.

- **Use when:** SaaS marketing sites, landing pages, launch/announcement pages. **Never for:** data-dense apps, admin tools, or anything with tables.
- **Typography:** Fraunces (600/700, optical sizing on) for headings; Libre Franklin (400/500/600) for body and UI. Display sizes via `clamp(2.5rem, 1rem + 5vw, 4.5rem)`; body 1.0625rem / 1.6.
- **Color:** warm paper + ink + one deep editorial red.

```css
--bg: oklch(0.98 0.005 90);
--surface: oklch(0.955 0.008 85);
--text: oklch(0.22 0.015 60);
--text-muted: oklch(0.46 0.02 60);
--accent: oklch(0.52 0.19 25);
--accent-contrast: oklch(0.98 0.005 90);
```

  Semantic: desaturated green/amber/red at the accent's lightness (≈0.52 L, ≤0.14 C) so they sit in the same family.
- **Spacing & layout:** 8px base, scale 8/16/24/40/64/96; max content width 72rem with generous side margins; 12-col grid with deliberate asymmetry (7/5 or 8/4 splits, not 6/6); section padding 96-128px desktop, 56-72px mobile.
- **Shape:** radius 2-6px only; 1px hairline rules (`--text` at 15% alpha) instead of boxes; shadows essentially banned — separate with rules and whitespace.
- **Motion:** 200-300ms `ease-out`; one fade-and-rise (12px) per section on first scroll-in, nothing looping; links underline on hover with `text-underline-offset`.
- **Signature details:** oversized section numerals ("01") in Fraunces at muted opacity; uppercase letterspaced eyebrows (0.08em) above headings, separated by a short 32px hairline; pull-quotes or stat callouts set huge in the heading face mid-page.
- **Don't:** never center-align every section — alternate left-anchored and asymmetric layouts so the page has an editorial spine.

### 2. Utility Dashboard

Quiet, dense, and fast — the interface disappears so the data can do the talking.

- **Use when:** internal tools, admin panels, analytics, anything data-dense. **Never for:** marketing pages or emotional first impressions.
- **Typography:** IBM Plex Sans (600 headings, 400/500 body/UI); IBM Plex Mono (400) for IDs, timestamps, and numerals in tables. Headings small and quiet (1.125-1.5rem); body 0.875-0.9375rem; `font-variant-numeric: tabular-nums` everywhere data lives.
- **Color:** cool neutral ground, white surfaces, one workhorse blue.

```css
--bg: oklch(0.97 0.003 250);
--surface: oklch(1 0 0);
--text: oklch(0.25 0.02 255);
--text-muted: oklch(0.5 0.02 255);
--accent: oklch(0.55 0.16 250);
--accent-contrast: oklch(0.99 0 0);
```

  Semantic: green oklch(0.55 0.14 150), amber oklch(0.65 0.13 80), red oklch(0.55 0.18 25) — always paired with a label, never color alone.
- **Spacing & layout:** 4px base, scale 4/8/12/16/24/32; fluid full-width shell with a 240-280px sidebar, content max ~1440px; table rows 40px, form rows 64px; pack density deliberately — whitespace between groups, not inside them.
- **Shape:** radius 6-8px; every surface gets a 1px border (`--text` at 10% alpha); shadows only on overlays (menus, dialogs) — never on resting cards.
- **Motion:** 120-160ms `ease-out`; only state changes animate (hover, expand/collapse, toast in/out); zero entrance animations — dashboards should feel instant.
- **Signature details:** 1px row dividers instead of card-per-row; status rendered as 8px dot + text label; sticky table headers and a persistent page-level toolbar that never shifts on hover.
- **Don't:** never nest cards — one container level deep, then dividers and spacing do the rest.

### 3. Premium Commerce

Gallery restraint — the product is the hero and everything else gets out of its way.

- **Use when:** refined e-commerce, fashion, objects, product-as-hero brands. **Never for:** discount-driven retail or dense catalog grids.
- **Typography:** Instrument Serif (400, display only — it has one weight, use size for hierarchy) for headings and prices; Manrope (400/500/600) for body, navigation, and buttons. Hero display via `clamp(2.75rem, 1rem + 6vw, 5rem)`.
- **Color:** warm gallery neutrals with an espresso-bronze accent.

```css
--bg: oklch(0.985 0.002 80);
--surface: oklch(0.96 0.005 80);
--text: oklch(0.18 0.01 80);
--text-muted: oklch(0.46 0.015 80);
--accent: oklch(0.35 0.06 55);
--accent-contrast: oklch(0.98 0.004 80);
```

  Semantic: keep them quiet — deep green / ochre / brick at low chroma (≤0.12 C); errors whisper here, they don't shout.
- **Spacing & layout:** 8px base, scale 8/16/32/56/88; max width 80rem; 12-col grid with wide (40-56px) gutters; imagery owns ≥60% of the viewport on product screens; alternate full-bleed image bands with narrow (~38rem) text columns.
- **Shape:** radius 0-2px — sharp and architectural; no visible borders, separation comes from whitespace and surface shifts; at most one soft shadow, reserved for the sticky add-to-cart bar.
- **Motion:** 300-400ms with `cubic-bezier(0.22, 1, 0.36, 1)`; image crossfades on variant change; hover zoom capped at `scale(1.02)`; nothing bounces.
- **Signature details:** uppercase letterspaced eyebrows (0.12em) for categories; prices set in Instrument Serif at heading scale; product images on `--surface` with consistent `aspect-ratio` and object framing.
- **Don't:** no badge clutter — one quiet text label maximum per product card, never stacked "SALE / NEW / HOT" ribbons.

### 4. Warm Commerce

Friendly and tactile — a shop that feels run by humans who'd remember your name.

- **Use when:** consumer e-commerce, food, kids, home goods, community brands. **Never for:** enterprise software or austere luxury positioning.
- **Typography:** Bricolage Grotesque (600/700) for headings — its quirk carries the warmth; Outfit (400/500) for body and UI. Headings sized modestly (`clamp(1.75rem, 1rem + 3vw, 2.75rem)`); body 1rem / 1.55.
- **Color:** cream ground, warm brown ink, leaf-green accent.

```css
--bg: oklch(0.975 0.015 85);
--surface: oklch(0.995 0.008 85);
--text: oklch(0.28 0.04 40);
--text-muted: oklch(0.5 0.05 45);
--accent: oklch(0.5 0.12 150);
--accent-contrast: oklch(0.98 0.01 110);
```

  Semantic: success leans on the accent green; warn is honey oklch(0.68 0.13 80); error is a warm clay red oklch(0.55 0.16 30).
- **Spacing & layout:** 8px base, scale 8/16/24/40/64; max width 75rem; product cards with chunky 20-24px internal padding; alternate cream and white full-width section bands to create rhythm without rules.
- **Shape:** radius 12-20px on cards and imagery, fully-rounded pill buttons; soft 1.5-2px borders in a tinted neutral (`--text` at 12% alpha); one soft low shadow allowed on hover-lift only.
- **Motion:** 200-250ms `ease-out` with a gentle overshoot on buttons (`scale 0.98 → 1`); cards lift 2px on hover; playful but never springy enough to feel like a toy.
- **Signature details:** rounded image masks (16px) matching the card radius; short friendly microcopy in `--text-muted` under section titles ("Picked this week"); pill-shaped category chips that wrap naturally on mobile.
- **Don't:** no emoji or sticker-style icons as design elements — warmth comes from color, shape, and copy, not decals.

### 5. Dev Tool

Terminal-literate and precise — credibility through real code, not pictures of code.

- **Use when:** developer-facing products: CLIs, APIs, SDKs, infra, docs. **Never for:** non-technical consumer audiences.
- **Typography:** Space Grotesk (500/700) for headings; Sora (400/500) for body and UI; JetBrains Mono (400/500) for code, paths, versions, and every numeral. Body 0.9375-1rem; code 0.875rem / 1.6.
- **Color:** dark-first deep slate with a mint terminal accent.

```css
--bg: oklch(0.18 0.012 260);
--surface: oklch(0.23 0.015 260);
--text: oklch(0.93 0.01 250);
--text-muted: oklch(0.7 0.02 255);
--accent: oklch(0.85 0.14 165);
--accent-contrast: oklch(0.2 0.02 260);
```

  Semantic: mint success (the accent), amber oklch(0.8 0.13 85) warn, soft red oklch(0.7 0.16 25) error — all light enough to read on `--bg`.
- **Spacing & layout:** 4px base, scale 4/8/16/24/40/64; marketing max width 72rem, docs prose capped at ~70ch; code blocks are first-class layout citizens — full column width, never squeezed into cards.
- **Shape:** radius 4-6px; 1px borders in a lifted surface tone (`--text` at 12% alpha) replace shadows entirely; elevation = surface shift + border, never blur.
- **Motion:** 120-150ms `ease-out`; tab/state transitions only; a single typing or cursor-blink effect allowed in the hero demo, fully disabled under `prefers-reduced-motion`.
- **Signature details:** the hero is a real, copy-pasteable install/usage snippet with a copy button; keyboard shortcuts rendered as actual `<kbd>` elements; syntax highlighting derived from the token palette, not a random theme import.
- **Don't:** no fake terminal screenshots or code-as-image — render real highlighted text, always selectable.

### 6. Refined Premium

Measured and assured — typography and restraint doing the work that trust requires.

- **Use when:** high-trust services: fintech, legal, consulting, wealth, healthcare. **Never for:** playful consumer brands or growth-hack landing pages.
- **Typography:** Newsreader (500/600, optical sizing on) for headings; Public Sans (400/500) for body, UI, and forms; financial figures use `font-variant-numeric: tabular-nums lining-nums` in Public Sans. Body 1.0625rem / 1.7.
- **Color:** porcelain ground, ink-navy text, deep sea-green accent.

```css
--bg: oklch(0.97 0.004 230);
--surface: oklch(1 0 0);
--text: oklch(0.24 0.03 250);
--text-muted: oklch(0.48 0.025 250);
--accent: oklch(0.42 0.08 195);
--accent-contrast: oklch(0.98 0.003 200);
```

  Semantic: deep green oklch(0.5 0.1 155), amber oklch(0.6 0.11 80), red oklch(0.5 0.15 25) — used sparingly, always with text.
- **Spacing & layout:** 8px base, scale 8/16/32/48/96/128; text column capped at 68rem (~65ch prose measure); predominantly single-column with occasional 2-col splits for facts/figures; section padding 96-128px — calm comes from vertical air.
- **Shape:** radius 2-4px; hairline borders (`--text` at 12% alpha); exactly one shadow level (soft, low, wide) reserved for elevated panels like comparison tables.
- **Motion:** 250ms `ease-out`, fades only; no parallax, no scroll-jacking, no counters that animate numbers — figures appear set, like print.
- **Signature details:** small-caps letterspaced labels (`font-variant-caps: all-small-caps`, 0.06em) for metadata and table headers; footnote-style disclosures with superscript markers that actually link; key figures set in Newsreader at display size with a one-line sans caption.
- **Don't:** no urgency patterns — countdowns, "only 2 spots left", pulsing CTAs. Trust is the product; pressure destroys it.

## Screen Blueprints

Blueprints are proven structural skeletons for common page types. The
design-engineer picks the matching one, builds on it, and **deviates
deliberately, never accidentally**. They are framework-agnostic — structure and
hierarchy, not markup.

### saas-landing

- **Purpose:** convert a cold visitor into a trial or demo within one scroll story.
- **Structure:** nav → hero with **ONE** value prop + a real product shot → social-proof strip (customer logos) → 3 feature sections alternating image/text sides → pricing teaser → final CTA → footer.
- **Hierarchy:** hero headline and primary CTA are loudest; one primary button style repeated down the page, secondary actions are text links; nav and footer stay quiet.
- **Responsive:**
  - ≥1280px: asymmetric hero split (copy ~5 cols, product shot bleeding right); features side-by-side.
  - ~768px: features stack image-above-text; logo strip wraps to two rows.
  - ~360px: single column, nav collapses to a drawer; product shot crops to the relevant detail instead of shrinking into illegibility.
- **Common mistakes:** two competing value props in the hero; an abstract illustration where the real product should be; center-aligning every section so the page loses its spine.

### pricing

- **Purpose:** let a ready buyer self-select a plan without talking to anyone.
- **Structure:** short heading + billing toggle (monthly/annual) → 2-4 plan cards with **one** highlighted recommendation → full feature-comparison table → pricing FAQ → final CTA.
- **Hierarchy:** the recommended plan is loudest (surface shift + "Most popular" label + the only accent CTA); price numerals large and unmissable; fine print quiet but present.
- **Responsive:**
  - ≥1280px: cards in one row; comparison table full width with sticky header.
  - ~768px: 2-up card grid; table compresses but stays a table.
  - ~360px: cards stack with the recommended plan first; the comparison table becomes per-plan stacked sections or an accordion — never a horizontally scrolled table.
- **Common mistakes:** highlighting every plan so none stands out; "Contact us" on all tiers with no anchor price; a billing toggle that reflows card heights and shifts the layout.

### dashboard

- **Purpose:** answer "is everything okay, and what needs my attention?" in five seconds.
- **Structure:** app shell (sidebar nav + top bar) → page title + date/scope filter → KPI row leading with the **1-3 numbers that matter** → primary chart or visualization → supporting table / activity list.
- **Hierarchy:** the KPI numbers are loudest on the page; chrome stays quiet; the accent color is reserved for alerts and anomalies, not decoration.
- **Responsive:**
  - ≥1280px: persistent sidebar; KPIs 3-up; chart and table share the width.
  - ~768px: sidebar collapses to icons or a drawer; KPIs 2-3 up; chart and table stack.
  - ~360px: nav in a drawer; KPIs stack or sit 2-up compact; tables become card lists or scroll horizontally with a sticky first column.
- **Common mistakes:** opening with a wall of equal-weight stat cards; charts placed before the numbers they summarize; decorative sparklines that encode nothing.

### auth

- **Purpose:** get the user in (or back in) with zero friction and zero doubt.
- **Structure:** logo → one-line heading → SSO buttons (if offered) → divider → email/password fields → **ONE** primary action → quiet links (forgot password, switch between sign-in/sign-up).
- **Hierarchy:** one column, one action — the submit button is the only loud element; everything else is muted; error states inline next to the field that caused them.
- **Responsive:**
  - All sizes: same single column, capped at ~24rem and centered.
  - ≥1280px: optionally add a brand panel beside the form (image/quote) — the form itself never moves or widens.
  - ~360px: full-width fields, ≥48px touch targets, no side panel.
- **Common mistakes:** two columns of form fields; equal-weight "Sign in" and "Sign up" buttons side by side; password rules revealed only after a failed submit.

### settings

- **Purpose:** let users find and change one specific thing fast, confident nothing else broke.
- **Structure:** page title → section nav (Profile, Billing, Team, Notifications, Danger zone) → content pane of grouped field rows → per-section save (or autosave with visible feedback) → danger zone last and visually separated.
- **Hierarchy:** section labels and current values readable at a scan; destructive actions quarantined — the only red on the page lives in the danger zone.
- **Responsive:**
  - ≥1280px: navigable two-pane — sticky left section nav (220-260px) + content pane.
  - ~768px: section nav becomes horizontal tabs (or a select) above the content.
  - ~360px: stacked sections — one grouped scrolling page, or a list of sections that drill into subpages with a back affordance.
- **Common mistakes:** one giant form with a single Save at the very bottom; destructive actions mixed in among normal ones; saving with no confirmation feedback so users re-save "just in case".

### product-listing

- **Purpose:** help a shopper narrow many products down to a confident shortlist.
- **Structure:** category header (title, item count, one-line description) → toolbar (sort + filter triggers) → filter rail beside the product grid → pagination or load-more → quiet SEO/footer copy.
- **Hierarchy:** product imagery is loudest — consistent `aspect-ratio` cards with image, name, price, and **one** quiet label max; filter/sort chrome stays compact.
- **Responsive:**
  - ≥1280px: persistent left filter rail + 3-4 column grid.
  - ~768px: rail collapses to a "Filter" button opening a slide-over panel; 2-3 column grid.
  - ~360px: 2-column grid (1 for detail-heavy products); filters in a bottom sheet with applied-filter chips visible above the grid.
- **Common mistakes:** cards drowning in badges, ratings, and ribbons; filtering that jumps scroll position or loses state; inconsistent image crops making the grid ragged.

### product-detail

- **Purpose:** give the shopper everything needed to decide — and the buy action — without scrolling.
- **Structure:** breadcrumb → two panes **above the fold**: gallery (~60% width) + buy-box (name, price, variant selectors, add-to-cart, shipping/returns reassurance) → details/specs accordion → reviews → related products.
- **Hierarchy:** gallery and add-to-cart are loudest; price sits unambiguous next to the action; reassurance copy quiet but adjacent to the commitment point.
- **Responsive:**
  - ≥1280px: gallery and buy-box side by side; buy-box may go sticky on long pages.
  - ~768px: split holds, or stacks with the gallery first.
  - ~360px: gallery becomes a swipeable carousel with dots; buy-box directly beneath; add-to-cart repeats in a sticky bottom bar.
- **Common mistakes:** pushing the buy-box below the fold under marketing copy; variant selection that doesn't update price, image, and stock together; thumbnails as the only gallery navigation on mobile.

### cart-checkout

- **Purpose:** take a committed buyer to a completed order with zero surprises.
- **Structure:** cart review (editable line items: quantity, remove) → order summary with **full costs shown early** (shipping + tax estimated in the cart, not at the end) → checkout steps: contact → shipping → payment → review → confirmation. Guest checkout is first-class; account creation is optional and offered *after* the order.
- **Hierarchy:** the order total and the single continue/pay action are loudest; checkout chrome strips down — minimal header, no full site nav, but always a clear way back; trust signals sit quietly near the payment fields.
- **Responsive:**
  - ≥1280px: two columns — form left, sticky order summary right.
  - ~768px: same split, or the summary collapses into an expandable strip above the form.
  - ~360px: single column; summary as a collapsed accordion that always shows the total; pay/continue button sticky at the bottom.
- **Common mistakes:** trapping users — the cart must stay editable and every step back-navigable; springing shipping/tax costs at the final step; forcing account creation before checkout.

### list-detail

- **Purpose:** work through a collection of records (inbox, tickets, customers) without losing context.
- **Structure:** list pane (search/filter on top; dense rows of title, status, timestamp) + detail pane (header with title, status, primary actions → body content → metadata/activity column or footer).
- **Hierarchy:** in the list, unread/active state and titles are loudest; in the detail, the title and primary action; the current selection is always visibly marked.
- **Responsive:**
  - ≥1280px: panes side by side (list 320-400px); metadata may take a third column.
  - ~768px: list narrows; metadata folds into tabs inside the detail pane.
  - ~360px: one pane at a time — list first, tapping a row pushes the detail full-screen with a back control; never both panes crushed side by side.
- **Common mistakes:** cramming both panes onto a phone; losing list scroll position and selection on back; hiding the 1-2 primary detail actions in an overflow menu when there is room to show them.

### onboarding

- **Purpose:** get a new user to first value fast — ask the minimum, then show the product working.
- **Structure:** welcome (one line restating the promise) → 2-4 short steps, **one question each** (role, goal, team size…) with a progress indicator → the magic moment: a working, personalized state (template applied, sample data loaded, first result shown) → handoff into the app with 1-3 clear next actions.
- **Hierarchy:** the current question and the Continue button are loudest; progress and "Skip" stay quiet but always visible; every step is skippable unless its answer is genuinely required.
- **Responsive:**
  - All sizes: the same single-column flow, content capped at ~32rem.
  - ≥1280px: centered with generous air — resist filling the space with extra fields.
  - ~360px: full-screen steps with a sticky Continue button.
- **Common mistakes:** front-loading a profile interrogation before showing any value; steps with no skip; ending onboarding on an empty dashboard instead of a populated one.

## Working Method
1. **Explore first** — read the existing UI kit, brand assets, and codebase;
   match the visual vocabulary instead of starting generic.
2. **Set direction** — purpose, audience, tone, one memorable detail, constraints.
   Choose **ONE Design Kit** (above) and commit to it for the whole product —
   never mix kits — and state the chosen kit in the delivery. If an existing
   brand/system is present, the brand wins; the kit only fills its gaps.
3. **Offer 2-3 variations** across layout / tone / color for meaningful screens.
4. **Build responsive + accessible**, reusing tokens/components.
5. **Prove it** — check the three breakpoints and contrast/focus before "done".

---

## My References (grow this section)

Add your own inspiration here so the agent keeps mastering your taste. For each:
what you like, the rule to extract, and a do/don't. Drop screenshots in
`skills/design-language/refs/` and link them.

<!-- Template:
### Reference: [name / source]
- What I like: …
- Rule to apply: …
- Do: … / Don't: …
- Image: refs/example.png
-->

_(No personal references added yet — paste screenshots or notes and we'll
distill them into rules here.)_
