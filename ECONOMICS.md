# Unit Economics

## Cost to Run SpendLens

### Infrastructure (monthly, steady state)

| Item | Cost | Notes |
|------|------|-------|
| Vercel Pro | $20/mo | Needed for serverless function timeout >10s on the AI summary call. Free tier might work at launch. |
| Supabase Free | $0/mo | 500MB database, 50,000 monthly active users. Sufficient for first 6 months. |
| Supabase Pro (when needed) | $25/mo | Upgrade trigger: >500MB storage or >500 concurrent connections |
| Anthropic API (summaries) | ~$0.015/audit | claude-opus-4: ~500 tokens input + 200 tokens output = ~700 tokens × $15/MTok ≈ $0.01. Rounding up for retries. |
| Resend (email) | $0/mo | Free tier: 100 emails/day, 3,000/month. Upgrade at $20/mo for 50,000 emails. |
| **Total at launch** | **~$20–45/mo** | Vercel + Anthropic API at ~1,000 audits/mo |

### Anthropic API cost at scale

At 10,000 audits/month:
- 10,000 × $0.015 = **$150/month** on AI summaries
- Optimization: cache summaries in the `audits` table after first generation → ~85% cache hit rate at scale → effective cost ~$22/month

**Total cost at 10k audits/month:** ~$200–250/mo (Vercel Pro + Supabase Pro + Anthropic + Resend)

---

## Lead Funnel Model

Based on comparable free audit/calculator tools in the SaaS ecosystem:

| Stage | Rate | At 1k audits | At 10k audits |
|-------|------|-------------|--------------|
| Audits run | 100% | 1,000 | 10,000 |
| Email captures | 12–18% | 150 | 1,500 |
| Opens email | 45% | 67 | 675 |
| Click → Credex landing | 15% | 22 | 225 |
| Book consultation call | 30% | 7 | 67 |
| Close as Credex client | 20% | 1–2 | 13–14 |

**Assumptions:**
- Email capture rate of 15% is conservative for tools that show meaningful savings (>$200/mo) — urgency drives capture
- High-savings leads (≥$500/mo) are segmented for direct outreach, converting at higher rates (est. 40% consultation rate, 30% close rate)

---

## Revenue Attribution to Credex

Credex's business is selling discounted AI infrastructure credits. Typical deal structure:
- SMB: $5,000–$20,000 credit purchase (Credex margin ~20% → $1,000–$4,000 gross profit)
- Growth stage: $20,000–$100,000 (Credex margin ~15% → $3,000–$15,000 gross profit)

**Conservative model at 1k audits/month:**
- 1–2 closes/month
- Average deal: $10,000 credits → ~$2,000 gross profit
- SpendLens contribution: 1 × $2,000 = **$2,000 gross profit/month**
- SpendLens cost: $45/month
- **ROI: ~44×**

**Conservative model at 10k audits/month:**
- 13–14 closes/month
- Average deal: $10,000 → $2,000 gross profit
- SpendLens contribution: ~14 × $2,000 = **$28,000 gross profit/month**
- SpendLens cost: $250/month
- **ROI: ~112×**

---

## Key Assumptions & Risks

**Assumption 1: 15% email capture rate**
*Risk:* If savings are too low for most users (e.g. the average user is actually well-optimized), the urgency to leave an email drops and capture rate falls to 5–8%. The form should track this by cohort.

**Assumption 2: 20% close rate on consultation calls**
*Risk:* Credex's sales process is the variable we can't control from SpendLens. If Credex can't close at this rate, the attribution model breaks. Mitigation: track lead quality metrics (savings amount, team size) to filter for high-intent leads before outreach.

**Assumption 3: Pricing data stays accurate**
*Risk:* Tool pricing changes frequently. Stale pricing data means wrong recommendations → erodes trust → reduces email captures. Mitigation: quarterly pricing audit; alerts in the audit engine source file with last-verified dates.

**Assumption 4: SpendLens doesn't cannibalize Credex**
*Risk:* If users optimize their spend based on SpendLens alone (without buying Credex credits), Credex gets less revenue, not more. Mitigation: SpendLens doesn't surface the API credit discount angle until after showing plan-level savings — the funnel is additive, not substitutive.

---

## Break-Even

SpendLens breaks even (covers its own infra costs) if it generates **1 Credex deal ≥$200 gross profit** per month. At $45/mo in infra costs and a 20% Credex margin, that means 1 deal × $1,000 credit sale covers all infra. This happens with fewer than 1,000 audits/month at current conversion assumptions.

SpendLens is economically justified from day one if it closes even a single deal.
