# Nexus-FMEA: Data-Driven PFMEA & Closed-Loop Control Plan Synchronization

*Full problem framing, rationale, scope, risks, deliverables, and interview positioning. The "why/what"; `execution.md` is the "how".*

---

## 1. Context & background

Advanced Product Quality Planning (APQP) rests on a chain of documents that must tell **one consistent story**: the Process Flow Diagram (PFD) defines the steps, the **PFMEA** identifies what can go wrong at each step and how risky it is, and the **Control Plan** specifies the controls that catch or prevent those risks on the shop floor. When the chain is intact, a high-risk failure mode in the PFMEA has a matching control in the Control Plan with the same characteristic, method, and reaction plan.

Two things routinely break this chain:
1. **Occurrence is guessed.** Teams assign the "how often" rating from memory, so the risk math is soft.
2. **The Control Plan drifts from the PFMEA.** A risk is identified but never gets a control — or a corrective action updates the PFMEA but not the Control Plan. This mismatch is a leading cause of PPAP rejection and audit findings.

This project attacks both with a real dataset: Occurrence comes from **measured defect rates**, and linkage is **proven**, not assumed.

## 2. Problem statement

> A multi-step machining process needs a risk analysis that (a) rates Occurrence from evidence rather than opinion, and (b) guarantees that every high-priority risk identified in the PFMEA is actually controlled on the shop floor. Without both, the risk analysis is unconvincing and the process is exposed to exactly the failures it claims to manage.

**Primary questions:**
1. What is the measured defect rate at each operation, and what Occurrence rating does that map to?
2. Which failure modes reach a High Action Priority once Occurrence is grounded in data?
3. Does every High/Medium-AP mode have a corresponding, correctly specified control in the Control Plan?

## 3. Stakeholders & personas

| Persona | Need |
|---------|------|
| Process / Quality Engineer | A defensible, evidence-based risk ranking to prioritize controls |
| Supplier Quality Engineer (customer) | A PFMEA and Control Plan that agree — the PPAP won't be rejected for linkage gaps |
| Operator / shop floor | A Control Plan with clear characteristics, sample plans, and reaction rules |
| Auditor (AS9100 / IATF 16949 / AS9145) | Traceable flow-down: risk → Special Characteristic → control |

## 4. Business & regulatory significance

- **PPAP acceptance:** OEM SQEs review the PFMEA and Control Plan together; a linkage gap gets the submission rejected. Proving 1-to-1 linkage removes the top rejection cause.
- **Right controls, right places:** grounding Occurrence in data focuses inspection effort on the operations that actually fail, instead of spreading controls evenly.
- **Standards:** AIAG-VDA FMEA (Action Priority), AS9145 (APQP/PPAP), IATF 16949 / AS9100 all require the PFMEA-to-Control-Plan flow-down, including Special Characteristic designation.

## 5. Objective

Produce a synchronized **PFMEA + Control Plan** package in which Occurrence is derived from measured defect rates and a **linkage matrix** proves that no high-priority risk is left uncontrolled.

## 6. Dataset rationale

Reuse Project 1's dataset so the two projects share a spine and the Occurrence numbers are real. The per-operation defect rates computed in P1 map directly onto the AIAG-VDA Occurrence scale. (Preferred CiP-DMD / fallback Bosch PLP — see `resources.md`.) No new dataset risk beyond P1's.

## 7. Solution approach (overview — detail in `execution.md`)

1. Build the PFD from the routing; summarize defect rate per operation.
2. Adopt AIAG-VDA S/O/D scales; define an explicit **defect-rate → Occurrence** mapping.
3. Author the PFMEA (modes, effects, causes, S/O/D, Action Priority).
4. Author the Control Plan (characteristic, spec, method, sample size/frequency, reaction plan), carrying SC/CTQ flags.
5. Build and check the **linkage matrix**; resolve any orphans.

## 8. Assumptions & constraints

- Defect rates from the dataset are a reasonable proxy for real-world Occurrence at each operation.
- The PFD is inferred from the dataset's routing; where the dataset lacks a step's detail, it's modeled explicitly and noted.
- This is a documentation-plus-analysis case study — the controls are *specified and justified*, not deployed on a live line.

## 9. Scope

**In scope:** one process (the P1 routing), a full AIAG-VDA PFMEA, a linked Control Plan, a linkage matrix, and the data-driven Occurrence derivation.
**Out of scope:** DFMEA, full APQP phases beyond process design, a software validator (kept as a lightweight stretch; the real tool lives in `quality-platform`).

## 10. Risk register

| Risk | Impact | Mitigation |
|------|--------|-----------|
| Defect-rate → Occurrence mapping is arbitrary | Weakens the "data-driven" claim | Use the published AIAG-VDA Occurrence rate bands; document the conversion explicitly |
| Sparse data at some operations | Unstable Occurrence for those steps | Aggregate to operation level; note confidence; don't over-rate thin evidence |
| PFMEA becomes generic | Reads as a template, not analysis | Tie each failure mode to a real defect/parameter observed in P1 |
| Linkage matrix is cosmetic | Doesn't actually prove flow-down | Machine-check the matrix (IDs must match on both sides); flag orphans explicitly |

## 11. Deliverables & acceptance criteria

| Deliverable | Acceptance criteria |
|-------------|--------------------|
| PFMEA (xlsx) | AIAG-VDA format; Occurrence values traceable to defect rates; Action Priority computed |
| Control Plan (xlsx) | Every High/Medium-AP mode has a control with characteristic, method, sample plan, reaction plan; SC/CTQ flags carried through |
| Linkage matrix | Zero orphaned high-AP risks; each link verified by matching IDs |
| Occurrence notebook | Reproducible defect-rate → Occurrence mapping |

## 12. Success metrics

- 100% of High-AP failure modes linked to a control (no orphans).
- Occurrence ratings each traceable to a measured defect rate.
- Special Characteristics flagged in the PFMEA appear in the Control Plan.

## 13. Interview / portfolio talking points

- **One-liner:** "I built a PFMEA whose Occurrence came from measured defect rates, then proved with a linkage matrix that every high-priority risk was actually controlled — the exact thing SQEs reject PPAPs over."
- **Likely question — "How did you set Occurrence?"** → measured defect rate per operation mapped onto the AIAG-VDA Occurrence bands, documented.
- **Likely question — "What's Action Priority vs RPN?"** → AIAG-VDA replaced RPN with the S→O→D Action Priority logic; explain why (RPN's equal-weighting problem).
- **Cross-link:** consumes Project 1's root cause as an input (D7 → this Control Plan update).

## 14. Résumé bullet variants

- **General/QE:** *"Built an AIAG-VDA PFMEA with Occurrence ratings derived from measured per-operation defect rates and a 1-to-1 linked Control Plan, eliminating the PFMEA-to-Control-Plan gaps that drive PPAP rejections."*
- **Aerospace track:** emphasize AS9145 flow-down and Special-Characteristic designation.
- **Semiconductor/automotive track:** emphasize AIAG-VDA Action Priority and data-based risk prioritization.
