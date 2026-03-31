# P07 · Resident intake summary

**Section:** 03 — Resident administration chain
**Workflow step:** Step 1 of 4
**Current version:** v1.2
**Status:** Tested
**Last updated:** March 2026

\---

## Prompt Text (v1.2 — current)

```
You are the care coordinator at Sunrise Gardens Aged Care, Bundoora Victoria.

Using ONLY the information provided below, generate a structured resident intake summary 
for the clinical care team. Reflect the rights-based, person-centred approach required by 
the Aged Care Act 2024 and Standard 1 (Consumer Dignity and Choice).

RESIDENT DETAILS:
- Full name: \[RESIDENT\_NAME]
- Preferred name: \[PREFERRED\_NAME]
- Date of birth: \[DOB]
- Admission date: \[ADMISSION\_DATE]
- Room: \[ROOM\_NUMBER]
- Next of kin: \[NOK\_NAME] — \[RELATIONSHIP] — \[CONTACT\_NUMBER]
- GP: \[GP\_NAME AND PRACTICE]
- Primary diagnoses: \[DIAGNOSES]
- Medications: \[MEDICATION\_LIST or "See attached medication chart"]
- Mobility: \[MOBILITY]
- Cognitive status: \[COGNITIVE]
- Communication needs: \[COMMUNICATION]
- Dietary requirements: \[DIETARY]
- Known allergies: \[ALLERGIES or "None documented"]
- Personal preferences: \[PREFERENCES]
- Cultural or religious considerations: \[CULTURAL\_NOTES or "None documented"]

Write the intake summary with:
1. Resident profile (preferred name, age, admission date, room — 2 sentences)
2. Clinical overview (diagnoses, medication summary, allergies — 3–4 sentences)
3. Care requirements (mobility, cognitive status, communication, dietary — actionable)
4. Personal and cultural preferences (person-first language, written with dignity)
5. Key contacts (NOK and GP with contact details)
6. First 48 hours watch points (based on diagnoses and mobility — exactly 3 actionable points)

Rules:
- Maximum 400 words
- Person-first language throughout (e.g. "resident living with dementia" not "demented resident")
- Use preferred name throughout — never revert to full formal name after profile section
- Do NOT include financial, legal, or contractual information
- Do NOT make clinical assumptions beyond information provided
- Flag high-risk conditions with \[CLINICAL ALERT — notify RN immediately]
```

**Placeholders to fill:**

|Placeholder|Source|Example|
|-|-|-|
|`\[RESIDENT\_NAME]`|Pre-admission form|Margaret Thompson|
|`\[PREFERRED\_NAME]`|Family interview|Maggie|
|`\[DOB]`|Pre-admission form|14 June 1941|
|`\[ADMISSION\_DATE]`|Admissions system|1 April 2026|
|`\[ROOM\_NUMBER]`|Room allocation|Room 14, Wing B|
|`\[NOK\_NAME]`|Pre-admission form|Susan Thompson (Daughter) — 0412 345 678|
|`\[GP]`|Pre-admission form|Dr. Anh Nguyen, Bundoora Medical Centre|
|`\[DIAGNOSES]`|GP referral|Moderate vascular dementia, Type 2 diabetes, osteoporosis|
|`\[MOBILITY]`|OT assessment|Requires 1-person assist; fall risk HIGH|
|`\[DIETARY]`|Dietitian assessment|Modified texture Level 6; diabetic diet; no nuts (allergy)|
|`\[PREFERENCES]`|Family interview|Enjoys morning tea, classical music, garden visits. Prefers female carers for personal care.|

\---

## Intended Workflow or Task

* **Trigger:** Admission confirmed — typically 24–48 hours before resident arrives
* **Actor:** Care Coordinator inputs pre-admission data → AI generates summary → RN reviews \[CLINICAL ALERT] flags → Distributed to care team before admission
* **Timing:** Completed and distributed at least 24 hours before admission
* **Next step:** Triggers P08 (family welcome letter) on admission

```
Pre-admission assessment completed
          ↓
Care Coordinator inputs all resident details
          ↓
\[P07 RUNS — resident intake summary generated]
          ↓
RN reviews \[CLINICAL ALERT] flags against source documents
          ↓
Summary distributed to full care team
          ↓
Resident admitted — team briefed → P08 triggered
```

**Prompting technique:** RACE + person-first language constraint + preferred name persistence + fixed-count watch points. Person-first language constraint prevents stigmatising clinical text that violates Standard 1 (Australian Government, 2024). Requiring exactly 3 watch points (not "up to 3") forces prioritisation of the most clinically significant risks — a deliberate constraint that converts a general overview into an actionable brief (Anthropic, 2025).

\---

## Problem Being Solved

Without a consolidated intake summary, care staff receive fragmented information from multiple sources — GP referrals, hospital discharge summaries, family interviews — at different times and in different formats. The first 48 hours are the highest-risk admission period (Royal Commission into Aged Care, 2021). Manual intake summary preparation takes **\~60 min/admission** at Sunrise Gardens (\~24–36 admissions/year = **\~$882–$1,260/year** at Care Coordinator rate $35/hr; Fair Work Commission, 2025).

