# ADR-004: Paired LAB and FIELD reference stations

- Status: Accepted
- Date: 2026-10-03

## Context

The production station hosts a MeshCore repeater at a valuable elevated radio site. Firmware and sensor experimentation must not reduce its availability. A second D5L/RAK4631 platform is available for development.

## Decision

Maintain two functionally equivalent reference stations:

- `LAB-01`: development, flashing, debugging, destructive tests and pre-production validation;
- `FIELD-01`: stable outdoor deployment and production telemetry.

Firmware/configuration changes follow the promotion path:

`development -> LAB-01 -> validation -> tagged release -> FIELD-01`

FIELD-01 is not used as the primary debugging target. Hardware differences between the two stations must be documented when unavoidable.

## Consequences

This increases the hardware BOM but substantially reduces operational risk. Reproducible LAB failures can be investigated without physical access to the field site. Tagged releases provide a known rollback point for FIELD-01.

The cloud data model identifies stations independently so LAB telemetry can be retained or filtered without changing the API contract.
