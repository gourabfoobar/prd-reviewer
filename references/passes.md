# PRD Review — Pass Criteria Reference

## Pass 1: Completeness Scan

All 17 sections must be substantively filled out. A section header with placeholder text
("TBD", "coming soon", a single blank row in a table) = **Missing**. Partial content = **Incomplete**.

| # | Section | What "Complete" means |
|---|---------|----------------------|
| 1 | Header | All 7 fields populated: feature name, one-line summary, status, release date, PM, designer, tech lead, Jira epic |
| 2 | Related Documents | OA, Figma, Eng spec, Mixpanel, Helpcenter, PKB, Marketing plan all linked or explicitly noted as N/A |
| 3 | Context from OA | Problem & audience recap, headline impact with dominant source, Atlas OKR + % contribution, updates since OA |
| 4 | Refined Metrics & ROI | Success metrics table (Target vs KPI, benchmarks, linked events), refined effort vs OA, ROI calculations |
| 5 | Requirements | User stories with full Given/When/Then ACs, importance, Jira issue, cross-squad dependency column |
| 6 | Edge Cases & Failure Modes | At minimum: API unavailable, malformed data, multi-country, edge user types, carrier-specific, sandbox vs prod, concurrency |
| 7 | Mixpanel Events Spec | Required + Exploratory events with trigger, properties, linked metric; structural questions answered |
| 8 | QA Strategy | Test approach, beta plan, rollout plan, rollback plan, launch criteria |
| 9 | Design | Figma link, flows covered, design decisions, open questions |
| 10 | Build-time Assumptions | Named assumptions with evidence + consequence if wrong |
| 11 | Decisions Log | Open and resolved questions from scoping |
| 12 | Out of Scope | Explicit exclusions listed |
| 13 | Milestones & Timeline | Beta + GA dates; per-squad dev/QA/go-live dates |
| 14 | Launch Dependencies | Non-code blockers with owner, date, status |
| 15 | Stakeholder Comms Plan | Support, CS, PMM, Sales rows with what/when/format/owner/success signal |
| 16 | Help & Support Articles | Helpcenter + PKB articles with status and owner |
| 17 | Sign-offs | Product, Design, Tech Lead rows present (even if not yet approved) |

---

## Pass 2: Customer Evidence Audit

**What to look for:**

**Context from OA:**
- Is the problem statement specific to a real Easyship segment, or generic?
- Does it cite real evidence (support tickets, NPS verbatims, Gong calls, merchant interviews, data)?
- "We hear this feedback a lot" is not evidence. "47 support tickets in Q1 from SMB merchants" is.

**Headline impact:**
- Is the number grounded with a methodology, or guessed?
- Is the dominant source of impact named explicitly?
- Does the impact figure match what's in the OA, or has it silently changed?

**Updates since OA:**
- If "no material changes" — is that credible? Has enough time passed that something should have changed?
- If changes are noted — do they affect the build case materially?

**Acceptance Criteria:**
- Do ACs reflect real user behaviour? "Given a merchant viewing the shipment list" = good.
- Or do they read like implementation notes? "System calls address validation API endpoint" = bad.
- ACs with "should" instead of "Given/When/Then" = not testable = flag it.

**Evidence Quality Verdicts:**
- **Strong** — cited data, research, or analytics backing the problem and impact
- **Adequate** — some evidence, some inference; case is plausible but not airtight
- **Weak** — mostly assertion; could go either way
- **Unverifiable** — no sources, no data, no way to check the claims

---

## Pass 3: Assumption Challenger

**What to look for:**

**Named assumptions (Build-time Assumptions section):**
- Is there a "why we believe it" and "what changes if wrong" for each one?
- Missing consequence = unacknowledged risk

**Implicit assumptions in Requirements:**
- What does the PRD assume about user behaviour? (e.g., "merchants will prefer inline editing")
- What does it assume about technical capacity? (e.g., "address validation API handles this load")
- What does it assume about business context? (e.g., "pricing unchanged during rollout")

**Edge Cases section:**
- Carrier-specific assumptions — if a carrier-specific behaviour isn't addressed, it's assumed away
- API availability — if the dependent API going down isn't handled, that's a 🔴 assumption
- Multi-country — if a feature changes address/tax/currency behaviour and multi-country isn't addressed, flag it

**Decisions Log:**
- Open questions that should have been resolved before PRD submission are 🟡 or 🔴 risk
- "TBD" or "to be decided" in the Decisions Log = acknowledged; no mention at all = hidden

**Risk ratings:**
- 🟢 **Low** — well-evidenced, low consequence if wrong
- 🟡 **Medium** — partially evidenced or moderate consequence
- 🔴 **High** — not evidenced, or would materially change scope/build if wrong

Always flag all 🔴 and any 🟡 not already acknowledged in the PRD.

---

## Pass 4: Goals & Metrics Stress-Test

**Success metrics table:**

Check each metric against:
1. **Type** — Is it marked Target (held to) or KPI (guardrail)? Both types needed.
2. **Benchmark** — Is the current value populated? A metric without a baseline can't be measured as an improvement.
3. **Target** — Is it specific and achievable, or aspirational and unverifiable?
4. **The gaming test** — Could you hit this metric without solving the user problem? If yes, flag it. Example: "reduce support tickets by 20%" could be gamed by making the feature harder to find.
5. **Linked event** — Every Target metric needs a Required Mixpanel event. If missing: "Metric X has no linked Required event — you're shipping blind on this metric."

**Metric set health:**
- 3–7 metrics is right. Fewer = underspecified. More = no one's tracking all of them.
- Needs at least one adoption metric, one quality/error guardrail, one efficiency or outcome metric.
- All-revenue or all-efficiency metrics without guardrails = flag.

**ROI math (check all three formulas):**
- ROI 1-year = (Annual Impact ÷ Effort) − 1
- ROI 5-year = (5 × Annual Impact ÷ Effort) − 1 [if impact persists — should be stated]
- Payback (months) = (Effort ÷ Annual Impact) × 12
- If the payback period seems implausibly short for the stated complexity, flag it.
- If the 5-year assumption (impact persists 5 years) isn't stated, it's a hidden assumption.

---

## Pass 5: Dependency & Alignment Check

**Requirements table — cross-squad dependencies:**
- Any feature touching: API squad, BFS app, tax/duty engine, carrier APIs, auth/permissions, billing
  → must have the dependency column populated
- "None" is acceptable if genuinely true. Blank = not checked.

**Launch Dependencies:**
- All non-code launch blockers must have owner + target date + status
- Minimum expected: Helpcenter article, CS/Support training, Mixpanel dashboards
- Missing any of these for a merchant-facing feature = flag

**Stakeholder Comms Plan:**
- Support: needs T-1 week minimum for training before a feature that changes any merchant workflow
- CS/Success: needs T-2 weeks for enterprise accounts; T-1 for SMB
- If support has been given less lead time than a week for a merchant-facing change, flag it
- "Sales not relevant" is acceptable — but must be stated, not absent

**Out of Scope:**
- Look for half-built experiences: feature works on Dashboard but not API (or vice versa)?
  Does it work on BFS? If scope excludes a platform the affected users are on, flag it.
- Any exclusion that will create merchant confusion at launch = flag

**Timeline:**
- Squads listed in Development Timeline but not in Requirements dependencies = flag
- Squads listed in dependencies but absent from timeline = flag
- QA end and Go LIVE on the same day = not realistic, flag
