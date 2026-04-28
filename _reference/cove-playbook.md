# Cove. — Production Playbook

> Version 1.0 — Built for shipping 50-100 sites in 30 days, solo.

---

## 1. The 7-day timeline (per client)

| Day | Action | Time | Tool |
|---|---|---|---|
| **0** | Client pays via Stripe | — | Stripe |
| **0** | Client redirected to brief | — | Auto |
| **0-1** | Brief submitted → email to nowebcontact@gmail.com | — | Web3Forms |
| **1** | You add client to tracker, send confirmation email | 10 min | Sheet + Gmail |
| **1-2** | You generate customized site (Claude session) | 1-1.5h | Claude |
| **2** | You send design preview to client | 15 min | Gmail |
| **3-5** | Client revisions (max 2 rounds) | 30 min/round | Email/WhatsApp |
| **6** | Buy domain, deploy to Vercel, point DNS | 30 min | Cloudflare + Vercel |
| **7** | QA pass, send delivery email | 30 min | Gmail |
| **30** | Free hosting month ends → Care subscription kicks in (or hand-off) | — | Stripe |

**Total active time per client: ~3-4 hours.** At 50 clients/month = 150-200h. Solo doable but tight. Start sub-contracting at client #15.

---

## 2. Stack — the only tools you need

| Function | Tool | Why | Cost |
|---|---|---|---|
| Payment | Stripe Payment Links | No backend, Aussie GST handling | 1.7% + 0.30 AUD |
| Brief form backend | Web3Forms | Free 250/mo, file uploads, email notif | Free → 5 USD/mo at scale |
| Site generation | Claude (this) | You already have it | — |
| Hosting + deployment | **Vercel** (recommended) or Netlify | Drag-and-drop deploy, free SSL, free CDN | Free up to 100 deploys/day |
| Domain registrar | **Cloudflare Registrar** | At-cost pricing, auto-DNS, free SSL | ~10-15 USD/yr per domain |
| Client tracker | Google Sheet | Free, syncs everywhere | Free |
| Client comms | Gmail (`nowebcontact@gmail.com`) | Already set up | Free |
| Quick chat | WhatsApp Business | Aussie owners use it, written = your strength | Free |
| File transfer | Google Drive | Client already shares assets here | Free |

**No CMS. No Webflow. No Wordpress.** Pure HTML/CSS/JS deployed static = 0 maintenance, 0 hosting cost beyond Vercel free tier, 0 chance of plugin breaking, 0 attack surface. This is your moat.

---

## 3. Workflow — from brief to live

### Step 1 — Brief comes in (auto)

Client paid → brief landed in `nowebcontact@gmail.com`. Email subject: `🎉 New Cove client brief submitted`.

**Your move:**
1. Open the email, read everything
2. Open the **Cove Client Tracker** Google Sheet (template below)
3. Add a new row: name, business, email, payment date, status = `BRIEF RECEIVED`
4. Reply to client within 12h with a confirmation email (template in §6)
5. Open client's Google Drive link, verify assets are there. If empty/incomplete, email them today asking for missing pieces.

**Don't do:** don't start designing without the assets. You'll waste 2 hours, regenerate, frustrate everyone.

### Step 2 — Generate the site (90 min)

Open a new Claude conversation. Paste this prompt template (filled with brief data):

```
You are Cove., a Perth web design studio. I need you to customize the
Saltwood Kitchen master template for a new client. Here's the brief:

BUSINESS NAME: [from brief]
INDUSTRY: [from brief]
TAGLINE: [from brief]
STORY: [from brief]
VIBE: [from brief]
COLORS: [from brief, or "design appropriate palette for {industry} + {vibe}"]
SERVICES/PRODUCTS: [from brief]
ADDRESS / PHONE / EMAIL / HOURS: [from brief]
SOCIALS: [from brief]
FEATURES NEEDED: [checked items from brief]
ASSETS: [Google Drive link content summary]

Use the Saltwood Kitchen master template as your base. Restructure
sections as needed for this industry. Replace all content. Adjust
color variables to match the new brand. Keep the same level of
typographic and animation polish. Output a complete single-file HTML
ready to deploy.
```

Claude returns the HTML. **Save as `[business-slug].html`** in a project folder on your machine (e.g. `/Cove/clients/saltwood-kitchen/index.html`).

**Quality check before sending:** open the file in your browser, view it on desktop AND mobile (DevTools responsive mode). Look for:
- Broken images (orange gradient fallbacks visible)
- Wrong business name leftover from template
- Wrong color references
- Layout breaks below 360px width

### Step 3 — Send design preview (15 min)

You don't deploy yet. You send a **screenshot or live preview link**.

