# ADR-001: RAK4631 as the reference edge platform

- Status: Accepted
- Date: 2026-10-03

## Context

The reference site already uses a RAK4631-class MeshCore repeater and requires very low power consumption.

## Decision

Use the RAK4631 (nRF52840 + SX1262) as the reference compute/radio platform and investigate direct attachment of weather sensors before introducing a second MCU or LoRa radio.

## Consequences

The design remains compact and energy efficient. Firmware integration must remain maintainable against upstream MeshCore changes.
