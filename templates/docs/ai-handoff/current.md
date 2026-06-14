# AI Handoff — Current

> **Template:** Copy this file to `docs/ai-handoff/current.md` in a new Cajun Craft
> Software app repo. Update the Active Lane and Summary File fields each time the
> active lane changes. This file is the session-start entry point for Claude Code.

---

**Active Lane:** [LANE-ID, e.g., app-001]
**Summary File:** docs/ai-handoff/[LANE-ID]-summary.md
**Updated:** [YYYY-MM-DD]

---

[Brief context about what just happened and what the active lane is. For example:
"[LANE-ID-PREV] merged to main with Glen approval on [DATE].
Active lane: [LANE-ID] — [Brief lane title].
Branch: [branch-name]."]

Open `docs/ai-handoff/[LANE-ID]-summary.md` and review **Next Actions** before
making code changes.

---

Candidate next lanes (after [LANE-ID]):

- [LANE-ID+1]: [Title] — [Brief description]
- [LANE-ID+2]: [Title] — [Brief description]

Deferred / backlog items (not yet assigned to a lane):

- [Description of deferred item]
- [Description of deferred item]

When a lane is selected, create `docs/ai-handoff/<lane>-summary.md` (use the
lane-summary-template.md), update this file, and review Next Actions before making
code changes.
