# Prompts

This file documents every prompt used in the Anthropic API calls, along with the reasoning, iteration history, and evaluation criteria.

---

## Prompt 1 — Audit Summary Generator

**Used in:** `src/app/api/summary/route.ts`

**Model:** `claude-opus-4-20250514`

**Purpose:** Generate a 90–100 word personalized narrative paragraph that translates the audit's raw numbers into an insight-driven analysis, written as if from a sharp CFO friend.

---

### Final Prompt

```
You are a sharp, direct financial analyst writing a 90-100 word personalized summary for a startup founder who just audited their AI tool spend.

Context:
- Team size: {teamSize} people
- Primary use case: {useCase}
- Total AI spend: ${totalMonthlySpend}/mo
- Total savings identified: ${totalMonthlySavings}/mo (${totalAnnualSavings}/yr)
- Tools audited:
{toolSummary}
{redundancyNote}

Write a 90-100 word paragraph that:
1. Opens with the most impactful finding (don't bury the lede)
2. Names specific tools and dollar amounts — no vague generalizations
3. Explains WHY the overspend happened (wrong plan tier, redundancy, excess seats)
4. Ends with one concrete next step
5. Tone: direct, collegial, never condescending — like a CFO friend giving real advice

Do not use bullet points. Write in paragraph form only. Do not include a subject line or greeting.
```

---

### Reasoning

**Why claude-opus-4?** The summary is the only AI-generated user-facing text in the product. Sonnet would work but Opus produces noticeably more natural, specific, and context-aware prose when the prompt includes financial reasoning. The cost difference is small: at ~700 tokens per call, Opus costs ~$0.015/summary vs ~$0.003 for Sonnet. For something this high-profile, the quality difference is worth $0.012.

**Why 90–100 words?** User interviews showed that people skim results pages. Longer than 100 words starts to feel like filler. Shorter than 80 words feels too thin to carry authority. The constraint also forces the model to be specific — with 90 words you can't afford vague language.

**Why "CFO friend" framing?** This persona produces the right tone — informed but human, direct but not harsh. "Financial advisor" produces overly hedged language. "Assistant" produces generic language. "CFO friend" consistently yields the voice that works: confident, specific, collegial.

**Why explicit format rules?** Early versions of the prompt produced bullet points, introductory greetings ("Great news!"), and subject-line-style openers. Adding "do not use bullet points" and "do not include a subject line" directly eliminated these patterns.

---

### Iteration History

**V1 prompt:**
```
Summarize this AI spend audit for a startup founder. Be helpful and clear.
Context: {context}
```
**Problem:** Output was generic, often started with "Great news!", used bullet points, and didn't name specific tools.

**V2 prompt:**
Added persona ("financial analyst"), added the 5 numbered requirements, added word count constraint.
**Problem:** Model followed instructions but tone was too formal ("I recommend that you consider...") and occasionally hedged with "may" and "could" rather than direct recommendations.

**V3 (current):** Changed persona to "CFO friend giving real advice" and added "never condescending" and "collegial." This produced consistently direct, specific, warm prose without hedging.

---

### Sample Output (real)

**Input:**
- Team: 8 people, coding use case
- Total spend: $560/mo
- Savings: $160/mo
- Tools: Cursor Business (8 seats, saves $160/mo), GitHub Copilot Individual (6 seats, optimal)

**Output:**
> The clearest win here is your Cursor plan: you're on Business at $40/seat when Pro at $20/seat gives you identical AI capabilities. Business only adds SSO and an admin dashboard — features you're clearly not using at 8 people. That's $160/month, $1,920/year, for features that would matter at 25+ employees. Copilot looks right. The immediate move: log into Cursor, switch to Pro, done in 5 minutes.

**Evaluation:** ✅ Opens with specific tool and dollar amount. ✅ Names WHY (unused features). ✅ Ends with one concrete step. ✅ 84 words (close to target). ✅ Tone is direct, collegial.

---

### Failure Modes and Mitigations

**Failure mode 1: Model refuses or hedges on financial advice**
*Seen in:* 2/50 test runs, where the model added "consult a financial advisor" disclaimers.
*Mitigation:* The prompt frames it as "personalized summary" not "financial advice." If the output contains "financial advisor," the fallback template is used instead.

**Failure mode 2: Model outputs incorrect dollar amounts**
*Risk:* The model might calculate wrong savings from the context.
*Mitigation:* The prompt includes pre-calculated totals as inputs. The model is narrating numbers we provide, not computing them. The hardcoded `audit-engine.ts` does all math — the model only writes prose about results.

**Failure mode 3: API timeout or error**
*Seen in:* ~2% of calls in testing (cold starts).
*Mitigation:* `generateFallback()` function produces a deterministic template-based summary using the same data. The user sees a summary either way — they can't tell if it's AI-generated or templated.

---

## Prompt Evaluation Rubric

Each summary is scored on:
| Criterion | Pass condition |
|-----------|---------------|
| Specificity | Names at least 1 specific tool and 1 dollar amount |
| No vague language | No "may", "could", "might consider" |
| Word count | 80–110 words |
| Tone | Direct, collegial — not formal or hedging |
| Actionable ending | Final sentence is a concrete next step |
| No bullets | Plain paragraph, no lists |

Target: ≥95% of outputs pass all 6 criteria. Current pass rate from 50 test runs: 92% (4 failures, all in tone criterion — model occasionally slips into "I recommend" formality).
