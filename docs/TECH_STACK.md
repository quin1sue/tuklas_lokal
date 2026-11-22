# Technology Stack and Decision Status

This document records proposed tools and why they are being considered. Treat status as tentative until the project validates API costs, terms, availability, and implementation constraints.

| Area | Proposed technology | Status / rationale |
|---|---|---|
| Mobile | Flutter + Dart | Recommended; cross-platform framework, Android-first |
| Backend | FastAPI + Python | Recommended; familiar, typed API and AI orchestration |
| Backend database | None in MVP | Deliberately excluded; no current persistence requirement |
| Curated backend data | JSON | Small app-owned data, version-controlled |
| Maps | Google Maps Flutter SDK | Candidate; validate cost, key restrictions, project needs |
| Places | Google Places API (New) | Candidate; rich listings/details, validate billing and policies |
| AI vision | Gemini API | Candidate; multimodal image clue extraction |
| HTTP client (Flutter) | Select during setup; package decision documented | Avoid premature dependency choice |
| HTTP client (backend) | httpx | Candidate for async integration calls |
| Location | Geolocator | Candidate for foreground device location |
| Flutter state | Riverpod | Candidate; evaluate learning/complexity |
| Navigation | GoRouter | Candidate; route-based navigation |
| Local storage | Shared preferences initially; SQLite/Drift only if complexity warrants | Keep MVP light |
| Image picking | Image Picker or camera package | Choose based on camera/gallery MVP behavior |
| Backend validation | Pydantic | FastAPI contracts and AI output validation |
| Tests | Flutter test; pytest | Standard testing tools |
| Containerization | None initially | Docker not needed for local academic demo |

## API provider considerations
### Google Maps SDK / Places
- Map SDK and Places API are separate products/configurations.
- Billing and quota requirements must be checked before implementation.
- Use restricted API keys and separate keys where appropriate.
- Request only necessary fields to control cost.
- Review current caching, storage, attribution, and content-use restrictions.
- Do not store Google-derived records as an independent permanent dataset.
- Reviews and photos may have extra attribution/field-cost requirements.

### Gemini
- Verify current stable model ID, image support, structured output support, quotas, pricing, and data-use terms at implementation time.
- Keep Gemini calls in backend.
- Validate output and treat model output as untrusted.
- Vision model extracts clues; it does not determine verified address or coordinates.

## Avoid in MVP
- Firebase unless a specific need is demonstrated.
- Supabase.
- PostgreSQL.
- Backend SQLite.
- ORM.
- Redis.
- Docker.
- Authentication framework.
- Complex event bus or microservices.
- Generic repository abstractions that do not solve an actual problem.
