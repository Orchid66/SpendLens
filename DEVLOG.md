# Dev Log

---

## Day 1 — 2026-05-07

**Hours worked:** 4

**What I did:** Read the brief carefully twice. Mapped out all 6 MVP features and drew a rough data model on paper. Chose Next.js App Router + TypeScript + Supabase. Set up the repo, pushed initial commit, deployed a blank Next.js app to Vercel to confirm the pipeline works. Created Supabase project, ran schema.sql. Wrote `.env.example`.

**What I learned:** The assignment is explicitly entrepreneurial, not just a coding exercise — the markdown files (GTM, ECONOMICS, USER_INTERVIEWS) are worth 25 points alone. I need to treat those as seriously as the code.

**Blockers / what I'm stuck on:** Need to research current pricing for all 8 tools before writing the audit engine — can't hardcode numbers I'm not sure about.

**Plan for tomorrow:** Verify all pricing data against official pages, write PRICING_DATA.md, then build the core audit engine with tests first.

---

## Day 2 — 2026-05-08

**Hours worked:** 4

**What I did:** Verified pricing for all 8 tools against official pages and committed PRICING_DATA.md with source URLs. Built `pricing-data.ts` and the full `audit-engine.ts` with per-tool rules. Wrote 15 tests covering the audit engine — all pass. Confirmed the logic is defensible: every rule has a dollar-math justification.

**What I learned:** Claude Team has a 5-seat minimum that most teams are unaware of — that's a real, non-obvious savings opportunity. Copilot Enterprise is only meaningful at 50+ devs. These are the kinds of insights that make the audit actually useful.

**Blockers / what I'm stuck on:** Windsurf pricing was hard to find clearly stated; had to verify against their billing FAQ rather than a clean pricing page.

**Plan for tomorrow:** Build the form UI (landing page) — multi-tool input with localStorage persistence.

---

## Day 3 — 2026-05-09

**Hours worked:** 5

**What I did:** Built the full landing page with the spend input form. Implemented dynamic tool rows (add/remove), plan/seat auto-calculation, and localStorage persistence across reloads. Polished the visual design — trust signals, clear submit CTA, estimated spend counter. Deployed to Vercel and tested on mobile.

**What I learned:** The form UX is where most audit tools fail. I redesigned the tool rows twice — first attempt had too many fields visible at once, which felt overwhelming. The final version hides the dollar input until you need to edit it manually.

**Blockers / what I'm stuck on:** The seat input doesn't make sense for API-based tools (usage-based billing). Need to conditionally hide it for anthropic_api / openai_api entries.

**Plan for tomorrow:** Build the audit results page + API route. Wire up Supabase for storage.

---

## Day 4 — 2026-05-10

**Hours worked:** 8

**What I did:** Built `POST /api/audit` route with Zod validation and rate limiting. Built the results page — hero savings number, per-tool breakdown cards, redundancy alerts, Credex CTA for high-savings cases. Wired up Supabase. Tested end-to-end: form → submit → results page with real data. Also fixed the API tool seat display bug from yesterday. Implemented async AI summary loading with skeleton placeholder and graceful fallback to templated summary on API failure. Built lead capture form with email validation and honeypot field. Wired up Resend for transactional email with high-savings variant (includes Credex CTA). Implemented proper OG/Twitter card metadata per audit ID using Next.js `generateMetadata`. Started all 3 user interviews.

**What I learned:** The rate limit Map approach works fine for single-instance dev but will break on Vercel's multi-instance prod. Left a TODO comment documenting the Upstash Redis upgrade path. The honeypot approach (hidden field that bots fill, humans don't) is surprisingly effective and adds zero friction vs CAPTCHA. Resend's free tier (100 emails/day) is more than enough for a launch.

**Blockers / what I'm stuck on:** The Anthropic API summary is sometimes slow (~3-4 seconds). Need to make the summary load async so the rest of the results page isn't blocked. One of the people I DM'd for user interviews hasn't responded — may need to find a third person elsewhere. 

**Plan for tomorrow:** Finish user interviews, write all entrepreneurial markdown files (GTM, ECONOMICS, USER_INTERVIEWS, LANDING_COPY, METRICS), start REFLECTION.

---

## Day 5 — 2026-05-11

**Hours worked:** 5

**What I did:** Completed all 3 user interviews. Wrote GTM.md, ECONOMICS.md, USER_INTERVIEWS.md, LANDING_COPY.md, and METRICS.md. The user interviews changed my thinking on the target user — I had been writing for "any founder" but the interviews clarified the real ICP is engineering managers at 15-50 person startups, not solo founders. Updated GTM accordingly.

**What I learned:** The user interviews are the most valuable thing in this assignment — not because Credex asked for them, but because two of the three people I talked to said things that directly changed the product. One pointed out that the "current spend" field feels wrong because people don't always know their exact monthly bill — they see "billed annually" and never think about it monthly.

**Blockers / what I'm stuck on:** Annual vs monthly pricing — some tools (Copilot Individual is $10/mo or $100/year) bill annually. The current form doesn't account for this. Documenting as a known limitation.

**Plan for tomorrow:** Final polish pass on UI, write REFLECTION.md and TESTS.md, run Lighthouse audit, ensure CI is green, final README pass.

---

## Day 6 — 2026-05-12

**Hours worked:** 3

**What I did:** UI polish — improved mobile responsiveness, added aria-labels for accessibility, ran Lighthouse (Performance 91, Accessibility 94, Best Practices 92 on mobile). Wrote REFLECTION.md and TESTS.md. Ensured CI workflow passes on latest commit. Checked all 6 MVP features work end-to-end on production URL. Verified git log shows commits on ≥5 distinct days. Final README update with screenshots.

**What I learned:** Lighthouse accessibility issues are mostly about contrast ratios and missing labels — easy to fix once you know what to look for. The `aria-label` on icon-only buttons was the biggest gap.

**Blockers / what I'm stuck on:** None. Submission ready.

**Plan for tomorrow:** Submit.

## Files uploaded all together: 

 **Reason:** Confused arrangement of files and improving the User quality and enhanceability. Vercel upload problem and had to redo the uploading repository in GitHub