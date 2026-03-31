Aged Care Prompt Library

Assessment 1 — BUS4005: Artificial Intelligence in Business
Student: [Your Name] | Student ID: [Your ID]
Business Field: Residential Aged Care — Victoria, Australia
Organisation: Sunrise Gardens Aged Care, Bundoora VIC (62 beds, 48 staff)
AI Model Tested On: Claude Sonnet 4 (private Azure OpenAI deployment)
Last Updated: April 2026


What This Library Does
This prompt library supports workflow automation for the Facility Manager of a residential aged care facility in Victoria, Australia. It contains 10 documented, tested, and iterated prompts organised across three operational workflow chains.
Each prompt directly addresses an operational pain point in aged care administration — reducing manual documentation time, strengthening regulatory compliance, and freeing staff to focus on direct resident care.
The library was designed in response to the critical workforce and compliance pressures identified by the Royal Commission into Aged Care Quality and Safety (2021) and aligns with the requirements of the Aged Care Quality Standards 2019, the Aged Care Act 2024, and the Australian Government's Voluntary AI Safety Standard 2024.

Folder Structure
aged-care-prompt-library/
│
├── README.md                                    ← You are here
│
└── prompts/
    ├── P01-daily-care-note-generator.md
    ├── P02-incident-report-drafter.md
    ├── P03-shift-handover-summary.md
    ├── P04-compliance-audit-report.md
    ├── P05-quality-standards-self-assessment.md
    ├── P06-regulatory-submission-draft.md
    ├── P07-resident-intake-summary.md
    ├── P08-family-update-letter.md
    ├── P09-staff-performance-review.md
    └── P10-ai-governance-policy.md

Library Summary Table
IDPrompt NameWorkflow ChainAutomation LevelRisk LevelStatusP01Daily care note generatorCare documentationHighMedium✅ TestedP02Incident report drafterCare documentationHighHigh✅ TestedP03Shift handover summaryCare documentationHighHigh✅ TestedP04Compliance audit reportRegulatory & complianceMedium-HighHigh✅ TestedP05Quality standards self-assessmentRegulatory & complianceMediumHigh✅ TestedP06Regulatory submission draftRegulatory & complianceMediumHigh✅ TestedP07Resident intake summaryResident administrationHighHigh✅ TestedP08Family update letterResident administrationHighMedium✅ TestedP09Staff performance reviewResident administrationMediumHigh✅ TestedP10AI risk governance policyGovernance (standalone)MediumHigh✅ Tested

🔗 Prompt Chaining Map
Prompts in this library are designed to work in sequence. Outputs from earlier prompts feed directly into later ones — eliminating information re-entry and creating an integrated workflow automation system.
CHAIN 1 — Care Documentation
─────────────────────────────────────────────────────────────────
P01 (Care note) ──[URGENT flag]──► P02 (Incident report)
                                        │
P01 + P02 outputs ──────────────────► P03 (Shift handover)
                                        │
P03 records ────────────────────────► P04 (Monthly compliance audit)


CHAIN 2 — Regulatory and Compliance
─────────────────────────────────────────────────────────────────
P04 ──[NON-COMPLIANT ALERT]──────► P05 (Quality self-assessment)
P04 ──[SIRS OVERDUE]─────────────► P06 (Regulatory submission)
P02 ──[MANDATORY REPORT]─────────► P06 (Regulatory submission)
P05 action plan ─────────────────► P06 (improvement plan input)


CHAIN 3 — Resident Administration
─────────────────────────────────────────────────────────────────
P07 (Intake summary) ────────────► P08 (Family welcome letter)
P01 monthly notes ───────────────► P08 (Monthly family update)
P02 incident report ─────────────► P08 (Post-incident letter)
P01 + P02 data ──────────────────► P09 (Performance review)


GOVERNANCE
─────────────────────────────────────────────────────────────────
P01–P09 risk registers ──────────► P10 (AI governance policy)
P10 governs authorised use of ───► P01–P09 (all prompts)

