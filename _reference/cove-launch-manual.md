# Cove. — Launch Day Manual

> The final manual. From "everything's built" to "first ad is live."
> Estimated time end-to-end: 4-6 hours.

---

## Master pre-launch checklist

Tick every box before you spend a single dollar on ads. Skip one and you waste budget.

### Brand & domain
- [ ] Final brand name decided (Cove. or your custom name)
- [ ] Domain purchased (covestudio.com.au or equivalent) on Cloudflare Registrar (~15 USD/yr)
- [ ] Domain DNS pointing to Vercel
- [ ] Three landing files deployed:
  - [ ] `landing.html` → main domain root (e.g. covestudio.com.au)
  - [ ] `brief.html` → covestudio.com.au/brief
  - [ ] `saltwood-kitchen.html` → covestudio.com.au/work/saltwood (portfolio piece)
- [ ] HTTPS works on all three
- [ ] Tested on real iPhone Safari + Android Chrome

### Stripe
- [ ] Stripe Australia account verified (ABN required)
- [ ] Two Payment Links created:
  - [ ] Link A — `Cove Website Package — $990 AUD one-time`
  - [ ] Link B — `Cove Website Package $990 + Website Care $40/week subscription`
- [ ] Both links' Success URL set to `https://covestudio.com.au/brief`
- [ ] Test payment of $0.50 in test mode → verify redirect to brief works
- [ ] GST collection enabled (Stripe handles this automatically once you're GST registered)
- [ ] Receipt customisation: logo, business name, ABN added

### Web3Forms (brief backend)
- [ ] Account created at web3forms.com
- [ ] Access Key generated
- [ ] Access Key pasted in `brief.html` line `REPLACE_WITH_WEB3FORMS_ACCESS_KEY`
- [ ] Test submission from brief.html → email lands at `nowebcontact@gmail.com`
- [ ] All form fields appear in the email correctly

### Email
- [ ] Email forwarding set up: `hello@covestudio.com.au` → `nowebcontact@gmail.com` (via Cloudflare Email Routing, free)
- [ ] Gmail signature configured (logo, ABN, contact)
- [ ] Email templates from playbook §6 saved as Gmail canned responses

### WhatsApp
- [ ] WhatsApp Business installed
- [ ] Business profile complete (logo, hours, address, description)
- [ ] Quick replies pre-saved for common questions
- [ ] Number updated in `landing.html` floating chat button

### Tracking
- [ ] Google Sheet client tracker created (16 columns from playbook §5)
- [ ] Bookmarked, accessible on phone

### Files & assets
- [ ] All 6 Meta creatives produced (3 images + 3 videos)
- [ ] Saved in `/Cove/ads/` folder
- [ ] Backup uploaded to Google Drive
- [ ] All copies (primary text, headlines, CTAs) saved in a doc for easy paste

### Legal
- [ ] ABN verified active on abr.business.gov.au
- [ ] **GST registration in progress or done** (see §3 below — non-negotiable)
- [ ] Privacy Policy + Terms of Service pages live (use Termly or similar generator, 15 min)
- [ ] Footer links to both

---

## 2. Meta Business Manager setup — step by step

### Step 1 — Create Business Manager
1. Go to business.facebook.com
2. Click "Create Account"
3. Business name: "Cove Studio" (or your real brand)
4. Your name + work email
5. Submit, verify email

### Step 2 — Create Ad Account
1. In Business Manager → Settings → Accounts → Ad Accounts
2. Click "Add" → "Create a new ad account"
3. Name: "Cove — Main"
4. Time zone: **Australia/Perth**
5. Currency: **AUD**
6. Add yourself as Admin

### Step 3 — Add payment method
1. Ad Account Settings → Payment Settings
2. Add card or PayPal
3. Set spending limit: **$500/day** initial cap (safety, you can raise later)

### Step 4 — Create the Pixel
1. Events Manager → Connect Data Sources → Web → Meta Pixel
2. Name: "Cove Pixel"
3. Enter your domain: `covestudio.com.au`
4. Get the pixel ID (a long number like `123456789012345`)
5. Copy the base code Meta gives you

### Step 5 — Install Pixel on your site
1. Open `landing.html`
2. Find the commented Pixel block at the top of `<head>`
3. Uncomment it (remove `<!--` and `-->`)
4. Replace `PIXEL_ID_HERE` with your real pixel ID
5. Do the same in `brief.html` (add the pixel block at top of head if not already)
6. Redeploy to Vercel (drag-and-drop again, or push to Git if you set that up)
7. Use the Meta Pixel Helper Chrome extension to verify it fires on your live landing page

### Step 6 — Set up custom conversion events
1. Events Manager → Pixels → Cove Pixel → Aggregated Event Measurement
2. Verify your domain (Meta gives you a meta-tag to add or DNS record)
3. Configure these 5 events in priority order:
   1. **Purchase** (highest priority)
   2. **InitiateCheckout**
   3. **Lead** (brief submitted)
   4. **ViewContent** (50% scroll on landing)
   5. **PageView**

You set these as priorities because iOS 14+ users only count the top-priority event triggered. Putting Purchase first is critical.

### Step 7 — Generate Stripe → Pixel Purchase event
This is the trickiest part. Stripe and Meta don't talk natively. Two options:

**Option A — Quick (recommended for launch):** trigger Purchase event on the brief page (since brief = paid customer)
1. In `brief.html`, the Lead event already fires on form submit
2. Add a Purchase event fire at the top of `brief.html` (when user lands after Stripe redirect):
```javascript
if (typeof fbq !== 'undefined') {
  fbq('track', 'Purchase', { value: 990, currency: 'AUD' });
}
```
3. The downside: it fires even if user lands on brief without paying. Mitigation: only people redirected from Stripe land here, since brief.html has `noindex`.

**Option B — Robust (do later):** use Stripe Webhooks → Zapier → Meta Conversions API
- Costs ~20 USD/month Zapier
- More accurate especially with iOS users
- Skip for launch, add at client #20+

---

## 3. GST registration — do this TODAY

You're projecting 50K+ AUD revenue in May alone. Australian Tax Office (ATO) requires GST registration once you cross **$75,000 AUD/year turnover**.

At your projected pace, you cross that threshold in **week 2 of May**. If you're not registered, you owe back GST on every sale, plus penalties.

### Process (~30 min, online)

1. Go to ato.gov.au → Business → Register for GST
2. Login with myGov linked to your ABN (or create the link if not)
3. Click "Add a new tax type" → GST
4. Effective date: **today's date** (or backdated to ABN start date if you've been operating)
5. Reporting frequency: **Quarterly** (default for small business)
6. Submit

