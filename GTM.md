# Go-to-Market Strategy

## ICP (Ideal Customer Profile)

**Primary:** Engineering managers and CTOs at B2B SaaS startups, 15–50 employees, Series Seed to Series A.

**Why this profile:**
- They have enough AI tooling to have meaningful spend (≥3 tools, ≥$300/mo)
- They feel the budget pressure but rarely have a dedicated finance person auditing SaaS
- They are technical enough to trust the audit reasoning but busy enough that they're grateful for something that does the analysis for them
- They have buying authority or direct influence over the purchase decision
- They share tools with their team on community-of-practice forums (Hacker News, X/Twitter, engineering Slack groups)

**Secondary ICP:** Head of Finance / COO at the same company size — they show up during budget reviews when someone shares the audit link.

**Anti-ICP:**
- Solo developers (spend too low to be meaningful Credex leads)
- Companies >200 employees (they have procurement teams; the self-serve audit isn't the right motion)
- Non-technical founders (struggle to trust or act on the recommendations without someone to explain them)

---

## Distribution Channels

### Channel 1: Hacker News "Show HN" (Week 1 launch)

**Pitch:** "Show HN: I built a free 90-second audit tool that tells startups exactly where they're overpaying for AI tools"

**Why this works:** HN "Show HN" posts by builders consistently drive 300–2,000 unique visitors in 24 hours when the project is genuinely useful and not obviously promotional. SpendLens has both: it's useful on its own, and the Credex mention is downstream of showing value, not the first thing.

**Success metric:** Top 3 on Show HN (≥200 upvotes), ≥500 audits run in 48 hours.

### Channel 2: Founder communities (Weeks 2–4)

Target Slack/Discord communities where the ICP congregates:
- Indie Hackers
- On Deck community
- SaaS founders Facebook groups
- YC Founders Slack (if accessible)
- Cerebral Valley (AI-focused startup community)

**Format:** Post as a member sharing a useful tool, not as a company announcement. "I built this for myself and found $400/mo in savings — ran it for a few friends too, thought people here might find it useful."

### Channel 3: Twitter/X organic (Ongoing)

Post the audit results as shareable content:
- "We audited 50 startups' AI spend in the last week. Here's what we found: [thread]"
- Screenshot a real audit result (anonymized) showing $800/month in savings
- Quote-tweet any HN discussion about AI tool costs

**Target:** One viral tweet per week. Even a modest 200-retweet thread drives 500+ site visits.

### Channel 4: Cold email to the list (Week 3+)

Once ≥200 leads are captured, segment by savings amount:
- **High savings (≥$500/mo):** Immediate 1:1 outreach from a Credex human. Subject: "Your SpendLens audit — want help acting on the $X/mo savings?"
- **Medium savings ($100–499/mo):** Automated nurture sequence (3 emails over 2 weeks). Email 2: case study of a similar company. Email 3: Credex credits intro.
- **Low savings (<$100/mo):** Quarterly check-in email when pricing data updates.

---

## Launch Week Execution Plan

| Day | Action |
|-----|--------|
| Mon | Soft launch — share with 10 trusted friends for feedback and bug reports |
| Tue | Fix bugs, add any missing tools people request |
| Wed | **HN Show HN post** at 9am EST (peak engagement window) |
| Wed | Post on Twitter/X with screenshot of a real audit result |
| Thu | Follow up on HN comments personally; post to 3 Slack communities |
| Fri | Compile first-week metrics; send personal email to all high-savings leads |

---

## Viral Loop

The product has a natural sharing mechanic:

1. User audits their stack → sees "Save $800/mo"
2. They click Share → copies `spendlens.vercel.app/audit/abc123`
3. They paste it in a Slack/Discord/Twitter with "we're apparently wasting $800/mo on AI tools"
4. Their network clicks → runs their own audit → generates new audits and leads

The shareable URL with custom OG tags (showing the savings amount) is essential for this loop. A link that previews "$800/mo savings found" gets clicked. A generic link doesn't.

---

## Metrics for Channel-Market Fit

Within 30 days of launch, we expect:
- ≥1,000 audits run
- ≥15% email capture rate (150+ leads)
- ≥5 leads converting to Credex consultation calls
- ≥1 closed Credex deal attributable to SpendLens

If email capture rate is <10%, the lead form placement or copy needs to change. If consultation call rate from leads is <3%, the email nurture or targeting is wrong.
