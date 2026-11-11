---
type: source
source_kind: user-provided-session-handoff
received: 2026-11-11
source_url: "https://chatgpt.com/c/6af45eb1-3820-83ed-b617-834a7e5b5bf0"
verification: not-independently-verified
---
> Provenance: the founder pasted the following handoff into the current development-agent conversation on 2026-11-11. The linked web conversation could not be read by the browser tool. This preserves the supplied text; it is not independent verification of technical, commercial, or legal claims. Quoted next actions are historical context. See [[2026-11-11 - Founder business intent]] for the current business direction.

# Sleep Intelligence Platform — Agent Handoff

**Date:** 2026-11-11  
**Status:** Discovery / architecture planning; no code or integrations implemented yet.  
**Purpose:** Transfer the full substantive context of this ChatGPT session to another development agent without assuming unverified integrations exist.

## 1. User intent and context

The user uses a **ResMed AirSense 11 CPAP** for sleep apnea and currently uses the **ResMed myAir** companion app. The user wants to build a **personal web application** around their own CPAP data, extend it with new features, and potentially turn it into a commercial product. The user specifically prioritized **direct Bluetooth connectivity** to AirSense 11 over other ingestion methods.

The concept expanded to include **Apple Watch**, **Apple Health**, and **Sleep Cycle** as additional sources, with cross-source sleep analysis as the differentiator.

The user has strong Python, data engineering, SQL/PostgreSQL, and AI experience; proposed stack should take advantage of that. The user is in Sweden, so EU/Swedish privacy and medical-device considerations matter if commercialized.

## 2. Chronological decisions and discussion

### Initial idea: myAir to web app
- User asked how to turn the myAir experience into a web app, first for personal use and later with additional features.
- Recommendation: **build an independent web application**, rather than modifying or copying ResMed's proprietary myAir app.
- Proposed baseline: device data -> ingestion -> FastAPI/Python processing -> PostgreSQL -> dashboard and optional AI insights.
- Possible features: AHI, usage hours, mask leak, pressure, historical trends, anomaly exploration, natural-language questions about personal data, and reports.
- A previous assistant mentioned an existing European myAir web portal at `https://myair.resmed.eu`; **verify availability and functions before relying on it**.

### Device and connection priority
- User confirmed **ResMed AirSense 11**.
- User confirmed **direct Bluetooth** as the first priority.
- Proposed gateway: Linux/Ubuntu laptop running Python `bleak` and BlueZ; possibly migrate to Raspberry Pi later.
- Key technical caveat: BLE discovery and pairing **do not guarantee access to therapy records**. Device may use proprietary authentication, encryption, and characteristics. Its cellular path to ResMed may be the primary channel for therapy uploads. There is **no verified public, stable BLE protocol/API for extracting all AirSense 11 therapy metrics** established in this session.
- Suggested fallback: **SD-card ingestion**, potentially informed by the open-source OSCAR CPAP analysis project (`https://www.sleepfiles.com/OSCAR/`). Validate actual model, SD-card support, data structure, and applicable licenses.
- Other alternatives discussed: official export, official API/authorized partner access if available. Do not assume any of these provide machine-readable full-fidelity data.

### BLE feasibility experiment proposed, NOT performed
Install and run a passive BLE scanner on Ubuntu near the CPAP device. No results have been provided yet. Suggested setup:

```bash
sudo apt update
sudo apt install -y bluetooth bluez
python3 -m venv cpap-env
source cpap-env/bin/activate
pip install bleak
```

```python
# scan_cpap.py
import asyncio
from bleak import BleakScanner

async def main():
    print("Scanning Bluetooth LE devices for 20 seconds...")
    devices = await BleakScanner.discover(timeout=20.0, return_adv=True)
    for device, adv in devices.values():
        name = adv.local_name or device.name or "Unknown"
        print(f"Name: {name}")
        print(f"Address: {device.address}")
        print(f"RSSI: {adv.rssi}")
        print(f"Service UUIDs: {adv.service_uuids}")
        print("-" * 40)

asyncio.run(main())
```

