# Output Formats

Both modes run the same pipeline (5 passes + 7-persona panel); they differ in **panel depth** and
**render target**. Summary = light panel → HTML scorecard. Full = complete panel → Excel workbook.
Build the analysis first; these are just renderers. Always also print a 3-line chat headline beside
the artifact: verdict · blocker count · the single most important next action.

Shared data (use the same values everywhere so the formats agree):
- `featureName`, `reviewedDate`, `status` (PRD Document Status)
- `verdict` ∈ {READY, NEARLY READY, NEEDS WORK, NOT READY} with colour {🟢, 🔵, 🟡, 🔴}
- `verdictReason` — one sentence
- `completeness` — "X/17"; `evidence` ∈ {Strong, Adequate, Weak, Unverifiable}
- `blockerCount`, `quickFixCount`
- Six **dimensions** (lights 🟢/🟡/🔴 + one-line read): Completeness (Pass 1), Customer evidence
  (Pass 2), Assumptions (Pass 3), Metrics & ROI (Pass 4), Dependencies & launch (Pass 5),
  Stakeholder panel (net of the 7 personas — 🔴 if any persona raised a ❌ blocker)
- Seven **personas** — Engineering, Design, Executive, Legal, UX Research, Skeptic, Customer Voice.
  Summary (light): each has a lens verdict (🟢/🟡/🔴) + one-line concern. Full: each also has
  strengths[], concerns[], blockers[], suggestions[].
- `convergent[]`, `conflicts[]` (each: topic, positionA, positionB, decision), `blindSpots[]`,
  `blockers[]` (ranked), `quickFixes[]`, `decisionsNeeded[]`, `nextAction`

---

## Summary: HTML Scorecard

Skim `/mnt/skills/public/frontend-design/SKILL.md` for visual quality, then build ONE self-contained
`.html` file in `/mnt/user-data/outputs/` — all CSS inline in a single `<style>` block, **no
frameworks, no CDNs, no external fonts or scripts**, so it opens offline in any browser and pastes
into a wiki. Use a system-font stack (and a monospace stack for the numerics). Make it responsive:
the stat strip and persona grid collapse to one column on narrow screens. Keep the review content
directly in the markup (no build step).

Layout, top to bottom (keep it to roughly one screen at desktop width):

1. **Header** — feature name, reviewed date, PRD status pill.
2. **Verdict banner** — large, colour-coded by verdict (green/blue/amber/red). Verdict label +
   one-sentence reason.
3. **Stat strip** — four cells in a row: Completeness (X/17), Evidence, 🚧 Blockers (N),
   🔧 Quick fixes (N).
4. **Health by dimension** — the six dimension rows (five passes + Stakeholder panel), each with a
   coloured dot/light and its one-line read. This is the scannable core.
5. **Stakeholder panel at a glance** — seven small persona chips (Engineering, Design, Executive,
   Legal, UXR, Skeptic, Customer) each showing its light (🟢/🟡/🔴) and its one-line concern from the
   light pass. The static view must stand alone (no required interaction).
6. **Top blockers** — up to 3, one line each.
7. **Do this first** — the single next action, visually emphasised.

Keep it calm and legible: one accent colour driven by the verdict, generous spacing, no charts
unless they add real signal. The card must be readable as a static screenshot.

---

## Full: Excel Workbook

Read `/mnt/skills/public/xlsx/SKILL.md` first and follow it for mechanics (openpyxl, styling,
column widths, frozen headers, autofilter). Build ONE `.xlsx` in `/mnt/user-data/outputs/`, named
`PRD-Review-<feature-slug>.xlsx`. Use five tabs in this order. Apply the verdict/light colours as
cell fills so the workbook is scannable; bold headers; freeze the header row; set sensible column
widths; wrap text in long cells.

**Tab 1 — Scorecard** (the at-a-glance, mirroring the HTML summary)
- Feature, Reviewed date, Status, Verdict (colour-filled), Verdict reason
- Stat block: Completeness X/17, Evidence, Blockers N, Quick fixes N
- Six dimension rows (five passes + Stakeholder panel): Dimension | Light | One-line read
- Next action

**Tab 2 — Pass-by-pass**
- Columns: Pass | Status | Finding | Reference (PRD section)
- Pass 1 expands to the 17-section completeness list (Section | ✅/⚠️/❌ | Note); Passes 2–5 one
  block each with their verdict + named findings. ROI math check belongs in Pass 4.

**Tab 3 — Persona Panel** (from `prd-review-panel`)
- One row per persona; columns: Persona | Lens verdict | ✅ Strengths | ⚠️ Concerns | ❌ Blockers |
  💡 Suggestions. Seven rows: Engineering, Design, Executive, Legal, UX Research, Skeptic,
  Customer Voice. The Customer Voice row should cite the VoC/ICP signal from `voc-icp-analyzer`
  (ticket themes, closure verbatims, NPS, affected segments), not a hypothetical reaction.

**Tab 4 — Conflicts & Convergence**
- Section A — Convergent issues: Issue | Flagged by (passes/personas) | Why it matters
- Section B — Conflicts needing a PM decision: Topic | Position A (persona) | Position B (persona) |
  Trade-off / recommendation
- Section C — Blind spots: Issue | Caught by | Why it could be critical

**Tab 5 — Actions**
- Section A — Blockers (before engineering): # | Blocker | Owner | Why critical
- Section B — Quick fixes (won't delay engineering): Fix | Owner
- Section C — Decisions needed: Question | Options | Owner
- Bottom: the single recommended Next action

After saving, present the file. If the user asked for a native Google Sheet and the Google Drive
connector is available, offer to push it to Drive as a Sheet — ask before writing to their Drive.
Otherwise tell them the .xlsx opens in Google Sheets via File → Import in one click.
