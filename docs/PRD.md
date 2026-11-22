# Product Requirements Document (PRD)
## Tuklas Lokal

**Status:** Initial product baseline; provider and implementation decisions are provisional where marked.
**Product type:** Android-first mobile application / academic project
**Hosting:** No public deployment planned
**Accounts:** None

## 1. Product vision
Tuklas Lokal helps people discover, identify, and choose local establishments, attractions, and experiences. It combines place search and real provider data with AI-assisted image understanding and curated discovery content.

## 2. Problem statement
People may know they want to visit somewhere but do not know what nearby options exist, what a place in a photo is called, or how to compare alternatives. Conventional map/search tools are primarily query-driven. Tuklas Lokal emphasizes exploratory discovery, image-based identification, interest-oriented recommendations, and decision support.

## 3. Target users
- Residents exploring their own city or nearby areas.
- Visitors looking for establishments and attractions.
- Users who have a photo of a place but do not know its name.
- Users deciding between several candidate destinations.
- Academic/demo users evaluating the app.

The MVP does not target professional venue owners or business administrators.

## 4. Product goals
1. Identify a photographed establishment or landmark with transparent uncertainty.
2. Make nearby discovery approachable through categories, interests, and curated sections.
3. Present reliable place information sourced from a place provider.
4. Help users compare options without manually opening multiple place pages.
5. Work without accounts and keep personal selections on-device.
6. Demonstrate a coherent end-to-end product with a manageable student-project scope.

## 5. Non-goals
- Public production hosting or commercial launch.
- Account creation, login, authentication, profiles, or cloud synchronization.
- Social graph, follows, messaging, or account-based community.
- Business-owner dashboards or listing management.
- Public reviews submitted by users, public moderation, or public reputation.
- A permanent database of Google Places-derived content.
- AI-generated factual claims about businesses not grounded in provider results.
- Full turn-by-turn navigation.
- Guaranteed offline maps or offline search.
- Building a comprehensive Philippine POI dataset.
- Multi-platform polish for iOS in the first delivery; Android-first, while avoiding unnecessary platform lock-in.

## 6. Core pillars and user journeys

### 6.1 Identify
**User story:** As a user, I want to photograph a storefront, establishment, landmark, or attraction and see plausible place matches so that I can identify it.

Journey:
1. Open scanner from a clear app entry point.
2. Take a photo or select one from gallery.
3. Optionally provide current location or choose an area when location is unavailable/denied.
4. Backend analyzes image clues with vision AI.
5. Backend searches the place provider using extracted text/category and optional geographic context.
6. Backend ranks candidates using transparent, deterministic logic.
7. App shows one likely match or multiple candidates with confidence/uncertainty.
8. User opens a candidate's place detail.

Requirements:
- Explain image upload/third-party AI processing in UI before or at first use.
- Request only permissions needed for the chosen flow.
- Do not treat visual AI as a source of verified address or GPS location.
- Do not invent or fabricate match data.
- Provide a clear no-match / low-confidence state.
- User can retry with another image or location context.
- Image size/type handling and request failures are user-friendly.
- Provide a loading/progress state and a cancel/retry path where feasible.

### 6.2 Discover
**User story:** As a user, I want to explore places nearby and browse by interest or category so I can find options without knowing a place name.

Requirements:
- Search places by name or text.
- Browse categories (MVP list to be finalized; initial suggestions: food, cafe, nature, scenic, shopping, attractions, study, hangout).
- Nearby discovery based on device location and a selectable radius.
- Manual area/location fallback when location permission is denied or unavailable.
- Interest selection stored locally.
- Discovery sections may include: Near You, Explore by Category, Recommended for Your Interests, Featured/Hidden Gems. These sections must be grounded in real provider data or our independently curated content.
- Basic filters such as category, radius, and available rating/price filters where provider supports them and they are useful.
- Empty, loading, permission-denied, offline, and API-error states.
- Place detail view.

Recommendations:
- Initial ranking is deterministic, explainable, and tested.
- AI is not required to rank nearby results.
- Ranking must not imply personal knowledge beyond locally stored preferences.
- Clearly distinguish curated/editorial content from provider content.

### 6.3 Decide
**User story:** As a user, I want to compare a few places in one view so I can decide which one fits my needs.

