# P05 · Quality standards self-assessment

**Section:** 02 — Regulatory and compliance chain
**Workflow step:** Step 2 of 3
**Current version:** v1.2
**Status:**Tested
**Last updated:** March 2026

\---

## Prompt Text (v1.2 — current)

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

## Intended Workflow or Task

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

## Problem Being Solved

Manual self-assessment preparation takes \~**3 hours/Standard** (32 assessments/year = **96 hours = \~$5,568/year** at Facility Manager rate $58/hr). Providers unable to produce current, evidence-based assessments risk formal non-compliance notices publicly reported on ACQSC website (ACQSC, 2023).

**Pain points addressed:**

* Reduces preparation by \~55% (3 hrs → 80 min, saving \~$3,074/year)
* Enforces SMART actions consistently across all 32 annual assessments
* Produces inspection-ready documentation on demand

\---

## Automation Potential

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

## Risks and Limitations

|Risk|Level|Mitigation|
|-|-|-|
|**Hallucination** — AI rates compliance higher than evidence supports; Facility Manager presents false assurance to ACQSC inspector|High|Rule: "Do NOT rate compliance higher than evidence supports." Clinical Governance Lead independently reviews each rating. Cross-referenced against ACQSC self-assessment guide.|
|**Bias** — AI generates improvement actions assuming resourcing not available at Sunrise Gardens; unachievable action plan damages credibility with ACQSC|High|Facility Manager reviews all actions for resource feasibility. Unrealistic timelines adjusted before approval.|
|**Privacy** — Evidence input may include identifiable resident complaint and staff training data|High|Data de-identified at individual level before prompting. Organisation-hosted AI only.|
|**Governance** — Self-assessment becomes documentation exercise without genuine improvement follow-through|Medium|Clinical Governance Lead audits action completion monthly. Facility Manager reports progress to governing board quarterly.|
|**Over-reliance** — Incomplete evidence input produces superficial assessment; gaps missed during ACQSC inspection|High|Evidence completeness checklist per Standard before prompting. Facility Manager certifies completeness.|

**Overall risk rating: HIGH** — all compliance ratings and action plans must be reviewed by the Clinical Governance Lead before filing.

\---

## Version History

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

