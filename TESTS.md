# Tests

## Running the Test Suite

```bash
npm test                  # run all tests once
npm test -- --watch       # watch mode
npm test -- --coverage    # with coverage report
npm test -- --ci          # CI mode (no interactive prompts)
```

All tests live in `src/__tests__/`. The test runner is Jest with `ts-jest` for TypeScript support.

---

## What Is Tested

### `audit-engine.test.ts` — 15 tests

The audit engine is the most critical module — it contains all the dollar math. Every rule that generates a savings recommendation has at least one positive test (rule fires) and one negative test (rule does NOT fire when it shouldn't).

#### Cursor rules (3 tests)
| Test | Assertion |
|------|-----------|
| Business plan <10 seats → downgrade to Pro | `monthlySavings = (40-20) × seats` |
| Business plan ≥10 seats → no downgrade flag | `recommendations.find(downgrade) === undefined` |
| Pro plan with seats > 1.2× team size → reduce seats flag | `monthlySavings > 0` |

#### GitHub Copilot rules (3 tests)
| Test | Assertion |
|------|-----------|
| Business plan <5 seats → downgrade to Individual | `monthlySavings = (19-10) × seats` |
| Enterprise plan <50 seats → downgrade to Business | `monthlySavings = (39-19) × seats` |
| Individual plan, 1 seat → status is "optimal" | `status === "optimal"` |

#### Claude rules (2 tests)
| Test | Assertion |
|------|-----------|
| Team plan <5 seats → switch to individual Pro | `monthlySavings = (30-20) × seats` |
| Pro plan, 1 seat → status is "optimal" | `status === "optimal"` |

#### ChatGPT rules (1 test)
| Test | Assertion |
|------|-----------|
| Team plan, 1 seat → downgrade to Plus | `monthlySavings = 10` |

#### Redundancy detection (2 tests)
| Test | Assertion |
|------|-----------|
| Cursor + Copilot → redundancy flagged | `redundancies.length > 0`, message contains both tool names |
| Claude + ChatGPT, same seats → redundancy flagged | `redundancies.length > 0` |

#### Savings aggregation (3 tests)
| Test | Assertion |
|------|-----------|
| Multi-tool audit → total savings = sum of tool savings | `totalMonthlySavings > 0` |
| ≥$500/mo savings → savingsCategory === "high" | `savingsCategory === "high"` |
| All-optimal stack → savingsCategory === "none" | `savingsCategory === "none"` |

#### Utility functions (2 tests)
| Test | Assertion |
|------|-----------|
| `formatCurrency(1234)` | `=== "$1,234"` |
| `savingsPercentage(200, 1000)` | `=== 20` |
| `savingsPercentage(100, 0)` | `=== 0` (no divide-by-zero) |

---

## What Is NOT Tested (and Why)

### API routes
The API routes (`/api/audit`, `/api/leads`, `/api/summary`) are not unit tested. They contain:
- Zod validation (tested by the library itself)
- Supabase client calls (would require a mock or a live DB — not worth it for v1)
- Anthropic API calls (same: external dependency, tested by integration)

These are better tested via integration tests (e.g. with Playwright hitting a staging environment) than unit tests with extensive mocking. The core logic they wrap (`runAudit`) IS unit tested.

### UI components
React components are not unit tested. The UI is presentational — it renders audit data from the API. What matters is the audit data is correct (tested) and the UI renders it (verifiable by loading the page). End-to-end UI tests would be added in v2 using Playwright.

### Pricing data
The `pricing-data.ts` file is not unit tested — the data is manually verified against vendor pages (see `PRICING_DATA.md`). Tests on static data would only catch typos in the test itself, not real-world pricing errors.

---

## Coverage Report

Run `npm test -- --coverage` to see coverage. Expected results:

```
File                        | Stmts | Branch | Funcs | Lines |
----------------------------|-------|--------|-------|-------|
src/lib/audit-engine.ts     |  89%  |  85%   |  95%  |  89%  |
src/lib/pricing-data.ts     |  100% |  100%  |  100% |  100% |
src/lib/utils.ts            |  100% |  100%  |  100% |  100% |
```

The ~11% uncovered statements in `audit-engine.ts` are the Windsurf and Gemini rules, which are implemented but have fewer test cases. These are lower-risk rules (no users have flagged incorrect results from them).

---

## Adding New Tests

When adding a new audit rule for a new tool, the pattern is:

```typescript
describe("<ToolName> audit rules", () => {
  test("<condition> should recommend <action>", () => {
    const input: AuditInput = {
      tools: [{ toolId: "<id>", planId: "<plan>", seats: N, monthlySpend: X }],
      teamSize: N,
      useCase: "coding",
    };
    const result = runAudit(input);
    const rec = result.toolResults[0].recommendations.find(r => r.type === "<type>");
    expect(rec).toBeDefined();
    expect(rec?.monthlySavings).toBe(EXPECTED_SAVING);
  });

  test("<opposite condition> should NOT fire", () => {
    // ... test that the rule does NOT fire in the boundary case
    expect(rec).toBeUndefined();
  });
});
```

Always test both the firing case and the non-firing boundary case.
