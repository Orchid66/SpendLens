# Landing Copy

All copy for the landing page, results page, email, and share card. This document is the source of truth — any copy changes should be made here first.

---

## Landing Page `/`

### Headline
**Primary:**
> Your team is probably **overpaying** for AI tools.

**Rationale:** "Probably" is doing work here — it's not an accusation, it's an invitation. Most engineers already suspect they're paying for things they don't need. The headline names the suspicion, which triggers the scroll.

**Rejected alternatives:**
- "Cut your AI spend by 40%" → sounds like an ad claim, not a tool
- "The AI tool audit every startup needs" → generic, no tension
- "Stop wasting money on AI" → shame-framing, negative opener

### Subheading
> Enter what you pay. Get an instant breakdown of where you're wasting money and exactly what to do about it.

**Rationale:** Two sentences. Sentence 1 is what you do. Sentence 2 is what you get. No buzzwords, no passive voice.

### Supporting line
> No login. No spam. Takes 2 minutes.

**Rationale:** Addresses the three biggest objections before the user asks them. "No login" removes the commitment barrier. "No spam" removes the email anxiety. "2 minutes" removes the time objection.

### CTA Button
> Run my free audit →

**Rationale:** First-person ("my") increases click-through by ~10-20% vs second-person ("Run your audit"). "Free" is load-bearing — this is a trust-building tool for a service company; "free" is the whole point.

### Trust Signals (below the fold, near the form)
- **No login required** — "Email optional — only after we show you results"
- **Instant results** — "Audit runs in under 2 seconds, 100% on-server"
- **Finance-grade reasoning** — "Every recommendation cites the exact dollar math"

---

## Results Page `/audit/:id`

### Hero — high savings case
```
Potential savings found

$800
per month · $9,600/year

That's 32% of your $2,500/mo AI spend
```

**Rationale:** Lead with the number, not the context. The number is the whole point — make it unavoidable. The percentage is secondary social proof ("this is significant relative to your total").

### Hero — no savings case
```
You're spending well 🎉

No significant overspend found for your $420/mo AI stack.
We'll notify you when new optimizations apply.
```

**Rationale:** Don't let users feel like the audit was a waste of their time if no savings are found. Reframe as a positive: you got confirmation you're spending well. The CTA ("notify me") converts them to an email lead anyway.

### AI Summary label
> Your personalized analysis

**Rationale:** "AI-generated" in the label would make it feel less trustworthy for financial advice. "Personalized" emphasizes that it's specific to their situation. The analysis is always backed by the hardcoded math, so it's trustworthy regardless.

### Redundancy alert header
> Tool overlap detected (2)

**Rationale:** "Overlap" is less accusatory than "redundancy" and more concrete than "duplication." The count in the header gives users the size of the issue before they read the details.

### Lead capture — high savings variant
**Heading:** Get your full report by email
**Subheading:** We'll send your audit breakdown + implementation steps.
**Button:** Email me the report

### Lead capture — no savings variant
**Heading:** Get notified when new savings apply to your stack
**Subheading:** AI tool pricing changes often. We'll flag new opportunities.
**Button:** Notify me

**Rationale for two variants:** High-savings users are in "I need to act on this" mode — the email is getting them the thing they need. No-savings users need a different motivation: future value, not present action.

### Credex CTA (high savings only)
> **💡 Capture even more with Credex credits**
> 
> Beyond plan optimization, Credex sources discounted AI infrastructure credits from companies that overforecast — Cursor, Claude, ChatGPT Enterprise, and more. At $2,500/mo in AI spend, the savings compound quickly.
> 
> [Book a Credex consultation →]

**Rationale:** The Credex mention comes after the user has already seen and processed the SpendLens savings. At this point they're in "I want to save money on AI" mode, which is exactly when the Credex pitch lands. Leading with Credex would feel like a bait-and-switch; showing it after is additive.

---

## Transactional Email

### Subject lines

**High savings (≥$500/mo):**
> Your AI spend audit — $800/mo savings found

**Medium savings ($100–499):**
> Your SpendLens audit — 3 savings opportunities found

**No savings:**
> Your SpendLens audit — you're spending well

**Rationale:** The savings number in the subject line is the whole click-through trigger. Tested email subject lines consistently show that specificity ("$800/mo" not "hundreds of dollars") dramatically outperforms vague claims.

### Body structure
1. Dollar number — big, green, unavoidable
2. "View your full audit" CTA button — one click to the result
3. [High savings only] Credex block — secondary offer after primary CTA
4. Footer — one sentence about why they're receiving this, no unsubscribe wall

---

## Share Card (OG Image copy)

**High savings:**
> SpendLens found **$800/mo** in AI tool savings for this team
> spendlens.vercel.app — free audit, no login

**No savings:**
> SpendLens: This team's AI spend is well-optimized ✓
> spendlens.vercel.app — free audit, no login

---

## Copy Rules for Future Writers

1. **Always lead with the number.** Dollar amounts before context. Always.
2. **Never say "up to."** "Up to $500/mo in savings" reads as $0. Say "X/mo" based on actual audit math.
3. **Never use passive voice for recommendations.** "Downgrade Cursor to Pro" not "Cursor could potentially be downgraded."
4. **Specificity > authority.** "Business adds SSO and admin controls — features with no practical value for teams under 10" beats "consider whether the premium features justify the cost."
5. **Credex comes after value.** The product earns the right to mention Credex by showing real savings first.
