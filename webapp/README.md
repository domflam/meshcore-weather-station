# Weather Web App

## Purpose

A reusable Flutter Web application and installable PWA for displaying hyperlocal observations produced by MeshCore Weather Station.

It is intentionally independent from the host organisation's website or native mobile application.

## Planned routes

### `/`

Full responsive dashboard / PWA.

Initial information architecture:

- current conditions;
- temperature and daily min/max;
- humidity;
- atmospheric pressure and trend;
- wind average, gust and direction;
- rainfall;
- station freshness (`online`, `stale`, `offline`);
- historical charts (6 h / 24 h / 7 d / 30 d);
- optional technical station information such as battery level in an advanced/status view.

### `/embed`

Compact, responsive dashboard intended for iframe integration into an existing website.

It should expose the most useful current conditions and a link to the full dashboard while requiring no knowledge of MeshCore or the cloud implementation.

Example integration:

```html
<iframe
  src="https://weather.example.org/embed"
  width="100%"
  height="650"
  loading="lazy"
  frameborder="0">
</iframe>
```

## Data source

The application consumes the public REST API only:

```text
GET /api/v1/weather/latest?station_id=field-01
GET /api/v1/weather/history?station_id=field-01&hours=24
```

No GCP or MeshCore credentials are stored in the client.

## PWA behaviour

The application should be installable on supported mobile/desktop browsers. A service worker may cache the application shell and the most recent successful observation so the UI can degrade gracefully during temporary connectivity loss.

Cached observations must always retain their original timestamp and must never be presented as live data.

## Product principles

- mobile first, responsive everywhere;
- no login for public weather data;
- clear station name and last-update time;
- measured data clearly distinguished from forecasts;
- configuration-driven branding and station identity;
- useful charts without turning the UI into an engineering dashboard;
- technical details available without dominating the public view.

## Status

Architecture/design only. Flutter project scaffolding will follow after the end-to-end sensor/telemetry path is validated on LAB-01.
