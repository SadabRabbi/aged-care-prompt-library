# P09 · Staff performance review summary

**Section:** 03 — Resident administration chain
**Workflow step:** Step 3 of 4
**Current version:** v1.2
**Status:** Tested
**Last updated:** March 2026

\---

## Prompt Text (v1.2 — current)

```
You are the HR manager at Sunrise Gardens Aged Care, Bundoora Victoria.

Using ONLY the information provided below, generate a structured performance review summary 
for the staff member identified. This document will be used in the formal review meeting and 
filed in the staff member's HR record.

STAFF DETAILS:
- Staff name: \[STAFF\_NAME]
- Role and classification: \[ROLE]
- Review period: \[DATE RANGE]
- Reviewer name and role: \[REVIEWER\_NAME AND ROLE]
- Employment type: \[EMPLOYMENT TYPE]
- Length of service: \[YEARS/MONTHS]

PERFORMANCE DATA FOR THIS PERIOD:
\[PASTE FACTUAL OBSERVATIONS ONLY — attendance record, incidents involving this staff member
(from P02 where relevant), training completed, specific compliments received, concerns raised,
documentation quality observations (from P01 quality reviews where relevant)]

PREVIOUS REVIEW OUTCOMES:
\[LIST GOALS FROM LAST REVIEW — achieved / partially achieved / not achieved]

Before writing: Screen the performance data for any language referencing protected attributes 
(age, gender, ethnicity, disability, religion, pregnancy, family responsibilities). 
Remove and replace with performance-based language.

Then write the review summary with:
1. Performance overview (balanced, evidence-based — 3–4 sentences)
2. Strengths demonstrated (3 specific, evidence-based with examples)
3. Areas for development (2–3 areas — growth-focused, non-punitive)
4. Progress on previous goals (achieved / partially achieved / not achieved — one sentence each)
5. Development goals for next period — exactly 3 goals, each with:
   - Goal (specific and measurable)
   - Why it matters for resident care
   - Aligned Quality Standard
   - Target completion date
   - How progress will be measured
6. Overall rating: \[Exceeds Expectations / Meets Expectations / Needs Improvement / Unsatisfactory]
   — one sentence evidence-based justification

Rules:
- Maximum 400 words
- Professional, constructive, non-discriminatory language
- All observations grounded ONLY in provided data
- Do NOT reference protected attributes in any section
- Do NOT assume reasons for absence or performance issues
- Flag conduct/performance requiring formal HR process with \[HR PROCESS REQUIRED]
```

**Placeholders to fill:**

|Placeholder|Source|Example|
|-|-|-|
|`\[STAFF\_NAME]`|HR system|Jane Nguyen|
|`\[ROLE]`|HR system|Personal Care Worker — Level 3, SCHADS Award|
|`\[DATE RANGE]`|Review calendar|1 October 2025 – 31 March 2026|
|`\[REVIEWER\_NAME AND ROLE]`|HR system|Sarah Chen, RN (Team Leader)|
|`\[EMPLOYMENT TYPE]`|HR system|Part-time (0.8 FTE)|
|`\[LENGTH OF SERVICE]`|HR system|2 years, 4 months|
|`\[PERFORMANCE DATA]`|Attendance, P02 logs, training records, P01 quality reviews|Attendance: 2 unplanned absences. Training: manual handling completed Jan 2026; dementia training NOT completed (due Dec 2025). P01 quality: 3 care notes flagged for insufficient detail. Compliment received from family. No P02 incidents.|
|`\[PREVIOUS GOALS]`|Last review|Goal 1: Dementia training by Dec 2025 — NOT ACHIEVED. Goal 2: Improve care note detail — PARTIALLY ACHIEVED.|

\---

## Intended Workflow or Task

* **Trigger:** Scheduled performance review (6-monthly for all 48 staff at Sunrise Gardens)
* **Actor:** Team Leader compiles performance data → AI generates review summary → Facility Manager reviews before meeting
* **Timing:** Draft completed 48 hours before scheduled review meeting

```
Review period ends — performance data compiled from
attendance, P02 incident logs, training records, P01 quality reviews
                    ↓
\[P09 RUNS — pre-screening for protected attributes
            Performance review summary generated]
                    ↓
Facility Manager reviews before meeting
                    ↓
Formal review meeting held with staff member
                    ↓
Goals agreed and signed; filed in HR record
                    ↓
\[HR PROCESS REQUIRED]? → Formal process initiated by Facility Manager
```

**Prompting technique:** RACE + pre-generation anti-bias screening + structured goal decomposition + evidence chain linkage. Pre-generation screening ("Before writing: screen for protected attributes") applies an anti-discrimination filter before any output is generated — the most common legal risk in performance documentation (Australian Human Rights Commission, 2023). Five-component goal structure enforces SMART criteria at the prompt level and links individual development to Quality Standards — directly strengthening Standard 7 evidence (ACQSC, 2019).

\---

## Problem Being Solved

With 48 staff reviewed 6-monthly, Sunrise Gardens generates **96 performance review summaries/year.** At 60 min each manually, this costs **\~$3,648/year** at Team Leader rate $38/hr (Fair Work Commission, 2025). Inconsistent quality creates HR liability risk and weak Standard 7 evidence for ACQSC inspections (Royal Commission into Aged Care, 2021).

**Pain points addressed:**

