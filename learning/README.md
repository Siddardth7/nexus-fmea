# Learning Runbook — NEXUS-FMEA (Day 1 → Day 7)

A day-by-day execution guide for building the **Data-Driven PFMEA + Linked Control Plan**
case study with minimum supervision. Each day is one focused sitting: read the concepts
primer, do the numbered steps, tick the *Definition of done* boxes, commit, move on.

> This folder is the **runbook** (the day-by-day *do this now*). The *why* and *how* behind
> it live in the source docs — keep them open as reference:
> [`../idea.md`](../idea.md) (why/what) · [`../execution.md`](../execution.md) (how) ·
> [`../roadmap.md`](../roadmap.md) (milestones) · [`../resources.md`](../resources.md) (links).

---

## How to use this folder

1. Work **one day file per session**, in order. Each assumes the previous day's outputs exist.
2. Copy-paste the commands/snippets; adapt paths only where noted.
3. At the end of each day, complete the *Definition of done* checklist and `git commit`.
4. Update the **Status** column below as you go (`☐` → `✅`).

The original schedule in [`execution.md`](../execution.md) spans 10 days; publish + buffer are
folded into **Day 7** here so the whole build fits Day 1–7.

---

## The 7 days

| Day | Focus | Key deliverable(s) | Milestone | Status |
|-----|-------|--------------------|-----------|:------:|
| [Day 1](Day-1.md) | Repo scaffold, `.venv`, reuse P1 defect data, start Occurrence notebook | working env, `data/processed/defect_rates.csv` imported | M1 | ☐ |
| [Day 2](Day-2.md) | Per-operation defect rate + Wilson CI; Process Flow Diagram | finalized `defect_rates.csv`, `reports/figures/pfd.png` | M1 | ☐ |
| [Day 3](Day-3.md) | AIAG-VDA S/O/D scales; defect-rate → Occurrence mapping | `notebooks/01_occurrence_from_data.ipynb`, committed scale tables | M1→M2 | ☐ |
| [Day 4](Day-4.md) | PFMEA pass 1: function → mode → effect (**Severity**) → cause → **Detection** | `reports/PFMEA.xlsx` draft | M2 | ☐ |
| [Day 5](Day-5.md) | PFMEA pass 2: **Occurrence** (from data) → **Action Priority** → SC/CTQ → actions | `reports/PFMEA.xlsx` complete | M2 | ☐ |
| [Day 6](Day-6.md) | Control Plan: one control per High/Med-AP mode; carry SC/CTQ | `reports/Control_Plan.xlsx` | M3 | ☐ |
| [Day 7](Day-7.md) | Linkage matrix + machine-check + package/publish (`v1.0`) | `reports/linkage_matrix.md`, tagged GitHub repo | M3→M4 | ☐ |

**Milestones** map to [`roadmap.md`](../roadmap.md): M1 Process & defect data · M2 PFMEA (data
Occurrence) · M3 Control Plan & linkage · M4 Package & publish.

---

## Definition of done for the whole project

From [`idea.md §11`](../idea.md) / [`execution.md §7`](../execution.md):

- [ ] Every Occurrence value traces to a defect rate in the notebook.
- [ ] Action Priority computed by AIAG-VDA logic (documented), **not** an ad-hoc RPN.
- [ ] Zero orphaned High-AP risks in the linkage matrix.
- [ ] SC/CTQ flags present in **both** PFMEA and Control Plan.
- [ ] Rating-scale tables committed to the repo for auditability.
- [ ] Published to GitHub, tagged `v1.0`.

Final deliverable set (see [`README.md`](../README.md)): `reports/PFMEA.xlsx`,
`reports/Control_Plan.xlsx`, `reports/linkage_matrix.md`,
`notebooks/01_occurrence_from_data.ipynb`.