Prompting Strategies Used
StrategyApplied InWhy ChosenRACE framework (Role–Action–Context–Evaluation)All prompts (P01–P10)Ensures consistent professional register and structured outputs across all staff and use casesGrounding constraint ("ONLY the observations provided")P01, P02, P03, P07Prevents hallucination in clinical and legal records — critical for aged care documentationChain-of-thought decomposition (Step 1 → Step 2)P02, P03, P04Makes regulatory classification and priority tiering explicit and auditable before narrative generationSelf-critique pre-assessmentP04, P06Forces evidence-based reasoning before output — prevents optimistic defaults in compliance ratings and liability admissionsSMART output constraints (5-component goal structure)P05, P09Enforces SMART criteria at structural level rather than as a general instructionConditional tone instructionP08Applies different communication registers (welcome / monthly / post-incident / care plan) from one promptPre-generation anti-bias screeningP06, P09Screens input for liability language (P06) and protected attributes (P09) before draftingFixed-count constraintP07Forces prioritisation — "exactly 3 watch points" produces a more actionable clinical brief than an open-ended listSynthesis prompting with risk categorisationP10Integrates risk registers from 9 prompts into a single categorised governance frameworkPerson-first language constraintP07, P08Enforces rights-based communication required by the Aged Care Act 2024 and Standard 1

Iteration Evidence Summary
All prompts were developed through a minimum of 3 documented versions. Full version histories, observed effects, and lessons learned are recorded in each individual prompt file.
PromptVersionsKey ImprovementP01v1.0 → v1.2Added grounding constraint — eliminated hallucination in clinical notesP02v1.0 → v1.2Added chain-of-thought SIRS classification — made regulatory decision auditableP03v1.0 → v1.2Added priority tiering — incoming team identifies urgent residents in 30 secondsP04v1.0 → v1.2Added self-critique pre-assessment — eliminated over-generous compliance ratingsP05v1.0 → v1.2Added 5-component SMART goals — vague commitments replaced with measurable actionsP06v1.0 → v1.2Added pre-generation legal screening — eliminated inadvertent liability admissionsP07v1.0 → v1.2Added fixed-count 48-hour watch points — summary converted from information to actionable briefP08v1.0 → v1.2Added non-minimisation legal constraint citing Standard 6 — post-incident transparency ensuredP09v1.0 → v1.2Added pre-generation anti-bias screening — protected attribute references eliminatedP10v1.0 → v1.2Added synthesis risk categorisation — 9 risk registers integrated into one governance framework

Estimated Business Value
Workflow ChainAnnual Time SavedAnnual Cost Saved (est.)Care documentation (P01–P03)~4,471 hours~$130,600Regulatory and compliance (P04–P06)~131 hours~$7,772Resident administration (P07–P09)~167 hours~$6,154Governance (P10)~5.5 hours~$7,540 (vs consultant)Total~4,775 hours/year~$152,066/year

Calculated using SCHADS Award rates: PCW Level 3 ~$27.50/hr, Care Coordinator ~$35/hr, RN ~$42/hr, Team Leader ~$38/hr, Facility Manager ~$58/hr (Fair Work Commission, 2025).


Risk Overview
All prompts in this library have been assessed across five risk categories. Full risk tables with specific mitigations are documented in each prompt file.
Risk CategoryPrompts AffectedPrimary MitigationHallucinationP01, P02, P06, P07, P09Grounding constraints; mandatory human review before any output is saved or filedBiasP02, P03, P08, P09Pre-generation screening; person-first language constraints; reviewer cultural checksPrivacy and securityP01–P09 (all)Organisation-hosted private AI (Azure OpenAI, Australian data residency); no public AI toolsGovernanceP02, P04, P06, P09Named human oversight roles; P10 governance policy; audit trails in resident management systemOver-relianceP01, P02, P03Mandatory input requirements; spot-check audit policies; verbal handover remains mandatory
Governance framework: All prompts operate under the AI Risk Governance Policy (P10), which must be approved by the governing board before any prompt is deployed in clinical or administrative practice.

Key References

Aged Care Quality and Safety Commission. (2019). Aged Care Quality Standards. Australian Government.
Aged Care Quality and Safety Commission. (2021). Serious Incident Response Scheme (SIRS). Australian Government.
Anthropic. (2025). Prompt engineering overview. https://docs.claude.ai
Australian Commission on Safety and Quality in Health Care. (2020). ISBAR — Communicating for safety. Australian Government.
Australian Government. (2024). Aged Care Act 2024; Voluntary AI Safety Standard 2024.
Australian Human Rights Commission. (2023). Employer guide to anti-discrimination law. Australian Government.
Fair Work Commission. (2025). SCHADS Award pay guide. Australian Government.
Royal Commission into Aged Care Quality and Safety. (2021). Final report: Care, dignity and respect (Vol. 1). Australian Government.
