# Weather API - Initial Contract

This document defines the proposed external contract. It is intentionally independent of MeshCore.

## Ingestion

`POST /api/v1/ingest`

Example payload:

```json
{
  "station_id": "archery-01",
  "timestamp": "2026-10-03T11:45:00Z",
  "temperature_c": 13.7,
  "humidity_pct": 71.0,
  "pressure_hpa": 1017.8,
  "wind_avg_kmh": 8.4,
  "wind_gust_kmh": 21.7,
  "wind_direction_deg": 247,
  "rain_1h_mm": 0.0,
  "rain_24h_mm": 1.4,
  "battery_v": 3.91,
  "solar_v": null,
  "rssi_dbm": -102,
  "snr_db": 7.5
}
```

The ingestion endpoint is authenticated. Authentication details will be selected during implementation and must not be embedded in the remote RAK4631 node.

## Latest observation

`GET /api/v1/weather/latest?station_id=archery-01`

Example response:

```json
{
  "station_id": "archery-01",
  "updated_at": "2026-10-03T11:45:00Z",
  "status": "online",
  "age_minutes": 7,
  "weather": {
    "temperature_c": 13.7,
    "humidity_pct": 71.0,
    "pressure_hpa": 1017.8,
    "wind_avg_kmh": 8.4,
    "wind_gust_kmh": 21.7,
    "wind_direction_deg": 247,
    "wind_direction_label": "WSW",
    "rain_1h_mm": 0.0,
    "rain_24h_mm": 1.4
  }
}
```

Suggested freshness policy:

- `online`: observation < 30 minutes old
- `stale`: 30 minutes to 2 hours old
- `offline`: > 2 hours old

## History

Proposed endpoints:

- `GET /api/v1/weather/history?station_id=archery-01&hours=24`
- `GET /api/v1/weather/history?station_id=archery-01&days=7`

## Compatibility

Breaking changes require a new API version. Fields may be added to a versioned response without changing the meaning of existing fields.
