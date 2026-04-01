P03 - Shift Handover Summary
Section: Care Documentation Chain
Workflow Step: 3 of 3
Version: v1.2
Status: Tested and Approved
Last Updated: April 2026
---
01. Prompt Text
```
You are the senior Registered Nurse conducting end-of-shift clinical handover 
at Sunrise Gardens Aged Care, Bundoora Victoria.

Using ONLY the information below, produce a structured shift handover summary 
applying the ISBAR framework (Identify, Situation, Background, Assessment, 
Recommendation) to each Priority entry.

SHIFT DETAILS:
- Outgoing shift: [SHIFT]
- Date: [DATE]
- Outgoing senior staff: [NAME AND ROLE]
- Incoming senior staff: [NAME AND ROLE]
- Total residents on floor: [NUMBER]

RESIDENT UPDATES (from P01 and P02 outputs this shift):
[PASTE DOT POINTS - resident name, key observation, any incident, actions taken]

OUTSTANDING TASKS:
[LIST INCOMPLETE TASKS FOR INCOMING TEAM]

Step 1 - Classify each resident into one tier only:
- PRIORITY: [URGENT] from P01 or active incident from P02
- WATCH: new observation, behavioural change, or elevated risk
- STABLE: standard shift, no concerns

Step 2 - Write handover with:
1. Priority residents - immediate attention (ISBAR entry per resident)
2. Watch list - monitor closely (name and specific concern only)
3. Stable residents - brief confirmation only
4. Outstanding tasks (specific, with responsible role and deadline)
5. Medications and clinical alerts (changes, refusals, new alerts)

Rules:
- Maximum 400 words
- Apply ISBAR within each Priority entry
- Do NOT include information not in the shift notes provided
- Flag unresolved SIRS incidents with [SIRS PENDING - notify Facility Manager]
```
Placeholders:
Placeholder	Source	Example
[SHIFT]	Roster system	Morning (0700-1500)
[DATE]	System date	1 April 2026
[OUTGOING STAFF]	Roster system	Sarah Chen, RN
[INCOMING STAFF]	Roster system	David Park, RN
[NUMBER]	Resident register	62
[RESIDENT UPDATES]	P01 summaries and P02 highlights	Margaret T - fall 2:15pm, treated, GP notified. John S - refused meals, confused.
[OUTSTANDING TASKS]	Outgoing RN notes	GP callback for Margaret T expected before 6pm
---
02. Intended Workflow or Task
Trigger: 30 minutes before shift end, RN compiles P01 and P02 outputs from the current shift
Actor: Outgoing RN inputs updates, AI classifies and generates handover, RN confirms classifications before verbal handover
Next step: [SIRS PENDING] flag triggers immediate Facility Manager notification
```
P01 summaries and P02 highlights from this shift
              |
Outgoing RN compiles dot-point updates
              |
P03 runs - Step 1: resident tier classification
           Step 2: structured handover generated
              |
RN confirms Priority classifications
              |
Verbal handover with incoming team
              |
[SIRS PENDING]? Yes - Facility Manager notified immediately
```
Prompting strategy: Chain-of-thought priority tiering combined with prompt chaining and ISBAR integration. Explicit classification before narrative generation ensures the handover is organised around clinical urgency, not documentation order (ACSQHC, 2020). P03 receives P01 and P02 outputs as direct structured inputs, eliminating information re-entry (Anthropic, 2025).
---
03. Problem Being Solved
Communication failures during handover contribute to approximately 24% of adverse events in residential care (ACSQHC, 2020). The outgoing RN spends 40 to 50 minutes preparing handover documentation manually. At RN rate of approximately $42 per hour, three handovers per day represents approximately $34,482 per year in RN handover preparation time (Fair Work Commission, 2025).
Reduces preparation time by 60% (45 minutes to 18 minutes)
Priority tiering allows incoming team to identify urgent residents within 30 seconds
[SIRS PENDING] flag ensures Facility Manager meets SIRS reporting deadlines
---
04. Automation Potential
Level: High
Dimension	Assessment
Repetitiveness	Very high - 3 handovers per day, 1,095 annually
Data availability	High - P01 and P02 outputs from the same shift are the direct inputs
Human judgment needed	High - RN must confirm Priority classifications before verbal handover
Integration possibility	High - future integration could auto-pull P01 and P02 outputs from resident management system
Time saving estimate	60% reduction - 45 minutes to 18 minutes per handover
Annual cost saving estimate	Approximately $20,706 in RN preparation time
Human-in-the-loop: Outgoing RN inputs shift updates. AI classifies and generates handover. RN confirms Priority classifications match clinical judgment and delivers verbal handover.
---
05. Risks and Limitations
Risk	Level	Mitigation
Hallucination: AI misclassifies a deteriorating resident as Stable due to incomplete input notes	High	RN confirms all classifications before verbal handover. Outgoing RN cross-checks resident count against register.
Bias: Non-standard clinical language from ESL carers is systematically under-escalated	High	RN reviews all Watch and Stable entries for potential under-escalation. Facility Manager monitors patterns quarterly.
Privacy: Full shift summary with resident health data processed through AI	High	Organisation-hosted private AI only. No shift data sent to public AI tools.
Governance: [SIRS PENDING] flag omitted, causing Facility Manager to miss SIRS deadline	High	RN cross-checks flag against all P02 outputs from this shift before filing.
Over-reliance: Verbal handover replaced entirely by AI document	Medium	Policy requires verbal handover to remain mandatory. AI document supports but never replaces it.
Overall risk rating: High
---
Iteration Log
Version	Change Made	Output	Lesson Learned
v1.0	Basic instruction, no structure, no resident input	Generic summary, no priorities, invented incidents, unusable	A handover without explicit prioritisation cannot fulfil its clinical purpose
v1.1	Added RACE framework, ISBAR reference, resident input field, five sections	Well-structured but all residents listed at equal priority, incoming RN still had to scan everything	Structure alone does not solve prioritisation - an explicit classification step is required
v1.2	Added chain-of-thought tiering, P01 and P02 chaining, [SIRS PENDING] flag	Prioritised, ISBAR-structured, incoming RN identified urgent residents within 30 seconds	Priority tiering is the most clinically significant upgrade - it reorganises the document around urgency
---
A/B Test Results
Test date: 31 March 2026 | Sample: 3 real shift scenarios | Evaluators: 2 RNs in outgoing and incoming roles (blind review)
Criteria	v1.0	v1.2
Priority classification accurate	0%	100%
ISBAR structure in Priority entries	0%	100%
Grounded in shift notes only	0%	100%
Immediately actionable by incoming team	0%	100%
[SIRS PENDING] flag accurate	N/A	100%
---
References
ACQSC. (2019). Aged Care Quality Standards - Standard 3. Australian Government.
ACQSC. (2021). Serious Incident Response Scheme. Australian Government.
ACSQHC. (2020). ISBAR - Communicating for safety. Australian Government.
Anthropic. (2025). Prompt engineering overview. https://docs.claude.ai
Fair Work Commission. (2025). SCHADS Award pay guide. Australian Government.
---
Related Prompts
Receives input from: P01 and P02
Feeds into: P04
Governed by: P10 - AI Risk Governance Policy
Library index: README.md
