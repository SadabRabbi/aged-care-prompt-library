P02 - Incident Report Drafter
Section: Care Documentation Chain
Workflow Step: 2 of 3
Version: v1.2
Status: Tested and Approved
Last Updated: April 2026
---
01. Prompt Text
```
You are a compliance officer at Sunrise Gardens Aged Care, Bundoora Victoria.

A reportable incident has occurred. Using ONLY the information below, draft a 
formal incident report aligned with the Serious Incident Response Scheme (SIRS) 
2021 and Standard 8 of the Aged Care Quality Standards.

INCIDENT DETAILS:
- Resident name: [RESIDENT_NAME]
- Age: [AGE]
- Date and time: [DATE_TIME]
- Location: [LOCATION]
- Incident type: [INCIDENT_TYPE]
- Staff present: [STAFF_NAMES AND ROLES]
- Witnesses: [WITNESS_NAMES or None]

WITNESS ACCOUNT:
[PASTE EXACT STAFF OBSERVATIONS - dot points are fine]

IMMEDIATE ACTIONS TAKEN:
[LIST ALL COMPLETED ACTIONS WITH TIMES]

Step 1 - Classify the incident:
SIRS STATUS: [Priority 1 / Priority 2 / Below threshold]
REASON: [One sentence based on SIRS criteria]

Step 2 - Write the incident report with:
1. Incident summary (factual, chronological - 3 to 4 sentences)
2. Immediate response (actions taken)
3. Persons notified (who and when)
4. Follow-up actions required
5. Risk assessment (Low / Medium / High recurrence - one sentence)
6. Reporting obligations (SIRS status and submission deadline)

Rules:
- Maximum 350 words excluding Step 1
- Formal, factual, non-judgmental language
- Do NOT assign blame to any individual
- Do NOT assume beyond information provided
- Do NOT include diagnoses or clinical recommendations
- Flag mandatory SIRS reporting with [MANDATORY REPORT - Priority 1: 24 hrs / Priority 2: 30 days]
```
Placeholders:
Placeholder	Source	Example
[RESIDENT_NAME]	Resident management system	Margaret Thompson
[AGE]	Resident profile	84
[DATE_TIME]	Incident log	1 April 2026, 2:15 PM
[LOCATION]	Staff account	Bathroom, Wing B
[INCIDENT_TYPE]	Classification list	Unwitnessed fall with skin tear
[STAFF_NAMES AND ROLES]	Roster system	Jane Nguyen (PCW), David Park (RN)
[WITNESS ACCOUNT]	Staff notes	Found on floor, alert, small laceration left forearm
[IMMEDIATE ACTIONS]	Incident log	First aid 2:18pm, RN notified 2:18pm, family 2:35pm, GP 3:00pm
---
02. Intended Workflow or Task
Trigger: [URGENT] flag from P01 or direct staff escalation
Actor: RN inputs details, AI classifies and drafts report, Facility Manager reviews and approves before filing
Next step: [MANDATORY REPORT] triggers P06. Incident data aggregated into P04 at month end.
```
[URGENT] from P01 or staff escalation
              |
Staff inputs incident details and witness account
              |
P02 runs - Step 1: SIRS classification
           Step 2: Incident report drafted
              |
Facility Manager reviews, approves, and signs
              |
Filed in RiskMan
              |
[MANDATORY REPORT]? Yes - P06 / No - internal file into P04
```
Prompting strategy: Chain-of-thought decomposition separates SIRS classification (Step 1) from report drafting (Step 2). This makes the regulatory decision auditable before the narrative is written, reducing misclassification risk under a 24-hour deadline (ACQSC, 2021). Embedding SIRS 2021 directly ensures structural compliance (Anthropic, 2025).
---
03. Problem Being Solved
Sunrise Gardens generates approximately 200 incident reports per year. Manual drafting takes 45 to 60 minutes per report. At RN rate of approximately $42 per hour, this represents approximately $7,000 per year in RN documentation time (Fair Work Commission, 2025). Missed SIRS Priority 1 deadlines constitute a regulatory breach under the Aged Care Act 2024.
Reduces drafting time by 65% (50 minutes to 18 minutes)
Automates SIRS classification with an audit trail
[MANDATORY REPORT] flag prevents missed 24-hour deadlines
---
04. Automation Potential
Level: High
Dimension	Assessment
Repetitiveness	High - approximately 200 reports per year
Data availability	High - staff already record witness accounts at time of incident
Human judgment needed	High - Facility Manager must confirm SIRS classification before filing
Integration possibility	Medium - SIRS STATUS field parseable by RiskMan
Time saving estimate	65% reduction - 50 minutes to 18 minutes per report
Annual cost saving estimate	Approximately $4,494 in RN documentation time
Human-in-the-loop: RN inputs details. AI classifies and structures the report. Facility Manager reviews SIRS classification, approves narrative, and personally submits via My Aged Care portal.
---
05. Risks and Limitations
Risk	Level	Mitigation
Hallucination: AI fabricates witness account, creating a legally invalid record under the Aged Care Act 2024	High	Grounding constraint enforced. Facility Manager verifies narrative against original staff notes before approving.
Bias: AI language implicitly attributes fault to a staff member, creating a discrimination risk under the Fair Work Act 2009 (Cth)	High	Explicit no-blame rule in prompt. Legal review of template language before deployment.
Privacy: Incident details and staff names processed through AI, creating liability under the Privacy Act 1988 (Cth)	High	Organisation-hosted private AI only. Data minimisation applied.
Governance: AI misclassifies Priority 1 as Priority 2, causing the Facility Manager to miss the 24-hour SIRS deadline	High	Facility Manager cross-checks Step 1 against ACQSC SIRS decision tool. Human makes final reporting decision.
Over-reliance: Staff submit incomplete witness accounts trusting AI to fill gaps	Medium	Full witness account required before running. Facility Manager audits input quality quarterly.
Overall risk rating: High
---
Iteration Log
Version	Change Made	Output	Lesson Learned
v1.0	Basic instruction, no structure, no input fields	Fictional narrative with invented names and injury details, unusable	Legal documents require legislative grounding and structured input fields
v1.1	Added RACE framework, SIRS reference, six-section structure, input fields	Correct format but AI invented witness narrative as no input field was provided	Witness account input is non-negotiable. SIRS classification must be an explicit step, not inferred
v1.2	Added chain-of-thought decomposition, witness input field, [MANDATORY REPORT] flag	SIRS-compliant, zero fabrication, correct Priority classification in all 4 test scenarios	Chain-of-thought decomposition makes the regulatory decision auditable before the narrative is written
---
A/B Test Results
Test date: 31 March 2026 | Sample: 4 real incident scenarios | Evaluators: Facility Manager and RN (blind review)
Criteria	v1.0	v1.2
Grounded in provided observations only	0%	100%
SIRS classification explicit and accurate	0%	100%
Non-judgmental language throughout	30%	100%
[MANDATORY REPORT] flag accurate	0%	100%
Ready to file after minor review	0%	75%
---
References
ACQSC. (2019). Aged Care Quality Standards - Standard 8. Australian Government.
ACQSC. (2021). Serious Incident Response Scheme. Australian Government.
Anthropic. (2025). Prompt engineering overview. https://docs.claude.ai
Australian Government. (2024). Aged Care Act 2024.
Fair Work Commission. (2025). SCHADS Award pay guide. Australian Government.
---
Related Prompts
Triggered by: P01 - [URGENT] flag
Feeds into: P03 and P04
Triggers: P06 - [MANDATORY REPORT] flag
Governed by: P10 - AI Risk Governance Policy
Library index: README.md
