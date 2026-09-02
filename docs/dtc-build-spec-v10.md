# FurnitureRx DTC — Build Spec v10

**Status:** working spec — owner iterates on sight ("render, look, adjust"). Supersedes the v9 wireframe (`furniturerx-dtc-refactor-draft.md`) where they differ. v9 remains the reference for anything v10 doesn't mention.

**Goal:** convert the in-store kiosk site into a direct-to-consumer page optimized for one job: a consumer buys a protection plan. One page, one goal, one accent color reserved for purchase actions.

---

## Ground rules (makers: read first)

1. **Refactor `index.html` in place.** Single-file HTML/CSS/JS, ES5, no build step, no npm dependencies in the shipped site. Keep the existing `:root` design tokens, fonts (Fraunces + Inter), and component classes — this is a re-layout, not a re-theme.
2. **The checkout drawer is load-bearing and stays as built.** Do not rebuild commerce. Strip *UI* of killed features but keep the `/api/create-checkout-session` payload contract identical (send empty kits, membership per current bundling rule). Zero backend risk is a requirement.
3. **DTC mode = remove `class="kiosk"` from `<html>` (line 2).** `?kiosk=1` remains as the kiosk opt-in. Do not delete kiosk CSS/JS gates in the first pass — the flag flip does the work.
4. **Dealer attribution is REMOVED from this product.** No "offered with [dealer]", no `/s/{code}` sales mode, no `?pt=` dealer SSO, no "Selling as" badge, single hero headline always. (Kiosk inherited these; the DTC site sells direct.) Strip these surfaces. The customer-facing receipt-QR handoff (`/api/kiosk-handoff*`, `/p/*`, `qrcode.min.js`) STAYS — it's how consumers add a receipt from their phone.
5. **Sections separated by spacing + hairline rules only.** No alternating background fills, no invented colors — existing `:root` tokens verbatim.
6. **No dead links.** Every header/footer link resolves — placeholder pages are rendered stubs, not `#`.
7. Commit per slice with clear messages. **Makers commit; the master agent reviews and pushes.**
8. Comments in code: English, keep the existing ticket-ID style (`DTC-n`).

---

## Page order

Header → 1 Hero → 2 Invoice strip → 3 Trust strip → 4 Plans → 5 How-checkout-works → 6 FAQ → Footer.
Mobile: photo band, headline, price line, button, invoice, then on down.

---

## Header (LOCKED)

Desktop: one bar ~56px, page background, hairline bottom border, **not sticky**, page max-width container.
- Left: FurnitureRx logo 28–32px tall, links home.
- Right, one row: `Home · Get Service · Shop Furniture · Cleaning Kit` — small muted text links, even spacing, no underline, hover = slight darkening only. After a wider gap: **My Dashboard** as an outlined pill (existing `DASHBOARD_URL`), the only styled element.
- `Get Service` → external claims site (5starservice.net link already in kiosk code).
- `Shop Furniture` and `Cleaning Kit` → rendered placeholder stub pages (see Placeholder pages).

Mobile (<768px): same height. Logo left. Right: My Dashboard icon button (outlined circle, user/house icon) + hamburger. Hamburger drops a full-width panel under the bar: the four links stacked, generous tap height, Home first, Cleaning Kit last. My Dashboard stays in the bar, not the panel. Panel closes on tap, link, or scroll. Bar never shrinks or changes color; hero starts immediately below.

Kill from current header: utility bar ("from $9.99/mo"), old nav links (Protection Plans / Care Kits / Uncovered Repairs / FAQ), cart icon+count (drawer opens from CTAs instead), drawer login/chat links.

## 1 · Hero (LOCKED)

One full-width band, ~520–600px tall desktop. Only four elements:
- **Photo**, right side (spec below).
- **H1:** `Protect the sofa before you buy it.`
- **One line:** `$19.99 a month. Starts the day it's delivered. Cancel anytime.`
- **Button:** `Protect my furniture` → anchor scroll to `#plans`. The hero's ONLY interactive element.

Left text column ~55%, left-aligned, vertically centered. Photo carries probability, headline names the peril, line carries price, button carries action. Nothing else — no eligibility line (moved to Plans), no invoice (moved to strip below).

