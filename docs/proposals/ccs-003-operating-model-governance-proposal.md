# CCS-003 — AI Operating-Model Governance: Structure Proposal

**Status:** Proposal — awaiting Glen's review. Docs only; nothing adopted yet.
**Prepared by:** Claude (DEV lane), 2026-07-06
**Discovery basis:** direct inspection of the governance files in
`home-inventory-system` (CC-SILS), `gpb-voice-generator` (VG),
`gpb-storyteller` (Storyteller), and this standards repository.

---

## 1. Recommendation in One Paragraph

Keep this repository as the single canonical source of the Cajun Craft AI
operating model, and standardize every app repository on **three concise,
role-addressed root files — `ChatGPT.md`, `CLAUDE.md`, `CODEX.md` — plus the
existing `AGENTS.md` kept as the Codex CLI's entry pointer.** Each root file
carries only app-specific identity, safety rules, and deviations, and defers
all role behavior to versioned canonical documents here. Alignment is
maintained with version headers and one adoption table — no monorepo, no
sync tooling, no duplicated long documents.

---

## 2. Current Reality (inspected 2026-07-06)

### 2.1 File inventory

| Governance file | CC-SILS | VG | Storyteller | standards repo |
|---|---|---|---|---|
| `CLAUDE.md` | ✅ app-specific | ✅ app-specific | ✅ app-specific (incl. §6 release exceptions) | template |
| `ChatGPT.md` | — | — | ✅ **but generic, org-wide content** | — |
| Codex review checklist | — | ✅ `CODEX_REVIEW.md` | — | template |
| `AGENTS.md` (Codex CLI entry file) | ✅ | — | ✅ | — |
| Codex ops lane guidance | ✅ `docs/codex-ops.md` + task template | — | — | — |
| `AI_WORKING_AGREEMENT.md` | ✅ v2.0 (pre-standards) | ✅ | — | template |
| Handoff protocol (`docs/ai-handoff/`) | ✅ | ✅ | ✅ | templates |
| Shared standards docs | referenced | referenced | referenced | ✅ 6 docs |

### 2.2 Conflicts, gaps, and obsolete assumptions found

1. **Live role conflict.** CC-SILS `AI_WORKING_AGREEMENT.md` §3.2 titles
   ChatGPT "Coordinator, Architect, **and Reviewer**"; the canonical
   [Development Operating Model §3.2](../development-operating-model.md)
   states ChatGPT does not review. Duplicated role definitions have already
   drifted.
