# Product Design Principles

**Scope:** All Cajun Craft Software products  
**Maintained in:** `cajuncraft/software-development-standards`

---

## 1. Purpose

These principles apply to every Cajun Craft Software application, regardless of platform
or purpose. They define the values that should guide product decisions, feature design,
and user experience choices.

They are not a checklist. They are a lens. When making a product decision, ask whether
the choice reflects these principles.

---

## 2. The Principles

### Technology should be quiet and helpful.

The app should not demand attention. It should do its job without announcing itself.
Notifications, badges, animations, and status indicators should be reserved for things
that genuinely matter. When in doubt, reduce the noise.

### Prefer obvious workflows over clever features.

A feature that requires explanation is a liability. The right solution is usually the
one the user would reach for on their own — not the one that demonstrates technical
sophistication. If a workflow needs a tutorial, redesign the workflow.

### Respect the user's intent.

When the user makes a choice, honor it. Do not override, second-guess, or silently
redirect. If the app cannot honor a choice, say so clearly. Do not substitute a
different behavior and hope the user does not notice.

### Reduce unnecessary decisions with safe, useful defaults.

Default settings should reflect what the typical user actually wants in the typical
situation. Defaults should be safe (choosing the wrong default should not cause data
loss or irreversible action) and useful (not so conservative that the user must
reconfigure everything). Let the user override; do not force them to.

### Keep the interface calm.

Avoid flashing, blinking, aggressive coloring, or layout shifts that pull focus away
from the user's task. The UI should feel stable and predictable. Motion, color, and
emphasis should signal meaning — not fill space.

### Make state visible when it matters.

When the app's state affects what the user should do next — or could affect whether an
action is safe — show that state clearly. Environment (DEV / TEST / PROD), connection
status, processing state, and version are examples of state that should be visible when
relevant, not hidden in configuration menus.

### Preserve flow; do not interrupt users unless the boundary matters.

Confirmations, warnings, and interstitials have a cost: they break focus. Reserve
interruptions for actions that are hard to reverse or that have consequences the user
may not have considered. For routine actions, let the user proceed. The barrier should
match the stakes.

### Guardrails over ceremony.

Safety should come from good defaults and clear boundaries, not from layers of
confirmation dialogs and warnings. If an action is dangerous, make it hard by design —
not by adding a checkbox the user will click without reading. Ceremony is not safety.

### Local-first and privacy-conscious by default.

Data should stay on the user's device unless there is a clear, user-understood reason
to leave it. Do not send data to external services without explicit user consent.
Do not log more than is necessary. Do not assume cloud availability.

### Design for the likely device and context.

Consider where the user actually is when using the app. A Windows desktop app used in
a home workshop has different interaction patterns than a mobile browser used in a
kitchen. Design for the real context, not an idealized one. When a feature depends on
a specific device or capability, name that dependency explicitly rather than silently
degrading.

### AI should not optimize for sounding productive.

When AI is part of the development process, its job is to preserve the operating model,
call out drift, and keep ownership boundaries clear — not to generate impressive output.
An AI actor that skips steps to appear efficient, agrees with scope expansions to avoid
friction, or softens a drift report to sound collaborative is not being helpful.
Correctness and process fidelity matter more than volume.

---

## 3. When Principles Conflict

Principles sometimes pull in different directions. When they do:

- Prefer user safety over user convenience.
- Prefer reversible actions over irreversible ones.
- Prefer transparency over brevity when the stakes are high.
- Defer to Glen when the correct trade-off is not clear.

These principles are a guide, not a hierarchy. Use judgment.
