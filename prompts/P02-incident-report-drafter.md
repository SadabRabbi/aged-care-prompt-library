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

