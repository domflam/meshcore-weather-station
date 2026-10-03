# Reference Architecture

## Goal

Provide low-power weather observations from a remote site with no Internet connection, while preserving the primary function of a MeshCore repeater.

## Logical flow

```text
BME280/BME680 ---- I2C ----+
Anemometer ------- GPIO ---|
Wind vane -------- ADC ----+--> RAK4631 / MeshCore repeater
Rain gauge ------- GPIO ---|             |
Battery/solar ----- ADC ----+             | LoRa / MeshCore
                                          v
                                  Internet-connected gateway
                                          |
                                      HTTPS POST
                                          v
                                  Google Cloud Run API
                                          |
                               +----------+----------+
                               |                     |
                           BigQuery             Public REST API
                                                     |
                                              Website / mobile app
```

## Edge behaviour

The weather subsystem acquires data locally. Wind pulses and rain-gauge tips are counted continuously. Environmental measurements are sampled periodically. Values are aggregated into compact observations before they cross the LoRa mesh.

The reference target is one observation every 10-15 minutes. Publishing frequency to end-user applications is independent of measurement frequency.

## Gateway responsibilities

The gateway bridges MeshCore to HTTPS. It must:

- identify the station;
- decode telemetry;
- add link metadata when available (RSSI/SNR);
- authenticate to the ingestion endpoint;
- retry transient failures without flooding the mesh;
- never expose cloud credentials to the remote station.

## Cloud responsibilities

Cloud Run exposes two logical surfaces:

- authenticated ingestion (`POST /api/v1/ingest`);
- read-only consumption (`GET /api/v1/weather/...`).

BigQuery stores the historical observations. The API calculates freshness so consumers can distinguish current, stale and offline stations.

## Failure domains

Weather acquisition, MeshCore repeating, gateway forwarding and cloud publication are separate failure domains. A failure in weather sensing or cloud publication must not stop the MeshCore repeater.

## Security boundary

The remote node contains no GCP credential. Cloud authentication terminates at the Internet-connected gateway. Public consumers receive read-only weather data.
