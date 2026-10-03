# ADR-005: Standalone Flutter Web/PWA frontend

- Status: Accepted
- Date: 2026-10-03

## Context

The weather station is intended to serve both the host club and nearby users. The club website and existing mobile application are maintained independently, so requiring their maintainer to implement and maintain weather visualisation would create an unnecessary dependency.

The project should also remain reusable by other clubs and communities.

## Decision

Provide a standalone responsive frontend built with Flutter Web and installable as a Progressive Web App (PWA).

The frontend consumes only the public versioned REST API and has no dependency on MeshCore internals.

Two presentation modes are planned:

1. `/` - full responsive weather dashboard / installable PWA;
2. `/embed` - compact view designed for embedding in an external website using an iframe.

The full dashboard is the primary product surface. The embed view is a lightweight projection of the same application and data model.

## User experience

The dashboard should prioritise locally measured conditions:

- temperature;
- relative humidity;
- atmospheric pressure and pressure trend;
- average wind speed;
- wind gusts;
- wind direction;
- rainfall (current / 1 h / daily as available);
- observation timestamp and station freshness.

Historical views should provide useful charts over periods such as 6 h, 24 h, 7 d and 30 d.

The UI may calculate descriptive trends from observations, but should clearly distinguish measured values from forecasts. The initial product does not require an external weather-forecast provider.

## Local-weather positioning

The PWA is intended to provide hyperlocal observations around the installation site. It should make clear that values are measured at the station rather than interpolated forecasts.

Wind can vary substantially over short distances due to terrain and buildings, so the UI should identify the station/site rather than imply that wind measurements represent an entire surrounding area.

## Distribution

- no user account or login is required for public weather viewing;
- installation as a PWA is optional;
- the same deployment supports desktop, tablet and mobile browsers;
- third-party websites can embed `/embed` with minimal integration effort.

## Open-source reuse

Station identity and UI labels must be configuration-driven so deployments are not tied to the original archery-club use case.

Example configuration:

```yaml
station:
  id: field-01
  name: "Terrain de tir"

ui:
  title: "Météo du terrain"
  show_pressure: true
  show_rain: true
  show_station_health: true
```

## Consequences

The repository gains a `webapp/` component. The public API becomes a stable integration boundary shared by the PWA, iframe view and any third-party mobile/web application.
