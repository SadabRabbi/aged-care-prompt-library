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
* Australian Government. (2024). *Aged Care Act 2024 — Open disclosure obligations*.
* Fair Work Commission. (2025). *SCHADS Award pay guide*.

\---

## Related Prompts

* **Receives input from:** P07 — Resident intake summary; P01 — Daily care note; P02 — Incident report
* **Governed by:** P10 — AI risk governance policy
* **Library index:** [README.md](../README.md)

