[P01-daily-care-note-generator.md](https://github.com/user-attachments/files/26382202/P01-daily-care-note-generator.md)[Upload# P01 · Daily care note generator

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

[P02-incident-report-drafter.md](https://github.com/user-attachments/files/26382209/P02-incident-report-drafter.md)
ing P01-daily-care-note-generator.md…]()
# P02 · Incident report drafter

**Section:** 01 — Care documentation chain
**Workflow step:** Step 2 of 3
**Current version:** v1.2
**Status:** Tested
**Last updated:** March 2026

\---

## Prompt Text (v1.2 — current)

```
You are a compliance officer at Sunrise Gardens Aged Care, Bundoora Victoria.

A reportable incident has occurred. Using ONLY the information provided below, draft a formal 
incident report aligned with the Serious Incident Response Scheme (SIRS) 2021 and Aged Care 
Quality Standards (Standard 8 — Organisational Governance).

INCIDENT DETAILS:
- Resident name: \[RESIDENT\_NAME]
- Age: \[AGE]
- Date and time: \[DATE\_TIME]
- Location: \[LOCATION]
- Incident type: \[INCIDENT\_TYPE]
- Staff present: \[STAFF\_NAMES AND ROLES]
- Witnesses: \[WITNESS\_NAMES or "None"]

WITNESS ACCOUNT:
\[PASTE EXACT STAFF OBSERVATIONS — dot points are fine]

IMMEDIATE ACTIONS TAKEN:
\[LIST ALL COMPLETED ACTIONS WITH TIMES]

Step 1 — Classify the incident:
SIRS STATUS: \[Priority 1 / Priority 2 / Below threshold]
REASON: \[One sentence justification based on SIRS criteria]

Step 2 — Draft the incident report with these sections:
1. Incident summary (factual, chronological — 3–4 sentences)
2. Immediate response (actions taken at time of incident)
3. Persons notified (who was informed and when)
4. Follow-up actions required (specific, assigned next steps)
5. Risk assessment (Low / Medium / High recurrence likelihood — one sentence)
6. Reporting obligations (SIRS status from Step 1 + submission deadline)

Rules:
- Maximum 350 words (excluding Step 1)
- Formal, factual, non-judgmental language
- Do NOT assign blame to any individual
- Do NOT assume beyond information provided
- Flag mandatory reporting with \[MANDATORY REPORT — Priority 1: 24 hrs / Priority 2: 30 days]
```

**Placeholders to fill:**

|Placeholder|Source|Example|
|-|-|-|
|`\[RESIDENT\_NAME]`|Resident management system|Margaret Thompson|
|`\[AGE]`|Resident profile|84|
|`\[DATE\_TIME]`|Incident log|1 April 2026, 2:15 PM|
|`\[LOCATION]`|Staff account|Bathroom, Wing B|
|`\[INCIDENT\_TYPE]`|Classification list|Unwitnessed fall with skin tear|
|`\[STAFF\_NAMES AND ROLES]`|Roster|Jane Nguyen (PCW), David Park (RN)|
|`\[WITNESS ACCOUNT]`|Staff notes|Found resident on floor, alert, small laceration on left forearm|
|`\[IMMEDIATE ACTIONS]`|Incident log|First aid 2:18pm; RN notified 2:18pm; family 2:35pm; GP 3:00pm|

\---

## Intended Workflow or Task

* **Trigger:** \[URGENT] flag from P01, or direct staff escalation at time of incident
* **Actor:** RN inputs incident details → AI classifies and drafts report → Facility Manager reviews → Filed in incident management system
* **Timing:** Draft within 2 hours; SIRS Priority 1 submitted within 24 hours; Priority 2 within 30 days
* **Next step:** Filed in RiskMan. \[MANDATORY REPORT] triggers P06. Incident data feeds monthly into P04.

```
\[URGENT] from P01 or direct staff escalation
              ↓
Staff inputs incident details + witness account
              ↓
\[P02 RUNS — Step 1: SIRS classification
            Step 2: Incident report drafted]
              ↓
Facility Manager reviews and approves
              ↓
Filed in RiskMan
              ↓
\[MANDATORY REPORT]? → P06    No? → Filed internally → P04
```

**Prompting technique:** RACE + chain-of-thought decomposition + regulatory grounding. Separating SIRS classification (Step 1) from report drafting (Step 2) makes classification an explicit, auditable reasoning step. Embedding SIRS 2021 and Standard 8 directly into the prompt ensures legally structured output (Anthropic, 2025; ACQSC, 2021).

\---

## Problem Being Solved

Under SIRS 2021, the Facility Manager must submit Priority 1 reports within **24 hours** (ACQSC, 2021). Sunrise Gardens generates \~200 incident reports/year. At 50 min each manually (\~$7,000/year at RN rate $42/hr; Fair Work Commission, 2025), drafting under deadline pressure creates compliance risk.

**Pain points addressed:**

* Reduces drafting time by \~65% (50 min → 18 min, saving \~$4,494/year)
* Automated SIRS classification with auditable reasoning trail
* \[MANDATORY REPORT] flag prevents missed reporting deadlines

\---

## Automation Potential

**Level: High**

|Dimension|Assessment|
|-|-|
|Repetitiveness|High — \~200 reports/year at Sunrise Gardens|
|Data availability|High — staff already record witness accounts and actions|
|Human judgment needed|High — Facility Manager must confirm SIRS classification and sign off|
|Integration possibility|Medium — SIRS STATUS field parseable by RiskMan for automated routing|
|Estimated time saving|\~65% — 50 min → 18 min (\~$4,494/year)|

**Human-in-the-loop role:** RN inputs incident details. AI classifies and drafts. Facility Manager confirms SIRS classification, approves narrative, and personally submits via My Aged Care portal. All submissions filed under Sunrise Gardens' RACS ID — Facility Manager is named responsible officer.

\---

## Risks and Limitations

|Risk|Level|Mitigation|
|-|-|-|
|**Hallucination** — AI fabricates witness account content; Facility Manager files a legally invalid record under Aged Care Act 2024|High|Grounding constraint: "ONLY the information provided." Facility Manager verifies narrative against original staff notes before approving.|
|**Bias** — AI language implicitly attributes fault to a staff member; discrimination risk under Fair Work Act 2009 (Cth)|High|Explicit rule: "Do NOT assign blame." Legal review of template language before deployment.|
|**Privacy** — Incident details including resident injury and staff names submitted to AI; Facility Manager liable under Privacy Act 1988 (Cth)|High|Organisation-hosted private AI only. No incident data to public AI tools.|
|**Governance** — AI misclassifies Priority 1 as Priority 2; Facility Manager misses 24-hour SIRS deadline|High|Facility Manager cross-checks Step 1 classification against ACQSC SIRS decision tool before filing. Human makes final reporting decision.|
|**Over-reliance** — Staff submit incomplete witness accounts trusting AI to fill gaps|Medium|Training: full witness account mandatory before running prompt. Quarterly input completeness audits.|

**Overall risk rating: HIGH** — must be reviewed and approved by Facility Manager before filing. SIRS reporting decisions always remain with qualified humans.

\---

## Version History

### v1.0 — Initial draft

**Date:** 24 March 2026
**Prompt:** `Write an incident report for an aged care facility.`
**Output:** Fictional narrative with invented names and injury mechanisms. No SIRS structure. Legally unusable.
**Lesson learned:** Legal documents need regulatory grounding, structured input fields, and grounding constraint.

\---

### v1.1 — Added RACE, SIRS reference, structured sections, input fields

**Date:** 27 March 2026
**Change:** Added compliance officer role, SIRS 2021 reference, incident placeholder fields, six-section output, formal tone instruction.
**Output:** Correct structure and tone. However AI still invented witness narrative — no observation input field. SIRS classification embedded in narrative, not explicit. No \[MANDATORY REPORT] flag.
**Lesson learned:** Witness account input field is non-negotiable. SIRS classification must be an explicit chain-of-thought step — not buried in narrative.

\---

### v1.2 — Added chain-of-thought decomposition, witness account field, \[MANDATORY REPORT] flag Current

**Date:** 31 March 2026
**Change:** Added Step 1 (explicit SIRS classification), `\[WITNESS ACCOUNT]` and `\[IMMEDIATE ACTIONS]` input fields, grounding constraint, \[MANDATORY REPORT] flag with deadline.
**Output:** Fully grounded, SIRS-compliant. SIRS classification accurate in all 4 test scenarios. Zero fabricated content when complete witness account provided.
**Lesson learned:** Chain-of-thought decomposition transforms AI from text generator into verifiable reasoning partner. Step 1 classification gives the Facility Manager something concrete to cross-check before signing.

\---

## A/B Test Results

**Test date:** 31 March 2026 | **Sample:** 4 real incident scenarios | **Evaluators:** Facility Manager and RN (blind review)

|Criteria|v1.0|v1.2|
|-|-|-|
|Grounded in observations (no fabrication)|0%|100%|
|SIRS classification explicit|0%|100%|
|Non-judgmental language|30%|100%|
|\[MANDATORY REPORT] flag accurate|0%|100%|
|Ready to file after minor review|0%|75%|

\---

## References

* ACQSC. (2019). *Aged Care Quality Standards — Standard 8*. Australian Government.
* ACQSC. (2021). *Serious Incident Response Scheme (SIRS)*. Australian Government.
* Anthropic. (2025). *Prompt engineering overview*. docs.claude.ai
* Australian Government. (2024). *Aged Care Act 2024*.
* Fair Work Commission. (2025). *SCHADS Award pay guide*.

\---

## Related Prompts

* **Triggered by:** P01 (\[URGENT] flag)
* **Feeds into:** P03 — Shift handover; P04 — Compliance audit (monthly)
* **Triggers:** P06 — Regulatory submission (\[MANDATORY REPORT] flag)
* **Governed by:** P10 — AI risk governance policy
* **Library index:** [README.md](../README.md)
[P03-shift-handover-summary.md](https://github.com/user-attachments/files/26382225/P03-shift-handover-summary.md)

# P03 · Shift handover summary

**Section:** 01 — Care documentation chain
**Workflow step:** Step 3 of 3
**Current version:** v1.2
**Status:** Tested
**Last updated:** March 2026

\---

## Prompt Text (v1.2 — current)

```
You are the senior Registered Nurse conducting end-of-shift handover at Sunrise Gardens 
Aged Care, Bundoora Victoria. Apply the ISBAR framework (Identify, Situation, Background, 
Assessment, Recommendation) to each priority resident entry.

SHIFT DETAILS:
- Shift: \[SHIFT — Morning / Afternoon / Night]
- Date: \[DATE]
- Outgoing senior staff: \[NAME AND ROLE]
- Incoming senior staff: \[NAME AND ROLE]
- Total residents on floor: \[NUMBER]

RESIDENT UPDATES FROM THIS SHIFT:
\[PASTE P01 CARE NOTE SUMMARIES AND P02 INCIDENT HIGHLIGHTS — 
one dot point per resident with name, key observation, any incidents, actions taken]

OUTSTANDING TASKS:
\[LIST INCOMPLETE TASKS THE INCOMING TEAM MUST ACTION]

Step 1 — Classify each resident with notable updates into:
- PRIORITY: \[URGENT] flag in P01 or active incident from P02
- WATCH: New observation, behavioural change, or elevated risk
- STABLE: Standard shift, no concerns

Step 2 — Write the handover summary with:
1. Priority residents — immediate attention required
   (ISBAR entry per resident: name, issue, action required, deadline)
2. Watch list — monitor closely (name and specific concern)
3. Stable residents — brief confirmation statement only
4. Outstanding tasks for incoming team (actionable, with role and deadline)
5. Medications and clinical alerts (changes, refused medications, new alerts)

Rules:
- Maximum 400 words
- ISBAR structure within each Priority entry
- Do NOT include information not in the provided notes
- Flag unresolved SIRS reportable incidents with \[SIRS PENDING — notify Facility Manager]
```

**Placeholders to fill:**

|Placeholder|Source|Example|
|-|-|-|
|`\[SHIFT]`|Roster|Morning (0700–1500)|
|`\[DATE]`|System date|1 April 2026|
|`\[OUTGOING STAFF]`|Roster|Sarah Chen, RN|
|`\[INCOMING STAFF]`|Roster|David Park, RN|
|`\[NUMBER]`|Resident register|62|
|`\[RESIDENT UPDATES]`|P01 summaries + P02 highlights|Margaret T — fall 2:15pm, treated, GP notified \[P02]. John S — refused meals, confused \[P01].|
|`\[OUTSTANDING TASKS]`|Outgoing RN notes|GP callback for Margaret T before 6pm. Medication review Room 14 incomplete.|

\---

## Intended Workflow or Task

* **Trigger:** 30 minutes before shift end — RN compiles P01 and P02 outputs from current shift
* **Actor:** Outgoing RN pastes shift updates → AI classifies residents and generates handover → RN reviews before verbal handover meeting
* **Timing:** Completed before incoming shift arrives; used as written basis for verbal handover
* **Next step:** \[SIRS PENDING] triggers immediate Facility Manager notification before RN departs

```
P01 care notes + P02 incident highlights (this shift)
                  ↓
Outgoing RN compiles updates as dot points
                  ↓
\[P03 RUNS — Step 1: Resident tier classification
            Step 2: Structured handover generated]
                  ↓
Outgoing RN reviews and confirms classifications
                  ↓
Verbal handover meeting with incoming team
                  ↓
\[SIRS PENDING]? → Facility Manager notified before RN departs
```

**Prompting technique:** RACE + chain-of-thought priority tiering + prompt chaining. Step 1 forces explicit classification before narrative generation — the most clinically critical feature, as incoming staff must instantly identify urgent residents (ACSQHC, 2020). P03 directly receives P01 and P02 outputs as inputs, creating an integrated three-prompt care documentation chain (Anthropic, 2025).

\---

## Problem Being Solved

Communication failures during handover contribute to \~24% of adverse events in residential care (ACSQHC, 2020). At Sunrise Gardens, the outgoing RN spends \~45 min preparing handover documentation. At 3 handovers/day, 365 days/year, this is **\~821 RN hours annually (\~$34,482/year at $42/hr; Fair Work Commission, 2025).**

**Pain points addressed:**

* Reduces handover preparation by \~60% (45 min → 18 min, saving \~$20,689/year)
* Priority tiering ensures incoming staff identify urgent residents within 30 seconds
* \[SIRS PENDING] flag ensures Facility Manager meets SIRS deadlines before shift changeover

\---

## Automation Potential

**Level: High**

|Dimension|Assessment|
|-|-|
|Repetitiveness|Very high — 1,095 handovers/year at Sunrise Gardens|
|Data availability|High — P01 and P02 outputs are the direct structured inputs|
|Human judgment needed|High — RN must confirm priority classifications before verbal handover|
|Integration possibility|High — future auto-pull from resident management system|
|Estimated time saving|\~60% — 45 min → 18 min (\~$20,689/year)|

**Human-in-the-loop role:** Outgoing RN pastes P01 and P02 summaries. AI classifies and structures the handover. RN confirms Priority classifications match clinical judgment and uses document as basis for verbal handover. Facility Manager alerted to any \[SIRS PENDING] flag before RN leaves the building.

\---

## Risks and Limitations

|Risk|Level|Mitigation|
|-|-|-|
|**Hallucination** — AI misclassifies deteriorating resident as Stable due to incomplete P01 input; adverse event occurs on incoming shift|High|RN mandatory review of all classifications. Outgoing RN cross-checks resident count in output against register. P01 completion audited monthly.|
|**Bias** — AI deprioritises residents whose conditions are described in non-standard English; inequitable care escalation|High|RN reviews Watch and Stable entries for potential under-escalation. Facility Manager monitors escalation patterns by carer cohort quarterly.|
|**Privacy** — Full shift summary with all resident health updates processed through AI|High|Organisation-hosted private AI only. Shift summary classified as clinical health information under Privacy Act 1988 (Cth).|
|**Governance** — \[SIRS PENDING] flag omitted; Facility Manager misses SIRS deadline|High|RN cross-checks \[SIRS PENDING] against all P02 outputs before handover is filed.|
|**Over-reliance** — Verbal handover replaced entirely by AI document; nuanced clinical communication lost|Medium|Policy: AI document supports but never replaces verbal handover. Verbal handover mandatory at Sunrise Gardens.|

**Overall risk rating: HIGH** — must be reviewed by RN before verbal handover. Priority classifications are clinical decisions confirmed by a qualified human.

\---

## Version History

### v1.0 — Initial draft

**Date:** 24 March 2026
**Prompt:** `Write a shift handover for an aged care facility.`
**Output:** Generic summary, no resident specifics, no priority structure, invented incidents. Unusable.
**Lesson learned:** A handover's clinical value is prioritisation. A prompt without explicit priority tiering will not produce it.

\---

### v1.1 — Added RACE, ISBAR reference, resident input field, structured sections

**Date:** 27 March 2026
**Change:** Added RN role, ISBAR reference, resident update input, five structured sections.
**Output:** Well-structured and appropriate tone. However all residents listed at equal priority — no classification. Incoming RN: "I still had to scan everything to find who was urgent." No \[SIRS PENDING] flag.
**Lesson learned:** Structure without prioritisation does not solve the handover problem. Explicit classification tiering — chain-of-thought — was the missing design element.

\---

### v1.2 — Added chain-of-thought priority tiering, P01/P02 chaining, \[SIRS PENDING] flag Current

**Date:** 31 March 2026
**Change:** Added Step 1 classification (Priority/Watch/Stable). Made P01 and P02 outputs explicit inputs. Added \[SIRS PENDING] flag with Facility Manager notification.
**Output:** Prioritised, ISBAR-structured, fully grounded handover. Incoming RN: "I knew who to check within 30 seconds." \[SIRS PENDING] triggered correctly in all test scenarios with unresolved incidents.
**Lesson learned:** Chain-of-thought priority tiering is the most clinically impactful technique in this library. Classification before narration mirrors how an experienced RN mentally organises a handover.

\---

## A/B Test Results

**Test date:** 31 March 2026 | **Sample:** 3 real shift scenarios | **Evaluators:** 2 RNs (blind review)

|Criteria|v1.0|v1.2|
|-|-|-|
|Priority classification accurate|0%|100%|
|ISBAR structure in Priority entries|0%|100%|
|Grounded in shift notes only|0%|100%|
|Immediately actionable by incoming team|0%|100%|
|\[SIRS PENDING] accurate|N/A|100%|

\---

## References

* ACQSC. (2019). *Aged Care Quality Standards — Standard 3*. Australian Government.
* ACQSC. (2021). *Serious Incident Response Scheme (SIRS)*. Australian Government.
* ACSQHC. (2020). *ISBAR — Communicating for safety*. Australian Government.
* Anthropic. (2025). *Prompt engineering overview*. docs.claude.ai
* Fair Work Commission. (2025). *SCHADS Award pay guide*.

\---

## Related Prompts

* **Receives input from:** P01 — Daily care note generator
* **Receives input from:** P02 — Incident report drafter
* **Feeds into:** P04 — Compliance audit report (monthly)
* **Governed by:** P10 — AI risk governance policy
* **Library index:** [README.md](../README.md)

[P04-compliance-audit-report.md](https://github.com/user-attachments/files/26382237/P04-compliance-audit-report.md)
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
[P05-quality-standards-self-assessment.md](https://github.com/user-attachments/files/26382242/P05-quality-standards-self-assessment.md)
# P05 · Quality standards self-assessment

**Section:** 02 — Regulatory and compliance chain
**Workflow step:** Step 2 of 3
**Current version:** v1.2
**Status:** ✅ Tested
**Last updated:** March 2026

\---

## 📌 Prompt Text (v1.2 — current)

```
You are the quality assurance specialist at Sunrise Gardens Aged Care, Bundoora Victoria.

Using ONLY the information provided below, conduct an evidence-based self-assessment against 
the Aged Care Quality Standard identified, aligned with ACQSC assessment methodology.

STANDARD BEING ASSESSED:
- Standard: \[STANDARD NUMBER AND NAME]
- Assessment trigger: \[REASON — e.g. flagged Non-Compliant in P04, scheduled quarterly review]
- Assessment period: \[DATE RANGE]

EVIDENCE PROVIDED:
\[PASTE ALL AVAILABLE EVIDENCE — care plan data, incident records, training records,
resident feedback, complaint records, policy documents. Source: P04 findings.]

KNOWN GAPS OR CONCERNS:
\[LIST KNOWN GAPS, COMPLAINTS, OR INCIDENTS RELEVANT TO THIS STANDARD]

Write the self-assessment with:
1. Standard overview (one sentence — what this standard requires)
2. Current compliance rating: \[Compliant / Partially Compliant / Non-Compliant]
   — justify with specific evidence cited from input (3–4 sentences)
3. Evidence of compliance (what is working well)
4. Identified gaps (what is missing — be direct, not diplomatic)
5. Improvement action plan — for each action include:
   - Action (specific and measurable)
   - Responsible role
   - Target completion date
   - Quality Standard reference
   (minimum 3, maximum 5 SMART actions)
6. Risk to residents if gaps not addressed (plain language — what could go wrong)

Rules:
- Maximum 450 words
- Evidence-based — cite specific data points from input
- Do NOT rate compliance higher than the evidence supports
- Flag overdue actions with \[OVERDUE]
- Flag High resident safety risks with \[RESIDENT SAFETY RISK]
```

**Placeholders to fill:**

|Placeholder|Source|Example|
|-|-|-|
|`\[STANDARD]`|ACQSC Quality Standards|Standard 3 — Care and Services|
|`\[REASON]`|P04 trigger|Flagged Partially Compliant in March 2026 audit|
|`\[DATE RANGE]`|Assessment period|1 January 2026 – 31 March 2026|
|`\[EVIDENCE]`|Clinical, training, survey records|38/62 care plans reviewed; 2 medication errors; satisfaction 74%; 12 staff dementia-trained|
|`\[KNOWN GAPS]`|P04 findings|24 care plans overdue; 2 unresolved medication complaints|

\---

## 🏢 Intended Workflow or Task

* **Trigger:** \[NON-COMPLIANT ALERT] from P04, scheduled quarterly review, or ACQSC inspection notification
* **Actor:** Facility Manager gathers evidence → AI drafts self-assessment → Clinical Governance Lead reviews → Action plan assigned
* **Timing:** Within 5 business days of P04 non-compliant flag

```
\[NON-COMPLIANT ALERT] from P04 or scheduled review
             ↓
Facility Manager gathers evidence for flagged Standard
             ↓
\[P05 RUNS — self-assessment generated]
             ↓
Clinical Governance Lead reviews and approves action plan
             ↓
Actions assigned with named responsible roles
             ↓
Findings filed for ACQSC inspection readiness
```

**Prompting technique:** RACE + SMART output constraints + resident safety reframing. Requiring 4 components per action (description, role, date, Standard reference) enforces SMART criteria at the prompt level. "Risk to residents" section reframes abstract compliance gaps in human terms — producing governance responses with greater operational urgency (Anthropic, 2025; ACQSC, 2023).

\---

## ❗ Problem Being Solved

Manual self-assessment preparation takes \~**3 hours/Standard** (32 assessments/year = **96 hours = \~$5,568/year** at Facility Manager rate $58/hr). Providers unable to produce current, evidence-based assessments risk formal non-compliance notices publicly reported on ACQSC website (ACQSC, 2023).

**Pain points addressed:**

* Reduces preparation by \~55% (3 hrs → 80 min, saving \~$3,074/year)
* Enforces SMART actions consistently across all 32 annual assessments
* Produces inspection-ready documentation on demand

\---

## ⚡ Automation Potential

**Level: Medium**

|Dimension|Assessment|
|-|-|
|Repetitiveness|Medium — 32 assessments/year (8 Standards × quarterly)|
|Data availability|Medium — evidence gathered from multiple systems before prompting|
|Human judgment needed|High — compliance rating confirmed by Clinical Governance Lead|
|Integration possibility|Low-Medium — partial integration with quality management systems|
|Estimated time saving|\~55% — 3 hrs → 80 min (\~$3,074/year)|

**Human-in-the-loop role:** Facility Manager certifies evidence completeness before prompting. Reviews AI-generated compliance rating — challenges any that overstate evidence. Clinical Governance Lead approves action plan. Facility Manager accountable for accuracy of all assessments prepared for ACQSC inspection.

\---

## ⚠️ Risks and Limitations

|Risk|Level|Mitigation|
|-|-|-|
|**Hallucination** — AI rates compliance higher than evidence supports; Facility Manager presents false assurance to ACQSC inspector|High|Rule: "Do NOT rate compliance higher than evidence supports." Clinical Governance Lead independently reviews each rating. Cross-referenced against ACQSC self-assessment guide.|
|**Bias** — AI generates improvement actions assuming resourcing not available at Sunrise Gardens; unachievable action plan damages credibility with ACQSC|High|Facility Manager reviews all actions for resource feasibility. Unrealistic timelines adjusted before approval.|
|**Privacy** — Evidence input may include identifiable resident complaint and staff training data|High|Data de-identified at individual level before prompting. Organisation-hosted AI only.|
|**Governance** — Self-assessment becomes documentation exercise without genuine improvement follow-through|Medium|Clinical Governance Lead audits action completion monthly. Facility Manager reports progress to governing board quarterly.|
|**Over-reliance** — Incomplete evidence input produces superficial assessment; gaps missed during ACQSC inspection|High|Evidence completeness checklist per Standard before prompting. Facility Manager certifies completeness.|

**Overall risk rating: HIGH** — all compliance ratings and action plans must be reviewed by the Clinical Governance Lead before filing.

\---

## 🔄 Version History

### v1.0 — Initial draft

**Date:** 24 March 2026
**Prompt:** `Write a self-assessment for aged care quality standards.`
**Output:** Generic checklist, all Standards rated Compliant, no evidence grounding. Unusable for ACQSC purposes.
**Lesson learned:** Without evidence input and constrained ratings, AI defaults to optimistic Compliant assessments.

\---

### v1.1 — Added standard field, evidence input, ACQSC reference, rating constraints

**Date:** 27 March 2026
**Change:** Added specific standard, evidence input, ACQSC reference, three-option rating.
**Output:** Evidence-grounded structure. However improvement actions were vague ("improve documentation") — no SMART criteria, no named roles, no Standard references. No resident safety risk framing.
**Lesson learned:** SMART criteria must be enforced structurally — requiring 4 components per action is more effective than asking for "SMART goals" generically.

\---

### v1.2 — Added SMART enforcement, \[RESIDENT SAFETY RISK] flag, \[OVERDUE] flag Current

**Date:** 31 March 2026
**Change:** Added 4-component SMART action requirement. Added "Risk to residents" section with \[RESIDENT SAFETY RISK] flag. Added \[OVERDUE] flag.
**Output:** Specific, evidence-grounded, inspection-ready assessments. All actions in test run rated SMART-compliant by Clinical Governance Lead. \[RESIDENT SAFETY RISK] correctly flagged care plan gap.
**Lesson learned:** Resident safety reframing is the feature most cited by the Facility Manager as improving governing board responses — concrete human consequences create urgency that abstract compliance ratings do not.

\---

## A/B Test Results

**Test date:** 31 March 2026 | **Sample:** 2 standards (Standards 3 and 7) | **Evaluators:** Facility Manager and Clinical Governance Lead

|Criteria|v1.0|v1.2|
|-|-|-|
|Evidence-grounded compliance rating|0%|100%|
|SMART actions (4 components each)|0%|100%|
|Named responsible roles|0%|100%|
|Quality Standard referenced per action|0%|100%|
|\[RESIDENT SAFETY RISK] flag accurate|N/A|100%|

\---

## References

* ACQSC. (2019). *Aged Care Quality Standards*. Australian Government.
* ACQSC. (2023). *Self-assessment guide for residential aged care*. Australian Government.
* Anthropic. (2025). *Prompt engineering overview*. docs.claude.ai
* Fair Work Commission. (2025). *SCHADS Award pay guide*.

\---

## Related Prompts

* **Triggered by:** P04 — Compliance audit report (\[NON-COMPLIANT ALERT])
* **Feeds into:** P06 — Regulatory submission draft (if ACQSC reporting required)
* **Governed by:** P10 — AI risk governance policy
* **Library index:** [README.md](../README.md)

[P06-regulatory-submission-draft.md](https://github.com/user-attachments/files/26382245/P06-regulatory-submission-draft.md)

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
[P07-resident-intake-summary.md](https://github.com/user-attachments/files/26382250/P07-resident-intake-summary.md)

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
[P08-family-update-letter.md](https://github.com/user-attachments/files/26382252/P08-family-update-letter.md)
# P08 · Family update letter

**Section:** 03 — Resident administration chain
**Workflow step:** Step 2 of 4
**Current version:** v1.2
**Status:** Tested
**Last updated:** March 2026

\---

## Prompt Text (v1.2 — current)

```
You are the care coordinator at Sunrise Gardens Aged Care, Bundoora Victoria.

Using ONLY the information provided below, write a professional letter to the family or 
next of kin of a resident. Reflect the open disclosure principles of the Aged Care Act 2024 
and Standard 6 (Feedback and Complaints).

LETTER DETAILS:
- Letter type: \[TYPE — Welcome on admission / Monthly update / Post-incident notification / 
  Care plan review notification]
- Resident preferred name: \[PREFERRED\_NAME]
- Family recipient: \[FAMILY\_NAME] — \[RELATIONSHIP]
- Date: \[DATE]
- Written by: \[STAFF\_NAME AND ROLE — include direct phone number]

CONTENT TO COMMUNICATE:
\[PASTE KEY POINTS — factual updates on resident wellbeing, activities, health observations,
or upcoming reviews. For incident notifications, include what happened and actions taken — 
source from P02 if applicable. Facts only.]

Apply tone based on letter type:
- WELCOME ON ADMISSION: warm, celebratory, reassuring
- MONTHLY UPDATE: warm, informative, one specific personal observation
- POST-INCIDENT NOTIFICATION: compassionate, transparent, factual — do not minimise
- CARE PLAN REVIEW: collaborative, informative, invite family input

Write the letter with:
1. Personal greeting (use family member's name — never "Dear Family")
2. Main content (3–4 short paragraphs covering key points)
3. Personal highlight (one genuine specific observation about \[PREFERRED\_NAME] — from content provided only)
4. Next steps (what happens next; invite family to visit or call)
5. Warm close (staff name, role, direct phone number)

Rules:
- Maximum 300 words
- Plain language — no medical jargon, acronyms, or clinical terminology
- Use \[PREFERRED\_NAME] throughout
- Do NOT include diagnoses, medication names, or financial information
- Do NOT minimise, omit, or soften negative events — factual accuracy is a legal obligation 
  under Standard 6 (ACQSC, 2019)
- For POST-INCIDENT letters: include \[FAMILY NOTIFIED — DATE AND TIME] at the very top
```

**Placeholders to fill:**

|Placeholder|Source|Example|
|-|-|-|
|`\[TYPE]`|Communication trigger|Post-incident notification|
|`\[PREFERRED\_NAME]`|P07 intake summary|Maggie|
|`\[FAMILY\_NAME]`|P07 NOK details|Susan Thompson — Daughter|
|`\[DATE]`|System date|1 April 2026|
|`\[STAFF\_NAME AND ROLE]`|Staff records|Sarah Chen, Care Coordinator — 03 9999 5678|
|`\[CONTENT]`|P02 summary / P01 highlights / P07 details|Maggie had a fall in her bathroom at 2:15pm. Alert and responsive. First aid applied. GP contacted. Full incident report completed.|

\---

## Intended Workflow or Task

* **Trigger:** New admission (welcome), monthly update cycle, post-incident within 24 hours, or care plan review scheduled
* **Actor:** Care Coordinator inputs content from P01, P02, or P07 → AI drafts letter with conditional tone → Facility Manager reviews before sending
* **Timing:** Welcome: within 48 hours. Monthly: last day of month. Post-incident: within 24 hours. Care plan review: 2 weeks before.

```
Trigger event identified
          ↓
Care Coordinator compiles content from P01, P02, or P07
          ↓
\[P08 RUNS — conditional tone applied
            Family letter drafted]
          ↓
Facility Manager reviews and approves
          ↓
Letter sent; copy filed in resident communication record
```

**Prompting technique:** RACE + conditional tone instruction + non-minimisation legal constraint. Four distinct tone conditions produce contextually appropriate output from a single prompt structure (Anthropic, 2025). The non-minimisation rule references the specific legal obligation under Standard 6 — a legal framing more effective than a general transparency instruction because AI models have a documented tendency to soften negative content.

\---

## Problem Being Solved

Sunrise Gardens sends \~20 family letters/month (240/year) across all four types. At 37.5 min each manually, this costs **\~5,250/year** at Care Coordinator rate $35/hr (Fair Work Commission, 2025). Poor family communication is a leading driver of formal ACQSC complaints — inconsistency and inadvertent omission of incident details are the most common issues (ACQSC, 2023).

**Pain points addressed:**

* Reduces drafting by \~65% (37.5 min → 13 min, saving \~$3,413/year)
* Conditional tone instruction ensures appropriate register for all four letter types
* Non-minimisation legal constraint ensures Standard 6 compliance in post-incident letters

\---

## Automation Potential

**Level: High**

|Dimension|Assessment|
|-|-|
|Repetitiveness|High — \~240 letters/year across all four types|
|Data availability|High — inputs from P01, P02, and P07 outputs|
|Human judgment needed|Medium — Facility Manager reviews for sensitivity before sending|
|Integration possibility|Medium — integrates with resident communication platforms|
|Estimated time saving|\~65% — 37.5 min → 13 min (\~$3,413/year)|

**Human-in-the-loop role:** Care Coordinator compiles content from P01, P02, and P07. Facility Manager reviews all letters — particularly post-incident notifications — before sending. Personally accountable for accuracy and appropriateness of all family communications sent under Sunrise Gardens' letterhead.

\---

## Risks and Limitations

|Risk|Level|Mitigation|
|-|-|-|
|**Hallucination** — AI invents a personal observation not in the provided content; family receives inaccurate information|High|Rule: personal highlight "from content provided only." Facility Manager verifies observation against source content before sending.|
|**Bias** — AI generates culturally inappropriate language for families from non-English-speaking backgrounds|High|Care Coordinator reviews cultural appropriateness before Facility Manager approval. Future: add `\[FAMILY\_LANGUAGE\_PREFERENCE]` placeholder.|
|**Privacy** — Resident health updates and incident information submitted to AI for letter generation|High|Organisation-hosted private AI only. Letter content classified as health information under Privacy Act 1988 (Cth).|
|**Governance** — Post-incident letter softens or omits incident details; Standard 6 breach; potential ACQSC complaint naming Facility Manager|High|Non-minimisation legal constraint in prompt. Facility Manager compares post-incident letter against original P02 before sending. \[FAMILY NOTIFIED] timestamp must match P02 record.|
|**Over-reliance** — Care Coordinator sends letter without Facility Manager review|Medium|Policy: Facility Manager approval required before sending any family letter.|

**Overall risk rating: MEDIUM** — incorrect or insensitive communication causes significant harm to family relationships and organisational reputation. Facility Manager review mandatory.

\---

## Version History

### v1.0 — Initial draft

**Date:** 24 March 2026
**Prompt:** `Write a letter to the family of an aged care resident.`
**Output:** Generic, corporate-sounding letter. Clinical terminology throughout. No personal observations. Care Coordinator: "This sounds like it came from a lawyer."
**Lesson learned:** Family communication requires emotional register matching. One static prompt cannot produce appropriate output for both a welcome letter and a post-incident notification.

\---

### v1.1 — Added RACE, four tone types, preferred name, personal observation

**Date:** 27 March 2026
**Change:** Added care coordinator role, four conditional tone instructions, preferred name field, personal observation requirement.
**Output:** Better tone differentiation. Welcome and monthly letters warm and personal. However post-incident letters softened incident details — omitting the skin tear when the AI applied the "compassionate" tone instruction. No \[FAMILY NOTIFIED] timestamp.
**Lesson learned:** "Compassionate" tone without an explicit non-minimisation rule produces incomplete post-incident letters. The non-minimisation constraint must reference the specific legal obligation — general transparency instructions are insufficient.

\---

### v1.2 — Added non-minimisation legal constraint, \[FAMILY NOTIFIED] timestamp, P01/P02/P07 chaining Current

**Date:** 31 March 2026
**Change:** Added "factual accuracy is a legal obligation under Standard 6" rule. Added \[FAMILY NOTIFIED] flag. Made P01, P02, P07 explicit input sources.
**Output:** Warm, transparent, person-centred across all four types. Post-incident letter correctly included full factual account without alarming language. Welcome letter rated "beautiful" by family member in pilot.
**Lesson learned:** Referencing the specific legal obligation in the non-minimisation rule is more effective than a general transparency instruction. Legal framing overrides the model's tendency to soften negative content.

\---

## A/B Test Results

**Test date:** 31 March 2026 | **Sample:** 4 letters (one per type) | **Evaluators:** Facility Manager and 2 family members (blind review)

|Criteria|v1.0|v1.2|
|-|-|-|
|Tone matched to letter type|0%|100%|
|No minimisation of negative events|0%|100%|
|Preferred name consistent|20%|100%|
|Plain language (no jargon)|0%|100%|
|Ready to send after minor personalisation|0%|75%|

\---

## References

* ACQSC. (2019). *Aged Care Quality Standards — Standard 6: Feedback and complaints*. Australian Government.
* ACQSC. (2023). *Complaints about aged care: Annual report 2022–23*. Australian Government.
* Anthropic. (2025). *Prompt engineering overview*. docs.claude.ai
* Australian Government. (2024). *Aged Care Act 2024 — Open disclosure obligations*.
* Fair Work Commission. (2025). *SCHADS Award pay guide*.

\---

## Related Prompts

* **Receives input from:** P07 — Resident intake summary; P01 — Daily care note; P02 — Incident report
* **Governed by:** P10 — AI risk governance policy
* **Library index:** [README.md](../README.md)

[P09-staff-performance-review.md](https://github.com/user-attachments/files/26382255/P09-staff-performance-review.md)
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

[P10-ai-governance-policy.md](https://github.com/user-attachments/files/26382259/P10-ai-governance-policy.md)
# P10 · AI risk governance policy

**Section:** 03 — Resident administration chain
**Workflow step:** Step 4 of 4 (standalone governance)
**Current version:** v1.2
**Status:** Tested
**Last updated:** March 2026

\---

## Prompt Text (v1.2 — current)

```
You are a clinical governance advisor working with the Facility Manager of Sunrise Gardens 
Aged Care, Bundoora Victoria (RACS ID: \[RACS\_ID]).

Using ONLY the information provided below, draft an AI Risk Governance Policy aligned with:
- Aged Care Quality Standards 2019 (Standard 8 — Organisational Governance)
- Australian Government Voluntary AI Safety Standard 2024
- Privacy Act 1988 (Cth) and Health Records Act 2001 (Vic)
- Aged Care Act 2024

ORGANISATION DETAILS:
- Total residents: \[NUMBER]
- Total staff: \[NUMBER]
- AI tools in use or proposed: \[LIST AI TOOLS]
- Policy effective date: \[DATE]
- Policy owner: \[FACILITY MANAGER NAME AND ROLE]
- Review frequency: \[e.g. Annual, or within 30 days of any AI error incident]

AI USE CASES IN THIS FACILITY:
\[LIST ALL PROMPTS WITH CHAIN GROUPINGS — reference P01–P09]

RISK REGISTER FROM PROMPT LIBRARY:
\[PASTE KEY RISKS FROM P01–P09 — grouped by category:
Hallucination / Bias / Privacy and Security / Governance]

Before drafting: Review the risk register and identify which use cases carry unresolved 
High-level risks from P01–P09. List these for governing board attention.

Then draft the policy with:
1. Purpose and scope
2. Guiding principles (5 principles aligned with Voluntary AI Safety Standard 2024)
3. Approved use cases (table: Prompt ID | Workflow | AI tool | Named oversight role | Timing)
4. Prohibited uses (minimum 5 specific aged-care prohibitions — not generic)
5. Human oversight requirements (named role, what they check, when — for every use case)
6. Data privacy and security controls (Privacy Act 1988 and Health Records Act 2001 (Vic))
7. Staff training requirements (role-specific, before AI tool authorisation)
8. AI error reporting and escalation pathway
9. Policy review and accountability

Rules:
- Maximum 700 words
- Formal policy language throughout
- Every oversight requirement must name a specific role — not "a staff member"
- Flag every use case with unresolved High risk with \[HIGH RISK — ADDITIONAL CONTROLS REQUIRED]
- Policy must explicitly state AI outputs are never the final clinical, legal, or HR authority
- Minimum 5 prohibited uses specific to aged care
```

**Placeholders to fill:**

|Placeholder|Source|Example|
|-|-|-|
|`\[RACS\_ID]`|Organisation records|123456|
|`\[TOTAL RESIDENTS]`|Resident register|62|
|`\[TOTAL STAFF]`|HR system|48|
|`\[AI TOOLS]`|IT records|Claude Sonnet 4 via Azure OpenAI (Australian data residency); RiskMan|
|`\[POLICY EFFECTIVE DATE]`|Governance calendar|1 April 2026|
|`\[FACILITY MANAGER]`|Organisation records|James Okafor, Facility Manager|
|`\[REVIEW FREQUENCY]`|Governance calendar|Annual, or within 30 days of any AI error incident or significant model update|
|`\[AI USE CASES]`|P01–P09|Chain 1: P01–P03 (care documentation). Chain 2: P04–P06 (compliance). Chain 3: P07–P10 (administration and governance).|
|`\[RISK REGISTER]`|P01–P09 risk sections|Hallucination: P01, P02, P06, P07, P09. Bias: P02, P03, P08, P09. Privacy/Security: all P01–P09. Governance: P02 (SIRS), P04 (compliance), P06 (liability), P09 (anti-discrimination).|

\---

## Intended Workflow or Task

* **Trigger:** Initial deployment of AI library (P01–P09), annual review, or within 30 days of any AI error incident
* **Actor:** Facility Manager compiles use cases and categorised risk register → AI reviews High risks and drafts policy → Governing board approves → Distributed to all staff before any AI tool authorised
* **Timing:** Policy must be approved before P01–P09 deployed in any clinical or administrative workflow

```
AI Prompt Library (P01–P09) finalised
                 ↓
Facility Manager compiles use cases + risk register
(categorised: Hallucination / Bias / Privacy / Governance)
                 ↓
\[P10 RUNS — pre-review flags High risks
            AI governance policy drafted]
                 ↓
Governing board reviews and formally approves
                 ↓
Policy published; role-specific staff training completed
                 ↓
P01–P09 authorised for use under governance framework
```

**Prompting technique:** RACE + synthesis prompting with risk categorisation + pre-generation risk review + minimum-count constraint on prohibited uses. Requiring risks grouped by category (Hallucination / Bias / Privacy / Governance) enables synthesis of 9 individual risk registers into a single organisational risk landscape — the level of governance thinking Standard 8 requires (ACQSC, 2019). Minimum 5 specific prohibitions prevents generic AI policy language and demonstrates aged-care-specific regulatory literacy (ACQSC, 2024).

\---

## Problem Being Solved

Under Standard 8 (Organisational Governance), the Facility Manager must ensure all systems — including AI — are governed by appropriate policies and oversight (ACQSC, 2019). The Voluntary AI Safety Standard 2024 reinforces this for high-risk AI deployments (Australian Government Department of Industry, Science and Resources, 2024). Most facilities deploy AI without formal governance, creating direct regulatory risk.

Manual policy drafting from scratch takes **6–10 hours** and typically requires external consultant engagement (\~$1,500–$2,000/day).

**Pain points addressed:**

* Reduces drafting by \~70% (8 hrs → 2.5 hrs, saving \~$7,540 vs external consultant)
* Synthesises all P01–P09 risk mitigations into a single organisation-wide framework
* Creates inspection-ready Standard 8 evidence for ACQSC

\---

## Automation Potential

**Level: Medium**

|Dimension|Assessment|
|-|-|
|Repetitiveness|Low-Medium — drafted once; reviewed annually or after significant AI events|
|Data availability|High — inputs drawn from P01–P09 risk sections|
|Human judgment needed|Very High — governing board approval required; organisation-wide legal implications|
|Integration possibility|Low — governance document; no system integration required|
|Estimated time saving|\~70% — 8 hrs → 2.5 hrs (\~$7,540 vs external consultant)|

**Human-in-the-loop role:** Facility Manager certifies accuracy of use cases and risk register. Governing board reviews all approved use cases, prohibited uses, and oversight requirements. Legal counsel review strongly recommended before final approval. Facility Manager is named policy owner, personally accountable for implementation and annual review.

\---

## Risks and Limitations

|Risk|Level|Mitigation|
|-|-|-|
|**Hallucination** — Policy omits a significant risk from P01–P09 due to incomplete risk register input; Facility Manager relies on inadequate governance during ACQSC inspection|High|Pre-generation risk review flags \[HIGH RISK] use cases. Governing board confirms all High-level risks from P01–P09 addressed before approval.|
|**Bias** — Policy oversight requirements systematically exclude certain staff cohorts from AI tool access without legitimate justification|High|Governing board reviews oversight requirements for equitable access. Role-specific training (not role-exclusion) is the preferred access management mechanism.|
|**Privacy** — Full categorised risk register including aggregated resident and staff risk data submitted to AI|High|Organisation-hosted private AI only. Risk register de-identified at individual level before prompting.|
|**Governance** — Policy language too generic to be enforceable; governance exists on paper only|High|Specific role requirements and minimum prohibition count in prompt. Facility Manager reviews for operational specificity before board submission.|
|**Over-reliance** — Policy becomes outdated as AI models and regulations evolve|Medium|Annual review cycle with 30-day trigger for AI error events or significant model updates built into policy.|

**Overall risk rating: HIGH** — organisation-wide legal, regulatory, and ethical implications. Must be reviewed by Facility Manager, Clinical Governance Lead, and governing board. Legal counsel review strongly recommended.

\---

## Version History

### v1.0 — Initial draft

**Date:** 24 March 2026
**Prompt:** `Write an AI policy for an aged care facility.`
**Output:** Generic two-page policy applicable to any industry. No specific use cases, no role-named oversight, no ACQSC references. Governing board: "This could be a policy for a supermarket."
**Lesson learned:** Governance policies must be grounded in specific AI use cases and the specific regulatory framework of the sector to satisfy ACQSC Standard 8.

\---

### v1.1 — Added RACE, organisation details, ACQSC references, use case table, prohibited uses

**Date:** 27 March 2026
**Change:** Added governance advisor role, ACQSC Standard 8 and Privacy Act references, organisation details, use case table, prohibited uses section.
**Output:** More specific and regulatory-grade. However oversight requirements generic ("a staff member must review") — no named roles. No Voluntary AI Safety Standard 2024. No pre-generation risk review. No minimum prohibition count.
**Lesson learned:** "A staff member" is not an accountable person in a governance document. Role-specific oversight and an AI error reporting pathway are what make a policy operationally enforceable rather than decorative.

\---

### v1.2 — Added pre-generation risk review, synthesis risk categorisation, Voluntary AI Safety Standard 2024, minimum prohibitions, role-specific oversight Current

**Date:** 31 March 2026
**Change:** Added pre-generation risk review with \[HIGH RISK] flag. Added categorised risk register input. Added Voluntary AI Safety Standard 2024. Added minimum 5 specific prohibitions. Added role-specific oversight for all 9 use cases.
**Output:** Comprehensive, operationally specific, multi-framework-aligned policy. Correctly flagged P02, P06, P09 as \[HIGH RISK — ADDITIONAL CONTROLS REQUIRED]. Governing board rated policy "implementation-ready" at first review.
**Lesson learned:** Synthesis prompting — integrating and categorising risk data from 9 separate files before drafting one governance document — produces systems-level AI risk thinking. P10 is the capstone of this library: it governs, legitimises, and provides the accountability framework for everything that precedes it.

\---

## A/B Test Results

**Test date:** 31 March 2026 | **Sample:** 1 full policy draft | **Evaluators:** Facility Manager, Clinical Governance Lead, governing board chair

|Criteria|v1.0|v1.2|
|-|-|-|
|Specific to Sunrise Gardens use cases (P01–P09)|0%|100%|
|Role-specific oversight for all use cases|0%|100%|
|Multi-framework alignment (ACQSC + Privacy + AI Safety Standard)|0%|100%|
|\[HIGH RISK] flags accurate|N/A|100%|
|Governing board rated implementation-ready|0%|100%|

\---

## References

* ACQSC. (2019). *Aged Care Quality Standards — Standard 8: Organisational governance*. Australian Government.
* ACQSC. (2024). *Regulatory bulletin: Governance and accountability*. Australian Government.
* ACQSC. (2025). *AI Transparency Statement*. Australian Government.
* Anthropic. (2025). *Prompt engineering overview*. docs.claude.ai
* Australian Government. (2024). *Aged Care Act 2024*.
* Australian Government Department of Industry, Science and Resources. (2024). *Voluntary AI Safety Standard 2024*.
* Health Records Act 2001 (Vic).
* Privacy Act 1988 (Cth). *Australian Privacy Principles.*

\---

## Related Prompts

* **Synthesises risk register from:** P01 through P09
* **Governs the authorised use of:** P01 through P09
* **Library index:** [README.md](../README.md)


