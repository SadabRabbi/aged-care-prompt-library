# P03 · Shift handover summary

**Section:** 01 — Care documentation chain
**Workflow step:** Step 3 of 3
**Current version:** v1.2
**Status:** Tested
**Last updated:** March 2026

\---

## Prompt Text (v1.2 — current)

```
You are the senior Registered Nurse conducting end-of-shift handover at Sunrise Gardens 
Aged Care, Bundoora Victoria. Apply the ISBAR framework (Identify, Situation, Background, 
Assessment, Recommendation) to each priority resident entry.

SHIFT DETAILS:
- Shift: \[SHIFT — Morning / Afternoon / Night]
- Date: \[DATE]
- Outgoing senior staff: \[NAME AND ROLE]
- Incoming senior staff: \[NAME AND ROLE]
- Total residents on floor: \[NUMBER]

RESIDENT UPDATES FROM THIS SHIFT:
\[PASTE P01 CARE NOTE SUMMARIES AND P02 INCIDENT HIGHLIGHTS — 
one dot point per resident with name, key observation, any incidents, actions taken]

OUTSTANDING TASKS:
\[LIST INCOMPLETE TASKS THE INCOMING TEAM MUST ACTION]

Step 1 — Classify each resident with notable updates into:
- PRIORITY: \[URGENT] flag in P01 or active incident from P02
- WATCH: New observation, behavioural change, or elevated risk
- STABLE: Standard shift, no concerns

Step 2 — Write the handover summary with:
1. Priority residents — immediate attention required
   (ISBAR entry per resident: name, issue, action required, deadline)
2. Watch list — monitor closely (name and specific concern)
3. Stable residents — brief confirmation statement only
4. Outstanding tasks for incoming team (actionable, with role and deadline)
5. Medications and clinical alerts (changes, refused medications, new alerts)

Rules:
- Maximum 400 words
- ISBAR structure within each Priority entry
- Do NOT include information not in the provided notes
- Flag unresolved SIRS reportable incidents with \[SIRS PENDING — notify Facility Manager]
```

**Placeholders to fill:**

|Placeholder|Source|Example|
|-|-|-|
|`\[SHIFT]`|Roster|Morning (0700–1500)|
|`\[DATE]`|System date|1 April 2026|
|`\[OUTGOING STAFF]`|Roster|Sarah Chen, RN|
|`\[INCOMING STAFF]`|Roster|David Park, RN|
|`\[NUMBER]`|Resident register|62|
|`\[RESIDENT UPDATES]`|P01 summaries + P02 highlights|Margaret T — fall 2:15pm, treated, GP notified \[P02]. John S — refused meals, confused \[P01].|
|`\[OUTSTANDING TASKS]`|Outgoing RN notes|GP callback for Margaret T before 6pm. Medication review Room 14 incomplete.|

\---

## Intended Workflow or Task

* **Trigger:** 30 minutes before shift end — RN compiles P01 and P02 outputs from current shift
* **Actor:** Outgoing RN pastes shift updates → AI classifies residents and generates handover → RN reviews before verbal handover meeting
* **Timing:** Completed before incoming shift arrives; used as written basis for verbal handover
* **Next step:** \[SIRS PENDING] triggers immediate Facility Manager notification before RN departs

```
P01 care notes + P02 incident highlights (this shift)
                  ↓
Outgoing RN compiles updates as dot points
                  ↓
\[P03 RUNS — Step 1: Resident tier classification
            Step 2: Structured handover generated]
                  ↓
Outgoing RN reviews and confirms classifications
                  ↓
Verbal handover meeting with incoming team
                  ↓
\[SIRS PENDING]? → Facility Manager notified before RN departs
```

**Prompting technique:** RACE + chain-of-thought priority tiering + prompt chaining. Step 1 forces explicit classification before narrative generation — the most clinically critical feature, as incoming staff must instantly identify urgent residents (ACSQHC, 2020). P03 directly receives P01 and P02 outputs as inputs, creating an integrated three-prompt care documentation chain (Anthropic, 2025).

\---

## Problem Being Solved

Communication failures during handover contribute to \~24% of adverse events in residential care (ACSQHC, 2020). At Sunrise Gardens, the outgoing RN spends \~45 min preparing handover documentation. At 3 handovers/day, 365 days/year, this is **\~821 RN hours annually (\~$34,482/year at $42/hr; Fair Work Commission, 2025).**

**Pain points addressed:**

* Reduces handover preparation by \~60% (45 min → 18 min, saving \~$20,689/year)
* Priority tiering ensures incoming staff identify urgent residents within 30 seconds
* \[SIRS PENDING] flag ensures Facility Manager meets SIRS deadlines before shift changeover

\---

## Automation Potential

**Level: High**

