# AI Place Scanner

## Goal
Given a photo of an establishment, storefront, landmark, or attraction, produce clues and search for plausible real listings. The scanner should help identify a place, not promise perfect visual geolocation.

## Proposed pipeline
1. Flutter obtains photo from camera/gallery.
2. UI displays image-upload/AI processing disclosure.
3. Client optionally supplies foreground coordinates or manually selected area.
4. FastAPI validates file and request.
5. Vision model extracts structured clues.
6. Backend uses clues as search terms for the place provider.
7. Candidate matcher scores provider candidates deterministically.
8. Backend applies confidence policy.
9. Flutter displays likely match, candidates, or no confident match.
10. User opens a provider-backed place detail.

## AI extraction schema (conceptual)
```json
{
  "place_type": "cafe",
  "visible_business_name": "Example Cafe",
  "visible_text": ["Example Cafe", "Coffee"],
  "visual_description": "Small cafe with a blue storefront",
  "identifying_clues": ["Readable sign above the entrance"],
  "search_keywords": ["Example Cafe", "coffee shop"],
  "needs_location_search": true
}
```
Use a validated Pydantic model. Treat every field as uncertain model output. Do not accept AI-generated address/coordinates as verified listing data.

## Candidate scoring
Candidate matching is deterministic and unit-testable. Potential signals:
- Visible business-name similarity.
- OCR/text clue match.
- Category/type compatibility.
- Geographic distance from user-supplied location, if available.
- Candidate query/source relevance.

Avoid pretending the scoring formula is universally reliable. Tune using test fixtures. Explain confidence categories and threshold behavior. A score is an internal heuristic, not a probability unless calibrated.

## Result policy
- High confidence: present a likely match, but allow user to inspect details.
- Medium confidence: present ranked candidates and label as possible matches.
- Low confidence: do not assert an identification; offer broader search/retry/manual area.
- No provider results: say no matching listing was found, not that the place does not exist.

Thresholds must be set and tested in an implementation task; they are not hardcoded by this document.

## Privacy and reliability
- Inform user photo is sent to external AI service.
- Avoid photos with private/sensitive content.
- Validate file format, size, and upload limits.
- Avoid saving image uploads unless there is a clear approved requirement.
- Do not log image content or secrets.
- Handle AI timeout, quota, malformed output, provider errors, and empty candidate results.
- Allow retry and, where practical, cancellation.

## Provider-policy boundary
Do not send Google Places content (reviews, descriptions, ratings, photos, etc.) to Gemini for summarization, transformation, or derivative generation unless current Google terms explicitly allow it and the project decision log is updated. Scanner vision input is user-provided image data; search clues from that image can be used to query the provider.
