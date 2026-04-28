# Noweb.

> Premium websites for Australian businesses. Live in 7 days. $990 AUD.

Noweb. is a Perth-based web design studio. Custom-designed, mobile-first, fast-loading websites delivered in one week, fixed price, with an optional Website Care subscription at $60/week.

Acquisition is 100% Meta Ads (Instagram + Facebook). Checkout via Stripe Payment Links direct from the landing page — no sales calls.

---

## Repository structure

```
.
├── noweb-site/                 # Deployable static site (Netlify drop)
│   ├── index.html              # Sales landing page → noweb.com.au
│   ├── brief.html              # Post-payment client brief → noweb.com.au/brief
│   └── work/
│       └── saltwood.html       # Portfolio piece → noweb.com.au/work/saltwood
├── noweb-site.zip              # Same bundle, zipped (for Netlify drag-drop)
├── _reference/                 # Operational docs (not deployed)
│   ├── cove-playbook.md        # Production playbook (per-client workflow)
│   ├── cove-launch-manual.md   # Pre-launch checklist + Meta Ads setup
│   ├── cove-meta-ads-pack.md   # Creative briefs for the 6 launch ads
│   └── cowork-brief-fr-v2.md   # Original sprint brief (FR)
└── _source/                    # Original Cove. files (pre-rebrand snapshot)
```

The `noweb-site/` folder is the only thing deployed.

## Deploy

### Quick (temporary URL)
1. Open https://app.netlify.com/drop
2. Drag the `noweb-site/` folder (or `noweb-site.zip`) onto the drop zone
3. Netlify gives you a `xyz-1234.netlify.app` URL within ~30 seconds

### Custom domain
1. In Netlify dashboard → Site → Domain settings → Add custom domain → `noweb.com.au`
2. Copy the DNS records Netlify shows
3. Paste them in VentraIP DNS Manager for `noweb.com.au`
4. Wait for propagation (~15 min), SSL provisions automatically

## Pre-launch placeholders to fill

Before paying for ads, replace these placeholders inside `noweb-site/`:

| File | Placeholder | Where to get it |
|---|---|---|
| `index.html` | `STRIPE_LINK_990_ONLY` | Stripe Dashboard → Payment Links → `$990 one-time` |
| `index.html` | `STRIPE_LINK_990_PLUS_CARE` | Stripe Dashboard → Payment Links → `$990 + $60/wk subscription` |
| `index.html` | `PIXEL_ID_HERE` (and uncomment the block) | Meta Events Manager → Pixels → Noweb Pixel |
| `index.html` | `wa.me/61400000000` | Replace with your real WhatsApp number |
| `brief.html` | `REPLACE_WITH_WEB3FORMS_ACCESS_KEY` | web3forms.com → Account → Access Key |

## Operational stack

| Function | Tool | Cost |
|---|---|---|
| Hosting | Netlify (drag-drop) | Free |
| Payments | Stripe Payment Links (AU) | 1.7% + $0.30 AUD |
| Brief form backend | Web3Forms | Free up to 250/mo |
| Domain | VentraIP (`noweb.com.au`) | Already owned |
| Email | Gmail (`nowebcontact@gmail.com`) + Cloudflare Email Routing | Free |
| Client comms | WhatsApp Business + Email | Free |
| Acquisition | Meta Ads (Instagram + Facebook) | Variable |

## Pricing

| Item | Price |
|---|---|
| Website (one-time) | $990 AUD |
| Website Care (optional, ongoing) | $60 / week |

Includes: custom design, mobile-first build, local SEO setup, contact + booking integrations, hosting + SSL (1st month), unlimited revisions for 14 days, domain setup, launch + handover. 30-day money-back guarantee.

## License

All rights reserved — Noweb. (Marwan, Perth WA, Australia).
