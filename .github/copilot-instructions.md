## Design Context

### Users
Raúl & Pao, two specific people, own trip 25-29 sept 2026 Bay Area. Not public product — personal reference tool. Used pre-trip on desktop while planning, then on phone (Safari iOS for Raúl, Firefox Android for Pao) as offline-ish backup during trip.

### Brand Personality
Calm, precise, trustworthy. Feels like a well-made travel document, not a generic dashboard. Reference: Apple Wallet / Calendar dark mode. Prior artifact (split-flap flight board) sets palette/type precedent — reuse it.

### Aesthetic Direction — REVISED (agency restyle, explicit user override)
Original direction was calm/minimal iOS Wallet-style with glass used sparingly. User explicitly invoked an "Awwwards agency" design skill and, when shown the conflict with this doc, chose to apply it in full anyway (`itinerario.html` / `itinerario-artifact.html`, same session). Current live direction:
- **Ethereal Glass vibe**: deep near-black warm ground (oklch ~0.12L), stronger warm glow orbs (amber/gold, not the cliché purple/cyan), heavier `backdrop-filter: blur()` on header/nav/cards — always with a `@supports` solid-color fallback underneath, never blur-only.
- **Double-bezel cards**: every card (timeline, summary, pretrip) sits in an outer "machined shell" (`.bezel-shell`, padding 5px, radius 28px) wrapping an inner glass core (own bg, inset highlight, radius 20px). Concentric, not flat.
- **Floating island nav**: tab bar is a detached rounded glass pill (inset margin, radius 999px), not an edge-to-edge bar — the one explicit "banned layout" from that skill.
- **Button-in-button**: the expand chevron is never bare — always nested in its own 28px circular pill (`.chevron-wrap`).
- Bento asymmetry only on the Summary tab at ≥720px (varying card spans) — collapses to plain single column on mobile, per that skill's own mobile-override rule. Never applied to the sequential day timelines (wrong content shape for bento).
- Scroll/tab-entry choreography: `riseIn` keyframe (translateY+blur+opacity, GPU-safe transform/opacity/filter only) staggers children on tab activation. Respects `prefers-reduced-motion`.
- **REVISED AGAIN — light, not dark.** User feedback: "todo el framework se ve cero agradable... sensación horrible de oscuridad y el amarillo mostaza me choca." Flipped to a warm light theme, sourced from a 4-color reference palette (Darlington sage `#ACCAB2`, Beeswax amber `#E9A752`, Grenadine orange-red `#D44720`, Cafe Latte brown `#78614D`). `--bg` is now warm cream (oklch 0.965L), `--text` deep Cafe-Latte-brown (oklch 0.3L), `--accent` is Grenadine-derived (oklch 0.55L 0.185C 34H) — no black, no mustard/gold anywhere. Structural stuff (double-bezel, floating nav, glass blur, riseIn motion) untouched — only the color tokens changed. All text/bg pairs re-verified for WCAG AA contrast (≥4.5:1) after the swap using canvas-composited luminance checks, not eyeballed — two tokens (`--text-faint`, the `move` food-tag mix %) needed adjusting because the obvious guess undershot on a saturated warm hue. Single theme still, no OS-driven light/dark toggle — same "commit to one look" pattern as before, just the opposite look now.

- **Moment-card internal layout — "flight-board" restructure** (user: "no me gusta el layout interno" of the timeline cards). Time moved OUT of the thin side spine (which now only shows dot+line) and INTO the card as a big tabular readout on its own top row ("board-line": time + status pill + chevron, one row), with title/location/desc as a full-width row below — title no longer squeezed beside the chevron. Went through the mandatory overdrive propose-3-directions step first; user picked "flight-board scroll reveal" over a boarding-pass/View-Transitions option and a conservative spring-physics-only option. Added `animation-timeline: view()` scroll-driven reveal per card (replaces the old one-shot tab-activation stagger, only in `@supports (animation-timeline: view())` — Firefox has no support as of writing, so it keeps the old JS stagger, a real fallback not a degraded one) plus a quick `rotateX` flap-in on the time digits specifically, calling back to the split-flap board lineage already documented above.

**Typography pairing (revised, three roles):**
- Body/UI (dense text, still `-apple-system`): unchanged, kept because it's genuinely native-iOS-correct, not because it was a default.
- **Display** (new): "Bricolage Grotesque" (Google Fonts) for trip title, day headings, card titles — the "agency" personality layer. Reason it was picked: wide geometric grotesk matching the vibe archetype, not in that skill's own banned-font list, not in the *other* impeccable skill's reflex-font list either.
- **Data/mono**: swapped Roboto Mono → **JetBrains Mono** (same tabular-nums role, times/seats/dates/money) — literal "Roboto" is on that skill's banned-font list, so it had to change even though the *reason* for a mono data accent hasn't.

### Design Principles
1. Reliability over spectacle — this is trip-critical reference info; broken render on real Safari iOS / Firefox Android is worse than a boring one. Every glass/blur effect MUST have a `@supports` fallback; this held even through the restyle.
2. One data source of truth in JS — no duplicated strings between "confirmed" and "pending" content.
3. Confirmed vs pending must be visually unambiguous at a glance (not just a color, since colorblind-safe).
4. No AI-slop tells **in the sense of that original list** (no left-border-stripe callouts, no gradient text, no generic icon-over-heading cards) — but "gratuitous glassmorphism" is no longer banned; it's now the deliberate vibe, per explicit user direction. If asked to tone it back down, revert toward the pre-restyle rules above, don't split the difference.
5. Mobile-first, single file, zero external JS deps, zero build step, zero horizontal scroll.
6. Three-font system: `-apple-system` for dense body/UI, Bricolage Grotesque for display/headings, JetBrains Mono for numeric/data. No Inter, no Roboto (plain), no swapping fonts casually — each swap should trace back to a documented reason like the ones above.
7. Color changes are not free — any token edit (`--accent`, `--confirmed`, `--pending`, etc.) must be re-checked for WCAG AA contrast against every surface it composites onto, not just eyeballed. OKLCH lightness does not track WCAG relative luminance in any simple way, especially on saturated warm/red hues — verify with actual rendered pixels (canvas readback), not the raw L value.