```bash
python scan_cpap.py
```

**Guardrails:** This is discovery only. Do not change treatment settings, send undocumented control commands, bypass encryption, or disrupt the user's existing myAir pairing. Redact Bluetooth MAC addresses, device identifiers, and personal health details before sharing logs. If no device appears, investigate whether the device advertises only during a user-initiated pairing window; avoid speculative conclusions.

**Decision gate:** If scan succeeds, review exposed services and feasibility of authorized read-only access. If no supported read-only therapy data is available, switch to SD-card or another authorized ingestion method while preserving the same backend interface.

### Commercialization question
- User asked whether the project could make money.
- Discussed hypothetical models:
  1. B2C subscription dashboard (illustrative price **49–99 SEK/month**, not validated).
  2. B2B reporting for sleep clinics.
  3. AI-assisted sleep insights as a premium feature.
  4. Data connector/integration services.
- Illustrative revenue arithmetic at **79 SEK/month** was discussed; **not a sales forecast**. Revenue excludes taxes, churn, acquisition, hosting, support, compliance, etc.
- Recommendation: validate real unmet user needs and willingness to pay before building a full SaaS. Interview ~10–20 CPAP users and compare against existing tools.
- Critical risks: proprietary device data access, terms/licensing, GDPR health-data handling, and possible EU MDR/CE medical-device requirements depending on intended purpose and claims. Avoid making diagnostic or treatment recommendations in an initial wellness/analytics product; even so, regulatory classification requires a proper assessment.
- The assistant asked whether the user wanted a modest side business or a SleepTech startup; **user did not answer**.

### Expanded multi-source idea (latest direction)
- User proposed adding **Sleep Cycle** and **Apple Watch** to make the product more compelling.
- Assistant reframed it as a **Multi-Source Sleep Intelligence Platform**.
- Intended sources and example data (availability varies by model, app, permissions, region, and settings):
  - **AirSense 11:** CPAP usage, AHI, mask leak, pressure, respiratory events where available.
  - **Apple Watch / Apple Health:** sleep intervals/stages, heart rate, HRV, respiratory rate, and oxygen-related measurements only when supported/available.
  - **Sleep Cycle:** sleep sessions, wake-up patterns, snoring and other available metrics; determine what is actually exportable and what syncs to Apple Health.
- Main value proposition: align sources by timestamp, timezone, session, and provenance to explore associations, e.g., whether high mask leak coincides with altered heart rate, sleep-stage estimates, or snoring. **Correlation is not causation**, and consumer sleep stages are estimates.
- **Apple Health/HealthKit** can serve as a partial aggregation hub for watch and app data, but **a browser-only web app cannot directly query HealthKit**. A small native iOS app/bridge using Swift + HealthKit and explicit user permission is a plausible path for automated sync. For initial MVP, investigate export-based ingestion.
- **Avoid duplicate counting** when Sleep Cycle and Apple Watch record the same night; retain raw source-specific records, identifiers, timestamps, and data provenance.
- The assistant asked whether the user wears Apple Watch overnight and uses Sleep Cycle on iPhone; **not answered**.

## 3. Proposed target architecture

```text
Apple Watch -----> Apple Health / HealthKit -----> iOS Bridge (Swift)
Sleep Cycle -----/                 |                       |
                                   \-- export (MVP) ------+
                                                        |
AirSense 11 -- BLE feasibility --> Linux BLE Gateway ---+--> Ingestion API (FastAPI)
          \-- SD card fallback --------------------------+         |
                                                                  v
                                                      Validation / normalization
                                                                  |
                                                                  v
                                                        PostgreSQL storage
                                                                  |
                                                                  v
                                                   Streamlit MVP / Web dashboard
                                                                  |
                                                                  v
                                                    Cross-source analysis / AI
```

