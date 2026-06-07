# CLAUDE.md

## Project Overview

Marketing site for **AI Voice Agent Receptionist** — a UK-wide service vertical from Antek Automation targeting the AI-receptionist / AI-voice-agent search cluster.

**Live domain:** aivoiceagentreceptionist.co.uk

**Repo type:** UK-wide service vertical. NOT a location satellite, NOT a trade-specific site. Owns the generic AI receptionist cluster + professions WITHOUT a dedicated network site (therapists, law firms, dentists, accountants, physiotherapists, plumbers, general trades).

## Tech Stack & Build

- Zero build step. Static single-page site.
- `index.html` is the entire site (~2,500 lines) with inline CSS + JS.
- Cloned from `aiforelectricians.co.uk` template — keeps same design system (dark theme, Sora/DM Sans, blue/cyan/green palette) so the Antek network looks consistent.
- Open `index.html` in a browser or any static server (`python3 -m http.server`).

## Anti-Cannibalisation Rules (CRITICAL)

This site is the SERVICE national page. Three lanes must stay clean:

1. **No geographic targeting.** No Hampshire, Andover, city, county. Locality sites (hampshire-ai, andover-ai etc.) own places.
2. **No trade-specific targeting where a dedicated trade site exists.** The Electricians card LINKS OUT to https://aiforelectricians.co.uk — it does NOT target electrician terms. When new trade sites launch, repeat the pattern.
3. **National service + buyer-language only.** Title/H1/H2/FAQ headings own "AI receptionist", "AI voice agent", "AI phone receptionist", "AI virtual receptionist", "automated receptionist", "AI auto attendant", "ai voice agents business uk".

## Architecture

`index.html` is organised:
1. `<head>` — meta, OG/Twitter, single `@graph` JSON-LD (Organization+ProfessionalService + Person), separate FAQPage JSON-LD, all CSS, Retell widget v2
2. Body sections (each with `id=`): `hero`, `pain-points`, `services`, `demo`, `how-it-works`, `who-we-help` (was `industries` in template), `why-us`, `founder`, `faq`, `contact`
3. About-micro (SEO-dense paragraph)
4. Footer (parent + sister site links)
5. Bolt demo modal + scripts at end of body

## Schema

ONE @graph in head — Organization+ProfessionalService node (@id `https://aivoiceagentreceptionist.co.uk/#organization`) + Person node (shared @id `https://www.antekautomation.com/#andynorman`, BYTE-IDENTICAL across every Antek site). FAQPage is a separate `<script type="application/ld+json">` block. Never create a second competing Organization.

## FAQ Parity

Every FAQ must appear identically in BOTH the visible accordion AND the FAQPage JSON-LD. If you add, remove, or edit one, change BOTH. Current count: 10 entries.

## Contact Form

Self-hosted n8n iframe — same shared form ID as the rest of the network: `https://auto.juxtarank.com/form/17d8ddcd-4860-4dde-bd4a-2de0adb518e2`. iframe min-height 1200px.

The legacy `<form id="contactForm">` POST-to-webhook code in the template was replaced with this iframe (cloud n8n blocks framing via X-Frame-Options; the self-hosted instance allows it).

## Cal.com Booking

Same antek-automation/30min embed as rest of the network.

## Retell Widget

v2 widget in `<head>` with shared public key + agent ID. `data-dynamic` is `{"site": "receptionist", "domain": "aivoiceagentreceptionist.co.uk"}`. `#retell-widget-root` is pinned bottom-right via `!important` CSS override.

## Demo

The "Open the Live Chat Demo" button opens the Bolt Electrical iframe modal (same JS/CSS from electricians-ai). It is the only live demo at the moment. **TODO (Andy):** swap for a profession-neutral live demo when one is available.

Phone demo: +44 7782 214455 (Retell agent on a real UK SME). Same number across the network.

## SEO Content Rules

- British English throughout (specialise, organised, colour, behaviour).
- Pricing stays vague — "book a call for a quote". No specific figures.
- "30+ years" is always attributed to **Andy Norman**, not to the company.
- Keep buyer terms in visible H2/H3: "AI receptionist", "AI voice agent", "AI phone receptionist", "AI virtual receptionist", "AI auto attendant", "answering service".

## .htaccess

Forces HTTPS, 301s `/index.html` → `/`, sets security headers (HSTS, X-Content-Type-Options, X-Frame-Options SAMEORIGIN, Referrer-Policy, Permissions-Policy). Permissions-Policy allows microphone for `self` + `https://boltelectrical.uk` to support the live demo iframe.

## Deployment

Static — push to any host. GitHub: `github.com/Nipstar/voice-ai-receptionist`, branch `main`.