**Photo treatment:** background layer, not a placed `<img>` in flow. Anchored right, covering ~right 55% at full strength, **full color** (the red is the stimulus — never desaturate). Left-to-right gradient of page background on top: solid from left edge to ~60–70% width, fading transparent over the next 10–15%, photo at full opacity in the right third. Headline never sits on the image.
**Asset:** `assets/masters/couple_with_dog.png` (1672×941 master). Crop ~15–20% off the LEFT edge so the tipping glass + splash move into the right half and the gradient seam lands on empty cushion. Serve as WebP ~150–200KB (keep PNG master untouched). Mobile: art-directed crop (tipping glass + dog centered), ~200px full-strength band ABOVE the text, no text overlay, no gradient.

## 2 · Invoice strip (LOCKED)

Full-width band directly under hero. Page background, hairline rule top + bottom, ~280–320px desktop, page container. **Nothing clickable.**
- Left ~55%, vertically centered, aligned with hero text column: `This is what one repair costs without a plan.` then `We find the technician, book the visit, and pay the bill.` — lead size, second sentence regular weight.
- Right ~45%: **invoice card**, max-width ~400px. Opaque, 1px border, faint document shadow — reads as paper. Monospace ~13px: eyebrow `Upholstery repair · typical quote`, then `Trip charge $89` / `Labor, 2 hrs $240` / `Re-web + fabric $185`, hairline rule, `Total $514`, then hassle line in regular (non-mono) type: `…plus finding someone, three quotes, a weekday at home.`
- Figures are PLACEHOLDERS pending owner's RAP claims data. `typical quote` label is load-bearing — keep.
- Mobile: sentence, then card full-width (cap ~400px, centered).

## 3 · Trust strip (LOCKED — narrow by construction)

NOT tiles/cards. A single text line between invoice strip and plans: ~64–72px tall, hairline rules above/below, no heading, no fill. Four items, small muted type (~13–14px), evenly spaced, middot-separated:

`4.5★ Google rating · 24/7 claim filing · Same-day response on most claims · US-based repair network`

