# Day 3 — AIAG-VDA rating scales & the defect-rate → Occurrence mapping

**Milestone:** M1 → M2 · **Prev:** [Day 2](Day-2.md) · **Next:** [Day 4](Day-4.md)

---

## Goal & why it matters

This is the intellectual core of the project. Today you (a) adopt the standard AIAG-VDA
Severity / Occurrence / Detection scales so scoring is auditable, and (b) define an
**explicit, documented conversion** from each operation's measured defect rate to its
Occurrence number. This conversion is exactly what an interviewer or SQE will probe — *"how
did you set Occurrence?"* — and the whole "data-driven" claim lives or dies on it being a
published rate-band lookup rather than a gut feeling.

## Concepts primer

- **AIAG-VDA S/O/D scales (1–10).** The AIAG & VDA FMEA Handbook (1st ed., 2019) defines three
  1–10 rating tables:
  - **Severity (S)** — how bad the *effect* is (safety/regulatory at the top).
  - **Occurrence (O)** — how often the *cause* leads to the failure mode; the handbook anchors
    each level to a **failure-rate band** (e.g. very high ≈ 1 in 10, …, very low ≈ 1 in 1M).
  - **Detection (D)** — how well current controls *catch* the cause/mode before it escapes.
- **Occurrence is band-based → that's your bridge to data.** Because each Occurrence level
  already corresponds to a failure-rate *range*, you map an operation's measured defect rate
  into the band whose range contains it, and read off that band's number. That's the entire
  trick: measured rate → band → Occurrence. Document the band table you use.
- **Action Priority (AP) replaced RPN.** Old FMEA multiplied S×O×D into an RPN, which treats a
  (10,1,1) and a (1,10,1) as identical and invites number-chasing. AIAG-VDA dropped RPN for
  **Action Priority** — a High/Medium/Low lookup on the S/O/D *combination* that weights
  Severity first. You compute AP on Day 5; understand *why* now so you can defend it. Explainers:
  search "AIAG VDA FMEA RPN to Action Priority why change" ([`../resources.md §3`](../resources.md)).
- **Thin evidence → conservative, flagged rating.** For operations you flagged
  `low_confidence` on Day 2, don't claim a precise low Occurrence off a handful of parts.
  Rate conservatively and note it (per [`../execution.md §3`](../execution.md)).

## Deliverables today

- A committed **scale reference** in the repo (e.g. `templates/aiag_vda_scales.md` or a
  notebook section) recording the S/O/D level definitions you're using — for auditability.
- A **documented defect-rate → Occurrence band table** plus a function that applies it.
- `notebooks/01_occurrence_from_data.ipynb` **complete**: every operation now has an
  Occurrence value traceable to its measured rate.

## Step-by-step

**1. Record the scales.** Create `templates/aiag_vda_scales.md` capturing the Severity,
Occurrence, and Detection level definitions you'll score against (summarize the AIAG-VDA
tables — the handbook itself is paywalled, so cite it and paraphrase the level anchors). This
file is what makes your scoring auditable.

**2. Define the Occurrence band table.** Encode the AIAG-VDA Occurrence rate bands as
ordered thresholds. Use the handbook's bands; the structure looks like:

```python
# (upper_rate_bound_inclusive, occurrence). Ordered high→low. Document each band's source.
OCC_BANDS = [
    (1e-0, 10),   # >= 1 in 10        -> O=10  (very high)
    (2e-2, 9),    # ~ 1 in 50         -> O=9
    (1e-2, 8),    # ~ 1 in 100        -> O=8
    (2e-3, 7),    # ~ 1 in 500        -> O=7
    (1e-3, 6),    # ~ 1 in 1,000      -> O=6
    (2e-4, 5),    # ~ 1 in 5,000      -> O=5
    (1e-4, 4),    # ~ 1 in 10,000     -> O=4
    (1e-5, 3),    # ~ 1 in 100,000    -> O=3
    (1e-6, 2),    # ~ 1 in 1,000,000  -> O=2
    (0.0,  1),    # < 1 in 1,000,000  -> O=1 (very low)
]
```

> **Important:** the exact rate cutoffs must match the edition of the AIAG-VDA Occurrence
> table you cite. Treat the numbers above as the *shape* of the mapping; set the real cutoffs
> from the handbook and note the source in the notebook. This is the number people will
> challenge — make it a lookup, not an opinion.

**3. Apply the mapping** to your Day-2 table:

```python
def rate_to_occurrence(rate, bands=OCC_BANDS):
    for upper, occ in bands:              # first band whose upper bound the rate falls under
        if rate >= upper:
            return occ
    return 1

g = pd.read_csv("../data/processed/defect_rates.csv")
g["occurrence"] = g["defect_rate"].apply(rate_to_occurrence)

# Conservative handling for thin evidence: don't let a tiny sample buy a very-low O.
g.loc[g["low_confidence"], "occurrence"] = g.loc[g["low_confidence"], "occurrence"].clip(lower=5)
g.to_csv("../data/processed/defect_rates.csv", index=False)
g[["operation", "defect_rate", "ci_high", "low_confidence", "occurrence"]]
```

**4. Write the justification** as a markdown cell: state the band source, show one worked
example ("OP30 measured 0.8% → falls in the 1-in-100 band → O=8"), and explain the
low-confidence clip. This paragraph is your interview answer, pre-written.

**5. Commit:**

```bash
git add -A && git commit -m "Day 3: AIAG-VDA scales + documented defect-rate->Occurrence mapping"
```

## Definition of done

- [ ] S/O/D scale definitions committed to the repo (`templates/aiag_vda_scales.md`).
- [ ] Occurrence band table encoded, with each band's rate cutoff sourced/noted.
- [ ] Every operation has an `occurrence` value in `defect_rates.csv`, traceable to its rate.
- [ ] Low-confidence operations rated conservatively and flagged.
- [ ] A written justification cell (with a worked example) in the notebook.
- [ ] Day 3 committed.

## References

- Scales + occurrence-mapping step — [`../execution.md §3`](../execution.md)
- AIAG & VDA FMEA Handbook (AP + S/O/D tables) — https://www.aiag.org/quality/automotive-core-tools/fmea
- Why AP replaced RPN — [`../resources.md §3`](../resources.md) (search "AIAG VDA FMEA RPN to Action Priority why change")
- SAE J1739 (PFMEA reference) — search "SAE J1739 PFMEA"