**Once registered:**
- Charge GST (10%) on all invoices to Australian customers — Stripe handles this if you enable AU tax in settings
- Lodge BAS (Business Activity Statement) every quarter
- Claim GST credits on business expenses (ads, software, hosting)

**Practical impact on your pricing:**
- Your $990 advertised price likely includes GST (you collect $90 GST per sale, keep $900)
- You can advertise as "$990 inc. GST" or "$900 + GST" — keep one consistent
- Recommended: keep "$990 inc. GST" for clean marketing optics

**Get an accountant by client #20.** $200/month is the going rate for a Perth small biz tax accountant. Pays for itself.

---

## 4. First campaign setup — Meta Ads Manager

Once Pixel is live and firing, you launch.

### Campaign structure

**Campaign 1: "Cove — May Launch — Cold"**
- Objective: **Sales**
- Conversion event: **InitiateCheckout** (for first 14 days)
- Bidding: Highest volume (default)
- Budget: **Campaign Budget Optimisation (CBO) ON, $280/day**

Inside this campaign, **3 ad sets** (one per angle):
- Ad Set A: Problem-Aware
  - Audience: Detailed targeting (small business owners, restaurant management, etc.)
  - Placements: Auto, exclude Audience Network
  - Ads: Creative #1 (image) + Creative #2 (video)
- Ad Set B: Price-Hammer
  - Same audience as A
  - Ads: Creative #3 + Creative #4
- Ad Set C: Social Proof
  - Same audience as A and B
  - Ads: Creative #5 + Creative #6

**Campaign 2: "Cove — May Launch — Retargeting"**
- Activate from Day 7 (need website visitor data first)
- Objective: Sales
- Conversion event: Purchase
- Budget: $80/day
- 1 ad set targeting: website visitors last 14 days, didn't purchase
- Use Creative #5 or #6 (testimonial-style) only

