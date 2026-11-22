# Decision Log

Use this file to record approved architectural/product decisions. Update entries when a decision changes and include date, context, choice, alternatives, and consequences.

## D-001 — No user accounts
**Status:** Accepted
**Context:** This is a class project, not a publicly hosted social product. Account-based personalization/sync is unnecessary.
**Decision:** No accounts, login, authentication, profiles, or cloud sync.
**Consequences:** Personal data is device-local; uninstall/clear-data can remove it; no cross-device sync.

## D-002 — Local backend; no public deployment
**Status:** Accepted
**Context:** Project is intended for classroom/demo use.
**Decision:** Run FastAPI locally during development/demo. Do not plan public hosting.
**Consequences:** App needs backend reachable on developer computer for backend-powered flows; external APIs still require internet.

## D-003 — No backend database in MVP
**Status:** Accepted
**Context:** Personal data is local and external place data is retrieved live. Curated content is small initially.
**Decision:** No PostgreSQL or backend SQLite in MVP. Use JSON for small app-owned curated content. Personal data stays on device.
**Consequences:** JSON is sufficient for simple curated content; revisit if editing, relationships, querying, or persistent backend-owned data becomes substantial.

## D-004 — AI scanner identifies clues, provider verifies place records
**Status:** Accepted
**Context:** Vision AI can infer signage/category but may hallucinate a location or business.
**Decision:** AI extracts clues; place-provider search supplies candidate listings; deterministic matcher ranks them; low confidence yields alternatives/no-match.
**Consequences:** Scanner requires both AI and place-provider integrations. It cannot guarantee identification.

## D-005 — Deterministic recommendations initially
**Status:** Accepted
**Context:** AI recommendations would add cost, opacity, and potential factual errors.
**Decision:** Initial nearby recommendations and compare highlights use deterministic logic over available provider/curated data and device-local interests.
**Consequences:** Easier testing and explanation. More sophisticated recommendation techniques may be considered later.

## D-006 — Vertical-slice and project-based learning
**Status:** Accepted
**Context:** Developer wants to learn concepts and build understanding, not receive opaque code dumps.
**Decision:** Implement one small end-to-end capability at a time; explain design and concepts; tests and verification are part of each slice.
**Consequences:** Slower than bulk generation but more understandable and maintainable.

## D-007 — Provider choices are provisional
**Status:** Pending validation
**Context:** Google Maps/Places and Gemini were recommended, but current terms, pricing, quotas, and course constraints need review.
**Decision:** Keep Google Maps/Places and Gemini as likely candidates, but verify current APIs, billing, terms, field costs, and key restrictions before integration.
**Consequences:** Architecture uses provider interfaces/contracts to limit coupling without creating unnecessary abstractions.

## D-008 — Single root Git repository; Android-only platform folders
**Status:** Accepted
**Date:** 2026-11-23
**Context:** The project had no repository at its root, while `api/` contained an empty nested Git repository created by `uv init`. The Flutter scaffold also generated five platforms the product does not target.
**Decision:** Use one Git repository at the project root (`main`). Remove `api/.git` (it had no commits) and remove the iOS, Linux, macOS, web, and Windows platform folders from `app/`.
**Alternatives:** Keeping per-directory repositories (rejected: splits history and complicates project-wide ignores); keeping all Flutter platforms (rejected: noise in every diff, no current target).
**Consequences:** One history and one root `.gitignore`. Non-Android platforms can be regenerated with `flutter create --platforms=<name> .` if a later task approves them. Any future subproject must not run `git init`.

## D-009 — Verified backend toolchain: Python 3.12, uv, FastAPI
**Status:** Accepted (toolchain only; framework versions may be upgraded deliberately)
**Date:** 2026-11-23
**Context:** Phase 0 needed a reproducible local backend environment with no database, no Docker, and no provider credentials.
**Decision:** Use `uv` with `api/.python-version` = 3.12 and `api/uv.lock` for a locked environment; FastAPI 0.165.0 is installed. No ASGI app, routes, settings module, or test tooling were added in Phase 0.
**Alternatives:** pip + requirements.txt or virtualenv (rejected: slower, weaker locking); adding pytest/ruff immediately (deferred until the first backend test exists, to avoid unused dependencies).
**Consequences:** Backend setup is `uv sync --locked`. The Phase 1 slice adds the FastAPI entrypoint and health route, and will introduce `pytest` at that point.

## D-010 — App identity
**Status:** Accepted
**Date:** 2026-11-23
**Context:** The Flutter scaffold's placeholder identity (`name: app`, `com.example.app`, label "app") would later have to match a restricted Google Maps API key, making a late rename costly.
**Decision:** Dart package `tuklas_lokal`; Android `applicationId` and `namespace` `ph.tuklaslokal.app`; display label "Tuklas Lokal".
**Alternatives:** Keeping `com.example.app` (rejected: not a real identity, and Maps key restrictions are per package name).
**Consequences:** The Kotlin `MainActivity` package and Dart test imports use the new identity. Changing it later requires updating the manifest, Gradle config, Kotlin package path, and any restricted API keys.
