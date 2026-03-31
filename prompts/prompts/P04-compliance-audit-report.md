# P04 · Compliance audit report generator

**Section:** 02 — Regulatory and compliance chain
**Workflow step:** Step 1 of 3
**Current version:** v1.2
**Status:** Tested
**Last updated:** March 2026

\---

## Prompt Text (v1.2 — current)

```
You are the compliance officer at Sunrise Gardens Aged Care, Bundoora Victoria.

Using ONLY the data provided below, generate a monthly compliance audit report aligned with 
the Aged Care Quality Standards 2019 (Standards 1–8) and the SIRS.

REPORTING PERIOD:
- Month: \[MONTH AND YEAR]
- Total residents: \[NUMBER]
- Total staff (FTE): \[NUMBER]

INCIDENT DATA FOR THIS PERIOD:
\[PASTE AGGREGATED INCIDENT SUMMARY — type, count, resolution status, SIRS status.
Source: monthly P02 outputs.]

CARE QUALITY DATA:
\[PASTE KEY METRICS — care plans reviewed, medication errors, falls, pressure injuries,
complaints received, staff training completion rate]

OUTSTANDING ACTIONS FROM PREVIOUS AUDIT:
\[LIST UNRESOLVED ITEMS FROM LAST MONTH]

Before writing: Assess each Standard 1–8 against the data. For each, ask:
"Does the data indicate compliance, partial compliance, or non-compliance?"

Then write the report with:
1. Executive summary (overall compliance status — 3–4 sentences)
2. Incident summary (by type, count, resolution rate, SIRS filings)
3. Quality Standards performance (rate each Standard 1–8 as
   Compliant / Partially Compliant / Non-Compliant with one evidence-based sentence each)
4. Outstanding actions (unresolved items with updated status)
5. Priority risks and recommended actions (top 3 risks with specific assigned actions)
6. Next audit focus areas

Rules:
- Maximum 500 words
- Formal regulatory language throughout
- Use ONLY the data provided — do NOT benchmark against external facilities
- Do NOT rate a standard Compliant if data shows a gap
- Flag Non-Compliant standards with \[NON-COMPLIANT ALERT]
- Flag overdue SIRS submissions with \[SIRS OVERDUE]
```

**Placeholders to fill:**

|Placeholder|Source|Example|
|-|-|-|
|`\[MONTH AND YEAR]`|Reporting calendar|March 2026|
|`\[TOTAL RESIDENTS]`|Resident register|62|
|`\[TOTAL STAFF FTE]`|HR system|48.5 FTE|
|`\[INCIDENT DATA]`|Aggregated P02 outputs|Falls: 8 (6 resolved, 2 under review); SIRS P1: 1 filed; P2: 3 filed, 1 overdue|
|`\[CARE QUALITY DATA]`|Clinical and HR systems|Care plans reviewed: 14/62; Medication errors: 2; Complaints: 3; Training: 78%|
|`\[OUTSTANDING ACTIONS]`|Previous audit|Medication management policy review — due Feb 2026, not completed \[OVERDUE]|

\---

## Intended Workflow or Task

* **Trigger:** First business day of each month — Facility Manager compiles data from all systems
* **Actor:** Facility Manager inputs data → AI generates report → Facility Manager and Clinical Governance Lead review → Filed with governing board by 5th of month
* **Next step:** \[NON-COMPLIANT ALERT] triggers P05. \[SIRS OVERDUE] triggers P06.

```
Monthly data compiled
        ↓
\[P04 RUNS — pre-assessment Standards 1–8
            Compliance audit report generated]
        ↓
Facility Manager + Clinical Governance Lead review
        ↓
Filed with governing board by 5th of month
        ↓
\[NON-COMPLIANT ALERT]? → P05
\[SIRS OVERDUE]? → P06
```

**Prompting technique:** RACE + self-critique pre-assessment + constrained output categories + no-benchmarking constraint. The pre-assessment step forces reasoning before rating, reducing positive bias in compliance outputs (Anthropic, 2025). Three-option rating constraint prevents vague assessments.

\---

## Problem Being Solved

Manual monthly report preparation takes **4–6 hours/month** (\~$3,480/year at Facility Manager rate $58/hr). Inconsistent or superficial monthly reporting is a direct predictor of poor ACQSC inspection outcomes (ACQSC, 2024).

**Pain points addressed:**

* Reduces preparation by \~60% (5 hrs → 2 hrs, saving \~$2,088/year)
* All 8 Standards assessed every month without exception
* \[NON-COMPLIANT ALERT] and \[SIRS OVERDUE] automatically trigger downstream prompts

