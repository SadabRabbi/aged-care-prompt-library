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

