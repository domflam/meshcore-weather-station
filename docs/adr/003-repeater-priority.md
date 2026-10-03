# ADR-003: Repeater availability takes precedence

- Status: Accepted
- Date: 2026-10-03

## Context

The weather capability is an additional service hosted at a valuable MeshCore relay site. The relay function is the primary infrastructure role.

## Decision

Weather acquisition, aggregation or publication must never intentionally prevent MeshCore repeating. Energy-saving and failure-recovery logic must shed non-essential weather functions before the repeater.

## Consequences

Firmware changes require conservative resource use and fault isolation. Low-battery policy may reduce weather sampling/transmission while retaining repeater operation.
