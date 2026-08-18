# Day 5 — PFMEA authoring, pass 2: Occurrence (from data), Action Priority, SC/CTQ, actions

**Milestone:** M2 (PFMEA with data-driven Occurrence) · **Prev:** [Day 4](Day-4.md) · **Next:** [Day 6](Day-6.md)

---

## Goal & why it matters

Today the PFMEA becomes *data-driven* and *complete*. You inject the Occurrence values you
derived on Day 3, compute **Action Priority** by the AIAG-VDA logic (not RPN), finalize the
Special-Characteristic flags, and write recommended actions for the High-AP modes. When you
finish, `PFMEA.xlsx` is done and every High/Medium-AP mode is queued for a control on Day 6.

## Concepts primer

- **Occurrence comes from the data — join, don't guess.** Pull each operation's `occurrence`
  from `defect_rates.csv` (Day 3) and drop it into the matching PFMEA rows by `Process Step
  ID`. Now every O value traces to a measured rate.
- **Action Priority (AP), the AIAG-VDA way.** AP is a **High / Medium / Low** result from a
  lookup on the *combination* of S, O, and D — Severity dominates, then Occurrence, then
  Detection. It is **not** S×O×D. High AP = act (or justify inaction); Medium = should act;
  Low = optional. Use the AP table from the AIAG-VDA handbook; if you encode it, encode the
  table, don't approximate with a product.
- **Special Characteristics finalized & flowed down.** Confirm which modes carry an SC/CTQ
  flag (safety/regulatory, high Severity). These flags **must** reappear in the Control Plan
  (Day 6) and be verified in the linkage matrix (Day 7) — that's the AS9145 / IATF 16949
  flow-down auditors check.
- **Recommended actions.** For every High-AP mode, state a recommended action (add/strengthen
  a control, error-proof, tighten detection). These become Control Plan rows tomorrow.

## Deliverables today

- `reports/PFMEA.xlsx` — **complete**: Occurrence filled from data, Action Priority computed,
  SC/CTQ finalized, recommended actions written for High-AP modes.

## Step-by-step

**1. Join Occurrence into the PFMEA.** If authoring via the notebook, merge on `Process Step
ID`; if editing the workbook by hand, copy each operation's Day-3 `occurrence` into its rows.

```python
pfmea = pd.read_excel("../reports/PFMEA.xlsx")
occ = pd.read_csv("../data/processed/defect_rates.csv")[["operation", "occurrence"]]
pfmea = pfmea.merge(occ, left_on="Process Step ID", right_on="operation", how="left")
pfmea["Occurrence (O)"] = pfmea["occurrence"]
assert pfmea["Occurrence (O)"].notna().all(), "Every row must get an Occurrence from data"
```

**2. Compute Action Priority** from the AIAG-VDA AP table. Encode the handbook's High/Med/Low
logic as a lookup on (S, O, D):

```python
def action_priority(s, o, d):
    # Replace this with the AIAG-VDA AP table logic (High/Medium/Low by S,O,D bands).
    # Severity-first: high S with meaningful O/D -> High. Document the table source.
    ...
    return "High" | "Medium" | "Low"

pfmea["Action Priority (AP)"] = pfmea.apply(
    lambda r: action_priority(r["Severity (S)"], r["Occurrence (O)"], r["Detection (D)"]),
    axis=1,
)
```

> Do **not** substitute `S*O*D`. If you can't reproduce the full AP table, cite it and map the
> key bands faithfully; note exactly what you implemented.

**3. Finalize SC/CTQ flags** — confirm the candidates from Day 4; add any high-Severity
safety/compliance modes you missed.

**4. Write recommended actions** for every `High` AP row (and note which Medium ones you'll
also control). Be specific — the action names the control you'll specify on Day 6.

**5. Save & sanity-check, then commit:**

```python
pfmea.drop(columns=["operation", "occurrence"]).to_excel("../reports/PFMEA.xlsx", index=False)
print("High-AP modes:", (pfmea["Action Priority (AP)"] == "High").sum())
```

```bash
git add -A && git commit -m "Day 5: PFMEA complete — data Occurrence, Action Priority, SC/CTQ, actions"
```

## Definition of done

- [ ] Every PFMEA row has an Occurrence value pulled from `defect_rates.csv` (no blanks/`TBD`).
- [ ] Action Priority computed by AIAG-VDA logic (documented), **not** RPN.
- [ ] SC/CTQ flags finalized on the relevant modes.
- [ ] Every High-AP mode has a recommended action.
- [ ] You know the count of High/Medium-AP modes (it drives Day 6's Control Plan rows).
- [ ] `reports/PFMEA.xlsx` complete and committed.

## References

- PFMEA pass-2 spec — [`../execution.md §4`](../execution.md)
- Action Priority vs RPN talking point — [`../idea.md §13`](../idea.md)
- AP & S/O/D tables — https://www.aiag.org/quality/automotive-core-tools/fmea
- Special-characteristic flow-down — search "IATF 16949 special characteristics control plan"
