# Metrics

## North Star

**Qualified leads generated per month** — defined as a lead with ≥$200/mo in identified savings, with an email address captured.

This is the north star because it directly measures SpendLens's core job: generating warm, high-intent leads for Credex. An audit with $50 in savings is educational but not commercially useful. An audit with $800 in savings from a company spending $3k/month on AI is exactly the customer Credex wants.

---

## Dashboard Metrics (check weekly)

### Funnel Metrics

| Metric | Target (Month 1) | Target (Month 3) | How to Measure |
|--------|-----------------|-----------------|----------------|
| Audits run | 500 | 3,000 | `count(audits)` in Supabase |
| Email captures | 75 (15%) | 450 (15%) | `count(leads)` in Supabase |
| High-value leads (≥$200/mo savings) | 30 (40% of captures) | 135 | `count(leads WHERE monthly_savings >= 200)` |
| Credex consultation books | 5 | 30 | CRM tag from Credex team |
| Credex deals closed | 1 | 5 | CRM close tag |

### Quality Metrics

| Metric | Target | How to Measure |
|--------|--------|----------------|
| Average savings identified per audit | ≥$150/mo | `avg(audit_data->>'totalMonthlySavings')` |
| % of audits with ≥1 recommendation | ≥65% | `count WHERE toolResults.some(r => r.status = 'overspending') / total` |
| % of audits flagging redundancy | ≥20% | `count WHERE redundancies.length > 0 / total` |

### Engagement Metrics

| Metric | Target | How to Measure |
|--------|--------|----------------|
| Audit completion rate | ≥85% | `audits started / audits stored` (add a started event) |
| Share link clicks | ≥10% of audits | Add UTM tracking to share URL |
| Return visits (same email, new audit) | ≥5% within 90 days | `count WHERE email appears in leads more than once` |

---

## Events to Track (via Vercel Analytics or Plausible)

```
audit_started         - user clicks "Run my free audit"
audit_completed       - audit stored in DB (POST /api/audit 201)
results_viewed        - /audit/:id page loaded
summary_loaded        - AI summary rendered (not fallback)
share_clicked         - copy button on results page
lead_form_opened      - "Email me the report" clicked
lead_submitted        - POST /api/leads 200
credex_cta_clicked    - "Book a Credex consultation" link clicked
```

All events should include:
- `savings_bucket`: "none" | "low" | "medium" | "high"
- `tool_count`: number of tools in the audit
- `team_size_bucket`: "1-5" | "6-20" | "21-50" | "50+"

---

## Pricing Data Accuracy Metric

**Monthly pricing audit score** — spot-check 3 tools per month against their official pricing pages. Track:
- Number of tools with stale pricing (>90 days since last verified)
- Number of pricing errors caught

Target: 0 errors per quarter. Every stale price is a trust risk.

---

## Credex Attribution Tracking

When a Credex sales rep contacts a lead from SpendLens:
1. Log the lead source in the CRM as "SpendLens"
2. After close (or loss), tag with `spendlens_influenced: true`
3. Record the audit ID from the lead row so we can analyze: what savings amounts correlate with closes?

**Goal:** Build a regression model within 3 months that predicts Credex close probability from SpendLens audit data. This tells us which lead segments to prioritize for direct outreach.

---

## What We Watch for Problems

### Red flags — investigate immediately
- Email capture rate drops below 8% → copy or placement problem
- Average savings per audit drops below $80 → audit rules may be too conservative, or ICP is shifting toward well-optimized teams
- Credex consultation-to-close rate drops below 10% → lead quality problem, not SpendLens's responsibility but worth auditing our ICP targeting

### Green flags — accelerate distribution
- Any single referral source (community, tweet, HN) drives >100 audits → double down on that channel
- Any tool combination (e.g. "Cursor + Copilot") appears in >30% of audits → create targeted content around that specific overlap
- Email open rate >50% → the subject line formula is working; don't change it

---

## Month 1 vs Month 3 Review

At the end of Month 1, hold a metrics review meeting. Answer:
1. What is the actual email capture rate? How does it differ by savings bucket?
2. Which tool combinations are most common?
3. Which recommendations are generating the most Credex leads?
4. Is the HN/community distribution working, or do we need to try paid acquisition?

At the end of Month 3, answer:
1. Can we predict Credex close likelihood from audit data?
2. What is the CAC (cost per acquisition) for a Credex customer via SpendLens?
3. Should SpendLens expand to cover more tools (e.g. Notion AI, Perplexity Pro, Otter.ai)?
