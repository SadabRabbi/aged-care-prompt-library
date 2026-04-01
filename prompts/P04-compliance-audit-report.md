P04 - Compliance Audit Report Generator
Section: Regulatory and Compliance Chain
Workflow Step: 1 of 3
Version: v1.2
Status: Tested and Approved
Last Updated: April 2026
---
01. Prompt Text
```
You are the compliance officer at Sunrise Gardens Aged Care, Bundoora Victoria.

Using ONLY the data below, generate a monthly compliance audit report aligned 
with the Aged Care Quality Standards 2019 (Standards 1 to 8) and SIRS 2021.

REPORTING PERIOD:
- Month: [MONTH AND YEAR]
- Total residents: [NUMBER]
- Total staff (FTE): [NUMBER]

INCIDENT DATA:
[PASTE AGGREGATED INCIDENT SUMMARY - type, count, resolution status, SIRS status.
Source: monthly P02 outputs.]

CARE QUALITY DATA:
[PASTE KEY METRICS - care plans reviewed, medication errors, falls, complaints,
staff training completion rate]

OUTSTANDING ACTIONS FROM PREVIOUS AUDIT:
[LIST UNRESOLVED ITEMS]

Before writing: Assess each Standard 1 to 8 against the data provided.
For each standard ask: Does the data indicate Compliant, Partially Compliant,
or Non-Compliant?

Then write a compliance audit report with:
1. Executive summary (overall compliance status - 3 to 4 sentences)
2. Incident summary (type, count, resolution rate, SIRS filings)
3. Quality Standards performance (rate each Standard 1 to 8 with one sentence
   evidence-based justification per standard)
4. Outstanding actions (unresolved items with updated status)
5. Priority risks and recommended actions (top 3 risks with specific actions)
6. Next audit focus areas

Rules:
- Maximum 500 words
- Formal regulatory language throughout
- Use ONLY data provided - do NOT benchmark against external facilities
- Do NOT rate a standard as Compliant if data shows a gap
- Flag Non-Compliant standards with [NON-COMPLIANT ALERT]
- Flag overdue SIRS submissions with [SIRS OVERDUE]
```
Placeholders:
Placeholder	Source	Example
[MONTH AND YEAR]	Reporting calendar	March 2026
[TOTAL RESIDENTS]	Resident register	62
[TOTAL STAFF FTE]	HR system	48.5
[INCIDENT DATA]	Aggregated P02 outputs	Falls: 8 (6 resolved, 2 open). SIRS Priority 1: 1 filed. Priority 2: 3 filed, 1 overdue.
[CARE QUALITY DATA]	Clinical and HR systems	Care plans reviewed: 14 of 62. Medication errors: 2. Complaints: 3. Training: 78%.
[OUTSTANDING ACTIONS]	Previous audit	Medication management policy review - due February 2026, not completed.
---
02. Intended Workflow or Task
Trigger: First business day of each month, Facility Manager compiles data from all systems
Actor: Facility Manager inputs data, AI pre-assesses Standards 1 to 8 and generates report, Facility Manager and Clinical Governance Lead review, filed with governing board by the 5th
Next step: [NON-COMPLIANT ALERT] triggers P05. [SIRS OVERDUE] triggers P06.
```
Monthly incident and quality data compiled
              |
P04 runs - pre-assessment of Standards 1 to 8
           Compliance report generated
              |
Facility Manager and Clinical Governance Lead review
              |
Filed with governing board by 5th of month
              |
[NON-COMPLIANT ALERT]? Yes - P05
[SIRS OVERDUE]? Yes - P06
```
Prompting strategy: Self-critique pre-assessment combined with constrained output categories and a no-benchmarking constraint. Requiring the AI to assess each Standard before writing prevents optimistic default ratings (Anthropic, 2025). The explicit rule that Compliant cannot be assigned if a gap exists forces honest evidence-based assessment (ACQSC, 2019).
---
03. Problem Being Solved
Generating a monthly compliance report manually requires aggregating data from multiple systems, taking 4 to 6 hours per month. At Facility Manager rate of approximately $58 per hour, this represents approximately $3,480 per year (Fair Work Commission, 2025). Inconsistent monthly reporting is a direct predictor of poor ACQSC inspection outcomes (ACQSC, 2024).
Reduces drafting time by 60% (5 hours to 2 hours)
Ensures all 8 standards are assessed every month without exception
Compliance flags automatically trigger P05 and P06 downstream
---
04. Automation Potential
Level: Medium-High
Dimension	Assessment
Repetitiveness	High - monthly cycle, 12 reports per year
Data availability	Medium - requires manual aggregation from multiple systems
Human judgment needed	High - Facility Manager must validate all 8 standard ratings
Integration possibility	Medium - future integration with RiskMan and Leecare could auto-populate data
Time saving estimate	60% reduction - 5 hours to 2 hours per report
Annual cost saving estimate	Approximately $2,088 in Facility Manager time
Human-in-the-loop: Facility Manager inputs and verifies all monthly data. Reviews all 8 standard ratings and approves recommended actions before presenting to the governing board.
---
05. Risks and Limitations
Risk	Level	Mitigation
Hallucination: AI rates a standard Compliant despite data showing a gap, creating false assurance for the Facility Manager and the ACQSC	High	Self-critique pre-assessment enforced. Explicit no-Compliant-if-gap rule. Facility Manager cross-references ratings against ACQSC compliance guidance.
Bias: Standards related to cultural safety rated lower than evidenced when minority resident data is absent from input	High	Facility Manager specifically reviews Standards 1, 2, and 4 for cultural completeness before filing.
Privacy: Aggregated resident health and staff data submitted to AI system	High	Organisation-hosted private AI only. Data de-identified at resident level where possible.
Governance: Recommended actions exceed Facility Manager's budget authority	Medium	All actions labelled as proposals requiring board approval, not directives.
Over-reliance: Incomplete input data causes AI to generate a report with unnoticed gaps	Medium	Month-end data completeness checklist required before running.
Overall risk rating: High
---
Iteration Log
Version	Change Made	Output	Lesson Learned
v1.0	Basic instruction, no data input, no structure	All 8 standards rated Compliant with no evidence, unusable	Without data input and constrained categories, AI defaults to positive assessments
v1.1	Added Standards 1 to 8 structure, data input fields, constrained rating categories	Correctly structured but AI gave borderline data benefit of the doubt and invented industry benchmarks	Constrained categories alone are insufficient - a self-critique step and no-gap rule are also needed
v1.2	Added self-critique pre-assessment, no-Compliant-if-gap rule, no-benchmarking constraint	Accurate ratings, no invented benchmarks, correctly flagged 1 non-compliant standard and 1 SIRS overdue	Self-critique pre-assessment prevents the statistical tendency toward optimistic outputs
---
A/B Test Results
Test date: 31 March 2026 | Sample: 2 real monthly data sets | Evaluators: Facility Manager and Clinical Governance Lead
Criteria	v1.0	v1.2
All 8 standards assessed with evidence	0%	100%
No over-generous Compliant ratings	0%	100%
No invented external benchmarks	0%	100%
[NON-COMPLIANT ALERT] accurate	N/A	100%
Board-ready after minor edits	0%	100%
---
References
ACQSC. (2019). Aged Care Quality Standards. Australian Government.
ACQSC. (2021). Serious Incident Response Scheme. Australian Government.
ACQSC. (2024). Regulatory bulletin: Governance and accountability. Australian Government.
Anthropic. (2025). Prompt engineering overview. https://docs.claude.ai
Fair Work Commission. (2025). SCHADS Award pay guide. Australian Government.
---
Related Prompts
Receives data from: P02 and P03
Triggers: P05 and P06
Governed by: P10 - AI Risk Governance Policy
Library index: README.md
