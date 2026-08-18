# Resources — Data-Driven PFMEA + Linked Control Plan

Datasets & papers as verified original links (✅); videos/tutorials as search strings (🔎).

---

## 1. Dataset

Reuses Project 1's data (defect rates → Occurrence):
- ✅ **CiP-DMD** (preferred) — https://zenodo.org/records/8420132 · paper: https://www.sciencedirect.com/science/article/pii/S2212827124009624 (data-access caveat: see Project 1 `resources.md`).
- ✅ **Bosch Production Line Performance** (fallback) — https://www.kaggle.com/c/bosch-production-line-performance/data

## 2. Standards & methodology (original references)

- **AIAG & VDA FMEA Handbook (1st ed., 2019)** — defines the PFMEA structure and the **Action Priority (AP)** method that replaced RPN, plus the Severity/Occurrence/Detection tables. Publisher (AIAG): https://www.aiag.org/quality/automotive-core-tools/fmea 🔎 also "AIAG VDA FMEA handbook Action Priority tables"
- **AIAG Control Plan (CQI / Core Tools)** — Control Plan format & linkage. https://www.aiag.org/quality/automotive-core-tools 🔎 "AIAG control plan core tool"
- **SAE J1739** — PFMEA reference standard. 🔎 "SAE J1739 PFMEA"
- **AS9145** — Aerospace APQP & PPAP (PFMEA→Control Plan flow-down). 🔎 "AS9145 APQP PPAP PFMEA control plan"
- **IATF 16949 / AS9100** — require the risk-to-control flow-down and Special Characteristic designation. 🔎 "IATF 16949 special characteristics control plan"

## 3. Research / reading

- ✅ AIAG-VDA transition explainers (why Action Priority replaced RPN): 🔎 "AIAG VDA FMEA RPN to Action Priority why change" (Quality-One, Plexus, ASQ articles).
- ✅ Jourdan et al., CiP-DMD paper — https://www.sciencedirect.com/science/article/pii/S2212827124009624 (source of the defect-rate structure).

## 4. Python libraries

| Library | Use | Docs |
|---------|-----|------|
| pandas / numpy | defect-rate computation | https://pandas.pydata.org/docs/ |
| statsmodels | Wilson CI for proportions | https://www.statsmodels.org/stable/ |
| openpyxl | write PFMEA / Control Plan xlsx | https://openpyxl.readthedocs.io/ |
| matplotlib | process-flow / defect figures | https://matplotlib.org/ |

Install: `pip install pandas numpy statsmodels openpyxl matplotlib jupyter`

## 5. Tutorials & video (search strings — 🔎)

- PFMEA walkthrough → 🔎 "PFMEA AIAG VDA example walkthrough Action Priority"
- Control Plan basics → 🔎 "control plan example manufacturing quality"
- PFMEA↔Control Plan linkage → 🔎 "PFMEA control plan linkage special characteristics flow down"
- Occurrence rating from data → 🔎 "FMEA occurrence rating failure rate table"

## 6. Books

- AIAG & VDA — *FMEA Handbook* (the primary reference).
- Carl Carlson — *Effective FMEAs*.
- Stamatis, D.H. — *Failure Mode and Effect Analysis: FMEA from Theory to Execution*.

---

*Links verified Aug 14, 2026. Standards bodies gate the actual PDFs behind purchase; names/links point to the official catalog pages.*
