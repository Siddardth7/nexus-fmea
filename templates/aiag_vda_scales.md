# AIAG & VDA FMEA Rating Scales & Action Priority Reference

**Reference Standard:** AIAG & VDA FMEA Handbook (1st Edition, 2019) / SAE J1739  
**Purpose:** Formal, auditable rating criteria for Process Failure Mode and Effects Analysis (PFMEA) and Control Plan synchronization.

---

## 1. Process Severity (S) Scale

Severity rates the seriousness of the effect of a potential failure mode on the customer, regulatory compliance, plant operations, and downstream assembly. Severity applies to the **effect** of failure only.

| Rating | Classification | Impact on Product / Customer (Downstream User) | Impact on Manufacturing / Plant Operations |
|:---:|:---|:---|:---|
| **10** | **Very High** | Failure may affect safe vehicle/product operation or noncompliance with government regulations with warning. | May endanger operator/assembler with warning. |
| **9** | **Very High** | Failure may affect safe vehicle/product operation or noncompliance with government regulations without warning. | May endanger operator/assembler without warning. |
| **8** | **High** | Loss of primary product function; inoperable; customer very dissatisfied. | $100\%$ of product must be scrapped; line shutdown $> 1$ hr. |
| **7** | **High** | Degradation of primary product function; customer dissatisfied. | Portion of product scrapped; major rework required ($> 30$ min). |
| **6** | **Moderate** | Loss of secondary product function; customer inconvenienced. | Minor line shutdown ($< 30$ min); $100\%$ sort/rework off-line. |
| **5** | **Moderate** | Degradation of secondary product function; noticeable defect to most customers. | Product must be sorted and a portion ($< 100\%$) reworked on-line. |
| **4** | **Low** | Appearance or fit/finish item nonconformance; noticed by $> 75\%$ of customers. | Minor rework on-line; slight speed reduction. |
| **3** | **Low** | Appearance or fit/finish item nonconformance; noticed by $50\%$ of customers. | Minor disruption; nonconforming product accepted under deviation. |
| **2** | **Very Low** | Appearance or fit/finish item nonconformance; noticed by $< 25\%$ of customers. | Slight inconvenience to operator; no scrap or rework. |
| **1** | **None** | No discernible effect. | No impact on plant operations. |

*Note on Special Characteristics (SC / CTQ):* Any failure mode with **Severity $\ge 9$** is automatically flagged as a Safety/Compliance Special Characteristic ($\nabla$). Modes with **Severity 7–8** combined with high occurrence are evaluated as Significant/Functional Characteristics.

---

## 2. Process Occurrence (O) Scale

Occurrence rates the likelihood that a specific failure cause will occur and lead to the failure mode. In NEXUS-FMEA, Occurrence is directly derived from **measured per-operation defect rates**.

| Rating | Likelihood / Prevention Capability | Quantitative Rate Criteria | Parts Per Thousand (PPM) | Approximate Benchmark Ratio |
|:---:|:---|:---|:---|:---|
| **10** | **Extremely High / Inevitable** | $\text{Rate} \ge 10.0\%$ | $\ge 100 \text{ / } 1k$ ($\ge 100,000 \text{ PPM}$) | $\ge 1 \text{ in } 10$ |
| **9** | **Very High** | $5.0\% \le \text{Rate} < 10.0\%$ | $50 \text{ / } 1k$ ($50,000 \text{ PPM}$) | $1 \text{ in } 20$ |
| **8** | **High** | $2.0\% \le \text{Rate} < 5.0\%$ | $20 \text{ / } 1k$ ($20,000 \text{ PPM}$) | $1 \text{ in } 50$ |
| **7** | **High (Occasional)** | $1.0\% \le \text{Rate} < 2.0\%$ | $10 \text{ / } 1k$ ($10,000 \text{ PPM}$) | $1 \text{ in } 100$ |
| **6** | **Moderate** | $0.2\% \le \text{Rate} < 1.0\%$ | $2 \text{ / } 1k$ ($2,000 \text{ PPM}$) | $1 \text{ in } 500$ |
| **5** | **Moderate (Low)** | $0.05\% \le \text{Rate} < 0.2\%$ | $0.5 \text{ / } 1k$ ($500 \text{ PPM}$) | $1 \text{ in } 2,000$ |
| **4** | **Low** | $0.01\% \le \text{Rate} < 0.05\%$ | $0.1 \text{ / } 1k$ ($100 \text{ PPM}$) | $1 \text{ in } 10,000$ |
| **3** | **Low (Rare)** | $0.001\% \le \text{Rate} < 0.01\%$ | $0.01 \text{ / } 1k$ ($10 \text{ PPM}$) | $1 \text{ in } 100,000$ |
| **2** | **Very Low** | $0.0001\% \le \text{Rate} < 0.001\%$ | $0.001 \text{ / } 1k$ ($1 \text{ PPM}$) | $1 \text{ in } 1,000,000$ |
| **1** | **Remote / Eliminated** | $\text{Rate} < 0.0001\%$ ($0\text{ observed}$) | $< 0.001 \text{ / } 1k$ ($< 1 \text{ PPM}$) | $< 1 \text{ in } 1,000,000$ |

