# FurnitureRx DTC — wireframe (v9)

Base: kiosk.furniturerx.net. Single-file HTML/CSS/JS, no build, Netlify. Checkout, Stripe, receipt flow, dashboard untouched except where marked.

```
┌──────────────────────────────────────────────────────────────────────────────┐
│ HEADER                                                                       │
│ FurnitureRx  · offered with [Dealer name]  (only when /s/{code})   🔒 Secure │
│                                                                     checkout │
└──────────────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────────────┐
│ 1 · HERO                                            ░░░░░░░░░░░░░░░░░░░░░░░░ │
│                                                     ░ PHOTO (ghosted)      ░ │
│ H1  Protect the sofa before you buy it.             ░ red wine spreading   ░ │
│     [dealer code → "This is what one stain          ░ into pale linen,     ░ │
│      costs without a plan."]                        ░ still wet, glass     ░ │
│                                                     ░ tipped at edge.      ░ │
│ We find the technician, book the visit, and pay     ░ No people, no pets.  ░ │
│ the bill. $19.99 a month, starting the day it's     ░ Full strength right, ░ │
│ delivered. Cancel anytime.                          ░ fades to solid bg    ░ │
│                                                     ░ under the headline.  ░ │
│ Shopping for furniture? Buy now, add your           ░                      ░ │
│ delivery date later. Also covers new, undamaged     ░ ┌──────────────────┐ ░ │
│ furniture from any store delivered in the last      ░ │ INVOICE (opaque) │ ░ │
│ 60 days.                                            ░ │ Upholstery repair│ ░ │
│ [dealer code → lead with "delivered in the last     ░ │ · typical quote  │ ░ │
│  60 days," then "haven't picked it out yet?"]       ░ │ Trip charge  $xx │ ░ │
│                                                     ░ │ Labor 2h    $xxx │ ░ │
│ ┌───────────────────────────┐                       ░ │ Re-web+fab  $xxx │ ░ │
│ │ Protect my furniture  ↓   │  → anchor #plans      ░ │ ──────────────── │ ░ │
│ └───────────────────────────┘  (only hero button)   ░ │ Total       $xxx │ ░ │
│                                                     ░ │ + finding someone│ ░ │
│                                                     ░ │ three quotes, a  │ ░ │
│                                                     ░ │ weekday at home  │ ░ │
│                                                     ░ └──────────────────┘ ░ │
│                                                     ░░░░░░░░░░░░░░░░░░░░░░░░ │
└──────────────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────────────┐
│ 2 · TRUST BANNER  (existing block, moved here from top)                      │
│ ┌────────────┐ ┌────────────┐ ┌────────────┐ ┌────────────┐                  │
│ │   4.5★     │ │   24/7     │ │  Same day  │ │  US-based  │                  │
│ │  Google    │ │ File claim │ │ Most claims│ │  Repair    │                  │
│ │  rating    │ │  in app    │ │ processed  │ │  network   │                  │
│ └────────────┘ └────────────┘ └────────────┘ └────────────┘                  │
└──────────────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────────────┐
│ 3 · PLANS   id="plans"                                                       │
│ ┌────────────────────────────────────┐ ┌────────────────────────────────┐    │
│ │ A · PRIMARY  (selected, 57% width) │ │ B                              │    │
│ │ [Most chosen]                      │ │ Stain Protection, monthly      │    │
│ │ Stain + Structure, monthly         │ │ $9.99 /month                   │    │
│ │                                    │ │ Food, drink, ink, pet stains · │    │
│ │ $19.99 /month   ← large, 2× B/C    │ │ up to $5,000 per plan ·        │    │
│ │                                    │ │ Safety Net included            │    │
│ │ • Stains, rips, burns, mechanical  │ │ Plans [ − 0 + ]                │    │
│ │   and electrical                   │ │ Choose this instead (link)     │    │
│ │ • Up to $5,000 coverage per plan   │ └────────────────────────────────┘    │
│ │ • We book and pay the repair;      │ ┌────────────────────────────────┐    │
│ │   replace if it can't be fixed     │ │ C                              │    │
│ │ • 24/7 claim filing · same-day     │ │ 3-year plan, pay once          │    │
│ │                                    │ │ $250  one payment · up to      │    │
│ │ ┌────────────────────────────────┐ │ │       $5,000 per plan          │    │
│ │ │ Repair Safety Net included,    │ │ │ What you paid, or plan to spend│    │
│ │ │ 3 months free. Member-rate     │ │ │ $ [ 2500      ]                │    │
│ │ │ repairs for the rest of the    │ │ │ ├────────●────────────┤ 500–5k │    │
│ │ │ room, incl. Marketplace and    │ │ │ From $199 · Safety Net incl. · │    │
│ │ │ inherited pieces.              │ │ │ need >$5,000? Add a 2nd plan.  │    │
│ │ └────────────────────────────────┘ │ │ Plans [ − 0 + ]                │    │
│ │                                    │ │ Choose this instead (link)     │    │
│ │ Plans [ − 1 + ]   ← default 1      │ │                                │    │
│ │ ┌──────────────┐                   │ │ price = max(199, retail×0.10)  │    │
│ │ │ Add to cart  │  ← only filled    │ │ (placeholder; RAP curve TBD)   │    │
│ │ └──────────────┘    button here    │ └────────────────────────────────┘    │
│ └────────────────────────────────────┘                                       │
│                                                                              │
│ Coverage starts 30 days after your delivery date · receipt needed before     │
│ coverage starts · not available in all states · terms                        │
│                                                                              │
│ mobile stack order: A, B, C                                                  │
└──────────────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────────────┐
│ 4 · CHECKOUT  (existing, as built)                                           │
│ ┃ Stain + Structure                                                     ×    │
│ ┃ Monthly · Furniture · up to $5,000                                         │
│ ┃ $19.99 /month · cancel anytime                                             │
│ ┃ ┌──────────────────────────────────────────────────────────────────┐       │
│ ┃ │ Maya · Questions about the form or your receipt? Ask here →      │ ← only│
│ ┃ └──────────────────────────────────────────────────────────────────┘  Maya │
│ TOTAL                                                          $19.99/mo     │
│                                                                              │
│ ┌ existing form ─────────────────────────────────────────────────────────┐   │
│ │ name · email · phone · address · ZIP                                   │   │
│ │ sales associate # (→ optional) · sale order # (→ optional)             │   │
│ │ purchase price (NEW; required for C, optional for A/B)                 │   │
│ │ delivery date (→ optional, "I'll add this later")                      │   │
│ │ receipt: photo / file / QR (now, or later from dashboard)              │   │
│ │ referral code · ☐ accuracy check  "Inaccurate info may inactivate…"    │   │
│ │                                     ↑ moved here from top of form      │   │
│ └────────────────────────────────────────────────────────────────────────┘   │
│ ┌────────────────────────────────────────────────────────────────────────┐   │
│ │                        Pay  $19.99 /mo  →                              │   │
│ └────────────────────────────────────────────────────────────────────────┘   │
│ Secure checkout by Stripe · No delivery date yet? Add it from your           │
│ dashboard and coverage starts then.      Email my cart to myself (text link) │
│                                                                              │
│ "Ways to pay" associate options hidden on consumer site if a mode flag exists│
└──────────────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────────────┐
│ 5 · FAQ  (accordion, 4 only)                                                 │
│ Can I buy before I've picked my furniture?                               ▾   │
│ Can I really cancel anytime?                                             ▾   │
│ What isn't covered?                                                      ▾   │
│ What does Repair Safety Net cover?                                       ▾   │
└──────────────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────────────┐
│ FOOTER                                                                       │
│ Administered by Risk Assurance Partners   Coverage terms · Privacy ·         │
│                                           Accessibility · Log in  (real URLs)│
└──────────────────────────────────────────────────────────────────────────────┘
```

## Not on this page

nav · standalone Safety Net product · care kits · gift codes · referral block · dashboard demo · dealer/customer login in body · floating chatbot · embedded membership terms · "used furniture" buttons · "authorized dealer" copy · any footer link to `#`
