# LAB-01 — Phase 1 Wiring Plan

## Objective

Validate the complete low-power sensor acquisition path before purchasing the outdoor wind/rain hardware:

`BME280 + simulated weather contacts -> RAK13002 -> RAK4631 -> MeshCore telemetry`

No FIELD-01 deployment is made until this path is validated on LAB-01.

## Phase 1 hardware

- Existing D5L enclosure / WisBlock base
- Existing RAK4631
- 1 x RAK13002 WisBlock IO module
- 1 x genuine BME280 breakout, 3.3 V / I2C capable
- 2 x normally-open momentary push buttons for wind/rain simulation
- 1 x 10 kOhm linear potentiometer for wind-vane simulation
- jumper wires / 2.54 mm headers as required
- optional 100 nF capacitors for contact-debounce experiments

## Logical wiring

```text
RAK4631
   |
WisBlock Base
   |
RAK13002
   |
   +-- 3V3 ----------------------+---------------------+
   |                             |                     |
   +-- GND ----------------------+---------+-----------+
   |                             |         |           |
   +-- I2C SDA ---------------- BME280     |           |
   +-- I2C SCL ---------------- BME280     |           |
   |                                       |           |
   +-- GPIO WIND ------ momentary switch --+           |
   |                                                   |
   +-- GPIO RAIN ------ momentary switch --------------+
   |
   +-- ADC VANE ------- 10k potentiometer wiper
                         |                 |
                        3V3               GND
```

## Important: physical pin assignment

This document intentionally does **not** freeze numeric nRF52840 GPIO assignments yet.

The RAK13002 exposes I2C, GPIO and ADC signals from the WisBlock Core. Before soldering the final harness we will:

1. select the physical WisBlock IO slot used in the D5L;
2. cross-check that slot against the RAK4631/WisBlock pin mapping;
3. verify which pins the current MeshCore repeater build already reserves;
4. select two interrupt-capable free GPIOs and one free ADC;
5. record the validated mapping here.

This avoids documenting a theoretical pinout as if it were tested hardware.

## BME280

Use 3.3 V logic and I2C. Expected address is normally `0x76` or `0x77`, depending on the breakout.

The first firmware test must:

- scan the I2C bus;
- identify the sensor address;
- verify BME280 chip identity where supported by the driver;
- read temperature, relative humidity and pressure;
- reject a BMP280-only device if humidity is unavailable.

The outdoor deployment will place the environmental sensor in a ventilated radiation shield, not inside the black D5L enclosure.

## Wind simulation

Each press of the WIND button represents one anemometer reed-switch pulse. Phase 1 validates interrupt counting, debounce and aggregation without requiring an anemometer.

The firmware should maintain at least:

- pulse count per aggregation window;
- average wind-speed placeholder derived from pulse frequency;
- maximum short-window pulse rate for later gust calculation.

The final conversion coefficient remains sensor-specific and must not be hard-coded until the outdoor sensor is selected.

## Rain simulation

Each press of the RAIN button represents one tipping-bucket event. Phase 1 validates interrupt counting and persistence across telemetry intervals.

The millimetres-per-tip coefficient remains sensor-specific until the final rain gauge is selected.

## Wind-vane simulation

A 10 kOhm linear potentiometer between 3V3 and GND provides a variable voltage to the selected ADC. This validates ADC acquisition and direction mapping without purchasing a wind vane.

Phase 1 should initially expose the raw ADC value and normalized percentage. Compass-sector conversion comes after the final vane resistance table is known.

## Acceptance criteria

LAB-01 Phase 1 is successful when:

- BME280 is detected reliably after cold boot;
- temperature, humidity and pressure are plausible;
- 100 simulated wind pulses are counted without unexplained loss;
- 20 simulated rain tips are counted without unexplained duplication;
- ADC values vary consistently across the potentiometer travel;
- one compact weather observation can be produced without disrupting MeshCore repeater operation;
- reboot/recovery behaviour is documented.

## FIELD-01 gate

FIELD-01 remains on a stable MeshCore repeater build until the Phase 1 firmware has passed LAB-01 acceptance testing. FIELD-01 receives only a tagged, reproducible version validated on LAB-01.
