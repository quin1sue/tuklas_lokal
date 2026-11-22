# Architecture

## Architecture goals
- Understandable for a student developer.
- Small, feature-oriented modules.
- Vertical-slice implementation.
- Backend used for secret-bearing integrations and AI orchestration.
- No database until a feature requires one.
- Clear boundaries between UI, application logic, data access, and external APIs.

## High-level topology
Flutter mobile client
  -> local FastAPI backend
  -> Gemini API (image clue extraction)
  -> Place provider API (search/details)

Flutter also uses the selected mobile map SDK and device location services. Personal settings/favorites are device-local.

## Flutter responsibilities
- Screens and UI widgets.
- Navigation.
- Camera/gallery picker.
- Foreground location permission and coordinates.
- Map SDK rendering.
- State management.
- HTTP requests to backend.
- Local persistence for personal preferences/favorites.
- Comparison UI and comparison state.
- Display provider attribution and source details.

## Backend responsibilities
- API routes and contracts.
- Configuration and secret handling.
- AI provider integration.
- Place-provider integration.
- Scanner orchestration.
- AI response validation.
- Candidate matching and ranking.
- Curated content loading from JSON.
- Error mapping and logging without secrets or unnecessary personal data.

## Backend request flow — scanner
1. Client sends image, optional coordinates, and optional manually selected area.
2. Route validates input and size/type constraints.
3. Service calls AI provider to extract clues (not to claim actual location).
4. Service converts clues into provider search queries.
5. Provider search returns candidate place records.
6. Matching module ranks candidates using visible-name/text similarity, category match, and location proximity when context is available.
7. Confidence policy determines whether to return a likely candidate, alternatives, or low-confidence/no-match.
8. Response schema is returned to client.

## Backend modules (proposed, adjust to project conventions)
- `app/main.py` — app creation and lifecycle
- `app/api/` — routers and HTTP boundary
- `app/core/` — settings, logging, error handling
- `app/schemas/` — Pydantic request/response contracts
- `app/services/` — use-case orchestration and business logic
- `app/integrations/` — Gemini and place provider clients
- `app/matching/` — candidate matching/scoring pure functions
- `app/content/` — curated JSON files
- `tests/` — unit/integration tests

Do not create all modules upfront. Add them when a vertical slice needs them.

## Flutter structure (proposed feature-first)
- `lib/app/` — app bootstrap, router, theme
- `lib/core/` — shared configuration, networking, errors, utilities
- `lib/features/discover/`
- `lib/features/place_details/`
- `lib/features/identify/`
- `lib/features/compare/`
- `lib/features/preferences/`
- `lib/features/favorites/`

Each feature can contain UI, models, repositories, and services as needed. Avoid rigid layer folders with empty scaffolding.

## API contracts (initial conceptual endpoints; not final)
- `GET /health` — local backend health.
- `GET /api/v1/discover/nearby` — nearby search with location/radius/category/filter parameters.
- `GET /api/v1/places/search` — text search.
- `GET /api/v1/places/{provider_place_id}` — provider place details.
- `POST /api/v1/identify` — image and optional location context; returns candidate matches.
- `GET /api/v1/content/featured` — app-owned curated content, if included.

Endpoint names and response shapes should be finalized in the contract-design slice before implementation. Avoid exposing provider raw responses directly to Flutter; define app-owned response schemas.

## No database in initial backend
- Curated data is JSON.
- User preferences and favorites are device-local.
- Place-provider data is queried live and handled under provider terms.
- In-memory state is not used as persistence for personal data.
- Add SQLite only when an approved feature needs durable backend-owned records, relational queries, or management capabilities beyond JSON.

## Security and local networking
- Never commit `.env`, API keys, tokens, or credentials.
- Backend AI/Places keys stay in local backend environment.
- Map SDK key is configured in the mobile app but must be restricted to the app/platform and only required APIs.
- Keep local backend bound to an appropriate interface; local device testing may require LAN binding. Document the risk and do not expose it to public networks unnecessarily.
- CORS is configured for actual development clients only where relevant.
- Validate uploads, content type, and size.
- Avoid logging images, API keys, raw tokens, or sensitive location data.
- Do not use background location.
- Do not assume local backend is safe if exposed to a LAN; use development-only configuration and network precautions.

## Errors and observability
Use consistent app-owned error types. Preserve useful diagnostics server-side without leaking secrets. Client states should distinguish:
- Network unavailable
- Backend unavailable
- Provider/API failure
- Rate limit/quota
- Invalid image/input
- No results
- Low-confidence match
- Permission denied
- Unexpected error

## Testing strategy
- Unit tests for pure ranking/matching/filtering functions.
- Schema tests for AI outputs and endpoint contracts.
- Integration tests for service orchestration with mocked external clients.
- Manual end-to-end testing on Android emulator and/or physical device.
- External API tests only when explicitly configured; keep regular tests deterministic and credential-free.
