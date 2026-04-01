P01 - Daily Care Note Generator
Section: Care Documentation Chain
Workflow Step: 1 of 3
Version: v1.2
Status: Tested and Approved
Last Updated: April 2026
---
01. Prompt Text
```
You are a qualified aged care worker at Sunrise Gardens Aged Care, Bundoora Victoria.

Write a professional daily care note for the following resident:

- Resident name: [RESIDENT_NAME]
- Age: [AGE]
- Primary conditions: [CONDITIONS]
- Shift: [SHIFT - Morning / Afternoon / Night]
- Date: [DATE]

Shift observations:
[PASTE CARER RAW DOT-POINT NOTES HERE]

Using ONLY the observations above, write a structured care note with:
1. Meals and fluid intake
2. Mobility and physical activity
3. Mood and emotional wellbeing
4. Personal care completed
5. Health observations and concerns

Rules:
- Maximum 250 words
- Clinical but compassionate language
- Do NOT add information not in the observations
- Do NOT include diagnoses or medical recommendations
- Flag urgent concerns with [URGENT] at the start of that section
```
Placeholders:
Placeholder	Source	Example
[RESIDENT_NAME]	Resident management system	Margaret Thompson
[AGE]	Resident profile	84
[CONDITIONS]	Care plan	Moderate dementia, Type 2 diabetes
[SHIFT]	Roster system	Morning
[DATE]	System date	1 April 2026
[RAW NOTES]	Carer dot-point observations	Ate 80% breakfast, refused lunch, confused at 10am
---
02. Intended Workflow or Task
Trigger: End of shift when carer completes rounds
Actor: Carer inputs observations, AI generates note, RN reviews and approves before saving
Next step: [URGENT] flag triggers P02. No flag feeds into P03.
```
Carer enters raw observations
           |
    P01 runs - care note generated
           |
    RN reviews and approves
           |
    Saved to resident management system
           |
[URGENT]? Yes - P02 / No - P03
```
Prompting strategy: RACE framework combined with a grounding constraint and structured output decomposition. RACE ensures consistent clinical register. The grounding constraint prevents hallucination in a legal clinical record (Anthropic, 2025). Five-section decomposition standardises output structure across all 48 staff (ACQSC, 2019).
---
03. Problem Being Solved
Sunrise Gardens generates approximately 186 care notes daily across 62 residents and 3 shifts. At 5 minutes per note manually, this equals 15.5 hours of carer time per day. At the SCHADS Award PCW Level 3 rate of approximately $27.50 per hour, this represents an estimated annual cost of $155,775 (Fair Work Commission, 2025). The Royal Commission into Aged Care (2021) identified inconsistent documentation as a systemic contributor to poor care outcomes.
Reduces per-note drafting time by 70% (5 minutes to 90 seconds)
Standardises structure across all carers and shifts
[URGENT] flag creates a consistent escalation path to P02
---
04. Automation Potential
Level: High
Dimension	Assessment
Repetitiveness	Very high - 186 notes per day, 365 days per year
Data availability	High - carers already record dot-point observations during rounds
Human judgment needed	Medium - RN review required before saving to clinical record
Integration possibility	High - compatible with Leecare, Civica, and Alis via API
Time saving estimate	70% reduction - 5 minutes to 90 seconds per note
Annual cost saving estimate	Approximately $109,400 in PCW documentation labour
Human-in-the-loop: Carer inputs observations. AI formats the note. RN reviews clinical accuracy and approves before saving. Facility Manager retains full legal accountability for all records filed under Sunrise Gardens' RACS ID.
---
05. Risks and Limitations
Risk	Level	Mitigation
Hallucination: AI fabricates observations, creating a falsified clinical record in breach of the Aged Care Act 2024	High	Grounding constraint enforced. Mandatory RN review before saving. Monthly audit log by Facility Manager.
Bias: Culturally inappropriate language for non-English-speaking residents, violating Standard 1 (ACQSC, 2019)	High	RN reviews cultural appropriateness. Future: add [CULTURAL_NOTES] placeholder.
Privacy: Resident PII sent to external AI, creating liability under the Privacy Act 1988 (Cth)	High	Organisation-hosted private AI only. Vendor data processing agreement required.
Governance: No audit trail distinguishing AI-generated from human-written notes	Medium	AI-generated notes tagged in resident management system. Disclosed in P10.
Over-reliance: Carer skips writing raw observations and runs prompt from memory	Medium	Policy requires raw observations before running. Quarterly audits by Facility Manager.
Overall risk rating: Medium
---
Iteration Log
Version	Change Made	Output	Lesson Learned
v1.0	Basic instruction, no role, no structure, no grounding	Invented observations, 380 words, unusable	Vague prompts generate fabricated clinical content
v1.1	Added RACE framework, placeholder fields, five-section structure	Correct structure but AI still invented content where no observation was provided	RACE improves structure but does not prevent hallucination
v1.2	Added grounding constraint, 250-word limit, [URGENT] flag, exclusion clauses	Zero fabrication across 5 test runs, 218 words average, 80% ready to save	Grounding constraint is the most critical safety feature for clinical documentation
---
A/B Test Results
Test date: 31 March 2026 | Sample: 5 shift scenarios | Evaluators: 2 RNs (blind review)
Criteria	v1.0	v1.2
Grounded in observations only	0%	100%
Correct five-section structure	20%	100%
Clinical tone appropriate	40%	100%
Within 250-word limit	0%	100%
Ready to save without edits	0%	80%
[URGENT] flag accurate	N/A	100%
---
References
ACQSC. (2019). Aged Care Quality Standards. Australian Government.
Anthropic. (2025). Prompt engineering overview. https://docs.claude.ai
Fair Work Commission. (2025). SCHADS Award pay guide. Australian Government.
Royal Commission into Aged Care Quality and Safety. (2021). Final report (Vol. 1). Australian Government.
---
Related Prompts
Next (urgent): P02 - Incident Report Drafter
Next (standard): P03 - Shift Handover Summary
Governed by: P10 - AI Risk Governance Policy
Library index: README.md