*Low-Confidence Rule:* For operations with small sample size ($n < 30$ parts), Occurrence is conservatively clipped to $\ge 5$ to prevent unwarranted low-risk claims on thin data.

---

## 3. Process Detection (D) Scale

Detection rates the ability of the current process controls (inspection, testing, poka-yoke) to detect the failure cause or failure mode **before the part leaves the manufacturing station or facility**.

| Rating | Detection Capability | Process Control Criteria / Inspection Method |
|:---:|:---|:---|
| **10** | **Almost Impossible** | No current inspection or test method established; cannot detect failure mode. |
| **9** | **Very Remote** | Unproven inspection method; visual check on random sample by operator. |
| **8** | **Remote** | Visual inspection under standard lighting with variable operator diligence ($< 100\%$). |
| **7** | **Very Low** | Manual attribute gauging (Go/No-Go) on sample basis at end of shift. |
| **6** | **Low** | Manual variable measurement (micrometer/caliper) on sample basis with SPC charting. |
| **5** | **Moderate** | Automated variable measurement on sample basis with automated out-of-spec alarm. |
| **4** | **Moderately High** | $100\%$ manual inspection / gauging at station before release to next operation. |
| **3** | **High** | $100\%$ automated in-line inspection (e.g., optical vision, pressure decay) with automatic part reject. |
| **2** | **Very High** | In-station automated error-proofing / poka-yoke detecting defect cause and stopping machine immediately. |
| **1** | **Almost Certain** | Error-proofing by design geometry that makes failure cause physically impossible to occur. |

---

## 4. Action Priority (AP) Logic (AIAG & VDA 2019)

The AIAG-VDA harmonized standard replaces the legacy Risk Priority Number ($\text{RPN} = S \times O \times D$) with **Action Priority (AP)**. 

### Why AP Replaced RPN:
1. **Severity Dominance:** RPN $(10 \times 1 \times 1 = 10)$ scored lower than $(2 \times 3 \times 2 = 12)$, concealing critical safety risks.
2. **Non-linear Risk:** RPN produced mathematical voids (e.g., 85% of values between 1–1000 never appear).
3. **Structured Hierarchy:** AP prioritizes first on **Severity (S)**, then on **Occurrence (O)**, and finally on **Detection (D)**.

### Action Priority Levels:
- **High (H) — Mandatory Action:** Highest priority for review and corrective action. The team must identify improved prevention and/or detection controls or justify why current controls are acceptable.
- **Medium (M) — Recommended Action:** Action is recommended to improve prevention/detection controls.
- **Low (L) — Discretionary Action:** Current controls are adequate; action is optional unless continuous improvement resources permit.

### Action Priority Lookup Logic:

```
IF Severity in [9, 10]:
    IF Occurrence in [8, 9, 10]: -> HIGH (H)
    IF Occurrence in [6, 7]:
        IF Detection in [5, 6, 7, 8, 9, 10]: -> HIGH (H)
        ELSE: -> MEDIUM (M)
    IF Occurrence in [4, 5]:
        IF Detection in [7, 8, 9, 10]: -> HIGH (H)
        IF Detection in [5, 6]: -> MEDIUM (M)
        ELSE: -> LOW (L)
    IF Occurrence in [2, 3]:
        IF Detection in [8, 9, 10]: -> MEDIUM (M)
        ELSE: -> LOW (L)
    IF Occurrence == 1: -> LOW (L)

IF Severity in [7, 8]:
    IF Occurrence in [8, 9, 10]: -> HIGH (H)
    IF Occurrence in [6, 7]:
        IF Detection in [7, 8, 9, 10]: -> HIGH (H)
        IF Detection in [5, 6]: -> MEDIUM (M)
        ELSE: -> LOW (L)
    IF Occurrence in [4, 5]:
        IF Detection in [8, 9, 10]: -> MEDIUM (M)
        ELSE: -> LOW (L)
    IF Occurrence in [2, 3]: -> LOW (L)
    IF Occurrence == 1: -> LOW (L)

IF Severity in [4, 5, 6]:
    IF Occurrence in [8, 9, 10]:
        IF Detection in [7, 8, 9, 10]: -> HIGH (H)
        IF Detection in [5, 6]: -> MEDIUM (M)
        ELSE: -> LOW (L)
    IF Occurrence in [6, 7]:
        IF Detection in [7, 8, 9, 10]: -> MEDIUM (M)
        ELSE: -> LOW (L)
    ELSE: -> LOW (L)

IF Severity in [1, 2, 3]: -> LOW (L) (all O and D combinations)
```
