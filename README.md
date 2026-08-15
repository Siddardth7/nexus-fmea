# Nexus-FMEA: Data-Driven PFMEA & Closed-Loop Control Plan Synchronization

> Constructing an AIAG-VDA Process FMEA whose **Occurrence** ratings are derived from measured per-operation defect rates, provably synchronized 1-to-1 to a shop-floor Control Plan under AIAG-VDA and AS9145 standards.

![status](https://img.shields.io/badge/status-planning-yellow)
![python](https://img.shields.io/badge/python-3.11-blue)
![framework](https://img.shields.io/badge/AIAG--VDA-Action%20Priority-informational)
![standards](https://img.shields.io/badge/standards-AS9145%20%7C%20IATF%2016949-purple)
![license](https://img.shields.io/badge/license-MIT-green)

**Codename:** `nexus-fmea`  
**Formal Case Study Title:** Data-Driven Process FMEA and Closed-Loop Control Plan Synchronization under AIAG-VDA & AS9145  
**Skill area:** PFMEA · Control Plan · APQP Risk Architecture · AIAG-VDA Action Priority · AS9145 Flow-Down  
**Domain:** Advanced Manufacturing Quality Systems (Automotive, Aerospace, Medical Device)  
**Headline deliverable:** A synchronized PFMEA + Control Plan package with a data-backed Occurrence justification and a machine-verified linkage matrix.

---

## The problem

A PFMEA is only as good as its numbers, and its numbers are usually guessed. Worse, the single most common audit finding is a PFMEA whose high-risk failure modes **don't appear** in the Control Plan — the risk is identified but never controlled. This project fixes both: it derives Occurrence from measured defect rates and then proves that every high-Action-Priority risk has a matching control, characteristic, method, and reaction plan.

## The dataset

Reuses the multi-station machining data from Project 1 — **CiP-DMD** (preferred, named parameters) or **Bosch Production Line Performance** (fallback). The per-operation defect rates measured there become the empirical basis for the Occurrence ratings here, so the FMEA is evidence-based rather than subjective.

## Approach in three moves

1. **Build the Process Flow** from the routing, and compute real defect rates per operation.
2. **Score the PFMEA** with AIAG-VDA Severity/Occurrence/Detection → Action Priority, anchoring Occurrence to measured rates.
3. **Build + verify the Control Plan** — translate each high-AP risk into a control, then prove 1-to-1 linkage with a cross-reference matrix.

## Deliverables

- `reports/PFMEA.xlsx` — AIAG-VDA-format PFMEA with data-driven Occurrence.
- `reports/Control_Plan.xlsx` — linked Control Plan (characteristic, method, sample size/frequency, reaction plan).
- `reports/linkage_matrix.md` — proof that every high-AP risk maps to a control.
- `notebooks/occurrence_from_data.ipynb` — the defect-rate → Occurrence derivation.

## Repository structure

```
nexus-fmea/
├── README.md · roadmap.md · idea.md · execution.md · resources.md
├── data/           # reused defect-rate summary from Project 1
├── notebooks/      # Occurrence derivation
└── reports/        # PFMEA, Control Plan, linkage matrix
```

## Status

Planning. See [`roadmap.md`](roadmap.md).

## Author

**Siddardth Pathipaka** — Quality & Process Engineer · M.S. Aerospace (UIUC) · Six Sigma Green Belt · [@Siddardth7](https://github.com/Siddardth7)
