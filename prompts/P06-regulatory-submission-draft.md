# P06 · Regulatory submission draft

**Section:** 02 — Regulatory and compliance chain
**Workflow step:** Step 3 of 3
**Current version:** v1.2
**Status:** Tested
**Last updated:** March 2026

\---

## Prompt Text (v1.2 — current)

```
You are the compliance officer preparing a formal regulatory submission on behalf of 
Sunrise Gardens Aged Care, Bundoora Victoria (RACS ID: \[RACS\_ID]).

Using ONLY the information provided below, draft a formal written submission to the ACQSC.

SUBMISSION DETAILS:
- Submission type: \[TYPE — e.g. SIRS Priority 1 report, response to non-compliance notice]
- Reference number: \[REF NUMBER or "New submission"]
- Date of incident or notice: \[DATE]
- Submitted by: \[NAME AND ROLE]

FACTUAL SUMMARY:
\[PASTE FACTUAL ACCOUNT — from P02 incident report or P04/P05 findings. Facts only.]

ACTIONS ALREADY TAKEN:
\[LIST COMPLETED ACTIONS WITH DATES]

IMPROVEMENT ACTIONS PLANNED:
\[LIST PLANNED ACTIONS WITH TARGET DATES AND RESPONSIBLE ROLES — from P05 action plan]

Before drafting: Review the factual summary for any language that could constitute an 
admission of legal liability or assign individual blame. Remove and replace with 
neutral factual alternatives.

Then draft the submission with:
1. Cover statement (facility name, RACS ID, submission type, reference, date — 2 sentences)
2. Factual account (chronological, non-judgmental — max 150 words)
3. Immediate actions taken (with dates)
4. Root cause analysis (based ONLY on evidence provided — systemic factors, not individual fault)
5. Improvement plan (actions referencing specific Quality Standards, responsible roles, target dates)
6. Commitment statement (2–3 sentences)

Rules:
- Maximum 500 words (excluding cover statement)
- Formal government submission language
- Do NOT include admissions of legal liability
- Do NOT assign fault to named individuals
- Do NOT assume beyond information provided
- All improvement actions must reference a specific Quality Standard
- Flag overdue actions with \[OVERDUE — IMMEDIATE ACTION REQUIRED]
```

**Placeholders to fill:**

|Placeholder|Source|Example|
|-|-|-|
|`\[RACS\_ID]`|Organisation records|123456|
|`\[TYPE]`|ACQSC notification or SIRS flag|SIRS Priority 1 — Unexplained serious injury|
|`\[REF NUMBER]`|ACQSC correspondence|ACQSC-2026-VIC-04821|
|`\[DATE]`|Incident or notice date|28 March 2026|
|`\[SUBMITTED BY]`|Facility Manager|James Okafor, Facility Manager|
|`\[FACTUAL SUMMARY]`|P02 incident report|Resident sustained unexplained skin tear, discovered during morning care 28 March 2026|
|`\[ACTIONS TAKEN]`|Incident records|Wound assessed by RN 8:45am; GP notified 9:00am; family notified 9:15am|
|`\[IMPROVEMENT ACTIONS]`|P05 action plan|Monthly skin integrity audits from April 2026 (Standard 3); wound care training all PCWs by 15 April 2026 (Standard 7)|

\---

## Intended Workflow or Task

* **Trigger:** \[MANDATORY REPORT] from P02, or \[NON-COMPLIANT ALERT] / \[SIRS OVERDUE] from P04
* **Actor:** Facility Manager prepares inputs from P02 and P05 → AI drafts submission → Facility Manager and legal counsel review → Submitted via My Aged Care portal
* **Timing:** SIRS Priority 1: within 24 hours. Priority 2: within 30 days. Non-compliance response: within ACQSC-specified timeframe.

```
\[MANDATORY REPORT] from P02 or \[SIRS OVERDUE] from P04
                    ↓
Facility Manager prepares factual summary + action plan
                    ↓
\[P06 RUNS — pre-review for liability language
            Regulatory submission drafted]
                    ↓
Facility Manager + legal counsel review
                    ↓
Submitted via My Aged Care portal
                    ↓
Reference number filed in RiskMan
```

**Prompting technique:** RACE + pre-generation legal review + systemic root cause reframing + Quality Standards anchoring. Pre-generation review ("Before drafting: screen for liability language") catches the most common legal risk before output is generated — not after (Anthropic, 2025). Systemic root cause framing aligns with ACQSC incident investigation methodology (ACSQHC, 2020).

\---

## Problem Being Solved

The Facility Manager spends **3–5 hours per submission** — at \~15 submissions/year this costs **\~$3,480/year** at $58/hr, plus external legal review where required. Most aged care facilities lack in-house legal counsel when SIRS Priority 1 deadlines require submission within 24 hours (ACQSC, 2021).

**Pain points addressed:**

* Reduces drafting by \~65% (4 hrs → 84 min, saving \~$2,610/year)
* Pre-generation liability screen protects Facility Manager's legal position
* All improvement actions reference specific Quality Standards — demonstrating regulatory literacy to ACQSC