Two options:
- **A. Quick & dirty (recommended for first preview):** screenshot full desktop + mobile views, send via email
- **B. Live preview:** drop the HTML on Vercel as a temporary preview deploy, send the URL

Email template in §6 (`Design preview ready`).

**Set client expectation:** they have 2 rounds of revisions, max 5 changes per round, response within 24h or you assume approval. Stick to this. Otherwise scope creep destroys your margins.

### Step 4 — Revisions (30 min per round)

Client emails back changes. You either:
- Make them yourself (CSS tweaks, text edits) — usually 15-30 min
- Open Claude again with the current HTML + the changes requested → Claude returns updated version

**After 2 rounds**, send the final preview with subject: `Final design — approval needed before launch`. Make it clear: any further changes after approval = additional cost ($50/change). This is your scope creep firewall.

### Step 5 — Buy domain & deploy (30 min)

Once approved:

**A. Domain (5 min)**
1. Go to Cloudflare → Domain Registration
2. Search the domain client requested (e.g. `saltwoodkitchen.com.au`)
3. Buy it (10-15 USD/yr, no markup)
4. Domain auto-added to Cloudflare DNS

**B. Deploy site (5 min)**
1. Go to vercel.com → New Project → "Deploy without Git"
2. Drag and drop the HTML file (rename it to `index.html` first)
3. Vercel gives you a deploy URL like `client-name-abc123.vercel.app`
4. Test it. If broken, fix and redeploy.

**C. Connect domain (15 min)**
1. In Vercel project → Settings → Domains → Add `saltwoodkitchen.com.au`
2. Vercel shows DNS records to add
3. Go to Cloudflare DNS, add the records (A record + CNAME)
4. Wait 2-15 min, domain propagates
5. SSL auto-issues
6. Test https:// works

**D. Email forwarding (5 min, optional, charge extra or include in Care)**
- Cloudflare Email Routing → forward `hello@saltwoodkitchen.com.au` → client's personal email
- Free, takes 2 min, looks pro

### Step 6 — QA pass (15 min) — see §4 checklist

### Step 7 — Delivery (15 min)

Send the **delivery email** (template §6). Includes:
- Live URL
- 5-minute Loom video walkthrough you record
- Client's "owner pack" (PDF with credentials, hand-off info — template §7)
- Reminder about 30-day money-back & Care subscription kick-in

**Update tracker:** status = `LIVE`, log delivery date, note Care active or hand-off.

---

## 4. QA Checklist — run on every site before delivery

Print this. Tick each box. Never skip.

### Visual
- [ ] Loads without flash of unstyled content
- [ ] All sections visible and properly spaced
- [ ] No leftover placeholder text (`Lorem ipsum`, `your business name here`)
- [ ] No leftover Saltwood references (search the file for "saltwood", "cottesloe", "marine parade")
- [ ] All images load (check console for 404s)
- [ ] Logo correctly displayed
- [ ] Brand colors applied consistently
- [ ] Typography renders correctly (Google Fonts loaded)

### Mobile (DevTools at 375px width)
- [ ] No horizontal scroll
- [ ] Hero readable, CTA tappable
- [ ] Nav works (hamburger if used, or hidden gracefully)
- [ ] Images scale correctly
- [ ] Footer readable
- [ ] All buttons min 44px tap target