Suggested boundaries:
- **Connector abstraction:** `CPAPConnector`, `HealthKitConnector`, `SleepCycleConnector` (actual connector methods depend on confirmed interfaces).
- **Raw ingestion:** immutable raw payloads/files plus source, ingestion time, source record ID, timezone, units, and parser version.
- **Canonical model:** sleep sessions, timestamped metrics, CPAP events, nightly summaries, source lineage and deduplication/overlap logic.
- **Storage:** PostgreSQL for personal MVP; encryption, access control, backups and data deletion/export should be planned from the beginning.
- **Frontend:** Streamlit for fast personal MVP; React + FastAPI later if product-market fit warrants it.
- **AI:** explanatory summaries grounded in available measurements, transparent uncertainty, no unsupported medical advice.

## 4. Recommended next actions for the receiving agent

1. **Confirm data reality before coding a full platform:** document AirSense 11 firmware/region, BLE behavior, availability of SD card, iPhone model/iOS version, Apple Watch model, and Sleep Cycle export/HealthKit permissions. Ask only essential questions.
2. **Run a read-only BLE feasibility spike** on Ubuntu, beginning with the passive scanner above. Collect redacted observations; do not claim BLE therapy-data access unless verified.
3. **Research legitimate supported data paths** and actual export formats for each source, including data granularity, update cadence, permissions, licensing and reliability. Prefer official docs and active projects; distinguish evidence from hypotheses.
4. **Write a short technical decision record** comparing BLE, SD card, myAir export/partner API, Apple Health export, native HealthKit bridge, and Sleep Cycle export.
5. **Implement a thin end-to-end MVP using one confirmed source**, then add second and third sources. Avoid building around mock BLE metrics presented as real.
6. **Design time alignment and deduplication carefully:** UTC storage plus original timezone, session crossing midnight, source identifiers, sleep-stage disagreements, missing data, and source-specific units/definitions.
7. **Produce a privacy/security checklist** suitable for personal health data in Sweden. For commercialization, assess GDPR and EU MDR intended-purpose boundaries with qualified advice.
8. **Define success criteria:** at least one real sleep session imported, reproducible pipeline, source attribution, nightly dashboard, and one cross-source comparison when two sources are actually available.

## 5. Open questions and unresolved blockers

- Does AirSense 11 expose any useful **authorized read-only** therapy data via BLE? **Unknown.**
- Has BLE scan been run? **No result supplied.**
- Can user's Sleep Cycle installation export detailed snoring metrics, or only sleep-session summaries to HealthKit? **Unknown.**
- Does the user wear Apple Watch while sleeping and have historic HealthKit data? **Unknown.**
- Does the user have access to the AirSense 11 SD card and an SD-card reader? **Unknown.**
- What is the desired business ambition: side income or full startup? **Unanswered.**
- What are the actual data access rights, vendor terms, and regulatory obligations for a future commercial product? **Requires verification.**

## 6. Prior assistant statements to treat as unverified until checked

- Specific claims about ResMed BLE encryption/keying and exact Bluetooth capabilities.
- Existence and features of `https://myair.resmed.eu` for this account/region.
- Whether myAir exports machine-readable data versus PDF reports.
- Exact OSCAR AirSense 11 compatibility details and supported signal granularity.
- Sleep Cycle's current HealthKit synchronization and export capabilities.
- Any suggested pricing, market size, customer demand, or regulatory classification.

## 7. Immediate handoff prompt

> Continue the personal Multi-Source Sleep Intelligence Platform project from this document. Prioritize validating **direct Bluetooth read-only access on ResMed AirSense 11** without changing therapy settings or disrupting myAir. In parallel, assess Apple Health/Apple Watch and Sleep Cycle data ingestion paths. Start with a feasibility matrix and a minimal experiment, clearly label unverified claims, and then propose a practical MVP architecture with an ingestion interface that supports BLE or SD-card fallback. Do not assume a public AirSense BLE API exists. Ask me for the first necessary hardware/environment detail, and report every test result before implementing the next step.