* Reduces preparation by \~55% (60 min → 27 min, saving \~$2,006/year)
* Pre-generation anti-bias screen protects against Fair Work Act 2009 discrimination claims
* 5-component goals aligned to Quality Standards strengthen Standard 7 evidence portfolio

\---

## Automation Potential

**Level: Medium**

|Dimension|Assessment|
|-|-|
|Repetitiveness|Medium — 96 reviews/year|
|Data availability|Medium — data compiled from attendance, P02 logs, training records, P01 quality reviews|
|Human judgment needed|High — Facility Manager reviews for fairness, legal compliance, and accuracy|
|Integration possibility|Medium — integrates with HR management systems; P01 and P02 are natural inputs|
|Estimated time saving|\~55% — 60 min → 27 min (\~$2,006/year)|

**Human-in-the-loop role:** Team Leader inputs performance data. AI screens for bias and generates review. Facility Manager reviews for anti-discrimination compliance, factual accuracy, and \[HR PROCESS REQUIRED] flags before formal meeting. Personally accountable for all HR decisions at Sunrise Gardens.

\---

## Risks and Limitations

|Risk|Level|Mitigation|
|-|-|-|
|**Bias** — AI references a protected attribute; Facility Manager approves discriminatory review; unfair dismissal claim under Fair Work Act 2009 (Cth)|High|Pre-generation anti-bias screening in prompt. HR representative or legal review before meeting.|
|**Hallucination** — AI generates specific performance example not in input data; Facility Manager uses fabricated evidence in formal HR process|High|Grounding constraint: "All observations grounded ONLY in provided data." Facility Manager verifies all examples against source data.|
|**Privacy** — Individual staff performance, attendance, and conduct data submitted to AI|High|Organisation-hosted private AI only. Staff data classified as sensitive personal information under Privacy Act 1988 (Cth).|
|**Governance** — \[HR PROCESS REQUIRED] flag omitted for serious conduct issue; formal process not initiated; legal liability for Facility Manager|High|Facility Manager cross-checks flag against all conduct issues before meeting. Ambiguous cases reviewed with legal counsel.|
|**Over-reliance** — Review used without human review; staff member receives AI-generated assessment without meaningful human judgment|Medium|Policy: Team Leader reviews output with Facility Manager before any formal meeting.|

**Overall risk rating: HIGH** — significant legal and HR implications under Fair Work Act 2009. All outputs must be reviewed by Facility Manager before use in any formal HR process.

\---

## Version History

### v1.0 — Initial draft

**Date:** 24 March 2026
**Prompt:** `Write a performance review for an aged care worker.`
**Output:** Generic entirely positive review with invented achievements. No goals. Legally and professionally unusable.
**Lesson learned:** Without performance data input and balanced language requirements, AI defaults to unreliable positive assessments.

\---

### v1.1 — Added RACE, data input, balanced language, previous goals, basic goal requirements

**Date:** 27 March 2026
**Change:** Added HR manager role, performance data input, balanced language, previous goals section, basic development goal requirement.
**Output:** Evidence-grounded and balanced. However development goals vague ("improve documentation") — no SMART criteria, no Quality Standard references. No anti-bias screening. No \[HR PROCESS REQUIRED] flag.
**Lesson learned:** SMART criteria must be enforced structurally. Requiring 5 components per goal is more effective than asking generically for "SMART goals." Pre-generation anti-bias screening is also essential.

\---

### v1.2 — Added pre-generation anti-bias screening, 5-component goal structure, \[HR PROCESS REQUIRED] flag Current

**Date:** 31 March 2026
**Change:** Added pre-generation anti-bias screening instruction. Added 5-component goal structure (exactly 3 goals). Added Quality Standard alignment. Added \[HR PROCESS REQUIRED] flag.
**Output:** Professional, evidence-based, legally appropriate. Anti-bias screen caught one "frequent absences" phrase that could have implied a protected characteristic — replaced with performance-neutral language. \[HR PROCESS REQUIRED] correctly triggered in test scenario.
**Lesson learned:** Pre-generation anti-bias screening is the most important legal safety feature. Making the anti-discrimination check an explicit first step ensures the Facility Manager receives an output that has already been processed for the most common legal risk in performance review documentation.

\---

## A/B Test Results

**Test date:** 31 March 2026 | **Sample:** 3 real review scenarios | **Evaluators:** Facility Manager and HR representative (blind review)

|Criteria|v1.0|v1.2|
|-|-|-|
|Evidence-grounded assessment|0%|100%|
|Anti-bias screening applied|0%|100%|
|5-component SMART goals|0%|100%|
|Quality Standard in all goals|0%|100%|
|\[HR PROCESS REQUIRED] accurate|N/A|100%|

\---

## References

* ACQSC. (2019). *Aged Care Quality Standards — Standard 7: Human resources*. Australian Government.
* Anthropic. (2025). *Prompt engineering overview*. docs.claude.ai
* Australian Human Rights Commission. (2023). *Employer guide to anti-discrimination law*. Australian Government.
* Fair Work Act 2009 (Cth). *Unfair dismissal and general protections.*
* Fair Work Commission. (2025). *SCHADS Award pay guide*.
* Royal Commission into Aged Care Quality and Safety. (2021). *Final report* (Vol. 1).

\---

## Related Prompts

* **Uses data from:** P01 — Daily care note (documentation quality); P02 — Incident report (staff incidents)
* **Next in chain:** P10 — AI risk governance policy
* **Governed by:** P10 — AI risk governance policy
* **Library index:** [README.md](../README.md)

