# Day 6 — Author the Control Plan (one control per High/Medium-AP mode)

**Milestone:** M3 (Control Plan & linkage) · **Prev:** [Day 5](Day-5.md) · **Next:** [Day 7](Day-7.md)

---

## Goal & why it matters

The PFMEA says *what's risky*; the Control Plan says *how the shop floor catches or prevents
it*. This is the document operators actually use. Today you translate every High/Medium-AP
mode from the PFMEA into a concrete control — characteristic, spec, measurement method, sample
plan, control method, and reaction plan — carrying the SC/CTQ flags across. Do this well and
tomorrow's linkage matrix has zero orphans; skip a mode and you've recreated the #1 PPAP
rejection cause on purpose.

## Concepts primer

- **What a Control Plan row must specify** (per [`../execution.md §5`](../execution.md)):
  - **Process step / characteristic** (+ SC/CTQ symbol carried from the PFMEA),
  - **Specification / tolerance**,
  - **Evaluation / measurement technique** (gauge or method),
  - **Sample size & frequency**,
  - **Control method** (e.g. SPC chart, 100% inspection, poka-yoke),
  - **Reaction plan** (what to do on an out-of-control / nonconforming result).
- **Every High/Medium-AP mode needs a row.** The Control Plan is where the PFMEA's recommended
  actions become standing controls. High-AP first (mandatory); Medium-AP as justified.
- **SC/CTQ must flow through.** Any characteristic flagged Special in the PFMEA carries its
  symbol here — auditors trace risk → Special Characteristic → control. A dropped flag is a
  finding.
- **Match the control to the failure, not to habit.** Grounding Occurrence in data (Day 3) was
  about putting effort where failures actually happen — honor that here: heavier controls on
  the high-Occurrence / high-Severity steps, lighter where evidence says the process is stable.

## Deliverables today

- `reports/Control_Plan.xlsx` — one control row per High/Medium-AP PFMEA mode, all six fields
  populated, SC/CTQ symbols carried across, keyed by the shared `Process Step ID`.

## Step-by-step

**1. Get or build a blank AIAG Control Plan template** in `templates/`. Minimum columns:

```
Process Step ID | Process Step / Characteristic | Special Char (SC/CTQ) | Spec / Tolerance
| Measurement Technique / Gauge | Sample Size | Sample Frequency | Control Method
| Reaction Plan | Linked PFMEA Mode
```

> Reuse the **same `Process Step ID`** and add a `Linked PFMEA Mode` column referencing the
> PFMEA row — these two columns are what Day 7 verifies.

**2. Pull the modes to control.** Filter the PFMEA to High (and chosen Medium) AP rows; each
becomes at least one Control Plan row:

```python
pfmea = pd.read_excel("../reports/PFMEA.xlsx")
to_control = pfmea[pfmea["Action Priority (AP)"].isin(["High", "Medium"])]
print(len(to_control), "modes need a control")
```

**3. Author each control row.** For every mode, decide and fill: characteristic, spec,
gauge/method, sample size, frequency, control method, reaction plan. Carry the SC/CTQ symbol
from the PFMEA row. Set the `Linked PFMEA Mode` reference.

**4. Cross-check coverage before you stop.** Confirm every High-AP mode now has ≥1 Control Plan
row and every PFMEA SC/CTQ appears here:

```python
cp = pd.read_excel("../reports/Control_Plan.xlsx")
high = pfmea[pfmea["Action Priority (AP)"] == "High"]["Process Step ID"].unique()
missing = set(high) - set(cp["Process Step ID"])
assert not missing, f"High-AP steps with no control: {missing}"   # fix before Day 7
```

**5. Save & commit:**

```bash
git add -A && git commit -m "Day 6: Control Plan — controls for High/Medium-AP modes, SC/CTQ carried"
```

## Definition of done

- [ ] `reports/Control_Plan.xlsx` exists with the columns above.
- [ ] Every High-AP mode has ≥1 control row (Medium as justified).
- [ ] All six control fields populated per row (characteristic, spec, method, sample size/freq, control method, reaction plan).
- [ ] SC/CTQ symbols carried from PFMEA to Control Plan.
- [ ] `Process Step ID` + `Linked PFMEA Mode` set for the Day-7 join.
- [ ] Coverage cross-check passes (no High-AP step missing a control).
- [ ] Day 6 committed.

## References

- Control Plan authoring spec — [`../execution.md §5`](../execution.md)
- AIAG Control Plan core tool — https://www.aiag.org/quality/automotive-core-tools
- Control Plan example (video) — search "control plan example manufacturing quality"
- Linkage / SC flow-down — search "PFMEA control plan linkage special characteristics flow down"
