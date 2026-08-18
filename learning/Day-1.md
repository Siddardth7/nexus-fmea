# Day 1 — Environment, scaffold & reuse the Project 1 defect data

**Milestone:** M1 (Process & defect data) · **Prev:** — · **Next:** [Day 2](Day-2.md)

---

## Goal & why it matters

The entire "data-driven" claim of this project rests on one thing: the Occurrence ratings in
the PFMEA come from **measured** per-operation defect rates, not from opinion. Those rates
were already computed in Project 1 (Sentinel-8D). Today you stand up a clean, reproducible
Python environment, create the repo skeleton, and pull that defect data in as the spine of
this project. Get this right and every downstream number is traceable; get it sloppy and the
"evidence-based" story falls apart in an audit.

## Concepts primer

- **Why reuse Project 1's dataset.** Sharing the dataset means the Occurrence numbers are
  *real* and the two projects form one continuous story (P1 finds/roots the defects → P2
  turns those rates into risk ratings and controls). See [`../idea.md §6`](../idea.md).
- **Tidy data = one row per part.** You want a table where each row is a part and columns
  record which operation(s) it passed through and whether it failed a characteristic. That
  shape makes per-operation defect rates a simple `groupby`. (You compute the rates on Day 2.)
- **Datasets:** CiP-DMD (preferred, named parameters) or Bosch Production Line Performance
  (fallback). Links in [`../resources.md §1`](../resources.md). If you already exported a
  processed table in P1, reuse that export directly — don't re-derive.

## Deliverables today

- A working `.venv` with the project libraries + `requirements.txt`.
- The repo scaffold (folders below) created and committed.
- `data/processed/defect_rates.csv` (or the raw P1 export) **imported and loading cleanly**
  in a fresh notebook cell.
- `notebooks/01_occurrence_from_data.ipynb` created with the data-load cell working.

## Step-by-step

**1. Create and activate the environment** (from [`../execution.md §1`](../execution.md)):

```bash
cd nexus-fmea
python3.11 -m venv .venv && source .venv/bin/activate
pip install pandas numpy statsmodels openpyxl matplotlib jupyter
pip freeze > requirements.txt
```

> `statsmodels` is for the Wilson confidence interval you use on Day 2 — install it now.

**2. Create the scaffold** (from [`../execution.md §1`](../execution.md)):

```bash
mkdir -p data/raw data/processed notebooks templates reports/figures
touch reports/linkage_matrix.md
```

Target structure:

```
nexus-fmea/
├── data/processed/defect_rates.csv     # from Project 1 (Sentinel-8D)
├── notebooks/01_occurrence_from_data.ipynb
├── templates/                          # blank AIAG-VDA PFMEA + Control Plan (added Day 4/6)
├── reports/PFMEA.xlsx · Control_Plan.xlsx · linkage_matrix.md
└── requirements.txt
```

**3. Bring in the Project 1 data.** Copy the processed defect-rate/summary export from
Sentinel-8D into `data/processed/` (or `data/raw/` if it still needs aggregating on Day 2):

```bash
cp ../sentinel-8d/data/processed/<your_p1_export>.csv data/processed/defect_rates.csv
```

**4. Start the notebook.** Launch Jupyter and create `notebooks/01_occurrence_from_data.ipynb`
with a first cell that loads and sanity-checks the data:

```python
import pandas as pd

df = pd.read_csv("../data/processed/defect_rates.csv")
print(df.shape)
df.head()
# Confirm: one row per part (or per operation), a pass/fail column, an operation identifier.
```

**5. Add a `.gitignore`** (keep `.venv/` and data blobs out of git as appropriate) and make
the first commit:

```bash
printf ".venv/\n__pycache__/\n*.pyc\n.ipynb_checkpoints/\n" > .gitignore
git add -A && git commit -m "Day 1: env, scaffold, import P1 defect data"
```

## Definition of done

- [ ] `.venv` active; `pip freeze > requirements.txt` written.
- [ ] Scaffold folders exist (`data/`, `notebooks/`, `templates/`, `reports/figures/`).
- [ ] P1 data copied into `data/` and loads without error in the notebook.
- [ ] `notebooks/01_occurrence_from_data.ipynb` created with a working load cell.
- [ ] Day 1 committed to git.

## References

- Env & scaffold spec — [`../execution.md §1`](../execution.md)
- Dataset rationale — [`../idea.md §6`](../idea.md)
- Dataset links (CiP-DMD / Bosch PLP) — [`../resources.md §1`](../resources.md)
- pandas docs — https://pandas.pydata.org/docs/
