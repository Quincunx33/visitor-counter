<div align="center">

# Visitor Counter

**A simple API-powered visitor counter for GitHub profiles and websites.**

Track profile visits, display a lightweight SVG badge, and access visitor data through a REST API.

[![Live API](https://img.shields.io/badge/API-Live-brightgreen?style=for-the-badge)](https://viscount-inzm9sse.manus.space/api/v1/health)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)](LICENSE)

</div>

---

## Overview

Visitor Counter provides a simple way to display visit counts on a GitHub profile, README, or website. Each account is identified by a username and can use the API to retrieve its counter or render the count as an SVG badge.

**Hosted API:**  
[https://viscount-inzm9sse.manus.space](https://viscount-inzm9sse.manus.space)

## Features

- **Visitor count API** — retrieve the counter for a username.
- **SVG badge** — embed a dynamically generated counter badge in Markdown or HTML.
- **Statistics endpoint** — request account statistics.
- **Simple HTTP interface** — use the endpoints from a browser, website, or other application.
- **API documentation** — access the hosted API documentation.
- **Health endpoint** — check the service health.

## Use the counter

### GitHub profile README

Add this Markdown to your profile README. Replace `YOUR_USERNAME` with the username registered with the service.

```html
<img
  src="https://viscount-inzm9sse.manus.space/api/v1/counter/YOUR_USERNAME.svg"
  alt="Profile visitor count"
/>
```

For example, the badge for the `quincunx33` account:

<div align="center">

<img src="https://viscount-inzm9sse.manus.space/api/v1/counter/quincunx33.svg" alt="Quincunx33 visitor count"/>

</div>

## API Reference

**Base URL**

```text
https://viscount-inzm9sse.manus.space
```

| Method | Endpoint | Description |
|:--|:--|:--|
| `GET` | `/api/v1/counter/:username` | Retrieve a user's visitor counter |
| `GET` | `/api/v1/counter/:username.svg` | Get the visitor counter as an SVG badge |
| `GET` | `/api/v1/stats/:username` | Retrieve a user's statistics |
| `GET` | `/api/v1/docs` | View API documentation |
| `GET` | `/api/v1/health` | Check API health |

### Example requests

**Get a visitor count**

```http
GET https://viscount-inzm9sse.manus.space/api/v1/counter/quincunx33
```

**Get the SVG badge**

```http
GET https://viscount-inzm9sse.manus.space/api/v1/counter/quincunx33.svg
```

**Get account statistics**

```http
GET https://viscount-inzm9sse.manus.space/api/v1/stats/quincunx33
```

**Check service health**

```http
GET https://viscount-inzm9sse.manus.space/api/v1/health
```

See the live [API documentation](https://viscount-inzm9sse.manus.space/api/v1/docs) for the current response formats and usage details.

## Links

- **Live service:** https://viscount-inzm9sse.manus.space
- **API documentation:** https://viscount-inzm9sse.manus.space/api/v1/docs
- **Health check:** https://viscount-inzm9sse.manus.space/api/v1/health

## License

This project is licensed under the [MIT License](LICENSE).
