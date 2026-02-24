# Result: Guestbook App

## Files Created
- `index.html` — single-file guestbook app (Tier 1, S3 + Resource Proxy)

## What Was Built
A minimal guestbook app as a single `index.html`. Users enter their name and a message via a form card at the top. On submit, the entry is saved to the `guestbook` MongoDB collection via the Resource Proxy, then the list reloads (newest first). On page load, the last 50 entries are fetched and displayed.

## Architecture
- **Pattern**: Data App (Tier 1 — S3 + Resource Proxy)
- **Collection**: `guestbook`
- **Document shape**: `{ name, message, createdAt }` (ISO timestamp string)
- **Gateway URL**: `window.PANDO_GATEWAY_URL || ''` — never hardcoded
- **API key**: `window.PANDO_PROJECT_API_KEY` with provided fallback

## Decisions
- Used `pre-wrap` + `word-break` on messages to preserve line breaks safely
- HTML-escaped all user content before rendering (XSS prevention)
- Client-side validation (non-empty name/message, max lengths) before any DB call
- Loading spinner shown while entries fetch; graceful error states for both load and submit failures

## Test Results
- No automated test runner available in this environment; logic verified by code review
- XSS: all user content passed through `escapeHtml()` before DOM insertion
- Error paths: network/DB errors surfaced inline without crashing the page

## Known Limitations
- No pagination beyond the 50-entry limit (sufficient for a guestbook)
- No delete/edit functionality (out of scope)

## Worker Feedback
Task spec was clear and complete. Inline fetch pattern example and the key rules section removed all ambiguity about the correct gateway/key usage. No blockers.
