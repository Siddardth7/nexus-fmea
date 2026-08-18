# Day 4 — PFMEA authoring, pass 1: structure, Severity & Detection

**Milestone:** M2 (PFMEA with data-driven Occurrence) · **Prev:** [Day 3](Day-3.md) · **Next:** [Day 5](Day-5.md)

---

## Goal & why it matters

Today you build the skeleton of the PFMEA and fill in the two ratings that come from
engineering judgment rather than the dataset: **Severity** (from the worst effect) and
**Detection** (from current controls). You deliberately hold **Occurrence** for Day 5, where
it comes from the data. Splitting it this way keeps the "judgment" columns and the "measured"
column cleanly separable — which is exactly the story that makes this PFMEA credible.

## Concepts primer

- **The AIAG-VDA PFMEA row structure** (per operation, one row per failure mode):
  **Function** → **Failure Mode** → **Failure Effect(s)** → **Severity (S)** → **Failure
  Cause(s)** → **Current Prevention/Detection Controls** → **Detection (D)** → *(Occurrence &
  AP come Day 5)*.
- **Severity is effect-based.** Rate S from the *worst* consequence of the failure mode
  (customer, downstream operation, safety/regulatory). One mode can have several effects —
  score the harshest.
- **Detection is control-based.** Rate D on how well *current* controls would catch the cause
  or mode before it escapes. No controls / detection only at the customer = high D (bad);
  robust in-station error-proofing that prevents escape = low D (good).
- **Anchor every mode to something real.** The single biggest failure of case-study FMEAs is
  genericness. Tie each failure mode to an actual defect or parameter observed in Project 1
  (per [`../execution.md §4`](../execution.md) and [`../idea.md §10`](../idea.md)) so it reads
  as analysis, not a template.
- **Special Characteristics (SC/CTQ) — first pass.** Where a mode has high Severity tied to
  safety or regulatory compliance, mentally flag it as a candidate Special Characteristic. You
  finalize SC/CTQ flags on Day 5 and carry them into the Control Plan on Day 6 — that flow-down
  is an AS9145 / IATF 16949 requirement.

## Deliverables today

- `reports/PFMEA.xlsx` — **draft**: one sheet, AIAG-VDA columns, all operations covered, with
  Function / Mode / Effect / **Severity** / Cause / Current Controls / **Detection** filled.
  Occurrence and Action Priority columns exist but are left blank for Day 5.

## Step-by-step

**1. Get or build a blank AIAG-VDA PFMEA template** in `templates/`. Either download an
AIAG-VDA-format template or create the columns yourself. Minimum columns:

```
Process Step ID | Process Step / Function | Failure Mode | Failure Effect(s) | Severity (S)
| Failure Cause(s) | Current Prevention Controls | Current Detection Controls | Detection (D)
| Occurrence (O) | Action Priority (AP) | Special Char (SC/CTQ) | Recommended Action
```

> Use the **same `Process Step ID`s** you assigned on Day 2 — this column is the key Day 7's
> linkage matrix joins on.

**2. Work operation by operation** down the PFD. For each operation, ask: *what can go wrong
here?* For each failure mode, fill:
- **Function** — what the step is supposed to achieve.
- **Failure Mode** — how it fails to achieve it (tie to a real P1 defect/parameter).
- **Effect(s)** + **Severity** — worst-case effect, scored on your Day-3 Severity scale.
- **Cause(s)** — the mechanism(s) that produce the mode.
- **Current Prevention/Detection Controls** + **Detection** — what exists today, scored on
  your Day-3 Detection scale.

**3. Leave Occurrence & AP blank.** Put a visible placeholder (e.g. `TBD-D5`) so it's obvious
they're intentionally deferred.

**4. First-pass SC/CTQ candidates.** Mark high-Severity safety/compliance modes as candidate
Special Characteristics in the SC column.

**5. Save & commit** (write with `openpyxl` from the notebook, or edit the template directly
and save as `reports/PFMEA.xlsx`):

```bash
git add -A && git commit -m "Day 4: PFMEA draft — structure, Severity, Detection"
```

## Definition of done

- [ ] `reports/PFMEA.xlsx` exists with the AIAG-VDA columns above.
- [ ] Every operation from the PFD is represented; each has ≥1 failure mode.
- [ ] Severity and Detection scored for every row using the Day-3 scales.
- [ ] Each failure mode traces to a real P1 defect/parameter (not generic).
- [ ] `Process Step ID` matches the Day-2 IDs.
- [ ] Occurrence & AP left as clearly-marked placeholders.
- [ ] Candidate SC/CTQ modes flagged.
- [ ] Day 4 committed.

## References

- PFMEA authoring spec — [`../execution.md §4`](../execution.md)
- Keep modes non-generic — [`../idea.md §10`](../idea.md) (risk register)
- AIAG & VDA FMEA Handbook — https://www.aiag.org/quality/automotive-core-tools/fmea
- PFMEA walkthrough (video) — search "PFMEA AIAG VDA example walkthrough Action Priority"
