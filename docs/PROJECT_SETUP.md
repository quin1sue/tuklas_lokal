# Project Setup Notes

Verification log for the local development environment. Commands and versions below were
observed by running them on the developer machine; anything not run is marked **Unverified**.

## Prerequisites
- Flutter SDK and Dart SDK.
- Android Studio or Android SDK tools (cmdline-tools, platform, build-tools).
- Android emulator or Android device with developer options/USB debugging.
- Python 3.12 environment for the FastAPI backend, managed with `uv`.
- API provider account/credentials only if external integrations are enabled (none yet).
- Git.

## Verified environment (2026-11-23)

| Tool | Version | Command run | Status |
|---|---|---|---|
| Flutter | 3.47.9 stable (framework `c197ca5199`) | `flutter --version` | Verified |
| Dart | 3.13.7 | `dart --version` | Verified |
| Python (system) | 3.14.10 | `python3 --version` | Verified |
| Python (backend venv) | 3.12.14 | `uv run python -c "import sys; print(sys.version)"` | Verified |
| uv | 0.13.8 | `uv --version` | Verified |
| FastAPI | 0.165.0 | `uv run python -c "import fastapi; print(fastapi.__version__)"` | Verified |
| Java (system JDK) | 27.0.1 | `java -version` | Verified, unsupported for AGP |
| Git | 2.56.1 | `git --version` | Verified |
| Gradle wrapper | 9.3.1 | `app/android/gradle/wrapper/gradle-wrapper.properties` | Configured |
| AGP / Kotlin | 9.1.0 / 2.4.0 | `app/android/settings.gradle.kts` | Configured |
| Android SDK / `adb` | — | `flutter doctor -v` | **Missing (blocker)** |
| Android emulator/device | — | `flutter doctor -v` | **Missing (blocker)** |

### Compatibility notes
- The backend pins Python 3.12 via `api/.python-version`; `pyproject.toml` requires `>=3.12`.
  The system Python 3.14 is only used by `uv` to create the 3.12 environment, not to run FastAPI.
- AGP 9.1 is tested against JDK 17–21. The system JDK 27 is likely incompatible; once Android
  Studio is installed, point Flutter at its bundled JDK
  (`flutter config --jdk-dir=/path/to/android-studio/jbr`). **Unverified** until an Android build runs.
- Gradle 9 + AGP 9.1 + Kotlin 2.4 is a new combination; the first
  `flutter build apk --debug` is the real compatibility test. **Not yet run.**

## Android setup blocker

`flutter doctor -v` currently reports `✗ Unable to locate Android SDK` and no `adb`. Until an SDK,
platform tools, and an emulator/device exist, no Android build or run can be verified. Desktop
Linux builds are available but are not the product target and were not required by Phase 0.

`app/android/local.properties` is machine-local and git-ignored. It must keep `flutter.sdk=<path>`;
Gradle reads it in `settings.gradle.kts`.

## Repository layout and Git
- Single Git repository at the project root (`main` branch).
- No nested repositories. `api/.git` was removed after confirming it had no commits.
- Root `.gitignore` is the source of truth for secrets and generated output; `app/.gitignore`
  and `api/.gitignore` add package-specific rules.
- App identity: Dart package `tuklas_lokal`, Android `applicationId`/`namespace`
  `ph.tuklaslokal.app`, display label "Tuklas Lokal".
- Platform folders for iOS, Linux, macOS, web, and Windows were removed in Phase 0: the product
  is Android-first. Re-add later only if a platform becomes an explicit target
  (`flutter create --platforms=<name> .`).

## Backend setup commands

```bash
cd api
uv sync --locked          # create/refresh .venv from uv.lock
uv run python -c "import fastapi; print(fastapi.__version__)"
```

There is intentionally no ASGI app or health route yet; the Phase 1 slice adds the FastAPI
entrypoint and `GET /health`. `api/src/api/__init__.py` still holds the `uv init` placeholder
and is replaced in that slice.

## Environment variables
- Backend template: `api/.env.example` (variable names and placeholders only).
- Local file: `cp api/.env.example api/.env` — `.env` is git-ignored and must never be committed.
- Current variables: `APP_ENV`, `LOG_LEVEL`, `API_HOST`, `API_PORT`, `GEMINI_API_KEY`,
  `GOOGLE_PLACES_API_KEY`. Only the first four have described behavior; provider keys are unused
  until their integration slices exist.
- Nothing reads these variables yet. Loading and validation belong to the backend settings module
  in the Phase 1/2 slice.
- Flutter has no env file. Client configuration will use `--dart-define` (for example a backend
  base URL) so no secrets or extra packages enter the app.

## Local backend access
- Android emulator generally reaches the host machine through `10.0.2.2`; confirm the emulator type.
- A physical device needs the host's LAN address and the same network; use a development-only
  binding and understand the firewall/network exposure. Do not expose the backend publicly.
- Expected base URLs once a route exists: `http://10.0.2.2:8000` (emulator),
  `http://<host-lan-ip>:8000` (device), `http://127.0.0.1:8000` (desktop/tests).

## External credentials
Before configuring any provider:
1. Check official current API docs.
2. Verify the needed endpoints and model IDs (Gemini model availability is still provisional).
3. Review terms, data retention, attribution, pricing, quotas, and free-tier limitations.
4. Restrict the mobile map key to the app package/identity and required APIs; keep it separate
   from backend keys.
5. Keep backend-only credentials in `api/.env`.
6. Configure budget alerts/quotas where supported.
7. Never paste credentials into agent prompts or commit them.

Provider decisions remain **Pending** (`docs/DECISIONS.md`, D-007). No provider integration exists yet.

## Setup acceptance status
- Flutter app resolves dependencies, analyzes cleanly, and passes its existing widget test: Verified
  (see task report for command output).
- Backend environment installs from `uv.lock` and imports FastAPI: Verified.
- Backend server starts and serves a health check: **Not yet** — Phase 1.
- Flutter app builds for Android: **Not yet** — blocked by missing Android SDK.
- Git repository contains no credentials: Verified by ignore checks and inspection.
