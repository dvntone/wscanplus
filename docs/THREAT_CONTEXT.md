# Threat Context (Real-World Motivation)

## Summary

wscan+ is motivated by repeated, suspicious wireless/network events experienced in a
multi-unit apartment environment. The pattern of activity suggested a **proximate,
human-operated attacker** (likely within the same building or an adjacent complex),
not a purely remote/internet-based threat. The most harmful gap was not only detection,
but **being able to explain and prove** what was happening to non-technical people.

This document captures the threat model and the product requirements it implies.

---

## Environment

- Multi-unit apartment complex (veterans housing; many residents are older/disabled and
  not technically experienced).
- High device density typical of apartments, but observed anomalies significantly
  exceeded normal residential levels.
- Primary connectivity shifted away from home internet due to risk (see "Current
  Defensive Posture" below).

---

## Key Observations

- **Network flooding / extreme SSID counts:** Wireless scanning tools intermittently
  showed hundreds to ~1,000 visible networks, inconsistent with typical residential
  density and suggestive of beacon spam or aggressive scanning nearby.
- **Timing correlation:** Activity often increased when arriving home or during certain
  routines, implying the attacker could be reacting to presence or schedule.
- **Reactive behavior:** Suspicious activity sometimes ceased temporarily when
  physically checking outside, reinforcing the hypothesis of a manual operator in close
  proximity.
- **Device/resource anomalies:** Phones becoming unusually hot during incidents,
  consistent with sustained wireless scanning/reconnect loops or heavy radio/CPU
  activity.
- **Defensive escalation:** Attempts to investigate (learning tools, rotating SSIDs,
  locking down devices) appeared to correlate with increased interference, consistent
  with an adversary monitoring and reacting.

---

## Impact

- Loss of trust in home network and connected devices.
- High stress and a "credibility gap": difficult to communicate the situation to others
  without shared technical knowledge.
- Concern that a vulnerable population (older/disabled residents with limited technical
  literacy) may be broadly exploited — credentials, financial accounts — not just a
  single target.

---

## Current Defensive Posture

- Home internet canceled to reduce attack surface.
- Reduced use of IoT devices (streaming devices, voice assistants, Wi-Fi cameras,
  consoles).
- Reliance on multiple carrier-unlocked cellular devices and multiple SIMs due to
  theft/compromise risk and the need for rapid rotation.

---

## Threat Model (Working Assumptions)

- Attacker is likely **local/proximate** (same building or nearby), capable of:
  - Rogue AP / evil twin attacks
  - Deauthentication / disassociation pressure
  - Beacon/SSID flooding to create noise and obscure malicious activity
  - Opportunistic credential theft via MITM or social engineering
- Attacker may be persistent and may target populations with low technical literacy.
- Attribution is not a goal; **detection, documentation, and reporting** are.

---

## Product Requirements Implied

### Must-have (Baseline — non-technical users)

- Passive-friendly detection with plain-language alerts (minimize jargon).
- "Proof" artifacts: simple event summaries with timestamps that a non-technical person
  can understand and share.
- Export/share capability: screenshots, PDFs, basic human-readable reports.

### Advanced / Pro (security professionals and advanced users)

- Evidence-grade exports (e.g., `.pcap`, JSON) compatible with analysis tools such as
  Wireshark.
- Event timelines, correlation, and anomaly baselining (e.g., "network count spike"
  detection above a configurable threshold).
- Optional AI-assisted summarization/report generation that turns raw events into
  coherent, human-readable incident narratives.

---

## Non-goals / Safety

- The app **must not** encourage or facilitate illegal activity or active interference
  with any network or device.
- Prefer minimal data collection and user-controlled, on-device exports.
- Prioritize user safety and privacy above feature completeness.
- Offensive capabilities (injection, jamming, exploitation) are explicitly out of scope.