\---

## Automation Potential

**Level: Medium-High**

|Dimension|Assessment|
|-|-|
|Repetitiveness|High — monthly cycle, 12 reports/year|
|Data availability|Medium — manual aggregation from multiple systems required|
|Human judgment needed|High — Facility Manager validates all 8 Standard ratings|
|Integration possibility|Medium — future integration with RiskMan and Leecare|
|Estimated time saving|\~60% — 5 hrs → 2 hrs (\~$2,088/year)|

**Human-in-the-loop role:** Facility Manager inputs data, reviews all 8 Standard ratings, challenges overstated assessments, approves recommended actions, and signs off before board presentation. Named compliance officer for Sunrise Gardens.

\---

## Risks and Limitations

|Risk|Level|Mitigation|
|-|-|-|
|**Hallucination** — AI rates a Standard Compliant when data shows a gap; Facility Manager presents inaccurate compliance picture to ACQSC|High|Pre-assessment step + "Do NOT rate Compliant if data shows a gap" rule. Facility Manager cross-references ratings against ACQSC compliance guidance.|
|**Bias** — AI systematically under-rates cultural safety standards (Standard 1) when minority resident data absent from input|High|Facility Manager specifically reviews Standards 1, 2, and 4. Future: add cultural diversity metrics as mandatory input.|
|**Privacy** — Aggregated resident health and staff data submitted to AI|High|Organisation-hosted private AI only. Data de-identified at resident level before prompting.|
|**Governance** — Recommended actions exceed Facility Manager's authority without board approval|Medium|Rule: recommendations are proposals, not directives. Facility Manager reviews all actions for budget feasibility.|
|**Over-reliance** — Incomplete input data; AI generates report with critical gaps unnoticed|Medium|Month-end data completeness checklist required before prompting.|

**Overall risk rating: HIGH** — all outputs must be reviewed and approved by Facility Manager before filing with governing board.

\---

## Version History

### v1.0 — Initial draft

**Date:** 24 March 2026
**Prompt:** `Write a monthly compliance report for an aged care facility.`
**Output:** Generic narrative, all 8 Standards rated Compliant, fictional data. Unusable.
**Lesson learned:** Without input data and constrained categories, AI defaults to Compliant — the most dangerous failure mode for a compliance tool.

\---

### v1.1 — Added Standards 1–8 structure, data fields, constrained ratings

**Date:** 27 March 2026
**Change:** Added compliance officer role, Standards 1–8 ratings, three-option constraint, data input fields.
**Output:** Better structure. However AI rated borderline Standards Compliant (positive bias) and invented industry benchmarks.
**Lesson learned:** Constrained categories alone insufficient. Pre-assessment self-critique step and no-benchmarking constraint both needed.

\---

### v1.2 — Added self-critique pre-assessment, no-Compliant-if-gap rule, no-benchmarking ✅ Current

**Date:** 31 March 2026
**Change:** Added pre-assessment instruction, "Do NOT rate Compliant if data shows a gap," no-external-benchmarking constraint.
**Output:** Evidence-grounded assessments. Correctly flagged 1 Non-Compliant Standard and 1 SIRS overdue in test with real March 2026 data.
**Lesson learned:** Self-critique pre-assessment is the most impactful technique upgrade — reasoning before rating eliminates positive bias.

\---

## A/B Test Results

**Test date:** 31 March 2026 | **Sample:** 2 real monthly data sets | **Evaluators:** Facility Manager and Clinical Governance Lead

|Criteria|v1.0|v1.2|
|-|-|-|
|All 8 Standards assessed with evidence|0%|100%|
|No generous Compliant ratings without evidence|0%|100%|
|No invented external benchmarks|0%|100%|
|\[NON-COMPLIANT ALERT] accurate|N/A|100%|
|Board-ready after minor edits|0%|100%|

\---

## References

* ACQSC. (2019). *Aged Care Quality Standards*. Australian Government.
* ACQSC. (2024). *Regulatory bulletin: Governance and accountability*. Australian Government.
* Anthropic. (2025). *Prompt engineering overview*. docs.claude.ai
* Fair Work Commission. (2025). *SCHADS Award pay guide*.

\---

## Related Prompts

* **Receives data from:** P02 (monthly incident data); P03 (shift records)
* **Triggers:** P05 — Quality standards self-assessment (\[NON-COMPLIANT ALERT])
* **Triggers:** P06 — Regulatory submission (\[SIRS OVERDUE])
* **Governed by:** P10 — AI risk governance policy
* **Library index:** [README.md](../README.md)

