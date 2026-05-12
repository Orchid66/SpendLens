# Architecture

## System Diagram

```mermaid
graph TD
    A[User — Browser] -->|GET /| B[Next.js: Landing Page + Form]
    B -->|POST /api/audit| C[Audit API Route]
    C --> D[audit-engine.ts\nPure hardcoded rules]
    C --> E[(Supabase\naudits table)]
    C -->|returns audit ID| B
    B -->|redirect| F[/audit/:id]
    F -->|Server fetch| E
    F --> G[AuditResultsClient\nClient Component]
    G -->|POST /api/summary| H[Summary API Route]
    H --> I[Anthropic API\nclaude-opus-4]
    H -->|fallback| J[Template summary]
    G -->|POST /api/leads| K[Leads API Route]
    K --> L[(Supabase\nleads table)]
    K --> M[Resend\nTransactional email]
```

## Data Flow

1. **User fills form** on `/` → selects tools, plans, seat counts, team size, use case. Form state persists in `localStorage`.

2. **Submit hits `POST /api/audit`**:
   - Validates input with Zod
   - Calls `runAudit()` from `audit-engine.ts` — pure function, no I/O, deterministic
   - Stores result JSON in Supabase `audits` table with a `nanoid(10)` primary key
   - Returns `{ id, result }` to the browser

3. **Browser redirects** to `/audit/:id`.

4. **Results page** (`/audit/[id]/page.tsx`) is a **Server Component** — it fetches the audit from Supabase server-side, so the page renders with full data and correct OG tags for sharing.

5. **Client component** (`AuditResultsClient.tsx`) renders the interactive parts and kicks off a `POST /api/summary` call to get the AI-generated narrative paragraph. This is async and degrades gracefully to a template.

6. **Lead capture** hits `POST /api/leads`, which stores the email in Supabase and fires a Resend transactional email. The honeypot field (`name="website"`) silently drops bot submissions.

## Stack Choice

| Layer | Choice | Why |
|-------|--------|-----|
| Framework | Next.js 14 (App Router) | Per-route metadata for OG tags; server components for SEO; fast Vercel deploys |
| Language | TypeScript | Type-safe audit engine is critical — bugs in money math are visible in types |
| Styling | Tailwind + Radix UI | No pre-built templates; utility-first for speed; Radix for accessible primitives |
| Database | Supabase (Postgres) | Free tier, row-level security, instant REST/JS client, SQL editor for schema |
| AI | Anthropic (claude-opus-4) | Preferred in brief; best instruction-following for structured prompts |
| Email | Resend | Clean API, generous free tier, React email templates |
| Deploy | Vercel | Native Next.js; preview URLs for every PR; instant edge network |

## Scaling to 10k Audits/Day

Current architecture would need these changes:

1. **Rate limiting**: Replace in-memory `Map` with Upstash Redis + sliding window. The current approach breaks under multiple server instances (Vercel scales horizontally). ~30min change.

2. **Database**: Supabase free tier has a 500MB storage limit. At 10k audits/day with ~5KB per audit, that's ~50MB/day — 10 days to hit the limit. Upgrade to Supabase Pro ($25/mo) or add a TTL-based cleanup job for old audits (most are read once, within 24hrs of creation).

3. **AI summary**: The `/api/summary` call adds latency on the results page. Move it to a background job triggered after audit creation (Supabase Edge Functions or a Vercel background function). Pre-compute and cache the summary in the `audits` row.

4. **CDN for share pages**: Audit result pages at `/audit/:id` are static after creation. Add `revalidate: false` and use ISR or a CDN cache with a `Cache-Control: public, max-age=31536000` header — audit data never changes.
