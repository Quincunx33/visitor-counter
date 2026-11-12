<div align="center">

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=28&pause=1000&color=36BCF7&center=true&vCenter=true&width=500&lines=Visitor+Counter;Simple.+Lightweight.+Dynamic." alt="Visitor Counter" />

**A tiny API to count visits and show them anywhere.**

<img src="https://viscount-inzm9sse.manus.space/api/v1/counter/quincunx33.svg?theme=aurora&size=large&v=20261113" alt="Visitor count" />

[![API](https://img.shields.io/badge/API-Live-36BCF7?style=flat-square)](https://viscount-inzm9sse.manus.space/api/v1/docs)
[![License](https://img.shields.io/badge/License-MIT-blue?style=flat-square)](LICENSE)

</div>

# GitHub Profile Visitor Counter

A public visitor-counter API for GitHub profiles, websites, blogs and README files. It returns JSON counters, premium animated SVG badges and public statistics. The service also includes an interactive Swagger UI and protected owner/admin endpoints.

## Base URL

```text
https://viscount-inzm9sse.manus.space
```

All examples below use the username `quincunx33`. Replace it with any valid GitHub username containing 1–39 letters, numbers or hyphens.

## Quick start

### JSON counter

```bash
curl https://viscount-inzm9sse.manus.space/api/v1/counter/quincunx33
```

Example response:

```json
{
  "username": "quincunx33",
  "total": "97723",
  "event": {
    "recorded": true,
    "reason": "counted"
  }
}
```

### GitHub README badge

```markdown
![Profile views](https://viscount-inzm9sse.manus.space/api/v1/counter/quincunx33.svg?theme=aurora&v=1)
```

GitHub caches remote images. Change the `v` query value when you need GitHub to request a fresh badge image.

### HTML embed

```html
<img
  src="https://viscount-inzm9sse.manus.space/api/v1/counter/quincunx33.svg?theme=aurora&size=large"
  alt="GitHub profile views"
/>
```

## Public API endpoints

| Method | Endpoint | Purpose | Authentication |
|---|---|---|---|
| `GET` | `/api/v1/counter/:username` | Record a visit and return the current JSON total | None |
| `GET` | `/api/v1/counter/:username.svg` | Record a visit and return an animated SVG badge | None |
| `GET` | `/api/v1/stats/:username` | Return public counter statistics | None |
| `GET` | `/api/v1/health` | Check service and database health | None |
| `GET` | `/api/v1/docs` | Open interactive Swagger API documentation | None |
| `GET` | `/api/v1/openapi.json` | Download the OpenAPI 3.0 specification | None |

### 1. `GET /api/v1/counter/:username`

Records a view and returns JSON. Usernames are case-insensitive and returned in lowercase.

```bash
curl https://viscount-inzm9sse.manus.space/api/v1/counter/quincunx33
```

For retry-safe clients, send an optional `Idempotency-Key` header. The same key for the same username is suppressed for 60 seconds, preventing a refresh or network retry from counting repeatedly.

```bash
curl \
  -H 'Idempotency-Key: profile-readme-001' \
  https://viscount-inzm9sse.manus.space/api/v1/counter/quincunx33
```

**Successful response:**

```json
{
  "username": "quincunx33",
  "total": "97723",
  "event": {
    "recorded": true,
    "reason": "counted"
  }
}
```

When a duplicate request is suppressed, `event.recorded` is `false` and `event.reason` identifies the suppression. The total is returned as a string so large counts remain safe across clients.

**Headers:**

| Header | Required | Rules |
|---|---:|---|
| `Idempotency-Key` | No | Maximum 128 characters; use a stable retry/request ID. |

**Common responses:**

| Status | Meaning |
|---:|---|
| `200` | Counter returned and visit processed or deduplicated |
| `400` | Invalid GitHub username or invalid idempotency key |
| `429` | Public rate limit exceeded |
| `500` | Unexpected server error |

### 2. `GET /api/v1/counter/:username.svg`

Records a visit and returns a premium animated SVG badge with rolling number animation, aurora/glass motion, animated gradient shine, border motion and milestone effects.

```bash
curl -L \
  'https://viscount-inzm9sse.manus.space/api/v1/counter/quincunx33.svg?theme=aurora&size=large&label=Profile%20Views'
```

#### SVG query parameters

| Parameter | Values | Default | Description |
|---|---|---|---|
| `theme` | `aurora`, `violet`, `ice`, `mono` | `aurora` | Select the badge color palette. |
| `mode` | `dark`, `light` | `dark` | Select dark glass or light glass styling. |
| `size` | `compact`, `default`, `large` | `default` | Select badge scale. |
| `label` | Letters, numbers, spaces, `.`, `_`, `-`; max 24 characters | Username | Custom text shown on the left side of the badge. |
| `v` | Any cache-busting value | None | Optional value for refreshing cached GitHub README images. It does not change the counter. |

`v` is ignored by the API renderer and is safe to change whenever a README image needs refreshing.

#### Badge examples

**Aurora default:**

```markdown
![Profile views](https://viscount-inzm9sse.manus.space/api/v1/counter/quincunx33.svg?theme=aurora)
```

**Violet large badge with custom label:**

```markdown
![Profile views](https://viscount-inzm9sse.manus.space/api/v1/counter/quincunx33.svg?theme=violet&size=large&label=Profile%20Views&v=2)
```

**Ice light badge:**

```markdown
![Profile views](https://viscount-inzm9sse.manus.space/api/v1/counter/quincunx33.svg?theme=ice&mode=light&v=3)
```

**Compact monochrome badge:**

```markdown
![Profile views](https://viscount-inzm9sse.manus.space/api/v1/counter/quincunx33.svg?theme=mono&size=compact&label=Views&v=4)
```

Milestone celebration effects activate around `100,000`, `250,000`, `500,000` and `1,000,000` views. The badge also respects `prefers-reduced-motion` where supported. GitHub may display the SVG as a remote image, so hover interaction is available in direct web embedding but may not be interactive inside a GitHub-rendered README image.

### 3. `GET /api/v1/stats/:username`

Returns public statistics for the username without exposing IP addresses, user-agent strings, credentials or internal database IDs.

```bash
curl https://viscount-inzm9sse.manus.space/api/v1/stats/quincunx33
```

The response contains the displayed total, recorded event total and recent UTC daily counts. The endpoint is intended for dashboards and lightweight analytics displays.

### 4. `GET /api/v1/health`

Returns service health without authentication.

```bash
curl https://viscount-inzm9sse.manus.space/api/v1/health
```

Healthy response:

```json
{"status":"ok"}
```

A database or service failure returns a generic `503` response without internal error details.

### 5. `GET /api/v1/docs`

Interactive Swagger UI:

<https://viscount-inzm9sse.manus.space/api/v1/docs>

The UI documents the public counter, animated SVG badge, statistics, health and admin overview routes. It is responsive for mobile browsers.

### 6. `GET /api/v1/openapi.json`

Machine-readable OpenAPI 3.0 specification:

<https://viscount-inzm9sse.manus.space/api/v1/openapi.json>

Use this URL with API clients, documentation tools or code generators.

## Protected admin API

Admin endpoints are not needed for normal README or website embeds. They require either HTTP Basic authentication (`admin` as the username and the private `ADMIN_API_KEY` as the password) or:

```http
Authorization: Bearer YOUR_ADMIN_API_KEY
```

Never put the admin key in a GitHub repository, frontend JavaScript, URL, query string or public issue.

### Admin endpoint list

| Method | Endpoint | Purpose |
|---|---|---|
| `GET` | `/api/v1/admin/overview` | Dashboard totals, event summaries, request aggregates and health information |
| `GET` | `/api/v1/admin/counters` | Paginated counter list with optional search |
| `GET` | `/api/v1/admin/events` | Paginated visitor-event list with optional search |
| `GET` | `/api/v1/admin/owner-config` | Read the owner baseline configuration |
| `PUT` | `/api/v1/admin/owner-config` | Update the owner baseline while preserving event history |
| `GET` | `/admin` | Open the browser dashboard after authentication |

### Admin examples

```bash
# Overview
curl -u "admin:${ADMIN_API_KEY}" \
  https://viscount-inzm9sse.manus.space/api/v1/admin/overview

# Counters: page 1, 20 rows, search for a username
curl -H "Authorization: Bearer ${ADMIN_API_KEY}" \
  'https://viscount-inzm9sse.manus.space/api/v1/admin/counters?page=1&pageSize=20&search=quincunx'

# Events: page 2, 50 rows
curl -H "Authorization: Bearer ${ADMIN_API_KEY}" \
  'https://viscount-inzm9sse.manus.space/api/v1/admin/events?page=2&pageSize=50'

# Read owner baseline
curl -H "Authorization: Bearer ${ADMIN_API_KEY}" \
  https://viscount-inzm9sse.manus.space/api/v1/admin/owner-config

# Update owner baseline (example only)
curl -X PUT \
  -H "Authorization: Bearer ${ADMIN_API_KEY}" \
  -H 'Content-Type: application/json' \
  -d '{"startingCount":97723}' \
  https://viscount-inzm9sse.manus.space/api/v1/admin/owner-config
```

For admin pagination, `page` starts at `1`, `pageSize` defaults to `20` and accepts values up to `100`. `search` is optional and searches usernames. The owner `startingCount` must be an integer from `0` to `2,147,483,647`.

## Rate limits and caching

- Public API requests are rate-limited per client IP over a 15-minute window.
- Admin requests have a separate, stricter rate limit.
- JSON counter responses are not cached.
- SVG badges and public statistics use short public caches; use a changed `v` value for README image cache refreshes.
- The short-lived idempotency/fingerprint guard prevents accidental duplicate retries; it is not a guarantee of unique human visitors.

## Privacy and security

Raw IP addresses, authorization headers, admin keys and user-agent strings are not stored in analytics. When no `Idempotency-Key` is supplied, the service uses a short-lived privacy-preserving fingerprint for retry suppression. Public statistics do not expose internal IDs or personal request data. All admin changes are validated and audited.

## License

<div align="center">

Made with care by **[Quincunx33](https://github.com/Quincunx33)** · [MIT License](LICENSE)

</div>
