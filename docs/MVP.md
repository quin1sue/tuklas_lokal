# Tuklas Lokal — MVP Definition

## MVP objective
Deliver a coherent Android-first application demonstrating the three product pillars: Identify, Discover, and Decide. The MVP should be achievable as a class project and should not require public hosting, user accounts, or a backend database.

## In-scope MVP features

### A. App shell and navigation
- App branding and basic onboarding/permission explanation.
- Main navigation for Discover, Identify/Scanner, and Compare (Compare may be a destination or accessible action).
- Responsive layouts for common Android phone sizes.
- Consistent loading, error, empty, and permission states.

### B. Location
- Request foreground location only when a feature needs it.
- Explain why location is requested.
- Current-location nearby search.
- Manual location/area fallback when permission is denied, GPS is unavailable, or user wants another area.
- Radius presets (suggested: 1, 3, 5, 10 km; validate UI).
- Avoid background location.

### C. Discover
- Nearby places feed.
- Search by name/text.
- Browse a modest category list (initial candidates: food, cafes, nature, scenic spots, shopping, attractions, study spots, hangout).
- Map/list toggle.
- Basic category and radius filters.
- Open place detail.
- Interest preferences stored locally and used in deterministic ranking.
- Featured/Hidden Gems section powered by independent curated JSON content if time permits.
- Clear indication of provider vs curated content.

### D. Place detail
- Name, category, address, coordinates/map placement where provided.
- Rating/count and price level when available.
- Opening hours/status when available.
- Photos and reviews only if provider access, API costs, and attribution requirements are validated.
- Save favorite locally.
- Add/remove from Compare.
- Missing fields handled gracefully.

### E. Identify — AI place scanner
- Capture photo or select from gallery.
- Show consent/notice that photo is sent to external AI for analysis.
- Optional location context; manual area fallback.
- Backend image clue extraction using vision AI.
- Search real provider listings using clues.
- Deterministic candidate scoring/ranking.
- Display likely candidate(s) with match explanation/uncertainty.
- No-match state and retry.
- Open result detail.
- Never fabricate address, coordinates, rating, hours, or listing.

### F. Decide — Compare
- Compare 2–3 places (default proposal; confirm in UI task).
- Comparison view for fields available across selected candidates.
- Add/remove candidates.
- Handle missing data.
- Comparison is based on factual provider fields and local curated metadata; no AI-generated scores in initial MVP.

### G. Local personalization
- Interests/preferences.
- Favorites.
- Persistence on device.
- No account or cloud sync.
- Collections, visited history, and recently viewed are optional and may be deferred if schedule is tight; do not add database to support them.

### H. Backend
- FastAPI local backend.
- Health endpoint.
- Typed/validated request and response contracts.
- Gemini vision integration.
- Place-provider search/details integration (provider decision remains to be confirmed).
- Scanner orchestration and matching/ranking logic.
- Curated JSON content endpoint if curated discovery is in MVP.
- Configuration through environment variables.
- Friendly, structured errors.
- No database.
- No public deployment.

## MVP exclusions
- Authentication/accounts.
- Cloud synchronization.
- Social features and public community submissions.
- Public reviews authored by app users.
- Moderation system.
- User profiles.
- Business-owner/admin portals.
- PostgreSQL, backend SQLite, ORM, migrations.
- Offline maps or guaranteed offline search.
- Turn-by-turn navigation.
- Events feed.
- Discovery Trails unless required for curated demo and scoped separately.
- Exploration challenges/gamification.
- Natural-language search (post-MVP unless core scanner/discovery are stable).
- Complex recommendation ML.
- AI-generated place descriptions from provider content.
- iOS polish or release deployment.
- Docker unless later justified.

## MVP acceptance checklist
### Discover
- [ ] User can launch app and enter Discover.
- [ ] User can search nearby based on permitted location.
- [ ] User can browse by category.
- [ ] User can search by place name/text.
- [ ] User can toggle map/list.
- [ ] User can use filters and radius.
- [ ] User can open a place detail.
- [ ] Denied location has a usable manual-location fallback.

### Identify
- [ ] User can take or choose a photo.
- [ ] Photo-processing notice is presented.
- [ ] Backend receives image and optional location context.
- [ ] AI response is schema-validated.
- [ ] Provider candidates are searched.
- [ ] Candidates are ranked by tested deterministic logic.
- [ ] Low-confidence/no-match results are handled honestly.
- [ ] User can open a candidate detail.

### Decide
- [ ] User can add places to comparison.
- [ ] User can compare at least two; up to three if design supports it.
- [ ] User can remove comparison items.
- [ ] Missing data is clearly shown.
- [ ] Comparison claims are grounded in available data.

### Personalization
- [ ] User can select interests.
- [ ] Interests persist across app restart.
- [ ] User can favorite a place.
- [ ] Favorites persist across app restart.
- [ ] No account is required.

### Reliability/security
- [ ] API keys are not committed.
- [ ] Backend secret keys are not embedded in Flutter.
- [ ] API failure states are handled.
- [ ] No unverified claim that a feature works; tested flows documented.
- [ ] Project builds and test checks pass for implemented slices.

## Recommended MVP ordering
See `ROADMAP.md`. Do not attempt to build every section in parallel.