Requirements:
- Add/remove candidates to a comparison list from discovery or place details.
- Compare a small number of places (recommended limit: 2–3 for readability; confirm during UI implementation).
- Show common fields side by side when available: name, category, distance (if location available), rating and rating count, price level (if provided), opening status/hours (if available), selected features/tags, and address.
- Handle missing values honestly (e.g., “Not available”) instead of fabricating or silently treating missing values as zero.
- Highlight differences or strongest values only when comparisons are valid and the meaning is clear.
- Do not claim a universal “best” based on incomparable or missing data.
- Comparison may be client-side; it does not require a database or AI.

## 7. Place detail information
Display provider data when available and permitted:
- Place name
- Primary category
- Address
- Map/location
- Distance from current or selected location (computed only with valid coordinates)
- Rating and rating count
- Price level (if provider supplies it)
- Opening hours / open status (if provider supplies it)
- Photos with required attribution
- Reviews with required attribution and links/source where required
- Relevant curated tags/editorial notes clearly labeled as Tuklas Lokal content
- Actions: save favorite locally, add to compare, share place (native share sheet, if included in MVP)

Fields may be unavailable; UI must adapt.

## 8. Personalization without accounts
Personalization uses local device storage only:
- Interest/category preferences.
- Favorites.
- Optional recently viewed list.
- Optional visited-place marking.
- Optional collections if included in MVP.

No data synchronization between devices. Clearing app data/uninstalling may remove these records. Provide appropriate explanation if needed.

## 9. AI requirements and guardrails
- AI is used for visual clue extraction and, if later approved, natural-language intent parsing.
- AI output must be structured and validated by backend schemas.
- AI identifies clues, not guaranteed locations.
- AI may extract visible business name/signage, text, category, visual characteristics, and search keywords.
- Matching against real listings uses provider search and deterministic ranking.
- Candidate details such as address, ratings, reviews, hours, coordinates, and photos come from place-provider records.
- If matching is uncertain, show candidates and explain uncertainty rather than selecting a false positive.
- Do not feed provider content into AI to create derivative results unless current provider terms explicitly allow and the decision log is updated.
- Protect API keys and apply usage limits/rate limiting appropriate to local demo use.
- Avoid uploading sensitive/private images; scanner UX should state images are sent to an AI service for analysis.

## 10. Data sources and content
- Place-provider data: live searches and details; follow current provider terms, field masks, attribution, caching, and retention restrictions.
- Tuklas Lokal editorial/curated data: app-owned JSON initially.
- Personal data: local device only.
- User-generated public content: out of MVP.
- No backend database initially.

## 11. UX principles
- Discovery-oriented, friendly, locally relevant, clear and approachable.
- Prioritize place photos and concise information over dense text.
- Consistent map/list toggle and map markers where useful.
- Do not overload the interface with feature tiles or AI gimmicks.
- Always provide a fallback for denied location permission.
- Clearly label data sources and editorial/curated information.
- Honest empty, error, loading, and uncertainty states.
- Accessibility: legible text, adequate contrast, semantic controls, tap targets, and avoid conveying status only through color.

## 12. Success criteria for the MVP
Qualitative/demo criteria:
- A user can find nearby places without typing a specific name.
- A user can search by name/category and open a detail screen.
- A user can scan a photo and receive real candidate listings with honest uncertainty.
- A user can compare 2–3 candidates in a useful format.
- Interests and favorites persist locally after app restart.
- Main flows work in a stable demo environment with configured API keys.
- API errors, no results, and denied location permissions do not crash the app.
- The architecture remains small enough for a student to explain and maintain.

## 13. Risks and mitigations
| Risk | Mitigation |
|---|---|
| API billing/quotas | Check current pricing/quotas; field masks; restrictions; budget alerts; cap requests |
| AI false identification | Candidate matching; confidence thresholds; show alternatives; no fabricated facts |
| Location permission denial | Manual area fallback |
| API availability/network | Error and retry states; curated sample content for demo fallback |
| API terms/data storage | Follow provider restrictions; avoid permanent storage of provider content |
| Scope creep | MVP boundary and staged roadmap |
| Secrets exposed in mobile app | Keep server-side AI/Places credentials in backend env; restrict mobile map SDK key |
| Student project time | Vertical slices; prioritize three pillars |

## 14. Open decisions
These must be resolved through research/implementation rather than silently assumed:
- Final map/place provider (Google Maps + Places likely candidate; alternatives exist).
- Current provider pricing, quotas, and API field costs.
- Exact category taxonomy.
- Compare maximum (2 or 3).
- Radius presets and manual-location UX.
- Exact local storage package/approach.
- Whether curated Featured/Hidden Gems is included in initial MVP.
- Whether native sharing is in MVP.
- Natural-language search is post-MVP unless explicitly promoted.
- Whether trails/events/challenges are later features.
