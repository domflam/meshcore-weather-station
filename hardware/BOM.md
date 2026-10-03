# Initial Hardware BOM

The reference deployment uses two functionally identical stations: `LAB-01` for development/validation and `FIELD-01` for production deployment. Quantities below are therefore per station and total for the paired setup.

| Function | Baseline | Qty/station | Qty total | Interface / notes |
|---|---|---:|---:|---|
| Compute + LoRa | D5L with RAK4631 | 1 | 2 | nRF52840 + SX1262, EU868 reference deployment |
| WisBlock I/O breakout | RAK13002 | 1 | 2 | IO-slot adaptor; exposes I2C, GPIO, ADC, UART and SPI on 2.54 mm headers |
| Antenna | ~5 dBi 868 MHz | 1 | 2 | Short ~20 cm feeder preferred |
| Solar | 5 W panel | 1 | 2 | South-facing and unobstructed on FIELD-01; shared by repeater and weather subsystem |
| Storage | 4 x 3.7 V 3000 mAh in parallel | 4 cells | 8 cells | 1S4P, 12 Ah / ~44.4 Wh nominal per station; appropriately matched/protected cells |
| Temp / humidity / pressure | BME280-class breakout | 1 | 2 | 3.3 V I2C; outdoor radiation shield required |
| Wind speed | Cup anemometer, passive pulse output | 1 | 2 | GPIO interrupt/counter |
| Wind direction | Resistive wind vane | 1 | 2 | ADC; energize only during measurement where practical |
| Rain | Tipping-bucket gauge, passive pulse output | 1 | 2 | GPIO interrupt/counter |
| Weather junction | IP-rated junction box | 1 | 2 | Keep field sensor connections serviceable |
| Cable / connector | Multi-conductor + serviceable connectors | 1 set | 2 sets | Use existing D5L cable gland where possible |

## Candidate weather mechanics

SparkFun/Fine Offset style weather meters are the current reference candidate because wind speed and rainfall use passive contact closures and the wind vane is resistive. Final supplier/model is not frozen until mechanical dimensions, cable lengths and outdoor mounting are checked.

## RAK13002 capability

The RAK13002 is the preferred prototype breakout. It exposes two I2C buses, up to six GPIOs, two ADC interfaces, two UARTs, SPI and 3.3 V from the WisBlock Core through standard 2.54 mm headers. Exact pin assignments remain deliberately unfrozen until they are checked against MeshCore usage and the selected WisBlock slot/base revision.

## Power baseline

FIELD-01 baseline is 5 W solar + 1S4P 3000 mAh cells (~44.4 Wh nominal). Existing D5L repeaters have demonstrated positive daytime energy recovery in the same general usage pattern with only two cells; this is useful field evidence but is not a winter qualification test.

Weather functions must degrade before repeater functionality when battery state becomes low. Proposed policy:

- normal: 10-15 minute weather observations;
- conservation: reduce observation/transmission rate;
- critical: disable non-essential weather functions and preserve MeshCore repeating.

## RF amplifier

A 1 W amplifier is intentionally excluded from the baseline. The reference deployment prioritizes antenna height, clear line of sight, suitable antenna gain and low feeder loss. Radio configuration and effective radiated power must comply with local regulations.

## Prototype sequence

1. Build `LAB-01` and `FIELD-01` with the same hardware baseline.
2. Validate RAK13002 + BME280 locally on `LAB-01`.
3. Validate weather telemetry over MeshCore.
4. Validate gateway decoding and HTTPS ingestion.
5. Add wind vane, anemometer and rain gauge.
6. Promote a tagged, validated firmware build from `LAB-01` to `FIELD-01`.

## Open items

- confirm exact D5L/WisBlock base revision and final RAK13002 pin mapping;
- confirm exact solar charge controller behaviour and battery telemetry;
- select final outdoor weather-meter kit and radiation shield;
- define surge/ESD/lightning strategy for permanent sensor cabling;
- characterize winter energy balance with real telemetry.
