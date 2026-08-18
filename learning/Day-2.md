# Day 2 — Per-operation defect rates (with Wilson CI) & the Process Flow Diagram

**Milestone:** M1 (Process & defect data) · **Prev:** [Day 1](Day-1.md) · **Next:** [Day 3](Day-3.md)

---

## Goal & why it matters

Today you produce the two artifacts M1 is built on: (1) a **defect rate per operation** — the
raw evidence that will set Occurrence — and (2) a **Process Flow Diagram (PFD)** that fixes
the operation sequence the PFMEA and Control Plan will both hang off of. Because some
operations have few failures, a bare percentage is misleading; you attach a **Wilson
confidence interval** so thin evidence is visibly thin and you don't over-rate it later.

## Concepts primer

- **Defect rate per operation** = (parts failing a characteristic attributable to that
  operation) ÷ (parts that went through that operation). This is a *proportion*.
- **Why a Wilson score interval, not a plain ±.** For proportions near 0 or with small
  samples (common at low-defect operations), the normal ("Wald") interval misbehaves — it can
  dip below 0 or claim false precision. The **Wilson score interval** stays inside [0,1] and
  is well-behaved for small `n`. You'll report the rate *and* its interval so Day 3's
  Occurrence mapping can flag low-confidence operations. Background:
  https://en.wikipedia.org/wiki/Binomial_proportion_confidence_interval#Wilson_score_interval
- **Process Flow Diagram (PFD).** The ordered list of operations with each step's key
  inputs/outputs and measured characteristics. In APQP the PFD is the backbone: the PFMEA
  analyzes risk *per PFD step*, and the Control Plan controls risk *per PFD step*. Same steps,
  same IDs, all the way through — that shared spine is what makes Day 7's linkage provable.

## Deliverables today

- `data/processed/defect_rates.csv` — finalized: one row per operation with
  `n_parts`, `n_fail`, `defect_rate`, `ci_low`, `ci_high`.
- `reports/figures/pfd.png` — the Process Flow Diagram.

## Step-by-step

**1. Compute the per-operation table.** In `01_occurrence_from_data.ipynb`, aggregate to
operation level and compute the rate + Wilson interval with `statsmodels`:

```python
import pandas as pd
from statsmodels.stats.proportion import proportion_confint

# Assumes df has columns: operation, failed (1/0). Adapt names to your P1 export.
g = df.groupby("operation")["failed"].agg(n_parts="count", n_fail="sum").reset_index()
g["defect_rate"] = g["n_fail"] / g["n_parts"]

ci = g.apply(
    lambda r: proportion_confint(r["n_fail"], r["n_parts"], alpha=0.05, method="wilson"),
    axis=1, result_type="expand",
)
g["ci_low"], g["ci_high"] = ci[0], ci[1]

g.to_csv("../data/processed/defect_rates.csv", index=False)
g
```

**2. Flag thin-evidence operations.** Add a boolean so Day 3 handles them conservatively:

```python
g["low_confidence"] = g["n_parts"] < 30   # tune threshold; note it in the notebook markdown
```

**3. Draw the Process Flow Diagram.** Keep it simple — an ordered box-and-arrow figure of the
operations with the key characteristic(s) each step controls. A minimal matplotlib version:

```python
import matplotlib.pyplot as plt

ops = g["operation"].tolist()   # ensure this is in true routing order
fig, ax = plt.subplots(figsize=(min(2*len(ops), 16), 2.5))
for i, op in enumerate(ops):
    ax.add_patch(plt.Rectangle((i*3, 0), 2, 1, fill=False))
    ax.text(i*3+1, 0.5, op, ha="center", va="center", fontsize=9)
    if i < len(ops)-1:
        ax.annotate("", xy=(i*3+3, 0.5), xytext=(i*3+2, 0.5),
                    arrowprops=dict(arrowstyle="->"))
ax.set_xlim(-0.5, 3*len(ops)); ax.set_ylim(-0.5, 1.5); ax.axis("off")
ax.set_title("Process Flow Diagram — machining routing")
fig.savefig("../reports/figures/pfd.png", dpi=150, bbox_inches="tight")
```

> If the dataset lacks detail for a step, model it explicitly and **note the assumption** in
> the notebook (per [`../idea.md §8`](../idea.md)) — don't silently invent a step.

**4. Assign a stable Process-Step / Characteristic ID** to each operation now (e.g. `OP10`,
`OP20`, …). You will reuse these exact IDs in the PFMEA, the Control Plan, and the linkage
matrix — decide them here, once.

**5. Commit:**

```bash
git add -A && git commit -m "Day 2: per-operation defect rates (Wilson CI) + PFD"
```

## Definition of done

- [ ] `defect_rates.csv` has `n_parts`, `n_fail`, `defect_rate`, `ci_low`, `ci_high` per operation.
- [ ] Low-sample operations flagged (`low_confidence`).
- [ ] Stable Process-Step IDs assigned (used everywhere downstream).
- [ ] `reports/figures/pfd.png` saved and legible.
- [ ] Any inferred/assumed process steps noted in the notebook.
- [ ] Day 2 committed.

## References

- Defect-rate + PFD step — [`../execution.md §2`](../execution.md)
- Assumptions on inferred steps — [`../idea.md §8`](../idea.md)
- Wilson score interval — https://en.wikipedia.org/wiki/Binomial_proportion_confidence_interval#Wilson_score_interval
- `statsmodels.proportion_confint` — https://www.statsmodels.org/stable/generated/statsmodels.stats.proportion.proportion_confint.html
