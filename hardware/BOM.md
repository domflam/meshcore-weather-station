# Initial Hardware BOM

This BOM captures the reference design, not final purchasing recommendations.

| Function | Baseline | Interface / notes |
|---|---|---|
| Compute + LoRa | RAK4631 | nRF52840 + SX1262, 868 MHz reference deployment |
| Antenna | ~5 dBi 868 MHz | Short ~20 cm feeder preferred |
| Solar | 5 W panel | Shared by repeater and weather subsystem |
| Storage | 5-6 x 3.7 V 3000 mAh in parallel | 1S, 15-18 Ah nominal; cells must be appropriately matched/protected |
| Temp / humidity / pressure | BME280-class | I2C; final outdoor packaging to be validated |
| Wind speed | Cup anemometer | Reed/Hall pulse input to GPIO |
| Wind direction | Resistive wind vane | ADC; power only during measurement where practical |
| Rain | Tipping-bucket gauge | Reed pulse input to GPIO |

## Optional components

- solar/battery voltage sensing;
- load switch/MOSFET for non-essential sensors;
- surge/ESD protection for long outdoor sensor cables;
- radiation shield for temperature/humidity sensor;
- IP-rated enclosure and cable glands.

## RF amplifier

A 1 W amplifier may be evaluated later but is intentionally excluded from the baseline. The reference deployment prioritizes antenna height, clear line of sight, suitable antenna gain and low feeder loss. Radio configuration and effective radiated power must comply with local regulations.

## Open items

- exact DC5 carrier revision and accessible RAK4631 pins;
- exact solar charge controller topology;
- battery cell chemistry, protection and balancing/matching strategy;
- selected outdoor weather-sensor models;
- lightning/surge strategy for permanent installation.
