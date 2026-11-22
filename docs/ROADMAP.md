# Roadmap — Vertical Slices

Build in small, demonstrable increments. Each slice should have an observable result, acceptance criteria, tests where practical, and a brief learning checkpoint.

## Phase 0 — Project decisions and environment
- Confirm Flutter/Dart versions and Android development environment.
- Confirm backend Python version and package manager.
- Validate provider choices, API terms, billing, quotas, and restricted key setup.
- Create repository, root AGENTS.md, docs, and `.gitignore`.
- Create basic app and backend skeleton only when ready.

**Exit:** App and backend run locally; no secrets committed; project docs reflect validated decisions.

## Phase 1 — App shell (vertical slice)
- Flutter app starts.
- Theme/branding baseline.
- Main navigation.
- Placeholder feature destinations with minimal useful layout.
- Backend health endpoint and client connectivity.

**Exit:** User can launch app, navigate, and see backend connectivity status.

## Phase 2 — Place search foundation
- Backend configuration and provider client.
- Typed place-search contract.
- Search by text/category.
- Display results in a basic list.
- Error/empty/loading handling.

**Exit:** Search works with provider data and clear failure states.

## Phase 3 — Place details
- Detail contract and provider retrieval.
- Detail screen.
- Attribution.
- Add to comparison action.
- Test unavailable fields and provider errors.

**Exit:** A user opens a search result and sees a valid provider-backed detail.

## Phase 4 — Nearby Discover
- Foreground location permission.
- Nearby search.
- Radius selection.
- Category browse.
- Map/list view.
- Manual area fallback.

**Exit:** User can discover nearby places or search another area without location access.

## Phase 5 — Local preferences
- Interest selection.
- Persist interests on device.
- Deterministic ranking/reordering using selected interests and supported fields.
- Favorites persistence.

**Exit:** Interests and favorites survive app restart and affect app experience.

## Phase 6 — Compare
- Add/remove compare candidates.
- Limit to 2–3.
- Side-by-side fields.
- Missing-data handling and truthful comparison highlights.

**Exit:** User can compare selected places and understand differences.

## Phase 7 — AI Scanner (vertical slice)
- Image picker/camera.
- Upload notice.
- Backend image upload endpoint.
- Gemini clue extraction.
- Place search.
- Candidate ranking.
- Confidence/no-match policy.
- Candidate result UI.
- Tests with mocked AI/provider clients.

**Exit:** End-to-end photo identification demo works with failure/uncertainty paths.

## Phase 8 — Curated discovery (time permitting)
- Curated JSON schema and sample data independently sourced/created.
- Featured/Hidden Gems endpoint.
- UI section.
- Editorial labels and data source handling.

**Exit:** Curated items render and can link to valid place details where appropriate.

## Phase 9 — Stabilization and presentation
- Accessibility and UI polish.
- Error states.
- API quota and cost safeguards.
- Demo script and test checklist.
- Verify Android build.
- Document setup, environment, limitations, and known issues.

## Post-MVP backlog (not to implement unless approved)
- Natural-language discovery query.
- Discovery Trails.
- Events.
- Community submissions/reviews/tips.
- Collections beyond basic local favorites.
- Visited/recent history.
- Offline features.
- Challenges/gamification.
- Additional platforms.
