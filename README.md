# MeshCore Weather Station

Open-source, solar-powered hyperlocal weather telemetry for remote sites using MeshCore and LoRa.

The reference deployment combines a RAK4631-based MeshCore repeater with low-power weather sensors. Telemetry crosses the MeshCore network to an Internet-connected gateway, which publishes observations to Google Cloud. A public REST API feeds a standalone Flutter Web/PWA dashboard, an embeddable view, and third-party applications without exposing MeshCore internals.

## Reference architecture

```text
Weather sensors
     |
RAK4631 / SX1262
MeshCore repeater
     |
  LoRa mesh
     |
MeshCore gateway
     |
 HTTPS / JSON
     |
Google Cloud Run
     |
  +-- BigQuery (history)
  |
  +-- Public REST API
          |
          +-- Flutter Web / PWA
          |       +-- full dashboard
          |       +-- /embed iframe view
          |
          +-- third-party Web / App
```

## Design principles

1. **Repeater first.** Weather telemetry must never compromise the availability of the MeshCore repeater.
2. **Low power.** The reference station targets a shared 5 W solar panel and a 1S Li-ion battery bank.
3. **Open interfaces.** Consumers integrate through a simple HTTPS/JSON API rather than MeshCore-specific protocols.
4. **Store useful weather, not noise.** Wind and rain are sampled/count continuously and aggregated before transmission.
5. **Observable infrastructure.** Battery voltage, link quality and station freshness are first-class operational data.
6. **Reusable by design.** Nothing in the architecture depends on the original archery-club deployment.
7. **Hyperlocal first.** The UI prioritises measurements made at the installation site and clearly distinguishes them from forecasts.
8. **Easy integration.** A host website can embed the compact dashboard without implementing weather logic itself.

## Reference hardware

The first field deployment is based on:

- D5L enclosure/base with RAK4631 (nRF52840 + SX1262)
- 868 MHz antenna, approximately 5 dBi
- short coaxial feed, approximately 20 cm
- 5 W south-facing solar panel with unobstructed exposure
- 4 x 3.7 V / 3000 mAh cells in parallel (1S4P, approximately 44.4 Wh nominal)
- BME280 temperature / humidity / pressure sensor
- cup anemometer (planned)
- wind vane (planned)
- tipping-bucket rain gauge (planned)

A 1 W RF power amplifier is deliberately **not** part of the baseline. Good antenna placement, line of sight and low feeder loss are preferred before increasing transmit power. Any deployment must comply with its local radio regulations.

## Lab / field model

Two equivalent stations are maintained:

- `LAB-01`: development, firmware, sensor and integration validation;
- `FIELD-01`: stable outdoor deployment.

Promotion path:

```text
development -> LAB-01 -> validation -> tagged release -> FIELD-01
```

FIELD-01 should not receive experimental firmware before LAB-01 validation.

## Repository layout

```text
docs/       Architecture, ADRs, API and power-budget documentation
firmware/   RAK4631 / MeshCore weather integration
hardware/   BOM, wiring and enclosure documentation
gateway/    MeshCore-to-HTTPS gateway
cloud/      Cloud Run API and infrastructure-as-code
webapp/     Flutter Web / installable PWA / iframe view
examples/   Integration examples and sample payloads
```

## Initial roadmap

- **v0.1** Architecture and interfaces
- **v0.2** BME280 environmental sensing on LAB-01
- **v0.3** Simulated wind/rain/vane inputs and final GPIO mapping
- **v0.4** MeshCore telemetry integration
- **v0.5** Physical wind and rain acquisition
- **v0.6** Gateway service
- **v0.7** Cloud Run + BigQuery + public REST API
- **v0.8** Flutter Web responsive dashboard + `/embed`
- **v0.9** Installable PWA and offline/stale-data behaviour
- **v1.0** Validated FIELD-01 outdoor reference station

## Current prototype status

Two BME280 modules have been ordered for LAB-01 and FIELD-01. Phase 1 will attempt to use accessible RAK19007 headers/pads for I2C/GPIO/ADC. A RAK13002 remains the fallback if direct wiring is mechanically inconvenient or fragile.

The mechanical wind/rain sensor set is intentionally deferred until the end-to-end BME280 -> MeshCore -> gateway path has been validated.

## License

Licensed under the Apache License 2.0. See [LICENSE](LICENSE).
