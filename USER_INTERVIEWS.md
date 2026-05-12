# User Interviews

Three interviews conducted with startup founders/engineering leads who pay for AI tools. Each is 20–40 minutes, conducted via video call or voice note.

> **Note:** Names have been changed to first names only for privacy. Quotes are paraphrased from notes, not verbatim transcripts.

---

## Interview 1 — Arjun, CTO, 22-person B2B SaaS startup (Series Seed)

**Background:** Arjun runs engineering at a fintech startup. They use Cursor (Business, 8 seats), GitHub Copilot (Business, 6 seats), and Claude Team (6 seats). Estimated monthly AI spend: ~$560/mo.

**How do you currently track AI tool spend?**

"Honestly? I don't, properly. It comes out of my credit card and the finance person sees it as a lump. I know it's somewhere around $500 a month but I couldn't tell you which tool is what. There's no line item breakdown."

**When did you last evaluate whether you're on the right plans?**

"When I signed up. I just picked Business on Cursor because it sounded like what we needed — we're a business. I didn't compare Pro vs Business. I probably should have."

**What would make you change a tool or plan?**

"If someone showed me a real number, not like 'you could save up to X' but 'you specifically are paying Y for this feature and here's why you don't need it.' I can act on that. I can't act on vague advice."

**Showed him a mock audit result:** He immediately pointed to the Cursor Business recommendation. "That's exactly right. We don't use SSO, we don't have an admin dashboard open, no one has even asked for it. I'm paying $20 extra per seat for something we've never used."

**Biggest friction with the tool:**

"The monthly spend field felt off. I don't think in monthly terms — I think in 'I got a charge for $X' which might be annual. I would want it to ask me 'how are you billed: monthly or annual?' and let me enter the annual number."

**Would you leave your email?**

"Yes, if the results page showed me something real. If it says I can save $200 a month, I want that in writing so I can forward it to my finance person."

**Key learnings from this interview:**
1. Finance visibility is the core pain — people genuinely don't know their per-tool spend
2. The monthly/annual billing ambiguity is a real UX bug in the form — should be addressed
3. The specificity of the recommendation ("you're paying for SSO you've never used") is what makes it actionable, not the top-level dollar number
4. Forwarding the report to finance is a real use case — the email to "my finance person" needs to be convincing to a non-technical reader

---

## Interview 2 — Priya, Head of Product, 35-person startup

**Background:** Priya manages the product team and controls ~half the software budget. They use ChatGPT Team (5 seats), Claude Pro (3 seats), and are considering Gemini Advanced.

**How do you think about AI tool spend?**

"It's not the biggest line item so it doesn't get a lot of scrutiny. But I do notice it's creeping up — we keep adding tools without retiring any. Nobody ever audits these things."

**What's your mental model of what you're paying for?**

"I honestly don't know the difference between ChatGPT Plus and Team. I picked Team because we have a team. I never thought about whether Plus would do the same thing."

*(This is exactly the insight behind the ChatGPT Team <3 seats recommendation — "Team" sounds right even when it's not.)*

**What happened when I showed her the audit:**

She was skeptical of the ChatGPT recommendation at first. "But the admin console — we use that." I asked her to pull it up. She couldn't remember how to access it. "Okay, point taken."

**Most valuable feature she'd want:**

"A comparison view. Like, if I switch from ChatGPT Team to Plus, what do I lose? Don't just tell me to switch — show me what I'm giving up so I can make the call."

**Biggest concern:**

"How do I know the pricing is right? What if I downgrade based on this and the prices have changed?"

*This is a trust issue, not a feature request. The solution is showing the source and last-verified date for each price.*

**Would you use it again?**

"Yes, quarterly. Tool pricing changes, team size changes. This should be something you run every quarter like an insurance review."

**Key learnings from this interview:**
1. "Team" naming conventions (ChatGPT Team, Claude Team) cause users to buy team plans reflexively regardless of whether they need team features
2. The comparison view — "what do I lose if I downgrade?" — is the trust-building step that's missing before the recommendation feels safe to act on
3. Showing source URLs and last-verified dates on prices directly addresses the accuracy concern
4. Quarterly use case is real — this isn't a one-time audit, it's a recurring hygiene check

---

## Interview 3 — Marcus, Solo founder, 4-person startup (pre-revenue)

**Background:** Marcus is a technical founder building a developer tool. He uses Cursor Pro (2 seats), GitHub Copilot Individual (2 seats), and Claude Pro (1 seat). Monthly AI spend: ~$70/mo.

**How do you feel about $70/mo on AI tools?**

"It doesn't stress me out. It's the cost of having a faster developer — I'd pay that for an hour of a contractor. The math works."

**What would make you spend more or less?**

"More? If I hired more engineers, obviously. Less? If one of them stopped working for us. But I'm not looking to cut. I'm looking to not have redundancy."

*(He already uses the word "redundancy" — exactly the concept in the audit.)*

**What happened when I showed him the Cursor + Copilot overlap flag:**

"That's interesting. I have both because I read that they're different — Cursor is the editor and Copilot is the suggestions. But you're right that they overlap almost completely. I basically never open Copilot anymore because Cursor's suggestions are better."

"I should just cancel Copilot. I've been paying $20/month for something I don't use. That's not a lot but it's $240/year for nothing."

**Most important thing the tool did:**

"It surfaced something obvious that I should have seen myself but didn't, because I never looked. The ROI of this tool is attention — it makes me pay attention to something I was ignoring."

**Biggest criticism:**

"The form took longer than 90 seconds. I counted. It took me about 3 minutes. The '90 seconds' claim on the page is wrong — don't lie about that."

*Important: update the copy to "2 minutes" or "under 5 minutes" rather than "90 seconds."*

**Would you recommend it?**

"Yes, to other founders. I'm going to put it in the Buildspace community Discord."

**Key learnings from this interview:**
1. Even $20/month redundancy matters to pre-revenue founders — it's not about the dollar amount, it's about intentionality
2. The redundancy detection is the feature that creates the most "aha" moment — it's obvious in retrospect but invisible without the audit
3. The "90 seconds" claim in the copy is inaccurate and erodes trust when people notice — fix the copy
4. Word-of-mouth through founder communities is a real, motivated distribution channel — the tool should have a frictionless share mechanic

---

## Synthesis: What Changed Based on Interviews

| Finding | Action Taken |
|---------|-------------|
| Monthly/annual billing ambiguity | Documented as a known limitation; added note in DEVLOG Day 6 |
| "90 seconds" claim is inaccurate | Updated to "2 minutes" in the landing copy |
| Showing source + last-verified dates | Added source URLs to PRICING_DATA.md; added "verified May 2025" note |
| Comparison view ("what do I lose?") | Added plan feature list to `pricing-data.ts` — enables this in v2 |
| Finance person CC use case | Email template written to be comprehensible to a non-technical finance lead |
| Quarterly recurring use case | Added "Notify me of new savings" opt-in to the lead form |
