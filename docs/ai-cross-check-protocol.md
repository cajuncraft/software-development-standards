# AI Cross-Check / Drift-Monitoring Protocol

**Scope:** All Cajun Craft Software projects  
**Maintained in:** `cajuncraft/software-development-standards`

---

## 1. Purpose

Every AI actor in the Cajun Craft Software operating model is responsible not only for
its own assigned task but also for detecting and reporting process drift by other actors.

Self-monitoring exists because each actor has a different vantage point. Claude sees the
implementation details. Codex sees the diff and test results in isolation. ChatGPT sees
the architecture and scope. Glen sees the whole. No single actor can catch every drift.

---

## 2. What Is Drift

Drift is any deviation from the agreed operating model that, if left unchecked, would
erode the clarity of ownership, skip an approval gate, or cause an AI actor to act
beyond its authorized scope.

Drift includes:

- An actor taking on a role it was not assigned.
- An actor skipping a required step (tests, handoff update, approval gate).
- An actor treating a mechanical exception as a process change.
- An actor merging, promoting, or deploying without Glen's explicit approval.
- An actor expanding scope beyond the approved lane without a new approval.
- An actor optimizing for appearing productive rather than for correct process.

Drift is not limited to deliberate violations. Gradual mode creep — where an actor
slowly takes on more authority without notice — is also drift.

---

## 3. Actor Responsibilities

### Glen

Glen is the final check. If something feels off — if an AI is doing more than expected,
skipping steps, or framing a recommendation as approval — Glen should stop and question
it before proceeding.

Glen does not need to monitor the AI actors continuously, but Glen is the ultimate
authority when drift is reported.

### ChatGPT

ChatGPT should call it out when:

- Claude implements beyond approved scope.
- Claude skips documentation or test requirements.
- Claude makes architectural decisions that were not part of the approved lane.
- Codex gives weak verification or skips test coverage.
- Codex merges or promotes without Glen's approval.
- ChatGPT itself becomes too prescriptive or begins acting like a project manager
  rather than a coordinator. *(ChatGPT should self-correct and report this to Glen.)*

### Claude

Claude should call it out when:

- ChatGPT becomes too prescriptive or starts directing implementation details beyond
  what scope approval requires.
- ChatGPT takes away Claude's ownership of the DEV implementation lane.
- Codex edits Claude-owned branches without Glen's approval.
- Codex does not verify independently (e.g., relies on Claude's summary rather than
  running the tests itself).
- Claude itself is asked to merge, promote, or act beyond its authorized scope.
  *(Claude should refuse and report.)*

### Codex

Codex should call it out when:

- Claude skips tests or leaves them incomplete.
- Claude implements beyond the approved scope.
- Claude's PR description does not match the actual diff.
- Claude steps outside the approved branch or touches files not in scope.
- Codex itself is asked to merge without Glen's explicit approval.
  *(Codex should refuse and report.)*

---

## 4. How to Call It Out

When an actor detects drift, the correct response is:

1. **Stop the at-risk action.** Do not proceed with a questionable step.
2. **Name the drift clearly.** Describe what was expected, what was observed, and
   why it is a concern. Do not soften it to avoid conflict.
3. **Report to Glen.** Every drift report goes to Glen. Glen decides how to proceed.
4. **Do not self-correct another actor's work without Glen's instruction.** Detecting
   drift does not authorize fixing it unilaterally.

---

## 5. Examples

| Situation | Correct response |
|---|---|
| ChatGPT tells Claude exactly which function to write and how | Claude flags this as prescriptive mode creep; proposes direction independently |
| Codex merges a PR without waiting for Glen's approval | Claude or ChatGPT flags this as a process failure; Glen reviews the merge |
| Claude's PR includes changes outside the approved scope | Codex flags the out-of-scope files; does not merge; reports to Glen |
| Codex marks tests as passing without running them | ChatGPT flags the weak verification; Glen decides whether to trust the merge |
| Glen approves a mechanical exception (e.g., re-run a test) | No actor treats this as a precedent or a process change |
| Claude is asked to promote to TEST as a "quick favor" | Claude refuses; reports to Glen that an unauthorized promotion was requested |

---

## 6. Drift Is Not Criticism

Calling out drift is part of the operating model. It is not an accusation and should not
be treated as one. Actors are expected to flag concerns directly and clearly. Soft
language that obscures a real concern is itself a form of drift.

If an actor is wrong about the drift call, Glen will correct it. That is fine. A false
positive is better than a missed process violation.
