---
type: experiment
status: planned-not-run
created: 2026-11-11
---
# BLE feasibility spike

## Question

Can the founder's AirSense 11 support a useful, authorized read-only data route for the selected outcome?

**Status: not run.** No scanner, installation, connection, service enumeration, or therapy-data read has been performed in this repository setup.

## Stage 1 — passive discovery

The supplied [[2026-11-11 - ChatGPT handoff]] contains a proposed Python/Bleak passive scanner. Before running it, confirm Ubuntu, adapter availability, and device context. Review current official/project documentation instead of assuming the historical snippet is sufficient.

If requested, run a passive scan near the device and report the actual result before proposing the next interaction. Raw logs belong under private/; record redacted observations here.

## Interpretation

- A matching advertisement establishes visibility only.
- No matching advertisement is inconclusive; document conditions instead of guessing.
- Pairing, connecting, or seeing services does not prove therapy records are readable.
- An authorized documented read route and representative measurements are needed before selecting BLE.

## Boundaries from the handoff

Do not change treatment settings, send undocumented control commands, bypass encryption, or disrupt myAir pairing. Do not infer a public API from discovery.

## Result record

- Date/environment: not collected.
- Device/firmware/region: not collected.
- Action: none.
- Observations: none.
- Supported conclusion: feasibility unknown.
- Next decision: after evidence review; alternatives in [[Data source feasibility]].
