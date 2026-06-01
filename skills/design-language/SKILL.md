---
name: design-language
description: The reference library of crafted, non-generic, fully responsive design that the design-engineer agent studies and applies. Captures concrete rules for typography, color, layout, motion, and the "AI-look" tells to avoid — plus a place to grow the user's own references over time. Use whenever designing or reviewing UI that must look deliberately crafted, not AI-generated.
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

## Working Method
1. **Explore first** — read the existing UI kit, brand assets, and codebase;
   match the visual vocabulary instead of starting generic.
2. **Set direction** — purpose, audience, tone, one memorable detail, constraints.
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
