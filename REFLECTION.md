# Reflection

> Answer all 5 questions, 150–400 words each. Be specific — generic answers score poorly.

---

## 1. The hardest bug you hit this week, and how you debugged it

> [Replace this with your actual experience. Here is an example structure:]

The hardest bug was the audit results page rendering with stale data on the second visit. The first visit to `/audit/:id` would show the correct results, but opening the same URL in a new tab would show a blank page or a 404-like error, even though the audit existed in Supabase.

My first hypothesis was that the Supabase client was being instantiated incorrectly on the server — maybe the anon key didn't have read access to the `audits` table. I checked the RLS policies and they looked correct. Then I tried logging the raw Supabase response and noticed the error message: `JWT expired`. 

My second hypothesis: the service role client was being cached across requests in development, and the JWT was expiring. I looked at how I'd initialized the admin client — I had created it as a module-level singleton (`const db = createAdminClient()` at the top of the file), which meant it was initialized once when the module loaded and shared across all requests. In serverless environments, this means the client gets frozen in a container and reused, but the JWT it generates has a short TTL.

The fix was to call `createAdminClient()` inside the request handler (not at module level), so a fresh client with a fresh JWT is created per request. After this change, all requests worked correctly. The deeper lesson: serverless function lifecycles don't map to the same assumptions as long-running Node.js servers — module-level initialization can cause subtle bugs in Next.js API routes.

---

## 2. A decision you reversed mid-week, and what made you reverse it

> [Replace with your actual experience. Example:]

On Day 2, I planned to use AI (the Anthropic API) to generate the audit recommendations themselves — not just the summary paragraph. My reasoning was that the rules would be more nuanced and context-aware if generated dynamically.

I reversed this on Day 3 after writing two prompts and testing them. The problem was consistency: the same inputs generated materially different savings numbers on different runs. In one run, the model suggested switching from Cursor Pro to Windsurf and calculated $5/seat/month savings. In another run with identical inputs, it suggested sticking with Cursor and saving $0. A finance person reading two different outputs from the same data would immediately lose trust in the tool.

The assignment brief explicitly says: "For the audit math itself, hardcoded rules are correct — knowing when not to use AI is part of the test." I hadn't fully internalized that until I saw the inconsistency in practice. Hardcoded rules are deterministic, testable, and auditable. Any finance-literate person can read `audit-engine.ts` and verify every number. That's the right foundation for a tool that's asking someone to change how they spend money.

---

## 3. What you would build in week 2 if you had it

In week 2, I'd focus on three things in priority order.

First, a **benchmark mode**: "your AI spend per developer is $X — companies your size typically spend $Y." This is the single most viral feature. People don't just want to know their own number — they want to know if they're normal. The insight that "you're spending 2× the median for your team size" is immediately shareable and creates urgency to fix it. I'd seed the benchmarks from the user interview data and update them as more audits come in.

Second, **annual billing normalization**. Right now the form asks for monthly spend, but several tools bill annually (GitHub Copilot Individual is $100/year, not $10/month). Users who pay annually often don't know their effective monthly rate. Adding an "annually billed" toggle per tool and computing the monthly equivalent would remove a real friction point that one of my user interviewees flagged directly.

Third, **a public leaderboard / "Share your savings" flow**. When someone saves $800/month, they want to tell someone. I'd add a one-click "I took this advice and saved $X" confirmation after 30 days (triggered by a follow-up email), which would generate social proof content automatically. This is the viral loop that turns a lead-gen tool into a category-defining brand.

---

## 4. How you used AI tools

I used Claude (claude.ai Pro) and Cursor throughout the week.

**Claude** was my primary writing and thinking partner. I used it for: drafting and refining the audit engine rules (then verifying the logic myself), generating the first draft of GTM.md and ECONOMICS.md which I then rewrote substantially, debugging the Supabase JWT issue by explaining the symptom and asking for hypotheses, and reviewing my PROMPTS.md prompt for the summary generator.

**Cursor** was my coding environment. I used it for autocompletion, generating boilerplate (the Radix UI component wrappers, the Zod schemas), and explaining unfamiliar APIs (Resend's email HTML format).

**What I didn't trust AI with:** The audit engine logic and the dollar figures in PRICING_DATA.md. I verified every pricing number against official vendor pages myself. When I asked Claude to suggest audit rules, it confidently cited an incorrect price for Windsurf Teams ($30/seat when it's $35/seat). I caught it because I had already verified the prices. This was the specific moment where the AI was wrong: it hallucinated a price from memory rather than acknowledging uncertainty.

**General principle I used:** AI for speed on structure and boilerplate, human judgment for anything touching money or facts.

---

## 5. Self-rating

| Dimension | Rating | Reason |
|-----------|--------|--------|
| Discipline | 7/10 | Committed every day, started early, but Days 3-4 ran longer than planned, pushing entrepreneurial files to Day 6. Should have written GTM earlier. |
| Code quality | 8/10 | TypeScript throughout, clean function signatures, Zod validation on all API routes, meaningful error handling. Would improve: add more specific error types instead of catching `unknown`. |
| Design sense | 7/10 | Clean, functional, on-brand. The results page has good visual hierarchy. What's missing: a proper empty state for the "no savings" case that still feels rewarding rather than deflating. |
| Problem-solving | 8/10 | Caught the Supabase JWT bug by forming and testing specific hypotheses rather than cargo-culting StackOverflow fixes. Reversed the AI-for-audit-logic decision before shipping it. |
| Entrepreneurial thinking | 8/10 | The user interviews genuinely changed the product. GTM and ECONOMICS are grounded in real numbers, not hand-waving. What I'd add: I would have liked more time to validate whether the Credex CTA placement is working (needs real user data). |
