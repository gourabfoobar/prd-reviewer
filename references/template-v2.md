# Easyship PRD Template v2 — Section Guide

This is the authoritative template all Easyship PRDs must follow (as of June 2026).

## Key changes from v1

1. Sections duplicating the OA collapsed into a "Context from OA" recap
2. Acceptance Criteria column added to Requirements table (Given/When/Then required)
3. New Edge Cases & Failure Modes section
4. Tracking promoted to a first-class Mixpanel Events Spec (Required vs Exploratory, PM-owned)
5. New QA Strategy section
6. Cross-team Collaboration split: Launch Dependencies (in PRD) + Marketing plan (separate doc)
7. New Stakeholder Communications Plan section
8. FAQ + Open Questions merged into Decisions Log
9. Architecture section removed (lives in eng spec)
10. ROI math corrected with explicit formulas
11. "User interaction and design" replaced with Figma-led Design section
12. Retrospective Analysis section removed

## Section-by-section guidance

### Header
Required fields: Feature Name, One-line Summary (one sentence, non-product reader can understand),
Document Status, Expected Release Date, PM, Product Designer, Tech Lead, Jira Epic.

One-line summary format: "What it is and who it's for." Not a feature description — a human sentence.

### Related Documents
Link to: OA, Feature Overview, Figma, Eng spec, Mixpanel dashboards, Helpcenter, PKB page, Marketing plan.
Do not duplicate content from these docs inside the PRD — link out.

### Context from OA
- **Problem & audience**: 2–3 sentences. Who has the problem, what is it, benefit if solved.
- **Headline impact**: 1 sentence. Total annual impact + dominant source.
- **Atlas OKR & contribution**: Which OKR, % of target. Copied from OA.
- **Updates since OA approval**: What changed during scoping. If nothing: "No material changes."

### Refined Metrics & ROI
Success metrics: max 7, min 3. Mark each as Target or KPI. Include current benchmark value.

ROI formulas (all three required):
- ROI 1-year = (Annual Impact ÷ Effort) − 1
- ROI 5-year = (5 × Annual Impact ÷ Effort) − 1
- Payback (months) = (Effort ÷ Annual Impact) × 12

Effort table: show OA estimate (60% accuracy) vs PRD estimate (90% accuracy) with reason for delta.

### Requirements
User story format: "As a [merchant / courier / platform partner / Easyship internal admin] I want to..."

Acceptance Criteria: Given/When/Then format. This is what QA tests against and what eng treats as "done."
Reviews block on missing or vague ACs. "System should handle X" is not an AC.

Importance: HIGH / MEDIUM / LOW.

Dependency column: name the specific squad if this requirement depends on another squad's work.

### Edge Cases & Failure Modes
Categories to address for every PRD:
1. Dependent API/system unavailable
2. Partial or malformed data
3. Multi-country / multi-currency
4. Edge user types (suspended, trial, returning)
5. Carrier-specific behaviors
6. Sandbox vs production differences
7. Concurrency / race conditions
8. Other

Each case needs a **defined behavior** — not "we'll handle it later."

### Mixpanel Events Spec
**PM owns this spec.** Eng implements what's listed — no inventing events at build time.

Required events = without these, Success Metrics can't be measured. Launch-blocking.
Exploratory events = usage patterns, funnel diagnostics. Not blocking.

If a Success Metric has no linked event: shipping blind on that metric.

Structural questions (all must be answered):
- User identifier scheme (merchant ID, account ID, user ID?)
- PII handling
- Dashboards to create or update
- Validation owner (T+48h post-launch)
- Fallback if tracking broken at launch

### QA Strategy
- Test approach: manual, automated, carrier sandboxes
- Beta plan: cohort, recruitment, feedback loop, duration
- Rollout plan: gradual %, feature flag, full release
- Rollback plan: how to revert if metrics go wrong
- Launch criteria: pre-flight go/no-go conditions

### Design
Figma is the source of truth. This section = pointer + key decisions.
- Figma link (interactive prototype + spec)
- Key flows covered (happy path, error states, empty states, loading states)
- Notable design decisions (2–4 bullets)
- Open design questions

### Build-time Assumptions
Different from OA assumptions (which were about whether to build).
These are about how we're building it — implementation dependencies, third-party behavior, capacity.

Format: Assumption | Why we believe it | What changes if it's wrong

### Decisions Log
Single source of truth for all questions from scoping + build.
Status: OPEN or RESOLVED.
Replaces old FAQ and Open Questions sections.

### Out of Scope
Explicit exclusions. Things deferred to a later release.
Being explicit prevents scope creep during build and disappointment at launch.

### Milestones & Development Timeline
Milestones: Beta, GA with dates and target audience.
Timeline: per-squad Dev Start, Dev End, QA Start, QA End, Go LIVE.

### Launch Dependencies
Non-code launch blockers only (not eng tasks).
Common: Helpcenter article, CS training, Mixpanel dashboards, in-app banners, PMM assets.
Each: owner + target date + status.

### Stakeholder Communications Plan
Per audience: what they need to know, when (relative to launch), format, owner, success signal.
Four standard audiences: Customer Support, Customer Success, Marketing/PMM, Sales.

Minimum lead times:
- Support: T-1 week training before launch
- CS/enterprise accounts: T-2 weeks
- PMM: T-3 weeks kickoff

### Help & Support Articles
List all Helpcenter + PKB articles updated, created, or deleted.
Each: status (DRAFT/PUBLISHED) + owner.

Helpcenter = external (for merchants).
PKB (Product Knowledge Base) = internal (vendor info, cost, monetization, risks).

### Sign-offs
Product, Design, Tech Lead.
Status: NOT APPROVED YET or APPROVED.
Must be present even at draft stage (with NOT APPROVED YET status).
