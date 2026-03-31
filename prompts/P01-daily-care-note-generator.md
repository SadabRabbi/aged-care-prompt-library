# P01 · Daily care note generator

**Section:** 01 — Care documentation chain
**Workflow step:** Step 1 of 3
**Current version:** v1.2
**Status:** Tested
**Last updated:** March 2026

\---

## Prompt Text (v1.2 — current)

```
You are a qualified aged care worker at Sunrise Gardens Aged Care, Bundoora Victoria.

Write a professional daily care note for the following resident:
- Resident name: \[RESIDENT\_NAME]
- Age: \[AGE]
- Primary conditions: \[CONDITIONS]
- Shift: \[SHIFT — Morning / Afternoon / Night]
- Date: \[DATE]

Observations from this shift:
\[PASTE CARER'S RAW SHIFT NOTES — dot points are fine]

Using ONLY the observations provided above, write a structured care note with:
1. Meals and fluid intake
2. Mobility and physical activity
3. Mood and emotional wellbeing
4. Personal care completed
5. Health observations and concerns

Rules:
- Maximum 250 words
- Clinical but compassionate language
- Do NOT assume or infer beyond what is provided
- Do NOT include diagnoses or medical recommendations
- Flag any urgent concern with \[URGENT] at the start of that section
```

**Placeholders to fill:**

|Placeholder|Source|Example|
|-|-|-|
|`\[RESIDENT\_NAME]`|Resident management system|Margaret Thompson|
|`\[AGE]`|Resident profile|84|
|`\[CONDITIONS]`|Resident care plan|Moderate dementia, Type 2 diabetes|
|`\[SHIFT]`|Roster system|Morning|
|`\[DATE]`|System date|1 April 2026|
|`\[RAW SHIFT NOTES]`|Carer dot-point observations|Ate 80% breakfast, refused lunch, confused at 10am|

\---

## Intended Workflow or Task

* **Trigger:** End of shift — carer completes rounds and records raw observations
* **Actor:** Carer inputs notes → AI generates formatted care note → RN reviews and approves before saving
* **Timing:** Within 30 minutes of shift end
* **Next step:** Saved to resident management system; \[URGENT] flag triggers P02

```
Carer enters raw observations
         ↓
\[P01 RUNS — care note generated]
         ↓
RN reviews and approves
         ↓
Saved to resident management system
         ↓
\[URGENT]? → P02    No flag? → P03
```

**Prompting technique:** RACE framework + grounding constraint + structured output decomposition. Grounding constraint ("ONLY the observations provided") prevents hallucination in clinical records. Five-section decomposition standardises output structure across all 48 staff (Anthropic, 2025).

\---

## Problem Being Solved

Sunrise Gardens generates \~186 care notes daily across 62 residents and 3 shifts. At 5 min per note manually, this consumes **\~15.5 hours of carer time per day** (\~$155,775/year at SCHADS PCW Level 3 rate; Fair Work Commission, 2025). Inconsistent note quality creates direct compliance risk under Standard 3 — Care and Services (ACQSC, 2019).

**Pain points addressed:**

* Reduces documentation time by \~70% (5 min → 90 sec per note)
* Standardises five-section structure across all carers and shifts
* \[URGENT] flag creates consistent escalation path to P02

\---

## Automation Potential

**Level: High**

|Dimension|Assessment|
|-|-|
|Repetitiveness|Very high — 186 notes/day, 67,890/year|
|Data availability|High — carers already record dot-point observations|
|Human judgment needed|Medium — RN review required before saving|
|Integration possibility|High — Leecare, Civica, Alis via API|
|Estimated time saving|\~70% — 5 min → 90 sec per note (\~$109,400/year)|

**Human-in-the-loop role:** Carer inputs raw observations. AI formats note. RN reviews for clinical accuracy and approves before saving to clinical record. Facility Manager retains legal accountability for all records filed under Sunrise Gardens' RACS ID.

\---

## Risks and Limitations

|Risk|Level|Mitigation|
|-|-|-|
|**Hallucination** — AI fabricates clinical observations not in carer's notes; falsified clinical record breaches Aged Care Act 2024|High|Grounding constraint: "ONLY the observations provided." Mandatory RN review before saving. Monthly audit log review.|
|**Bias** — AI produces culturally inappropriate language for residents from non-English-speaking backgrounds; dignity violation under Standard 1 (ACQSC, 2019)|High|RN checks cultural appropriateness before saving. Future: add `\[CULTURAL\_NOTES]` placeholder.|
|**Privacy** — Resident PII and health data submitted to AI system; Facility Manager liable under Privacy Act 1988 (Cth)|High|Organisation-hosted private AI only (e.g. Azure OpenAI, Australian data residency). No public AI tools. Vendor data processing agreement required.|
|**Governance** — No audit trail of AI-generated vs manually written notes under Standard 8 (ACQSC, 2019)|Medium|AI-generated notes tagged with metadata flag in resident management system. Covered under P10 governance policy.|
|**Over-reliance** — Carer skips writing raw observations; AI generates note with no evidential basis|Medium|Policy: raw observations mandatory before running prompt. Quarterly spot-check audits by Facility Manager.|

**Overall risk rating: MEDIUM** — suitable with mandatory RN review, private AI infrastructure, and P10 governance policy in place.

\---

## Version History

### v1.0 — Initial draft

**Date:** 24 March 2026
**Prompt:** `Write a care note for a resident in an aged care facility.`
**Output:** Generic note with invented observations. No structure. 380 words. Completely unusable — AI fabricated activities never mentioned.
**Lesson learned:** Need role, resident context, structured sections, and grounding constraint.

\---

### v1.1 — Added RACE framework and structured sections

**Date:** 27 March 2026
**Change:** Added role assignment, resident placeholder fields, five-section structure, clinical tone instruction.
**Output:** Correct structure and tone. However AI still invented content — no actual observations provided as input. Output \~310 words, over limit.
**Lesson learned:** RACE improves structure but does not prevent hallucination. Observation input field and grounding constraint are non-negotiable for clinical documents.

\---

### v1.2 — Added grounding constraint, word limit, \[URGENT] flag ✅ Current

**Date:** 31 March 2026
**Change:** Added `\[RAW SHIFT NOTES]` input field, grounding constraint, 250-word limit, \[URGENT] flag, exclusion clauses.
**Output:** Fully grounded, no fabricated content in all 5 test runs. 4/5 rated "ready to save without edits" by RN reviewer.
**Lesson learned:** Grounding constraint is the most critical safety feature for AI in clinical documentation. \[URGENT] flag creates a reliable escalation bridge to P02.

\---

## A/B Test Results

**Test date:** 31 March 2026 | **Sample:** 5 real shift scenarios | **Evaluators:** 2 RNs (blind review)

|Criteria|v1.0|v1.2|
|-|-|-|
|Grounded in observations only|0%|100%|
|Correct five-section structure|20%|100%|
|Clinical tone appropriate|40%|100%|
|Within 250-word limit|0%|100%|
|Ready to save without edits|0%|80%|
|\[URGENT] triggered correctly|N/A|100%|

\---

## References

* ACQSC. (2019). *Aged Care Quality Standards*. Australian Government.
* Australian Government. (2024). *Aged Care Act 2024*.
* Fair Work Commission. (2025). *SCHADS Award pay guide*.

\---

## Related Prompts

* **Next (urgent):** P02 — Incident report drafter
* **Next (standard):** P03 — Shift handover summary
* **Governed by:** P10 — AI risk governance policy
* **Library index:** [README.md](../README.md)