### Functional
- [ ] All internal anchor links scroll correctly (#story, #menu, etc.)
- [ ] All external links open correctly (social, maps)
- [ ] Contact form works (test submit if applicable)
- [ ] WhatsApp button opens chat with correct number
- [ ] Phone number is clickable (tel: link) on mobile
- [ ] Email is clickable (mailto: link)
- [ ] Map embed loads (if used)

### SEO basics
- [ ] `<title>` tag has business name + key phrase
- [ ] Meta description present (~150 chars)
- [ ] Open Graph tags filled (for social sharing)
- [ ] All images have meaningful `alt` attributes
- [ ] Heading hierarchy makes sense (one H1, then H2s, etc.)
- [ ] Favicon present

### Performance
- [ ] Run on PageSpeed Insights (https://pagespeed.web.dev) — score ≥ 85 mobile
- [ ] Total page weight < 2MB
- [ ] No console errors on load

### Final
- [ ] Domain resolves with HTTPS
- [ ] No mixed content warnings
- [ ] Tested on Safari iOS + Chrome Android (real phone if possible)
- [ ] Tested on desktop Chrome + Safari

**If 1 box unchecked → don't deliver. Fix first.**

---

## 5. Client Tracker — Google Sheet template

Create a sheet with these columns:

| Col | Field | Type | Notes |
|---|---|---|---|
| A | Client # | Auto | C-001, C-002… |
| B | Business name | Text | |
| C | Industry | Text | from brief |
| D | Client email | Email | |
| E | Client phone | Phone | |
| F | Domain | URL | once bought |
| G | Payment date | Date | from Stripe |
| H | Brief received | Date | |
| I | Preview sent | Date | |
| J | Live date | Date | |
| K | Status | Dropdown | BRIEF RECEIVED / IN DESIGN / IN REVISIONS / READY TO DEPLOY / LIVE / CARE ACTIVE / HANDED OFF / REFUNDED |
| L | Care subscription | Yes/No | |
| M | MRR | Number | $40 if Care, $0 if not |
| N | Refund risk flag | Yes/No | manual flag if client unhappy |
| O | Notes | Text | anything weird |
| P | Acquisition source | Text | from brief Q "where did you hear" |

**Conditional formatting suggestions:**
- Status `LIVE` → green
- Status `IN REVISIONS` for >3 days → red (escalate)
- Refund risk flag → red row

**Weekly review (every Sunday, 30 min):**
- Total MRR (sum col M)
- Conversion by acquisition source (group by col P)
- Average days from brief to live (col H to J)
- Active refund risk count

---

## 6. Email templates (copy-paste ready)

### 6.1 — Confirmation email (within 12h of brief submission)

**Subject:** Welcome to Cove. — your build is starting, [Client First Name]

```
Hi [first name],

Thank you for trusting us with your website — your brief is in our hands and the kitchen is officially open.

Here's what happens next:

→ Within 48 hours: I'll send you a full design preview of your homepage. Real layout, your content, your colours, your photos. Not a rough mockup — a proper look at where we're heading.

→ From there: You get up to 2 rounds of revisions. We refine until you love it.

→ Day 7 from now: You're live. Domain pointed, SSL active, the whole thing handed over.

A few quick things before we go heads-down:

1. Your project number is **[C-XXX]** — quote it on any reply.
2. If you need to reach me fast, reply to this email or message me on WhatsApp: [your number].
3. If anything in your brief was unclear, expect a quick clarifying email from me later today.

Talk soon,
[Your name]
Cove.

---
ABN [your ABN] · Perth WA
30-day money-back guarantee · No questions asked
```

### 6.2 — Design preview email (Day 2)

**Subject:** Your design preview is ready — [Business Name]

```
Hi [first name],

Here's your homepage. Take your time looking through it on both desktop and mobile.

→ Live preview: [Vercel preview URL]
→ Desktop screenshot: [attach]
→ Mobile screenshot: [attach]

What I'd love from you:
1. Sit with it for a few hours before responding (first reactions can be misleading)
2. Then send me ONE consolidated email with all your changes — colours, copy, layout, images
3. Limit to 5 main change requests for round 1 — keeps us fast and focused

If you don't reply within 48 hours, I'll assume green light and start building.

Cheers,
[Your name]
```

### 6.3 — Final approval email (after revision rounds)

**Subject:** Final design — approval needed before launch

```
Hi [first name],

Round 2 done. Here's the final version: [preview URL]

Once you say "approved," we move to launch (3-4 days). Any changes after this point will incur a $50 admin fee per change to keep things fair on both sides.

Quick reply with "approved" and we're off.

[Your name]
```

### 6.4 — Delivery email (Day 7)

**Subject:** 🎉 [Business Name] is LIVE — your website is ready

```
Hi [first name],

Your site is live. Welcome to the internet.

→ **Your live site:** https://[domain]
→ **Walkthrough video (5 min):** [Loom link]
→ **Owner pack PDF:** [attach]

The owner pack contains:
- Your domain registrar login (Cloudflare)
- Your hosting access (Vercel)
- How to make basic content updates yourself (or via me)
- What's covered by Website Care if you've subscribed

A few important notes:

1. **30-day money-back guarantee starts today.** If anything feels off, reply to this email — we'll fix it or refund.

2. **First month of hosting is free.** After that:
   - If you're on Website Care ($40/week): nothing changes, hosting is included.
   - If not: you'll need to migrate. I'll send a reminder at day 25 with options.

3. **Tell me about your launch.** Share it with your customers. Tag us on Instagram if you want — we love seeing them in the wild.

Welcome aboard.

[Your name]
Cove.
```

### 6.5 — Refund request response (when shit hits the fan)

**Subject:** RE: refund

```
Hi [first name],

Got your message. No drama — the guarantee is real and unconditional.

Two paths from here:

→ **Refund now:** I'll process within 48h via Stripe (back to your original card).
→ **One last fix:** If there's a specific thing that's off, I'll have a final pass at it tonight. If you still want a refund after, no questions.

Reply with whichever you prefer.

[Your name]
```

---

## 7. Owner Pack — what you give the client at delivery

A simple PDF (1-2 pages) containing:

```
COVE. — OWNER PACK FOR [BUSINESS NAME]

Project: C-XXX
Live URL: https://[domain]
Live since: [date]

— ACCESS —
Domain registrar: Cloudflare (login: [client email] · I will transfer ownership on request)
Hosting: Vercel (managed by Cove. on your behalf)
Email forwarding: hello@[domain] → [client personal email]

— SUPPORT —
Email: hello@covestudio.com.au
WhatsApp: [your number]
Response time: <24h business days

— UPDATES —
On Website Care ($40/week): unlimited content updates included.
Without Care: $50 per update request.

— GUARANTEE —
30-day money-back from [delivery date]. Email us, no questions asked.

— OWNERSHIP —
You own the design, the code, the domain, and all content.
On request, we hand over Vercel access and source code at any time.

Welcome to the internet.
— Cove.
```

You can generate this on the fly from a template in Google Docs / Pages, export as PDF, attach to delivery email.

---

## 8. Sub-contracting decision tree (for client #15+)

You're solo. At 15+ active clients in flight at once, you'll start dropping balls. Decision tree:

**At client #1-14:** do everything yourself. You learn the workflow, refine the templates, build the muscle.

**At client #15-30:** sub-contract the **deployment + domain setup** (the boring 30-min part). 
- Hire 1 VA on OnlineJobs.ph or Upwork
- 50-80 AUD/site for: deploy on Vercel, point DNS, run QA checklist, send delivery email from a template
- You keep: client comms, generation, revisions, edge cases

**At client #30+:** sub-contract **revisions** too.
- Same VA or hire a junior dev
- They handle round 1 revisions from a clear delta brief you write
- You keep: closing, generation, escalations

**Never sub-contract:** initial generation, refund handling, brand-sensitive comms.

---

## 9. Things that will go wrong (and how to handle)

| Failure | Likelihood | Fix |
|---|---|---|
| Client doesn't fill brief properly | High | Email Day 1 asking for missing info, set 48h deadline before refunding |
| Client wants 5 rounds of revisions | High | Stick to 2-round policy. Charge for extras. |
| Client unhappy with first preview | Medium | Listen, ask 3 specific questions, regenerate. Don't argue style. |
| Domain registration fails (already taken) | Medium | Have 3 backup names from brief. Email client immediately. |
| Site breaks on iPhone Safari | Medium | Always test on real device before deploy. iOS Safari is the silent killer. |
| Client asks for refund after Day 30 | Low | Politely decline, offer 50% credit toward Care subscription. |
| Stripe chargeback | Low | Reply with proof of delivery (email, screenshots, URL). Stripe usually rules in your favor on services. |
| Vercel deploy fails | Very low | Try Netlify as backup. Drag-and-drop, same flow. |

---

## 10. Pricing increase ladder (your real revenue plan)

You're starting at $990. That's the **acquisition price**, not the steady-state price. Move the goalposts:

| Phase | Price | When | Why |
|---|---|---|---|
| Launch | $990 | May | Acquisition + portfolio building |
| Validation | $1,290 | June | After 50+ delivered, raise 30% |
| Steady state | $1,490 | July | Default price |
| Premium tier | $1,990 | Aug | "Premium" with extras (custom illustration, copywriting included) |

Care subscription stays at $40/week → can go to $60/week at Premium tier.

**Always test the price increase on 30% of traffic first.** Split your Meta Ads audience: 70% sees $990 ad, 30% sees $1,290 ad. If the ROAS is comparable on the higher price, scale it.

---

## 11. Files index

After Day 1, your `/Cove/` folder structure should look like:

```
/Cove/
├── master/
│   ├── saltwood-kitchen.html        ← reference template
│   ├── landing.html                 ← cove-landing.html, your sales page
│   ├── brief.html                   ← cove-brief.html, post-payment
│   └── this-playbook.md             ← this file
├── clients/
│   ├── C-001-saltwood-kitchen/
│   │   ├── index.html
│   │   ├── brief.pdf                ← exported from email
│   │   └── owner-pack.pdf
│   ├── C-002-bondi-cafe/
│   └── ...
├── tracker.gsheet                   ← link to your client tracker
└── assets/
    └── stock-photos/                ← royalty-free fallbacks
```

Keep it ruthlessly organized. At client #20 you will lose your mind without this structure.

---

## End of playbook

Print this. Pin it. Reference it on every delivery for the first 10 clients.
After client #10, you'll know the workflow by heart.

— Nova, your Jarvis.