|Dimension|Assessment|
|-|-|
|Repetitiveness|Very high — 1,095 handovers/year at Sunrise Gardens|
|Data availability|High — P01 and P02 outputs are the direct structured inputs|
|Human judgment needed|High — RN must confirm priority classifications before verbal handover|
|Integration possibility|High — future auto-pull from resident management system|
|Estimated time saving|\~60% — 45 min → 18 min (\~$20,689/year)|

**Human-in-the-loop role:** Outgoing RN pastes P01 and P02 summaries. AI classifies and structures the handover. RN confirms Priority classifications match clinical judgment and uses document as basis for verbal handover. Facility Manager alerted to any \[SIRS PENDING] flag before RN leaves the building.

\---

## Risks and Limitations

|Risk|Level|Mitigation|
|-|-|-|
|**Hallucination** — AI misclassifies deteriorating resident as Stable due to incomplete P01 input; adverse event occurs on incoming shift|High|RN mandatory review of all classifications. Outgoing RN cross-checks resident count in output against register. P01 completion audited monthly.|
|**Bias** — AI deprioritises residents whose conditions are described in non-standard English; inequitable care escalation|High|RN reviews Watch and Stable entries for potential under-escalation. Facility Manager monitors escalation patterns by carer cohort quarterly.|
|**Privacy** — Full shift summary with all resident health updates processed through AI|High|Organisation-hosted private AI only. Shift summary classified as clinical health information under Privacy Act 1988 (Cth).|
|**Governance** — \[SIRS PENDING] flag omitted; Facility Manager misses SIRS deadline|High|RN cross-checks \[SIRS PENDING] against all P02 outputs before handover is filed.|
|**Over-reliance** — Verbal handover replaced entirely by AI document; nuanced clinical communication lost|Medium|Policy: AI document supports but never replaces verbal handover. Verbal handover mandatory at Sunrise Gardens.|

**Overall risk rating: HIGH** — must be reviewed by RN before verbal handover. Priority classifications are clinical decisions confirmed by a qualified human.

\---

## Version History

### v1.0 — Initial draft

**Date:** 24 March 2026
**Prompt:** `Write a shift handover for an aged care facility.`
**Output:** Generic summary, no resident specifics, no priority structure, invented incidents. Unusable.
**Lesson learned:** A handover's clinical value is prioritisation. A prompt without explicit priority tiering will not produce it.

\---

### v1.1 — Added RACE, ISBAR reference, resident input field, structured sections

**Date:** 27 March 2026
**Change:** Added RN role, ISBAR reference, resident update input, five structured sections.
**Output:** Well-structured and appropriate tone. However all residents listed at equal priority — no classification. Incoming RN: "I still had to scan everything to find who was urgent." No \[SIRS PENDING] flag.
**Lesson learned:** Structure without prioritisation does not solve the handover problem. Explicit classification tiering — chain-of-thought — was the missing design element.

\---

### v1.2 — Added chain-of-thought priority tiering, P01/P02 chaining, \[SIRS PENDING] flag Current

**Date:** 31 March 2026
**Change:** Added Step 1 classification (Priority/Watch/Stable). Made P01 and P02 outputs explicit inputs. Added \[SIRS PENDING] flag with Facility Manager notification.
**Output:** Prioritised, ISBAR-structured, fully grounded handover. Incoming RN: "I knew who to check within 30 seconds." \[SIRS PENDING] triggered correctly in all test scenarios with unresolved incidents.
**Lesson learned:** Chain-of-thought priority tiering is the most clinically impactful technique in this library. Classification before narration mirrors how an experienced RN mentally organises a handover.

\---

## A/B Test Results

**Test date:** 31 March 2026 | **Sample:** 3 real shift scenarios | **Evaluators:** 2 RNs (blind review)

|Criteria|v1.0|v1.2|
|-|-|-|
|Priority classification accurate|0%|100%|
|ISBAR structure in Priority entries|0%|100%|
|Grounded in shift notes only|0%|100%|
|Immediately actionable by incoming team|0%|100%|
|\[SIRS PENDING] accurate|N/A|100%|

\---

## References

* ACQSC. (2019). *Aged Care Quality Standards — Standard 3*. Australian Government.
* ACQSC. (2021). *Serious Incident Response Scheme (SIRS)*. Australian Government.
* ACSQHC. (2020). *ISBAR — Communicating for safety*. Australian Government.
* Anthropic. (2025). *Prompt engineering overview*. docs.claude.ai
* Fair Work Commission. (2025). *SCHADS Award pay guide*.

\---

## Related Prompts

* **Receives input from:** P01 — Daily care note generator
* **Receives input from:** P02 — Incident report drafter
* **Feeds into:** P04 — Compliance audit report (monthly)
* **Governed by:** P10 — AI risk governance policy
* **Library index:** [README.md](../README.md)