\---

## Automation Potential

**Level: Medium**

|Dimension|Assessment|
|-|-|
|Repetitiveness|Medium — \~15 submissions/year (SIRS reports + non-compliance responses)|
|Data availability|High — inputs drawn directly from P02 and P05 outputs|
|Human judgment needed|Very High — Facility Manager personally accountable for all submissions|
|Integration possibility|Low — My Aged Care portal requires human authentication|
|Estimated time saving|\~65% — 4 hrs → 84 min (\~$2,610/year)|

**Human-in-the-loop role:** Facility Manager compiles inputs from P02 and P05. Reviews AI draft for legal language accuracy and factual completeness. Legal counsel review for Priority 1 or formal non-compliance responses. Personally submits via My Aged Care portal as named responsible officer under Sunrise Gardens' RACS ID.

\---

## Risks and Limitations

|Risk|Level|Mitigation|
|-|-|-|
|**Hallucination** — AI invents facts in root cause analysis not in provided evidence; Facility Manager submits factually inaccurate document to ACQSC|High|Rule: root cause "based ONLY on evidence provided." Facility Manager verifies all root cause claims against P02 witness account before submission.|
|**Bias** — AI root cause analysis reflects unsupported assumptions about staff roles or resident demographics|High|Systemic framing rule. Facility Manager reviews root cause for unsupported assumptions about individuals or groups.|
|**Privacy** — Sensitive incident and compliance details including resident injury data submitted to AI|High|Organisation-hosted private AI only. Data minimisation — only PII necessary for submission included.|
|**Governance** — Liability admission not caught in pre-review; Facility Manager signs submission that compromises organisation's legal position|High|Pre-generation legal review built into prompt. Legal counsel review required for all Priority 1 SIRS reports and formal non-compliance responses.|
|**Over-reliance** — Facility Manager relies entirely on AI draft without reading for factual accuracy|Medium|Policy: Facility Manager reads entire submission against original P02 and P05 documents before signing.|

**Overall risk rating: HIGH** — all submissions must be reviewed by Facility Manager and legal counsel before submitting to ACQSC.

\---

## Version History

### v1.0 — Initial draft

**Date:** 24 March 2026
**Prompt:** `Write a response to an aged care compliance notice.`
**Output:** Vague letter with multiple liability admission phrases. No SIRS structure. Legally dangerous — external legal review flagged four liability admission phrases.
**Lesson learned:** Regulatory submissions without legal guardrails produce language that exposes the organisation to risk. A pre-generation review step is needed, not just a post-generation instruction.

\---

### v1.1 — Added RACE, legal guardrails, submission structure, input fields

**Date:** 27 March 2026
**Change:** Added compliance officer role, no-liability rule, ACQSC structure, factual summary and actions input fields.
**Output:** Appropriate caution and correct structure. However root cause invented causes not in evidence ("possible understaffing" with no staffing data). No Quality Standards references in improvement actions. No \[OVERDUE] flag.
**Lesson learned:** Root cause must be grounded in evidence — without explicit constraint, AI generates plausible but unsupported causal theories. Pre-generation review step needed.

\---

### v1.2 — Added pre-generation legal review, evidence-grounded root cause, Standards anchoring Current

**Date:** 31 March 2026
**Change:** Added pre-generation liability review instruction. Added "based ONLY on evidence" root cause constraint. Added Quality Standards reference requirement. Added \[OVERDUE] flag.
**Output:** Legally appropriate, evidence-grounded, ACQSC-aligned draft. External legal counsel rated output "90% submission-ready." Zero liability admissions identified.
**Lesson learned:** Pre-generation legal review is the most important legal safety feature. Structuring the prompt to screen before drafting produces consistently protective language from the first sentence.

\---

## A/B Test Results

**Test date:** 31 March 2026 | **Sample:** 2 real submission scenarios | **Evaluators:** Facility Manager and external legal counsel

|Criteria|v1.0|v1.2|
|-|-|-|
|Zero liability admissions|0%|100%|
|Root cause grounded in evidence|0%|100%|
|Quality Standards referenced in improvement actions|0%|100%|
|\[OVERDUE] flag accurate|N/A|100%|
|Legal counsel rated submission-ready|0%|50%|

\---

## References

* ACQSC. (2021). *How to submit a SIRS report*. Australian Government.
* ACQSC. (2023). *Self-assessment guide for residential aged care*. Australian Government.
* ACSQHC. (2020). *Incident management and open disclosure*. Australian Government.
* Anthropic. (2025). *Prompt engineering overview*. docs.claude.ai
* Australian Government. (2024). *Aged Care Act 2024*.

\---

## Related Prompts

* **Triggered by:** P02 (\[MANDATORY REPORT]); P04 (\[SIRS OVERDUE])
* **Uses input from:** P05 — Quality standards self-assessment (improvement plan)
* **Governed by:** P10 — AI risk governance policy
* **Library index:** [README.md](../README.md)