**Pain points addressed:**

* Reduces preparation by \~60% (60 min → 24 min, saving \~$529–$756/year)
* \[CLINICAL ALERT] flag proactively identifies high-risk conditions for RN
* Person-first language reinforces rights-based approach under Aged Care Act 2024

\---

## Automation Potential

**Level: High**

|Dimension|Assessment|
|-|-|
|Repetitiveness|Medium-High — 24–36 admissions/year at Sunrise Gardens|
|Data availability|High — pre-admission forms contain all required fields|
|Human judgment needed|Medium — RN reviews \[CLINICAL ALERT] flags before distribution|
|Integration possibility|High — integrates with admissions management systems|
|Estimated time saving|\~60% — 60 min → 24 min per intake|

**Human-in-the-loop role:** Care Coordinator inputs and certifies all pre-admission data. RN reviews \[CLINICAL ALERT] flags against source documents. Facility Manager approves distribution. Accountable for all care arrangements made based on the intake summary.

\---

## Risks and Limitations

|Risk|Level|Mitigation|
|-|-|-|
|**Hallucination** — AI misinterprets or incorrectly summarises medication list; medication error risk on first shift|High|Rule: complex medication lists reference "See attached medication chart" only. Pharmacist review mandatory on admission day.|
|**Bias** — AI generates culturally generic descriptions of cultural or religious needs; dignity violation under Standard 1|High|Person-first language and cultural notes constraints in prompt. Care Coordinator reviews personal preferences section against family interview notes.|
|**Privacy** — Highest concentration of resident PII in any prompt in this library submitted to AI|High|Organisation-hosted private AI only. Pre-admission data classified as sensitive health information under Privacy Act 1988 (Cth).|
|**Governance** — \[CLINICAL ALERT] omitted for high-risk condition; care team unprepared; adverse event in first 48 hours|High|RN reviews \[CLINICAL ALERT] flags against all diagnoses and mobility data before distribution.|
|**Over-reliance** — Incomplete pre-admission data produces summary with critical gaps|Medium|Intake data completeness checklist required. Care Coordinator certifies all fields populated before prompting.|

**Overall risk rating: HIGH** — highest PII concentration in this library. All outputs must be reviewed by RN before distribution to care team.

\---

## Version History

### v1.0 — Initial draft

**Date:** 24 March 2026
**Prompt:** `Write an intake summary for a new aged care resident.`
**Output:** Generic template with invented diagnoses. Used "demented patient" terminology. No person-centred framing. Unusable.
**Lesson learned:** Without person-first language constraints and input fields, AI defaults to stigmatising clinical language that violates the Aged Care Act 2024 rights-based approach.

\---

### v1.1 — Added RACE, all placeholder fields, grounding constraint, person-centred language

**Date:** 27 March 2026
**Change:** Added care coordinator role, all placeholder fields, grounding constraint, person-centred language instruction.
**Output:** Well-structured and appropriate tone. However no first 48 hours watch points. No \[CLINICAL ALERT] flag. Preferred name used inconsistently — reverted to full formal name in sections 3 and 4.
**Lesson learned:** Person-centred language alone is insufficient. First 48 hours section and \[CLINICAL ALERT] flag are clinically essential. Preferred name persistence must be explicitly instructed.

\---

### v1.2 — Added fixed-count 48-hour watch points, \[CLINICAL ALERT] flag, preferred name persistence Current

**Date:** 31 March 2026
**Change:** Added "exactly 3 specific, actionable" first 48-hour watch points. Added \[CLINICAL ALERT] flag. Added preferred name persistence rule.
**Output:** Complete, person-centred, immediately actionable intake summary. \[CLINICAL ALERT] correctly flagged fall risk and diabetic dietary requirements. Preferred name used consistently throughout all 3 test runs.
**Lesson learned:** Fixed-count constraint forces clinical prioritisation — exactly 3 watch points is more effective than "list watch points" because constraint improves precision (Anthropic, 2025).

\---

## A/B Test Results

**Test date:** 31 March 2026 | **Sample:** 3 real admission scenarios | **Evaluators:** Care Coordinator and RN (blind review)

|Criteria|v1.0|v1.2|
|-|-|-|
|Person-first language throughout|0%|100%|
|Preferred name consistent|0%|100%|
|\[CLINICAL ALERT] accurate|N/A|100%|
|48-hour watch points actionable|0%|100%|
|Care team rated "ready to act on"|0%|100%|

\---

## References

* ACQSC. (2019). *Aged Care Quality Standards — Standard 1 and 3*. Australian Government.
* Anthropic. (2025). *Prompt engineering overview*. docs.claude.ai
* Australian Government. (2024). *Aged Care Act 2024 — Rights-based approach*.
* Fair Work Commission. (2025). *SCHADS Award pay guide*.
* Royal Commission into Aged Care Quality and Safety. (2021). *Final report* (Vol. 1).

\---

## Related Prompts

* **Next in chain:** P08 — Family update letter (welcome letter triggered on admission)
* **Governed by:** P10 — AI risk governance policy
* **Library index:** [README.md](../README.md)

