# Versioning and Environment Standard

**Scope:** All Cajun Craft Software applications  
**Maintained in:** `cajuncraft/software-development-standards`

---

## 1. Purpose

This document defines how Cajun Craft Software applications expose version information
and active environment, and how environment values are interpreted at runtime.

---

## 2. Application Version

### 2.1 Version source of truth

Each app must define its version in one place. That source of truth should be:

- A `__version__` constant in the top-level package `__init__.py` (Python apps).
- An equivalent version constant for other languages/frameworks.
- Not duplicated in multiple places. If multiple files must reference the version, they
  should read from the single source.

No build tooling or version-bumping script is required. A manual update to the single
source is sufficient for small apps.

### 2.2 Version format

Use semantic versioning: `MAJOR.MINOR.PATCH`

| Segment | When to increment |
|---|---|
| MAJOR | Breaking change or significant product milestone |
| MINOR | New feature or meaningful capability added |
| PATCH | Bug fix, documentation, or minor improvement |

### 2.3 Version exposure

The application version must be:

- Visible on the About page.
- Available in the status/health API response where one exists (e.g., `/api/status`).
- Logged at application startup.

### 2.4 Core / library version

If the app has a separate core or engine package (e.g., `voice_core`, `sils_core`),
the core version should also be exposed on the About/Status page and in the API
response. This helps distinguish API-layer changes from engine-layer changes.

---

## 3. Environment

### 3.1 Supported values

Every Cajun Craft Software application recognizes exactly three environment values:

| Value | Meaning |
|---|---|
| `DEV` | Local development; no PROD data; no external exposure |
| `TEST` | Controlled validation; isolated from PROD data |
| `PROD` | Production; Glen-gated; real data |

### 3.2 How the environment is set

The active environment must be set via an environment variable, not hardcoded:

- Each app may define its own variable name (e.g., `VG_ENV`, `CC_SILS_ENV`, `APP_ENV`).
- The app must read the variable at startup; never cache the value between restarts
  without good reason.

### 3.3 Default on missing or unknown value

If the environment variable is absent or contains an unrecognized value, the application
must default to `DEV`. It must not default to `PROD` or `TEST`.

This is a safety rule: a misconfigured deployment should never silently become
production-facing.

The default-to-DEV rule applies unless a later per-app release policy explicitly
overrides it with Glen's approval.

### 3.4 Unknown value handling

If an unrecognized value is provided (e.g., `STAGING`, `qa`, `production`):

- Log a warning naming the unrecognized value.
- Fall back to `DEV`.
- Do not raise an unhandled exception; continue starting up in DEV mode.

### 3.5 Environment visibility

The environment must be visible to the user at all times, not only on the About or
Status page. See the [App Shell Standard](app-shell-standard.md) for badge placement
and color convention.

The badge must be distinct enough that a user switching between DEV and PROD cannot
confuse them. Red for PROD is mandatory.

### 3.6 Merging to `main` is not a promotion

Merging code to the `main` branch does not change the active environment of any running
instance. Environment is controlled by the environment variable at runtime, not by the
Git branch. A deployment or environment variable change is required to move between
environments.

---

## 4. API / Status Response

Where a status or health endpoint exists, it must include at minimum:

```json
{
  "environment": "DEV",
  "app_version": "0.2.0",
  "status": "ok"
}
```

Additional fields (core version, model state, uptime, etc.) are encouraged.

---

## 5. Jinja2 / Template Injection Pattern (Python Web Apps)

For Python web apps using Jinja2 templates, the recommended pattern is to set version
and environment as template globals once in the application factory:

```python
templates.env.globals.update({
    "environment": environment,
    "app_version": app_version,
    "core_version": core_version,
})
```

This makes the values available to every template without per-route passing.

The environment CSS class suffix should be derived from the value using a lowercase
filter (`{{ environment | lower }}`), enabling `.env-badge.dev`, `.env-badge.test`, and
`.env-badge.prod` CSS overrides.

---

## 6. Summary Table

| Requirement | Rule |
|---|---|
| Version source | Single constant (`__version__` or equivalent) |
| Version format | Semantic versioning (`MAJOR.MINOR.PATCH`) |
| Version exposure | About page + status API |
| Environment values | `DEV`, `TEST`, `PROD` only |
| Environment source | Environment variable; never hardcoded |
| Missing / unknown env | Default to `DEV`; log a warning |
| Environment visibility | Visible on every page via badge |
| Merge to main = PROD? | No — environment is a runtime setting, not a branch |
