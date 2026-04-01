P05 - Quality Standards Self-Assessment
Section: Regulatory and Compliance Chain
Workflow Step: 2 of 3
Version: v1.2
Status: Tested and Approved
Last Updated: April 2026
---
01. Prompt Text
```
You are the quality assurance specialist at Sunrise Gardens Aged Care,
Bundoora Victoria.

Using ONLY the information below, conduct an evidence-based self-assessment
against the Aged Care Quality Standard identified. Align with the ACQSC
self-assessment methodology (ACQSC, 2023).

STANDARD BEING ASSESSED:
- Standard: [STANDARD NUMBER AND NAME]
- Trigger: [REASON - e.g. flagged Non-Compliant in monthly audit, quarterly review]
- Assessment period: [DATE RANGE]

EVIDENCE PROVIDED:
[PASTE ALL AVAILABLE EVIDENCE - care plan data, incident records, training logs,
resident feedback, complaint records, policy documents reviewed]

KNOWN GAPS:
[LIST GAPS, COMPLAINTS, OR INCIDENTS RELEVANT TO THIS STANDARD - from P04]

Write a self-assessment with:
1. Standard overview (one sentence - what it requires)
2. Compliance rating: [Compliant / Partially Compliant / Non-Compliant]
   Justify with specific data points from the evidence provided (3 to 4 sentences)
3. Evidence of compliance (what is working)
4. Identified gaps (what is missing - be direct)
5. Improvement action plan - for each action include:
   - Specific measurable description
   - Responsible role (named)
   - Target completion date
   - Quality Standard reference
   (minimum 3 actions, maximum 5)
6. Risk to residents if gaps are not addressed (plain language)

Rules:
- Maximum 450 words
- Cite specific data points in compliance rating justification
- Do NOT rate compliance higher than the evidence supports
- All improvement actions must be SMART
- Flag overdue actions from previous assessments with [OVERDUE]
- Flag High resident safety risks with [RESIDENT SAFETY RISK]
```
Placeholders:
Placeholder	Source	Example
[STANDARD]	ACQSC Quality Standards	Standard 3 - Care and Services
[REASON]	P04 audit trigger	Flagged Partially Compliant in March 2026 monthly audit
[DATE RANGE]	Assessment period	1 January to 31 March 2026
[EVIDENCE]	Clinical records, training logs, surveys	38 of 62 care plans reviewed. 2 medication errors. Resident satisfaction 74%. 12 staff completed dementia training.
[KNOWN GAPS]	P04 findings	24 care plans overdue. 2 unresolved medication complaints.
---
02. Intended Workflow or Task
Trigger: [NON-COMPLIANT ALERT] from P04, scheduled quarterly review, or ACQSC inspection notification
Actor: Facility Manager gathers evidence, AI drafts self-assessment, Clinical Governance Lead reviews and approves action plan
Timing: Within 5 business days of a non-compliant flag
```
[NON-COMPLIANT ALERT] from P04 or quarterly schedule
              |
Facility Manager gathers evidence for flagged standard
              |
P05 runs - self-assessment generated
              |
Clinical Governance Lead reviews and approves action plan
              |
Actions assigned with named responsible roles
              |
Findings documented for ACQSC inspection readiness
```
Prompting strategy: SMART output constraints combined with an evidence citation requirement and resident safety reframing. Five named action components enforce SMART criteria at the prompt level, preventing vague improvement language that ACQSC inspectors flag as inadequate (ACQSC, 2023). The risk to residents section reframes compliance gaps in human terms, producing more decisive governance responses (Anthropic, 2025).
---
03. Problem Being Solved
Conducting a rigorous self-assessment manually takes approximately 3 hours per standard (ACQSC, 2023). With 8 standards assessed quarterly, this represents 96 hours per year. At Facility Manager rate of approximately $58 per hour, this represents approximately $5,568 per year (Fair Work Commission, 2025).
Reduces assessment time by 55% (3 hours to 80 minutes per standard)
SMART actions aligned to Quality Standards strengthen Standard 7 evidence
Creates inspection-ready documentation available on demand
---
04. Automation Potential
Level: Medium
Dimension	Assessment
Repetitiveness	Medium - 32 assessments per year (8 standards multiplied by 4 quarters)
Data availability	Medium - evidence gathered from multiple systems before prompting
Human judgment needed	High - compliance rating confirmed by Clinical Governance Lead
Integration possibility	Low to Medium - partial integration with quality management systems possible
Time saving estimate	55% reduction - 3 hours to 80 minutes per standard
Annual cost saving estimate	Approximately $3,074 in Facility Manager time
Human-in-the-loop: Facility Manager gathers and certifies evidence completeness. Reviews compliance rating and challenges any overstatement. Clinical Governance Lead approves action plan and assigns owners.
---
05. Risks and Limitations
Risk	Level	Mitigation
Hallucination: AI rates compliance higher than evidence supports, providing false assurance to the ACQSC	High	Explicit rule that compliance cannot be rated higher than evidence supports. Clinical Governance Lead independently reviews rating before filing.
Bias: AI generates SMART actions assuming staffing or funding levels unavailable at Sunrise Gardens	High	Facility Manager reviews all actions for resource feasibility before approval.
Privacy: Evidence input may include identifiable resident complaint data and staff training records	High	Evidence de-identified at individual level before prompting. Organisation-hosted AI only.
Governance: Self-assessment becomes a documentation exercise without genuine improvement follow-through	Medium	Clinical Governance Lead audits action completion monthly. Facility Manager reports progress to governing board quarterly.
Over-reliance: Incomplete evidence produces a superficial assessment and genuine gaps are missed	High	Evidence completeness checklist required. Facility Manager certifies completeness before running.
Overall risk rating: High
---
Iteration Log
Version	Change Made	Output	Lesson Learned
v1.0	Basic instruction, no evidence input, no rating constraints	All standards rated Compliant with no evidence, unusable	Without evidence input, AI defaults to optimistic assessments
v1.1	Added specific standard field, evidence input, ACQSC reference, rating constraints	Evidence-grounded but improvement actions vague with no SMART criteria or Standards references	SMART criteria must be enforced at the structural level, not requested in general
v1.2	Added five-component SMART requirement, resident safety reframing, [OVERDUE] and [RESIDENT SAFETY RISK] flags	Specific, inspection-ready assessment with measurable actions and named accountability	Resident safety reframing creates urgency that abstract compliance ratings do not
---
A/B Test Results
Test date: 31 March 2026 | Sample: 2 standard assessments (Standards 3 and 7) | Evaluators: Facility Manager and Clinical Governance Lead
Criteria	v1.0	v1.2
Evidence-grounded compliance rating	0%	100%
SMART actions produced	0%	100%
Named responsible roles in all actions	0%	100%
Quality Standard referenced in each action	0%	100%
[RESIDENT SAFETY RISK] flag accurate	N/A	100%
---
References
ACQSC. (2019). Aged Care Quality Standards. Australian Government.
ACQSC. (2023). Self-assessment guide for residential aged care. Australian Government.
Anthropic. (2025). Prompt engineering overview. https://docs.claude.ai
Fair Work Commission. (2025). SCHADS Award pay guide. Australian Government.
Royal Commission into Aged Care Quality and Safety. (2021). Final report (Vol. 1). Australian Government.
---
Related Prompts
Triggered by: P04 ([NON-COMPLIANT ALERT])
Feeds into: P06
Governed by: P10 - AI Risk Governance Policy
Library index: README.md
