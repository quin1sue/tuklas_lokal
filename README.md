# Tuklas Lokal

**Discover local places. Find your next experience.**

Tuklas Lokal is an Android-first, AI-assisted local discovery mobile app for identifying places from photos, discovering nearby places based on interests, and comparing destinations. It is a class project and is not intended for public hosting.

## Core experiences
- **Identify:** Scan/upload an image and find plausible real-world place matches.
- **Discover:** Explore nearby places, categories, interests, and curated recommendations.
- **Decide:** Compare selected places using factual attributes.

## Intended MVP
See [`docs/MVP.md`](docs/MVP.md) and [`docs/PRD.md`](docs/PRD.md).

## Proposed stack (decisions subject to validation)
- Flutter + Dart mobile app
- FastAPI + Python local backend
- Google Maps Flutter SDK for map rendering (provider decision needs final validation)
- Google Places API (New) for search and place details (provider decision needs final validation)
- Gemini API for image clue extraction and optional natural-language search parsing
- Geolocator for location permissions and coordinates
- Device-local preferences and data; no account system
- Curated editorial content in JSON initially; no backend database in MVP

## Development environment
- Backend runs locally on the developer machine.
- Android emulator can reach host backend using `10.0.2.2` (Android emulator convention); confirm environment configuration.
- Physical Android device can reach host backend via the computer's LAN address while both are on the same network; do not assume the address is fixed.
- External APIs require internet and appropriate API credentials.
- No Docker requirement for MVP.

## Docs
- `docs/PRD.md`: product requirements and scope
- `docs/MVP.md`: MVP scope, exclusions, and acceptance criteria
- `docs/ARCHITECTURE.md`: system architecture and data flow
- `docs/TECH_STACK.md`: technologies and decision status
- `docs/AI_SCANNER.md`: scanner workflow and confidence handling
- `docs/ROADMAP.md`: staged vertical slices
- `docs/DECISIONS.md`: decision log
- `docs/PROJECT_SETUP.md`: verified local environment, commands, and known blockers

## Agent instructions
Read root [`AGENTS.md`](AGENTS.md) before implementing. Keep work vertical, small, testable, and educational. Do not generate the full app in one pass.
