# Day 7 — Linkage matrix, machine verification & publish (v1.0)

**Milestone:** M3 → M4 (Linkage · Package & publish) · **Prev:** [Day 6](Day-6.md) · **Next:** —

---

## Goal & why it matters

The finish line. Today you **prove** — with code, not assertion — that every high-priority
risk in the PFMEA is actually controlled in the Control Plan, output that proof as a linkage
matrix, then package and publish the case study. The machine-checked matrix is the headline
deliverable: it's the exact thing SQEs reject PPAPs over, turned into a green checkmark. A
matrix that never finds an orphan isn't verifying anything, so the check must be able to fail.

## Concepts primer

- **Linkage = a join on a shared key.** Both documents carry the same `Process Step ID`. The
  matrix is built by joining PFMEA rows to Control Plan rows on that key and asking three
  questions:
  1. Does **every High-AP PFMEA row** have ≥1 matching Control Plan row?
  2. Does **every SC/CTQ flag** in the PFMEA appear in the Control Plan?
  3. Are the **detection controls** named in the PFMEA represented in the Control Plan's method
     column?
- **Orphans, both directions.** An *orphan risk* = a High-AP mode with no control (the
  dangerous one). An *orphan control* = a control with no corresponding risk (usually a
  bookkeeping slip). Flag both.
- **Machine-check, don't eyeball.** The value is that the check is reproducible and *falsifiable*
  — it flags real gaps. Keep the assertion in the notebook/script so re-running regenerates the
  proof.

## Deliverables today

- `reports/linkage_matrix.md` — every link listed; **zero orphaned High-AP risks**; orphans (if
  any) explicitly flagged.
- A one-paragraph outcome summary added to the project `README.md` + a résumé bullet.
- Published GitHub repo, tagged **`v1.0`**.

## Step-by-step

**1. Build the matrix and run the checks** (in `01_occurrence_from_data.ipynb` or a small
`scripts/check_linkage.py`):

```python
import pandas as pd

pfmea = pd.read_excel("../reports/PFMEA.xlsx")
cp    = pd.read_excel("../reports/Control_Plan.xlsx")

high = pfmea[pfmea["Action Priority (AP)"] == "High"]
linked = high.merge(cp, on="Process Step ID", how="left", suffixes=("_pfmea", "_cp"))

orphan_risks   = linked[linked["Control Method"].isna()]["Process Step ID"].unique()
sc_pfmea = set(pfmea.loc[pfmea["Special Char (SC/CTQ)"].notna(), "Process Step ID"])
sc_cp    = set(cp.loc[cp["Special Char (SC/CTQ)"].notna(), "Process Step ID"])
sc_missing = sc_pfmea - sc_cp
orphan_controls = set(cp["Process Step ID"]) - set(pfmea["Process Step ID"])

assert len(orphan_risks) == 0, f"Orphaned High-AP risks: {orphan_risks}"
assert len(sc_missing) == 0,   f"SC/CTQ in PFMEA but not Control Plan: {sc_missing}"
```

**2. Write `reports/linkage_matrix.md`.** Emit a table of each High-AP mode → its control(s),
plus an explicit orphans section (empty if clean). Generate it from the join so it stays true:

```python
lines = ["# Linkage Matrix — PFMEA ↔ Control Plan\n",
         "| Process Step ID | PFMEA Failure Mode | AP | SC/CTQ | Control Method | Reaction Plan |",
         "|---|---|---|---|---|---|"]
for _, r in linked.iterrows():
    lines.append(f"| {r['Process Step ID']} | {r.get('Failure Mode','')} | "
                 f"{r.get('Action Priority (AP)','')} | {r.get('Special Char (SC/CTQ)_pfmea','')} | "
                 f"{r.get('Control Method','')} | {r.get('Reaction Plan','')} |")
lines += ["\n## Orphans",
          f"- Orphaned High-AP risks (no control): {list(orphan_risks) or 'NONE ✅'}",
          f"- SC/CTQ in PFMEA missing from Control Plan: {list(sc_missing) or 'NONE ✅'}",
          f"- Orphaned controls (no risk): {list(orphan_controls) or 'NONE ✅'}"]
open("../reports/linkage_matrix.md", "w").write("\n".join(lines))
```

**3. Run the full validation checklist** (from [`../execution.md §7`](../execution.md)):

- [ ] Every Occurrence value traces to a defect rate in the notebook.
- [ ] Action Priority computed by AIAG-VDA logic (documented), not RPN.
- [ ] Zero orphaned High-AP risks in the linkage matrix.
- [ ] SC/CTQ flags present in both documents.
- [ ] Rating-scale tables committed to the repo.

**4. Update `README.md` + write the résumé bullet.** Add the outcome (e.g. "N High-AP modes,
100% linked, 0 orphans; Occurrence traceable to measured rates"). Résumé variants are in
[`../idea.md §14`](../idea.md) — pick/adapt one.

**5. Publish and tag:**

```bash
git add -A && git commit -m "Day 7: linkage matrix verified (0 orphans) + README outcome"
git push -u origin main
git tag -a v1.0 -m "NEXUS-FMEA v1.0 — data-driven PFMEA + linked Control Plan"
git push origin v1.0
```

**6. (Optional stretch)** Promote the linkage check into a standalone
`scripts/check_linkage.py` that fails CI on any orphan — a nod to the `quality-platform`
validator, kept lightweight ([`../roadmap.md` Stretch](../roadmap.md)).

## Definition of done

- [ ] `reports/linkage_matrix.md` generated from the join; **0 orphaned High-AP risks**.
- [ ] SC/CTQ present in both documents (asserted, not eyeballed).
- [ ] Full validation checklist passes.
- [ ] `README.md` outcome paragraph + résumé bullet written.
- [ ] Repo pushed and tagged `v1.0`.
- [ ] Final deliverables present: `PFMEA.xlsx`, `Control_Plan.xlsx`, `linkage_matrix.md`, `01_occurrence_from_data.ipynb`.

## References

- Linkage matrix + verification spec — [`../execution.md §6`](../execution.md)
- Validation checklist & definition of done — [`../execution.md §7,§10`](../execution.md)
- Résumé bullet variants & talking points — [`../idea.md §13,§14`](../idea.md)
- Stretch: auto linkage checker — [`../roadmap.md`](../roadmap.md)
