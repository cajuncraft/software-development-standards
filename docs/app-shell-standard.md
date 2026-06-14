# Application Shell Standard

**Scope:** All Cajun Craft Software local web applications  
**Maintained in:** `cajuncraft/software-development-standards`

---

## 1. Purpose

This document defines the required and recommended shell elements for every Cajun Craft
Software local web application. The goal is a consistent, recognizable product identity
across apps, while giving each app room to adapt the implementation to its own needs.

The first implementation of this standard is the GPB Voice Generator (VG-001 through
VG-005). New apps should adopt this standard from the start, not retrofit it later.

---

## 2. Required Shell Elements

Every Cajun Craft Software local web app must include all of the following:

### 2.1 App name

The application's name must be clearly visible to the user — in the sidebar header,
the browser tab title, and the About page. The name should be consistent across all
three locations.

### 2.2 Vendor / byline

Every app must display **Cajun Craft Software** as the vendor or byline. This belongs
on the About page and is encouraged in the sidebar header or footer.

### 2.3 Version indicator

The app version must be:

- Defined as a single source of truth in the application code (e.g., `__version__` in
  `__init__.py` or an equivalent constant).
- Visible on the About page.
- Available in the status/health API endpoint where one exists.

Version format should follow semantic versioning (`MAJOR.MINOR.PATCH`).

### 2.4 Environment indicator: DEV / TEST / PROD

The active environment must be:

- Visible on every page, not just the About or Status page.
- Shown via a badge or label in the top bar or sidebar that is distinct enough to
  prevent confusion between environments.
- Set from an environment variable (e.g., `APP_ENV`, `VG_ENV`); unknown or missing
  values must default to DEV.

Recommended CSS color convention:

| Environment | Color |
|---|---|
| DEV | Teal / blue-green |
| TEST | Amber / yellow |
| PROD | Red |

### 2.5 About page / menu item

Every app must have an About page or panel that includes:

- App name
- Vendor / byline (Cajun Craft Software)
- Version
- Environment
- Copyright notice
- Brief description of the app's purpose
- Getting Started / Help section or link
- Relevant local/runtime notes (e.g., "Runs locally on this machine only")
- Security boundary statement where applicable

The About item must be the **last item in the sidebar or main navigation menu** before
any footer controls (collapse toggle, etc.).

### 2.6 Copyright notice

Each app must display a copyright notice on the About page:

```
© [YEAR] Cajun Craft Software. All rights reserved.
```

### 2.7 Help / Getting Started section or link

Every app must include a Getting Started or Help section, either directly on the About
page or linked from it. The minimum content is a brief explanation of what the app does
and how to get started.

### 2.8 Relevant local/runtime notes

The About page must include notes relevant to the app's runtime context. For local-only
apps, this should explain that the app runs on the local machine and is not accessible
from other devices or the internet (unless that has been explicitly enabled by Glen).

---

## 3. Recommended Shell Elements

The following elements are strongly recommended and should be included unless the app's
specific context makes them impractical.

### 3.1 Consistent left sidebar navigation

Prefer a collapsible left sidebar for primary navigation. The sidebar should:

- Always use dark chrome (not themed with the content area).
- Show the app name or logo at the top.
- List navigation items with icons and labels.
- Persist collapse state in `localStorage`.
- Include the About item as the last entry before the collapse control.

### 3.2 Consistent header / top bar

A 56px top bar is recommended, containing:

- The current page title.
- The theme selector (System / Light / Dark) where applicable.
- The environment badge.

### 3.3 Status page

Apps that have a meaningful runtime state (server health, model load state, active
connections, dependency versions) should include a Status page:

- Accessible from the sidebar.
- Showing environment, app version, and relevant runtime/dependency versions.
- Updated without requiring a page refresh where practical.

### 3.4 Dark / light / system theme

Local web apps should support three theme modes:

| Mode | Behavior |
|---|---|
| System | Follows OS preference via `prefers-color-scheme` |
| Light | Always light |
| Dark | Always dark |

Theme choice should persist in `localStorage`. A flash-prevention script should run
before the first CSS paint to avoid a visible theme switch on page load.

---

## 4. Layout Reference

The recommended shell layout:

```
+----------+---------------------------------------------+
| sidebar  | top-bar (56px: page title, theme, env-badge)|
| (240px / +---------------------------------------------+
|  56px    | main content area (themed, flex-fill)       |
| collapsed|                                             |
|          |                                             |
| [toggle] |                                             |
+----------+---------------------------------------------+
```

The sidebar uses dark chrome regardless of the current theme. The main content area
uses the active theme tokens.

---

## 5. Local-First Assets

The default for Cajun Craft Software apps is local-first and privacy-conscious, with no
accidental external dependencies:

- **Static shell assets should be local/self-hosted by default.** CSS, JS, fonts, and
  images that make up the app shell should be served from the app itself.
- **External assets and CDNs should not be used without explicit approval.** Pulling
  in CDN-hosted libraries, web fonts, or analytics scripts is not permitted by default
  and requires Glen's explicit approval for the app or lane.
- **Runtime integrations and external URLs may be allowed only when explicitly approved**
  for a specific app or lane. When an app legitimately needs to reach an external
  service (e.g., an approved API integration), that exposure must be approved, scoped,
  and documented — it is not an accidental dependency.

The goal is to prevent *accidental* external dependencies, not to prohibit approved
integrations. An app should never reach outside the local machine by default; it may do
so only where Glen has explicitly approved that behavior.

Where an app is intended to be fully local with no approved external calls, a test
should verify that no unexpected `http://` or `https://` references exist in its web
assets.

---

## 6. Reference Shell Pattern

The GPB Voice Generator is the current reference implementation of this shell pattern
(as of VG-005). It is a worked example of the standard, not the canonical source of
files that every app must clone.

New apps should follow or adapt the reference shell pattern. They may copy useful
structure or styling from existing apps when appropriate, but should not be required to
clone exact files (e.g., `shell.css`, `shell.js`, `base.html`) if the framework or app
context differs. A Flask app, a FastAPI app, and a future app on a different stack can
all satisfy this standard while differing in implementation.

What matters is that the app honors the required and recommended shell elements in
sections 2–4 — not that it reuses any specific file. When the standard is updated, the
reference implementation should be updated to match so it remains a useful example.
