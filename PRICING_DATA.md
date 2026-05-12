# Pricing Data

All prices verified against official vendor pages. Last updated: May 2025.

---

## Cursor

**Source:** https://cursor.com/pricing

| Plan | Price | Notes |
|------|-------|-------|
| Hobby | $0/mo | 2,000 completions, 50 slow requests |
| Pro | $20/seat/mo | Unlimited completions, 500 fast requests, Claude + GPT-4o |
| Business | $40/seat/mo | All Pro features + SSO, admin dashboard, centralized billing |
| Enterprise | Custom | On-prem option, dedicated support |

**Key insight:** Business vs Pro — the only meaningful additions in Business are SSO and admin controls. For teams under 10, these have no practical value.

---

## GitHub Copilot

**Source:** https://github.com/features/copilot#pricing

| Plan | Price | Notes |
|------|-------|-------|
| Individual | $10/seat/mo (or $100/yr) | Code suggestions, chat in IDE, CLI |
| Business | $19/seat/mo | Team management, audit logs, IP indemnity |
| Enterprise | $39/seat/mo | Codebase personalization, fine-tuning, security scanning |

**Key insight:** Codebase personalization (the core Enterprise differentiator) only meaningfully improves suggestions when there are 50+ devs' worth of code patterns to learn from. At smaller scales, Business and Enterprise are functionally identical day-to-day.

---

## Claude (Anthropic)

**Source:** https://claude.ai/pricing

| Plan | Price | Notes |
|------|-------|-------|
| Free | $0 | Limited usage, Claude 3.5 Haiku |
| Pro | $20/seat/mo | 5× more usage vs Free, Claude 3.5 Sonnet/Opus, Projects |
| Max | $100–$200/seat/mo | 5×–20× usage vs Pro, extended thinking |
| Team | $30/seat/mo (min 5 seats) | Shared Projects, admin console, higher rate limits |
| Enterprise | Custom | SSO/SAML, custom context window, SLAs |

**Key insight:** Team plan has an explicit 5-seat minimum. Teams with fewer than 5 users buying "Team" are paying $30 vs the $20 Pro rate for no added value.

---

## ChatGPT (OpenAI)

**Source:** https://openai.com/chatgpt/pricing

| Plan | Price | Notes |
|------|-------|-------|
| Free | $0 | GPT-3.5, limited GPT-4o |
| Plus | $20/seat/mo | GPT-4o priority, DALL-E, advanced data analysis |
| Team | $30/seat/mo (min 2 seats) | Team workspace, higher limits, admin console |
| Enterprise | Custom | SSO, unlimited GPT-4o, custom compliance |

---

## Gemini (Google)

**Source:** https://one.google.com/about/plans, https://workspace.google.com/pricing

| Plan | Price | Notes |
|------|-------|-------|
| Free | $0 | Gemini 1.5 Flash |
| Advanced (Google One AI Premium) | $19.99/seat/mo | Gemini Ultra, 2TB Google storage, Deep Research |
| Workspace Business | $30/seat/mo | Gemini integrated in Workspace apps |

**Key insight:** Advanced bundles 2TB Google storage — if a team is not using the storage, they're effectively paying a $10/seat premium for pure AI access vs Claude/ChatGPT at the same price.

---

## Windsurf (Codeium)

**Source:** https://windsurf.com/pricing

| Plan | Price | Notes |
|------|-------|-------|
| Free | $0 | Unlimited autocomplete, 25 flow credits/mo |
| Pro | $15/seat/mo | 500 flow credits/mo, Claude + GPT-4o, priority models |
| Teams | $35/seat/mo (min 2 seats) | All Pro features + admin dashboard, SSO, custom models |

---

## Anthropic API Direct

**Source:** https://www.anthropic.com/pricing

| Model | Input | Output |
|-------|-------|--------|
| Claude 3.5 Haiku | $0.80 / MTok | $4 / MTok |
| Claude 3.5 Sonnet | $3 / MTok | $15 / MTok |
| Claude Opus 4 | $15 / MTok | $75 / MTok |

---

## OpenAI API Direct

**Source:** https://openai.com/api/pricing

| Model | Input | Output |
|-------|-------|--------|
| GPT-4o mini | $0.15 / MTok | $0.60 / MTok |
| GPT-4o | $2.50 / MTok | $10 / MTok |
| o1 | $15 / MTok | $60 / MTok |

---

## Notes on Audit Rule Thresholds

- **Seat excess threshold:** >1.2× team size. E.g. team of 10 with 13 seats = 3 excess seats flagged.
- **API spend CTA threshold:** >$50/month triggers Credex credit recommendation (conservative — at this level, a 10–15% discount saves real money monthly).
- **High savings threshold:** ≥$500/month. This level triggers the stronger Credex CTA in the UI and the high-savings email variant.