Only the ★ carries color (gold token). Nothing clickable in dev (Google-profile link may attach to item 1 later). Mobile: same items, compact 2×2 grid.
(Claims softened to what's defensible; owner may restore stronger wording if true.)

## 4 · Plans `id="plans"` (per v9 — owner review pending, build to this)

Eligibility line at top, directly above cards:
`For new, undamaged furniture from any store. Buy before you shop and add your delivery date later, or within 60 days of delivery.`

Three cards, mobile stack A→B→C:
- **A · PRIMARY** (selected by default, ~57% width): badge `Most chosen` · `Stain + Structure, monthly` · `$19.99 /month` (price 2× the size of B/C) · bullets: stains, rips, burns, mechanical & electrical · up to $5,000 coverage per plan · we book and pay the repair, replace if it can't be fixed · 24/7 claim filing, same-day. **Included callout box:** `Repair Safety Net included, 3 months free. Member-rate repairs for the rest of the room, incl. Marketplace and inherited pieces.` Qty stepper default 1. `Add to cart` — the only filled button in the section — **adds AND opens the checkout drawer immediately.**
- **B**: `Stain Protection, monthly` · `$9.99 /month` · food, drink, ink, pet stains · up to $5,000 per plan · Safety Net included · qty 0 · `Choose this instead` link.
- **C**: `3-year plan, pay once` · `$250 one payment · up to $5,000 per plan` · purchase-price input `$[2500]` + slider 500–5k · `From $199 · Safety Net incl. · need >$5,000? Add a 2nd plan.` · qty 0 · `Choose this instead` link. Pricing rule placeholder `max(199, retail×0.10)` — RAP curve TBD.
  **C ships visually complete but NOT purchasable** ("Coming soon" state) until the variable-price backend endpoint exists (Adrian). A/B use existing monthly SKUs unchanged.

Below cards, fine print: `Coverage starts 30 days after your delivery date · receipt needed before coverage starts · not available in all states · terms` (→ `/terms/`). [Legal copy pending owner/Adrian sign-off — build as written.]

Maya door #1: quiet link near the cards — `Not sure which plan? Ask Maya` → opens existing chat panel.

## 5 · How checkout works (replaces v9 inline checkout)

Short strip, no form. Three steps in plain text: `Pick your plan → 2-minute checkout → add delivery date & receipt now, or later from your dashboard.` Plus the Stripe-secure line and ONE button — second entrance to the same drawer.

**Checkout drawer: existing, untouched** except these deltas:
- Sales associate # and sale order # → optional, de-emphasized (kiosk flag removal handles associate; relax order # required rule for DTC).
- Delivery date: optional, "I'll add this later" affordance (dashboard path already exists in copy).
- Accuracy checkbox stays a hard gate, positioned at the bottom of the form (v9: moved from top).
- Maya door #2: compact icon/link row where the old horizontal Maya container sat → opens existing chat popup. Coverage-Assistant prefill wiring untouched.
- Kill the global floating chat FAB. Maya is reachable only via the two doors.
- Purchase-price field: add only when C activates (with Adrian's endpoint).
- "Ways to pay" associate options: auto-hidden by kiosk-flag removal. Verify hidden.
- Keep: `Email my cart to myself`, receipt photo/file/QR, referral code field, phone/ZIP/date validation, draft persistence.

## 6 · FAQ (accordion, 4 only)

1. `Can I buy before I've picked my furniture?`
2. `Can I really cancel anytime?`
3. `What isn't covered?`
4. `What does Repair Safety Net cover?`

Answers: draft from existing FAQ copy, tightened; flag new-claim language for owner.

## Footer

`Administered by Risk Assurance Partners` + links: `Coverage terms` (→ `/terms/`) · `Privacy` · `Accessibility` · `Log in` (existing `CUST_LOGIN`). Privacy + Accessibility → rendered stub pages until real ones exist. **No `#` links anywhere.**

## Placeholder pages (rendered stubs, dev mode)

- `/shop-furniture/index.html` — future affiliate page. Header/footer, headline, one line "coming soon" body. Asset optional.
- `/cleaning-kit/index.html` — future kit product page. May use `assets/masters/childwjuice.png` (toddler + juice) as its visual when built out.
- `/privacy/index.html`, `/accessibility/index.html` — minimal stubs.
Stubs share the site header/footer and tokens; keep them tiny (they can be simple standalone files — no need to make index.html multi-page-aware).

## Kill list (strip UI, keep payload contract)

Utility bar · old nav + mobile drawer contents · care kits section (+ kit cart UI; payload still sends empty kits) · gift coupon overlay + entry links · standalone membership/Safety Net section (becomes included-callout in cards) · embedded 28-clause membership T&C modal (link `/terms/` instead) · "used furniture" buttons ×2 · "authorized dealer" copy ×2 (→ "any store") · referral perk strip (already gone) · dashboard demo remnants (keep `#dashqr` only if Log in uses it) · floating chat FAB · dealer attribution everything (rule 4) · dead CSS for removed markup (hero/configurator CSS may be cannibalized for the new hero first).

## Open items (owner/Adrian — do not block build)

| # | Item | Owner |
|---|---|---|
| 1 | Invoice card real figures (RAP claims data) | Doug |
| 2 | Plan C variable-price one-time endpoint + `purchase_price` in payload; RAP price curve | Adrian |
| 3 | Safety Net grant shape ("3 months free" vs current free-bundled) — backend truth must match copy | Adrian |
| 4 | Legal: 30-day coverage start, "any store", state availability, $5k cap on C | Doug/Adrian |
| 5 | Google rating link target + claim verification | Doug |
| 6 | Real Privacy/Accessibility pages | Doug |
| 7 | `ALLOWED_ORIGINS` += new Netlify origin | Adrian |
| 8 | Stripe live vs test ("test mode" note at old line 1643) | Adrian |
| 9 | AI agent + Supabase DB (backlog, post-layout) | later |

## Build slices (one commit each, master reviews + pushes)

1. **Skeleton flip:** remove kiosk class · new header + placeholder stubs · section order · kill-list strip · payload contract intact.
2. **Hero + invoice strip + trust strip:** photo treatment (WebP derivatives from masters), locked copy.
3. **Plans:** A/B/C cards per spec, C coming-soon, eligibility line, Maya door #1, card CTA opens drawer.
4. **Checkout deltas + How-it-works strip + Maya door #2 + kill FAB.**
5. **FAQ + footer + copy sweep + dead-CSS reap.**

After each slice: master reviews diff, pushes, owner views on Netlify, iterates.