**Campaign 3: "Cove — May Launch — Lookalike"**
- Activate from Day 14
- Audience: 1% Lookalike Australia from purchaser pixel data
- Budget: $40/day
- Use winning creative from Campaign 1

### Pre-launch ads test

Before going live, use Meta Ads Manager Preview:
- For each of the 6 creatives, click Preview → Mobile (Instagram Reel + Feed + Stories + Facebook Feed)
- Check: subtitles readable? CTA visible? logo placed? text legible?
- Save preview screenshots — useful for the Sheet record

---

## 5. Launch day protocol (your first 24 hours live)

### Hour 0 — Click Publish
1. Final check: all 6 ads in Draft → Review for compliance issues (Meta scans automatically)
2. If yellow warnings: read carefully. Usually about claims like "guaranteed results." Adjust if flagged.
3. Click "Publish"
4. Note the time. Meta typically takes 30-90 minutes to start delivery on a fresh account.

### Hour 0-4 — Watch the slow-burn
1. Check Ads Manager every hour
2. Look for:
   - Spend starting (good sign — algo is finding people)
   - Reach > 0 (good)
   - Any "Disapproved" status (bad — fix immediately, don't appeal unless certain)
3. **Don't change anything in first 24h.** The algo needs at least 48h of learning before any data is meaningful.

### Hour 4-12 — Stay off the dashboard
- Refreshing every 10 minutes will drive you insane and tempt you to make bad decisions
- Set yourself a check-in alarm at hour 12

### Hour 12 — First check-in
- Spend so far: should be ~$140 (half of $280 daily, if launched at noon)
- Reach: 1,500-3,000 people
- CTR: ideally > 0.8%
- InitiateCheckouts: ideally 1-3 by now

If reach is 0 or under 100 after 4+ hours of "Active" status:
- Check ad approval status
- Check audience size (might be too narrow)
- Check budget pacing (should be linear)
- If still nothing: pause the campaign, ask Meta support chat

### Hour 24 — First real data
By now you should have:
- Spent ~$280
- Reach 3,000-6,000
- 1-5 InitiateCheckouts (cost $50-280 per IC)
- Possibly 0-1 Purchases (it's early)

**Don't panic if 0 purchases yet.** The algo is in learning phase. Real signal comes Day 3-4.

---

## 6. The 48-72 hour window — first decisions

### After 48 hours
Look at each of the 6 creatives' performance:

| Metric | Healthy | Warning | Kill |
|---|---|---|---|
| CTR | > 1.2% | 0.7-1.2% | < 0.7% |
| CPM | < $20 | $20-30 | > $30 |
| Cost per InitiateCheckout | < $40 | $40-80 | > $80 |
| Cost per Purchase (if any) | < $400 | $400-800 | > $800 |

**Actions:**
- Any creative in "Kill" zone in 2+ metrics → pause it
- Any creative in "Healthy" zone → leave it alone (DO NOT increase budget yet)
- Any creative with no purchase but good CTR → wait until Day 5

### After 72 hours
- Pause your bottom 2 creatives definitively
- Note which audience (problem / price / social proof) is winning
- Budget redistributed to winners by Meta CBO automatically

### Day 5 milestone
- You should have your **first Cove. customer** by now if pacing is healthy
- If 0 purchases by Day 5: review the funnel
  - Are people clicking but not initiating checkout? → Landing page issue (price, copy, trust signals)
  - Are they initiating checkout but not paying? → Stripe page issue (looks scammy, high friction)
  - Are they not clicking at all? → Creative issue (rewrite copy, test new angle)

---

## 7. Decision framework — week 1 to week 4

### Week 1: Test
- All 6 creatives running
- Don't touch budgets
- Cut losers at Day 7

### Week 2: Concentrate
- Top 3 creatives at $100/day each
- Activate retargeting campaign
- Refresh creative on losing angles (don't kill the angle; refresh the visual/copy)

### Week 3: Scale
- Top creative budget to $200/day if ROAS > 2x
- Activate Lookalike campaign
- Switch optimisation to Purchase event (you should have 50+ purchases by now)
- Refresh winning creatives — fatigue starts Day 18-21 in Perth

### Week 4: Reflect & raise
- Calculate true ROAS:
  - Total spent: ~$12,000
  - Total revenue: ?
  - True ROAS = Revenue / Spend
- If ROAS > 3x → consider raising prices to $1,290 in early June
- If ROAS 1.5-3x → keep going, optimise creatives
- If ROAS < 1.5x → fundamental funnel issue, audit landing + brief

---

## 8. The "Things won't go to plan" section

### Scenario: ads disapproved on Day 1
- Common reasons: "before/after" claims, dollar signs in image (Meta is strict), guarantee language
- Fix: rewrite the flagged creative, resubmit
- If repeat disapprovals: your ad account may be flagged. Contact Meta business support chat.

### Scenario: 7 days in, $1,500 spent, 0 purchases
- This is the "kill or pivot" moment
- Audit funnel:
  1. Open Meta Ads Manager → View landing URL → does it load fast?
  2. Click your CTA → does it go to Stripe smoothly?
  3. Check Stripe Dashboard → any abandoned checkouts? How many?
- If lots of abandoned checkouts → your offer is appealing but pricing or trust is the issue
- If no checkouts initiated → your landing isn't converting → revise copy/proof above the fold
- Pivot before Day 10. Don't burn the next $5,000 hoping it'll click.

### Scenario: client wants refund on Day 25
- Honour it without drama
- Process refund in Stripe (Refunds tab)
- Update tracker: status → REFUNDED
- Send the refund email template (playbook §6.5)
- Don't let it shake your confidence — it's part of the model. Aim for < 8% refund rate.

### Scenario: you're drowning in clients by client #15
- Triggered: you have 15 active deliveries in flight
- Action: hire your first VA on OnlineJobs.ph today
- Don't wait until you're at 25 active. By then, quality slips and refunds spike.

---

## 9. The launch sequence — final ordered list

This is the literal order to execute today, end of session:

1. ✅ Brand name finalised (Cove. or other)
2. ✅ Domain purchased on Cloudflare
3. ✅ 3 HTML files deployed on Vercel + custom domain pointed
4. ⏳ Stripe AU account → 2 Payment Links → Success URL set → test transaction
5. ⏳ Web3Forms account → Access Key → pasted in brief.html → redeploy
6. ⏳ Test full flow: landing → click CTA → Stripe checkout (test mode) → redirect to brief → submit brief → email received
7. ⏳ Privacy Policy + Terms generated and added to footer
8. ⏳ ABN/GST registration submitted on ATO
9. ⏳ Meta Business Manager + Ad Account + Pixel installed
10. ⏳ All 6 creatives produced in Canva (~3-6h)
11. ⏳ Campaign 1 built in Ads Manager: 3 ad sets × 2 creatives each
12. ⏳ Tracker Google Sheet ready
13. ⏳ Email templates saved as Gmail canned responses
14. ⏳ WhatsApp Business set up with quick replies
15. ⏳ Final pre-launch end-to-end test on real phone
16. 🚀 **Click Publish on Campaign 1**
17. 🍷 Pour something. You earned it.

---

## 10. The "what's next" preview

After launch day, the operational rhythm:

**Daily (15 min, morning):**
- Check Ads Manager for spend pace + flagged ads
- Check Stripe for new payments
- Check Gmail for new briefs
- Update tracker

**Twice weekly (Tuesday + Friday — your batch days, 4-6h):**
- Generate sites for new clients
- Run QA
- Deploy
- Send delivery emails

**Weekly (Sunday, 1h):**
- Review tracker: MRR, conversion by source, days-to-live average
- Review ad performance, kill losers, refresh top creatives
- Plan next week's batch

**Monthly:**
- Lodge BAS if quarterly cycle hits
- Review pricing
- Decide on subcontracting / hiring

---

## End of manual

You have everything. Five files:
1. **saltwood-kitchen.html** — your portfolio piece
2. **cove-landing.html** — your sales engine
3. **cove-brief.html** — your post-purchase flow
4. **cove-playbook.md** — your operations bible
5. **cove-meta-ads-pack.md** — your acquisition channel
6. **cove-launch-manual.md** — this document

Plus a Google Sheet, a Stripe account, a Pixel, and 6 creatives.

The first ad goes live when you decide. Not before.

— Nova.
