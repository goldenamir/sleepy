---
type: architecture-proposal
status: proposed
created: 2026-11-11
---
# Architecture options

No application has been implemented. These boundaries preserve the handoff's direction while allowing the actual problem and confirmed route to shape the MVP.

1. Confirmed source or local export.
2. Source-specific adapter.
3. Raw input with source metadata.
4. Validation and normalization.
5. Canonical sessions and measurements.
6. Nightly view or selected customer outcome.

## Proposed tools

Python for parsing/analysis, FastAPI for an ingestion API when needed, PostgreSQL for persistence, and Streamlit for a personal prototype. React and Swift/HealthKit are later options. The founder's experience supports considering this stack; no selection or installation has occurred.

## Ingestion boundary

An implemented adapter should yield source records and provenance without requiring the rest of the system to know how they arrived. BLE and SD-card adapters would share this boundary if both become feasible. Do not present fake BLE records as evidence of access.

Retain source, source record ID where available, original payload reference, ingestion time, measurement time, original timezone/offset, units, parser version, and validation outcomes.

## Time and overlap

Plan for sessions crossing midnight, daylight-saving changes, partial nights, missing data, different measurement definitions, and overlapping app records. Store UTC instants with original timezone context. Preserve source records; derive a view with explicit overlap/deduplication rules instead of summing everything.

Consumer sleep-stage estimates can disagree. Preserve attribution and make comparisons explainable.

## Before commercial implementation

Research access control, encryption, backups, retention, deletion/export, vendor terms, and intended-purpose questions in [[Open questions]]. These are design/assessment tasks, not completed controls or legal conclusions.

Select the source through [[Data source feasibility]] and the useful outcome through [[MVP options]].
