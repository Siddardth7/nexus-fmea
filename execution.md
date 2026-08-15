# Execution — Nexus-FMEA: Data-Driven PFMEA & Linked Control Plan

*Minute-detail build plan: tools, the defect-rate → Occurrence derivation, PFMEA and Control Plan construction, the linkage check, validation, schedule, and pitfalls. Paired with `idea.md`.*

---

## 1. Environment & tooling

```bash
python3.11 -m venv .venv && source .venv/bin/activate
pip install pandas numpy openpyxl matplotlib jupyter
pip freeze > requirements.txt
```

- `pandas`/`numpy` — defect-rate computation from the P1 data.
- `openpyxl` — write formatted PFMEA / Control Plan spreadsheets.
- The PFMEA and Control Plan are authored in Excel-format templates (industry-standard); the notebook only *produces the Occurrence inputs* and the linkage check.

**Scaffolding:**
```
nexus-fmea/
├── data/processed/defect_rates.csv     # from Project 1 (Sentinel-8D)
├── notebooks/01_occurrence_from_data.ipynb
├── templates/                          # blank AIAG-VDA PFMEA + Control Plan
├── reports/PFMEA.xlsx · Control_Plan.xlsx · linkage_matrix.md
└── requirements.txt
```

## 2. Step 1 — Process flow & defect rates (Day 1–2)

1. Import the tidy one-row-per-part table from Project 1.
2. Compute, per operation: number of parts, number failing a characteristic attributable to that operation, and the **defect rate** (with a Wilson confidence interval, since some operations have few failures).
3. Draw the **Process Flow Diagram** (operation sequence, key inputs/outputs, measured characteristics). Save `reports/figures/pfd.png`.

## 3. Step 2 — Rating scales & the Occurrence mapping (Day 3)

- Adopt the **AIAG-VDA** Severity, Occurrence, and Detection tables (1–10 scales). Record the scale definitions in the repo so scoring is auditable.
- Define the **defect-rate → Occurrence** conversion explicitly using the AIAG-VDA Occurrence *rate bands* (each Occurrence level corresponds to a failure-rate range). Example logic: map each operation's measured defect rate into the band whose range contains it → that band's number is the Occurrence.
- Document edge handling: operations with too few parts get a flagged, conservative rating (don't over-claim precision on thin data).

## 4. Step 3 — Author the PFMEA (Day 3–5)

For each operation, complete the AIAG-VDA structure:
- **Function** → **Failure Mode** → **Failure Effects** (rate **Severity** from the worst effect) → **Failure Causes** → **Current Prevention/Detection Controls** (rate **Detection**).
- **Occurrence** ← the data-driven mapping (§3).
- **Action Priority (AP)** ← AIAG-VDA S/O/D logic (High/Medium/Low) — *not* a multiplied RPN.
- Flag **Special Characteristics (SC/CTQ)** for high-Severity, safety/compliance modes.
- Recommend actions for High-AP modes.

*Tie each failure mode to something real* — a defect or parameter observed in Project 1 — so the PFMEA reads as analysis, not a template.

## 5. Step 4 — Author the Control Plan (Day 6–7)

For every High/Medium-AP mode, add a Control Plan row with:
- Process step / characteristic (and SC/CTQ symbol carried from the PFMEA),
- Specification / tolerance,
- **Evaluation/measurement technique** (gauge/method),
- **Sample size & frequency**,
- **Control method** (e.g., SPC chart, 100% inspection, poka-yoke),
- **Reaction plan** (what to do on an out-of-control/nonconforming result).

## 6. Step 5 — Linkage matrix & verification (Day 7–8)

- Build a matrix keyed by a shared **Process-Step / Characteristic ID** present in both documents.
- Programmatically check: **every High-AP PFMEA row has ≥1 matching Control Plan row**; every SC/CTQ flag in the PFMEA appears in the Control Plan; detection controls named in the PFMEA are represented in the Control Plan's method column.
- Output `linkage_matrix.md` listing each link and **flagging any orphan** (a risk with no control, or a control with no risk).

## 7. Validation & reproducibility checklist

- [ ] Every Occurrence value traces to a defect rate in the notebook.
- [ ] Action Priority computed by AIAG-VDA logic (documented), not ad-hoc RPN.
- [ ] Zero orphaned High-AP risks in the linkage matrix.
- [ ] SC/CTQ flags present in both documents.
- [ ] Rating-scale tables committed to the repo for auditability.

## 8. Detailed schedule

| Day | Focus | Output |
|-----|-------|--------|
| 1–2 | Defect rates + PFD | `defect_rates.csv`, PFD figure |
| 3 | Scales + Occurrence mapping | documented conversion |
| 3–5 | PFMEA authoring | `PFMEA.xlsx` |
| 6–7 | Control Plan authoring | `Control_Plan.xlsx` |
| 7–8 | Linkage matrix + verification | `linkage_matrix.md` |
| 9 | README summary + résumé bullet | portfolio-ready |
| 10 | Buffer / stretch (auto linkage checker) | optional |

## 9. Pitfalls & how to avoid them

- **Arbitrary Occurrence** → bind it to the published AIAG-VDA rate bands, not a feeling.
- **Generic failure modes** → anchor each to a real P1 defect/parameter.
- **Cosmetic linkage** → machine-check matching IDs; a matrix that never finds an orphan isn't verifying anything.
- **RPN habits** → use Action Priority; be ready to explain why AIAG-VDA moved away from RPN.

## 10. Definition of done

An AIAG-VDA PFMEA (data-driven Occurrence) + a linked Control Plan + a verified linkage matrix with zero orphaned High-AP risks, plus a reproducible Occurrence notebook and README outcome. Then publish to GitHub, tag `v1.0`.