2. **Org-wide guidance trapped in one app.** Storyteller's `ChatGPT.md` is
   written as the general Cajun Craft dev-chat guide ("applies whenever a
   chat opens with `Dev - <Project Name>`") yet exists only in the
   Storyteller repo. CC-SILS and VG dev chats have no ChatGPT guidance at
   all.
3. **Codex guidance is fragmented and nowhere complete.** CC-SILS has
   `AGENTS.md` + an ops-lane doc but no review checklist; VG has the review
   checklist but no `AGENTS.md`; Storyteller has `AGENTS.md` but neither. No
   repo gives Codex both its ground rules and its review expectations.
4. **Stale duplicated state.** CC-SILS `DEVELOPMENT_RULES.md` §2 still names
   `feature/devpath-isolation` as "current branch" — evidence that
   per-repo prose that restates changeable facts rots.
5. **No version identity on the standard itself.** The six standards docs
   carry no version numbers or changelog; a repo cannot state which revision
   of the operating model it conforms to, and nothing tracks adoption.
6. **Tooling fact:** the Codex CLI auto-loads `AGENTS.md`, not `CODEX.md`.
   A new `CODEX.md` that nothing points to would be invisible to Codex.
7. **What is already right and should not move:** Storyteller `CLAUDE.md`
   §6.1/§6.2 (single-use TEST/PROD release exceptions) is repo-specific
   execution governance — exactly the kind of content that must stay local.
   The Development Operating Model, Cross-Check Protocol, and the
   "Ability Is Not Authorization" principle (CCS-002) are healthy canonical
   docs; this proposal builds on them rather than replacing them.

---

## 3. Recommended Structure

### 3.1 Canonical (this repo — policy lives here and only here)

| Document | Status | Change proposed |
|---|---|---|
| `docs/development-operating-model.md` | exists | **Extend** with: §Startup Expectations per actor (§7 below), §Codex review expectations (§8 below), and the root-file authority order (§5 below) |
| `docs/ai-cross-check-protocol.md` | exists | Unchanged (already covers drift both directions) |
| Other four standards docs | exist | Unchanged |
| `templates/ChatGPT.md` | **new** | Org-wide dev-chat guide, generalized from Storyteller's current file, with a short app-specific header block |
| `templates/CODEX.md` | **new** | Codex role charter + review expectations + the review checklist (absorbs `templates/CODEX_REVIEW.md`) |
| `templates/CLAUDE.md` | exists | Minor: add the authority-order block and pointers to the two new files |
| `templates/AGENTS.md` | **new (thin)** | Codex CLI entry pointer: repo ground rules summary + "read `CODEX.md` before any review" |
| `CHANGELOG.md` + version headers | **new** | Every standards doc and template gets `**Standard-Version:** X.Y`; CHANGELOG records bumps |
| README adoption table | **new section** | One row per app repo: which template versions it instantiates |

`AI_WORKING_AGREEMENT.md` (template and instances) is **superseded for role
definitions**: the trio + canonical standard replace its role sections. Keep
the file in existing repos as project identity + a pointer (or retire it
per repo during adoption — Glen's call, §11.3). Do not create it in new
repos.

### 3.2 Per app repository root (thin, app-specific only)

| File | Owner-reader | Contents (target ≤ ~1–2 pages each) |
|---|---|---|
| `ChatGPT.md` | ChatGPT | App identity, lane prefix, where handoff/status lives, app-specific product boundaries; defers all behavior to canonical template/standard |
| `CLAUDE.md` | Claude | Already exists everywhere: app safety rules, allowed work, branch rules, handoff protocol, **repo-specific execution governance** (e.g., Storyteller §6) |
| `CODEX.md` | Codex | App-specific review checks (e.g., VG's frozen-baseline rules, CC-SILS print-safety), test command, ops-lane pointer where one exists; defers role behavior to canonical |
| `AGENTS.md` | Codex CLI (auto-loaded) | Kept for tool compatibility: ground rules + "read `CODEX.md`" pointer |

**Why three files instead of one:** each AI reads its own file first by
convention or tooling (Claude Code → `CLAUDE.md`; Codex CLI → `AGENTS.md` →
`CODEX.md`; ChatGPT → `ChatGPT.md` named in the chat workflow). One combined
file would make every actor parse the other actors' rules to find its own,
and tooling would not auto-load it.

**Why thin:** every finding in §2.2 that involved drift (role conflict,
stale branch name) came from a long local file restating shared policy or
changeable state. Local files that contain only what is genuinely local
have nothing to drift against.

---

## 4. Responsibility Matrix

| Responsibility | ChatGPT | Claude | Codex | Glen |
|---|---|---|---|---|
| Product intent, outcomes, acceptance criteria | **Owns** | proposes gaps | — | approves |
| Status awareness + next-actor/model recommendation | **Owns** | — | — | decides |
| Outcome-based prompt writing | **Owns** | — | — | approves scope |
| Scope / meaningful-boundary identification | **Owns** | flags during work | flags during review | **decides** |
| Implementation design, coding, tests | — | **Owns** | — | — |
| PR preparation + verification evidence | — | **Owns** | — | — |
| Independent diff/test review, defect finding | — | — | **Owns** | reads |
| Merge-readiness recommendation | — | — | **Owns** | **decides** |
| Merge to `main` | never | never | only with Glen's explicit per-PR approval | **gate** |
| TEST / STAGING / PROD promotion | never | never (repo-specific single-use exceptions excepted, per local `CLAUDE.md`) | never without explicit approval | **sole authority** |
| Drift call-outs | required | required | required | final check |

Lane-crossing rule (all actors): *recommend across lanes, never act across
them.* ChatGPT may suggest an implementation risk; it does not design.
Claude may note a review concern; it does not self-certify merge readiness.
Codex may flag scope questions; it does not rewrite Claude's branch.

---

## 5. Authority Order

When guidance conflicts, resolution order (highest first), and the conflict
is **flagged to Glen** rather than silently resolved:

1. Glen's explicit, current, action-specific instruction.
2. Repo-local safety rules and execution governance (`CLAUDE.md` safety
   sections, `CODEX.md`/`AGENTS.md` prohibitions, release-exception
   artifacts) — most specific wins because they encode app-specific risk.
3. Canonical standards in this repo (operating model, cross-check protocol,
   protected-actions list).
4. Role-file behavioral guidance (`ChatGPT.md`/`CODEX.md`/`CLAUDE.md`
   non-safety sections).
5. The active lane prompt.

A lane prompt can narrow scope below any layer; it can never widen authority
above one. "Ability is not authorization" (Operating Model §2.1/§6) binds at
every layer.

---

## 6. What Is Shared vs. What Must Be Repo-Local

**Canonical only (never restated locally):** role definitions and the
matrix; approval gates and protected actions; what counts as authorization;
drift/cross-check duties; startup expectations; Codex review expectations;
prompt-writing standard; app-shell/versioning/release standards.

**Repo-local only (never in the standard):** app identity and lane prefix;
app-specific safety rules (frozen baselines, print hardware, live paths);
repo-specific release exceptions and authorization-artifact formats; test
commands and environment paths; handoff file locations; app-specific review
checks. **No changeable state** (current branch, current version, deployed
SHA) belongs in any governance file — that lives in handoff files and
release artifacts.

---

## 7. Startup Expectations (per actor — goes into the operating model)

**ChatGPT** — reads `ChatGPT.md` + provided handoff/status; states known
status and names uncertainty explicitly instead of guessing (never assumes
deployed state from memory); recommends next actor + model with a one-line
why and one outcome-based prompt; stops and asks Glen when product intent,
scope, or a meaningful boundary is undecided; avoids Claude's lane by
specifying destination, not steps.

**Claude** — runs the repo's session-start handoff protocol
(`docs/ai-handoff/current.md` → lane summary → Next Actions) before any
change; states lane status and any mismatch between handoff and observed
repo state; stops at protected actions, scope edges, and when the approved
framing would force unsafe engineering (propose, don't expand); avoids
others' lanes by never merging, never self-certifying review, and never
redefining product intent mid-lane.

**Codex** — reads `AGENTS.md` then `CODEX.md`; works in an isolated
worktree/`codex/*` branch; independently establishes the facts (checkout,
diff, tests) before reading claims; states explicitly what it verified
versus what it could not verify; stops before merge (Glen's gate), on dirty/
unexpected repo state, and at any secret or environment boundary; avoids
Claude's lane by never editing Claude-owned branches without an approved
handoff.

**Glen** — reads the recommendation and the verification result, not just
the summary; gives approvals that are explicit, current, and
action-specific (Operating Model §6.1); treats "looks good/ready/PASS" as
status, never as his own approval.

---

## 8. Codex Review Expectations (anti-shallow — goes into the operating model + `CODEX.md` template)

A Codex review is invalid unless Codex:

1. **Independently inspected** the checked-out diff and the surrounding
   code it touches — not just the PR description.
2. **Independently ran** the test suite and **assessed whether the new
   tests would fail if the claimed behavior were broken** — existence of
   tests is not verification; a suite that passes with the feature stubbed
   out is a finding.
3. **Verified claims** made in the PR (versions, counts, "suite green",
   "docs-only") against observed reality, and said which claims were
   checked.
4. **Classified findings** as *blocking* (defect, safety-rule violation,
   scope drift, missing/weak tests for risky behavior, unverified release
   assumption) versus *non-blocking observations* — and recommends merge,
   merge-with-notes, or do-not-merge accordingly.
5. **Called out** missing tests, files changed outside the stated lane,
   release/promotion assumptions, weakened or deleted tests, and any
   protected action taken or staged on the basis of ability alone.
6. **Never merges or promotes** without Glen's explicit per-action
   approval — a completed review is a recommendation, nothing more.
7. **Escalates gaps in the process itself** (e.g., a lane so vague that
   scope adherence cannot be judged) instead of approving around them.

Repeating the PR summary back with checkboxes ticked is the definition of
the shallow approval this section exists to prevent.

---

## 9. Keeping Three Files Aligned Without a Monorepo

1. **Policy edits happen only in this repo.** A change to role behavior,
   gates, or review expectations is a CCS lane here — never an edit to an
   app repo's copy.
2. **Version headers.** Each canonical doc/template carries
   `**Standard-Version:** X.Y`; each repo-local instance carries
   `Instantiates: <template> vX.Y`. Drift becomes a one-line diff check.
3. **Adoption table** in this repo's README: app repo × instantiated
   template versions. Updated by the adoption PR that bumps it — no
   automation at four repos; the table *is* the tracker.
4. **Bump rule.** MAJOR/MINOR bumps to the operating model or a template
   trigger one small docs lane per app repo ("governance sync"); PATCH
   (typo/clarity) bumps do not require repo updates.
5. **Thin local files** (§3.2) are the real mechanism: what is not
   duplicated cannot drift.

---

## 10. Adoption Plan

**Order: canonical first, then donors, then the rest. One docs-only PR per
repo, each reviewed by Codex and merged by Glen.**

| Step | Lane | Repo | Work |
|---|---|---|---|
| 1 | CCS-003 impl. | standards | Extend operating model (§5, §7, §8); add `templates/ChatGPT.md` (generalized from Storyteller), `templates/CODEX.md` (absorbing `CODEX_REVIEW.md` template), thin `templates/AGENTS.md`; version headers + CHANGELOG; README adoption table + bootstrap rule |
| 2 | ST-00x | Storyteller | Replace generic `ChatGPT.md` with the thin per-repo instance (content having moved upstream in step 1); add `CODEX.md`; point `AGENTS.md` at it. **`CLAUDE.md` §6 release exceptions untouched** |
| 3 | HI-05x | CC-SILS | Add `ChatGPT.md` + `CODEX.md` (folding review expectations; `docs/codex-ops.md` stays and is referenced as the ops lane); point `AGENTS.md` at `CODEX.md`; resolve the `AI_WORKING_AGREEMENT.md` §3.2 role conflict (per Glen's §11.3 decision); fix the stale branch line in `DEVELOPMENT_RULES.md` |
| 4 | VG-00x | VG | Add `ChatGPT.md`, `AGENTS.md`, and `CODEX.md` (absorbing local `CODEX_REVIEW.md`) |
| 5 | PL-001 | CC-PL (future) | Bootstrap rule applies — trio + `AGENTS.md` + handoff templates present from the first commit |

Each adoption PR also adds its row to the adoption table (step 1 creates
the table with a "pending" row per repo).

**New-project bootstrap rule (added to this repo's README):** a new Cajun
Craft repo is not ready for its first implementation lane until it contains
`ChatGPT.md`, `CLAUDE.md`, `CODEX.md`, `AGENTS.md`, and
`docs/ai-handoff/` instantiated from the current templates with an adoption-
table row recorded. The project's scaffold lane (like CC-PL's planned
PL-001) must include this, and ChatGPT's opening-behavior check should
refuse to recommend implementation lanes for a repo missing them.

---

## 11. Decisions Needing Glen's Approval

1. **`CODEX.md` as the Codex charter, with `AGENTS.md` retained as the
   auto-loaded pointer** (vs. putting everything in `AGENTS.md`).
   Recommended as proposed because `AGENTS.md` is also read by non-Codex
   agent tooling and should stay minimal, while the charter needs room.
2. **Upstreaming Storyteller's `ChatGPT.md`** into `templates/ChatGPT.md`
   and thinning the Storyteller copy.
3. **`AI_WORKING_AGREEMENT.md` disposition:** slim each existing instance
   to identity + pointer, or retire the file where the trio covers it.
   (Recommended: slim in CC-SILS and VG during their adoption lanes; do not
   add to new repos.)
4. **Standard versioning + adoption table** as the alignment mechanism
   (accepting the manual-table trade-off over sync tooling).
5. **Adoption sequence and lane numbering** in §10, including one small
   governance-sync lane per app repo.

---

## 12. Validation Statement

This proposal was checked against every governance file listed in §2.1 as it
exists today. The §2.2 findings are the material conflicts, gaps, and
obsolete assumptions identified during this review; they reflect the
evidence inspected, not a claim of exhaustive coverage beyond it. Nothing
here modifies the HI-056 PR, any
application behavior, or any infrastructure; no repository is created; all
changes described are future docs lanes gated on Glen's approval of this
document.
