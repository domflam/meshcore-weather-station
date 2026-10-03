# MeshCore Weather Station

Open-source, solar-powered weather telemetry for remote sites using MeshCore and LoRa.

The reference deployment combines a RAK4631-based MeshCore repeater with low-power weather sensors. Telemetry crosses the MeshCore network to an Internet-connected gateway, which publishes observations to a Google Cloud endpoint. A public REST API then lets websites and mobile applications consume the data without knowing anything about MeshCore.

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
       Web / App
```

## Design principles

1. **Repeater first.** Weather telemetry must never compromise the availability of the MeshCore repeater.
2. **Low power.** The reference station targets a shared 5 W solar panel and a 1S Li-ion battery bank.
3. **Open interfaces.** Consumers integrate through a simple HTTPS/JSON API rather than MeshCore-specific protocols.
4. **Store useful weather, not noise.** Wind and rain are sampled/count continuously and aggregated before transmission.
5. **Observable infrastructure.** Battery voltage, link quality and station freshness are first-class operational data.
6. **Reusable by design.** Nothing in the architecture depends on the original archery-club deployment.

## Reference hardware

The first deployment is based on:

- RAK4631 (nRF52840 + SX1262)
- 868 MHz antenna, approximately 5 dBi
- short coaxial feed (approximately 20 cm)
- 5 W solar panel
- 5-6 x 3.7 V / 3000 mAh cells in parallel (15-18 Ah nominal)
- temperature / humidity / pressure sensor
- cup anemometer
- wind vane
- tipping-bucket rain gauge

A 1 W RF power amplifier is deliberately **not** part of the baseline. Good antenna placement, line of sight and low feeder loss are preferred before increasing transmit power. Any deployment must comply with its local radio regulations.

## Repository layout

```text
docs/       Architecture, ADRs, API and power-budget documentation
firmware/   RAK4631 / MeshCore weather integration
hardware/   BOM, wiring and enclosure documentation
gateway/    MeshCore-to-HTTPS gateway
cloud/      Cloud Run API and infrastructure-as-code
examples/   Integration examples and sample payloads
```

## Initial roadmap

- **v0.1** Architecture and interfaces
- **v0.2** Environmental sensing on RAK4631
- **v0.3** Wind and rain acquisition
- **v0.4** MeshCore telemetry integration
- **v0.5** Gateway service
- **v0.6** Cloud Run + BigQuery backend
- **v0.7** Public REST API and web integration example
- **v1.0** Validated outdoor reference station

## Status

Early design / prototype. Interfaces and hardware choices may change before v1.0.

## License

Licensed under the Apache License 2.0. See [LICENSE](LICENSE).
