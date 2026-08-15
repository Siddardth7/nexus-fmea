# Roadmap — Data-Driven PFMEA + Linked Control Plan

**Target duration:** 1–2 weeks (part-time) · **Definition of done:** an AIAG-VDA PFMEA with Occurrence justified by data, a linked Control Plan, and a linkage matrix showing zero orphaned risks.

---

## Milestones

### M1 — Process definition & defect data (Days 1–2)
- [ ] Reuse Project 1's dataset; produce a per-operation defect-rate summary table.
- [ ] Build the Process Flow Diagram (operation sequence, inputs/outputs per step).
- [ ] Choose the AIAG-VDA rating scales (Severity/Occurrence/Detection tables) to standardize scoring.

### M2 — PFMEA with data-driven Occurrence (Days 3–5)
- [ ] For each operation: list failure modes, effects, causes.
- [ ] Score **Severity** (effect-based) and **Detection** (current controls).
- [ ] Score **Occurrence** by mapping measured defect rate → the AIAG-VDA Occurrence scale (documented conversion).
- [ ] Compute **Action Priority (AP)** = High / Medium / Low per AIAG-VDA logic.

### M3 — Control Plan & linkage (Days 6–8)
- [ ] For each High/Medium-AP mode: define the control (characteristic, spec, method, sample size & frequency, reaction plan).
- [ ] Carry Special Characteristic (CTQ/SC) flags from PFMEA into the Control Plan.
- [ ] Build the **linkage matrix**: every high-AP PFMEA row ↔ a Control Plan row. Flag any orphans.

### M4 — Package & publish (Days 9–10)
- [ ] Export PFMEA + Control Plan (xlsx) and the linkage matrix.
- [ ] README summary + résumé bullet; push to GitHub, tag `v1.0`.

---

## Progress

| Milestone | Status |
|-----------|--------|
| M1 Process & defect data | ☐ Not started |
| M2 PFMEA (data Occurrence) | ☐ Not started |
| M3 Control Plan & linkage | ☐ Not started |
| M4 Package & publish | ☐ Not started |

## Stretch
- [ ] A small script that auto-checks PFMEA↔Control-Plan linkage from the two spreadsheets (a nod toward the `quality-platform` validator, kept lightweight).
