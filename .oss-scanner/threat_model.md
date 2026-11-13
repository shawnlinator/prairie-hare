# Threat model

## What this project does and where untrusted input enters
A web client for the Matrix chat protocol (TypeScript/React). Untrusted input:
Matrix homeserver content (message bodies, HTML-formatted events, media and
file attachments, avatars, display names), room/alias metadata, and external
links the client renders or previews.

## Components that matter most / least
- most: the event/HTML rendering and sanitization path, media/file download
  handling, session-token auth (login, refresh, storage), and link-preview /
  URL handling.
- less: the settings UI, localization, test fixtures, and build tooling.

## How to exercise it
Log a test client into a homeserver you control and send it crafted events
(HTML bodies, malformed or hostile media, crafted link-preview targets); also
exercise the auth/session-token storage and refresh paths.

## How you rate severity
- XSS via event rendering or link preview: critical.
- Session-token theft or auth bypass: critical. SSRF via link preview or
  media proxy: high. Local data exposure: medium.

## Anything to leave alone
- Do not report PWA/service-worker caching behavior or test fixtures as
  vulnerabilities; focus on the rendering and auth paths.
