---
type: feasibility-matrix
status: unverified
created: 2026-11-11
---
# Data source feasibility

This matrix records supplied context and unanswered questions. **No integration or hardware experiment has been run in this project, and no current vendor documentation has been independently checked for this vault.**

| Route | Current knowledge | Unknown / evidence required | Next minimal check | Fallback / dependency |
| --- | --- | --- | --- | --- |
| AirSense 11 direct BLE | Founder priority; scanner proposed in handoff | Authorized read-only therapy records, firmware/region, advertising/pairing behavior | Review official interface information; passive scan if requested | SD card or another confirmed authorized route |
| AirSense 11 SD card | Suggested fallback | Card/reader availability, compatibility, formats, signals, parser licensing | Confirm hardware and inspect a permitted local sample | No sample supplied |
| myAir export / authorized API | Discussed as possible routes | Account/region features, export format, rights, supported API/partner access | Check official docs and actual export UI | Do not assume a public API or full-fidelity export |
| Apple Health export | Suggested initial path | Actual contents, history, provenance, units, timezone behavior | Confirm setup and inspect an export if supplied | Original health data stays outside Git |
| Native HealthKit bridge | Swift bridge proposed for automation | Current APIs, permissions, records, background behavior | Verify official Apple documentation before design | No bridge exists; do not assume browser access to HealthKit |
| Sleep Cycle export / Health sync | Proposed additional source | Which session/snoring metrics export or sync for this installation | Check official docs and available export/settings | May overlap with Watch records |

## Decision rule

A BLE advertisement establishes visibility, not therapy-data access. A connection or service listing alone does not establish a useful or supported feed.

Select a route after documenting access, representative data, granularity, cadence, reliability, and applicable permissions/licensing. Retain failed and inconclusive results.

## Evidence record for each route

- Source URL or local observation, date, device/software context.
- Exact action, expected result, actual result, and limitations.
- Record types, identifiers, units, timestamps.
- Read-only access and effect on existing device/app use.
- Status: unknown, observed, verified for this setup, or unsuitable, with reason.

See [[BLE feasibility spike]], [[Architecture options]], and [[Evidence register]].
