# PRD: Spectrum Agentic Platform

*Date: 2026-04-22*
*Status: Final Draft — Ready for Review*
*Author: Product — Digital Service & Customer Experience*

---

## 1. Problem Statement

The Spectrum customer contacting digital service — across IVR, chat, and app — is being handled by a platform that resolves the stated issue but misses the actual opportunity: every inbound contact is a signal-rich moment containing account state, service history, propensity data, and behavioral context that the current platform discards the moment the interaction ends. The result is a system that closes tickets but doesn't prevent the next one.

Three compounding failures define the current state. First, the platform is reactive by architecture — it waits for a customer to initiate contact, respond to a prompt, and explicitly state a need before it acts. Proactive intervention, where the system surfaces a credit, an offer, or a resolution before the customer asks, is not a feature — it is structurally absent. Second, the highest-value contact type — retention, at 50M contacts annually and $500+ per saved customer — is handled without a confidence architecture sufficient for irreversible decisions at scale. A miscalibrated model offering the wrong retention incentive to a customer who wasn't actually leaving is not a tuning problem. At 50M contacts, it is an exposure problem. Third, the 60% repeat contact rate — 150M redundant contacts annually against a 250M contact base — is the most direct evidence that the current platform is resolving contacts, not customer issues. Each 1% improvement in First Contact Resolution eliminates 2.5M redundant contacts at fully-loaded cost.

The moment to act is now: the Aura agentic platform has established the context retrieval, tool orchestration, and confidence threshold architecture that NBA requires. The reactive baseline (80%+ self-service resolution, <20% repeat contact rate, ~5 min AHT) is established and measurable. NBA is the next architectural layer — the move from resolving what customers ask to anticipating what they need.

---

## 2. User Persona

**Primary: Spectrum Residential Customer — Repeat Contact Segment**

| Attribute | Detail |
|---|---|
| Tenure | 5–8 years; broadband anchor with TV and/or mobile bundle |
| Contact frequency | 3–5 contacts in prior 12 months — billing, service, retention-adjacent |
| Primary channel | IVR first; switches to chat if queue >5 min |
| Tech comfort | Moderate — uses My Spectrum app for payment only |
| Household profile | Homeowner, 35–50, one or more Spectrum Mobile lines added in prior 24 months |
| Risk profile | Medium-high churn propensity — has engaged competitor offers but not acted; promotional rate expiration in next 90 days |
| Core constraint | Does not distinguish between contact types — every interaction is "dealing with Spectrum." A billing call that surfaces a retention offer feels intrusive if trust hasn't been established. A service call that proactively applies a credit before asked feels exceptional. The difference is sequencing and confidence. |

**Goals:** Resolve the immediate issue without repeating himself. Trust that the platform knows his account before the conversation starts. Not be transferred. Receive value — credit, offer, resolution — without having to negotiate for it.

**What he fears:** Being asked to verify his account for the fifth time this year. Receiving an offer that makes him realize he was already eligible for it six months ago. Being transferred to retention and told there's nothing available.

---

## 3. Jobs to Be Done

**Functional job:**
Get the immediate issue resolved — billing discrepancy, service disruption, equipment question — in one contact, without transfer, and ideally with something unexpected that makes the interaction worth remembering.

**Emotional job:**
Feel known by a company that has his account data, contact history, and service record — and uses it. The bar is not delight. The bar is the absence of the "they have no idea who I am" feeling that defines most IVR interactions.

**Social job:**
Not have to explain Spectrum to a Spectrum agent. Customers who are asked to re-verify, re-explain, and re-escalate share those experiences. Customers who receive proactive credits and relevant offers share those too. NBA determines which story gets told.

---

## 4. Success Metrics

**Primary metric**

| Metric | Baseline | Target | Timeframe |
|---|---|---|---|
| NBA action acceptance rate — customer accepts proactively surfaced offer, credit, or resolution without requesting it | 0% (no proactive NBA capability exists) | ≥35% acceptance on NBA-triggered interactions | 90 days post-GA |

**Secondary metrics**

| Metric | Baseline | Target | Timeframe | Notes |
|---|---|---|---|---|
| Repeat contact rate (30-day same-issue) | ~20% (post-Aura baseline) | ≤12% on NBA-handled contacts | 6 months post-GA | NBA should close the residual repeat contact gap Aura alone cannot reach |
| Retention save rate on NBA-triggered retention interactions | Internal baseline required | ≥15% improvement vs. reactive retention baseline | 2 quarters post-GA | Requires holdout experiment — cannot be a 30-day metric |
| Churn rate — NBA cohort vs. matched control | Internal baseline required | Statistically significant reduction in NBA cohort at 6 months | 6 months post-GA | Primary business case metric; requires experiment design before launch |
| Proactive credit acceptance rate | 0% (no proactive credits exist) | ≥60% acceptance on eligible outage credit offers | 60 days post-GA | Lower bar than retention — outage credits are low-risk, high-trust actions |
| CSAT on NBA-triggered interactions | +27pts vs. pre-Aura baseline | ≥+5pts incremental lift vs. Aura-without-NBA baseline | 90 days post-GA | Measures NBA contribution above Aura baseline — not total CSAT |

**Guardrail metrics** *(must not worsen)*

| Metric | Threshold |
|---|---|
| Self-service resolution rate | Must not decline below 80% on NBA-handled contacts |
| Average handle time | Must not increase above 6 min average on NBA-handled contacts |
| NBA action false-positive rate (offer surfaced to wrong segment) | ≤5% of NBA actions surfaced to customers with no eligibility signal |

---

## 5. Requirements

### Must Have (P0)

- [ ] **REQ-1 — Pre-session NBA signal assembly**
  *Acceptance criteria:*
  - At session initialization — before the first customer utterance — the system assembles a ranked NBA signal set from: account state (billing status, promotional expiration date, equipment age), contact history (prior 90-day contacts by type and resolution status), propensity model score (churn propensity, upgrade propensity, outage credit eligibility), and active outage or service event in the customer's service area.
  - Signal assembly completes within 800ms of session initialization and does not delay the first IVR or chat prompt to the customer.
  - If one or more signal sources are unavailable (backend timeout, system degradation), session proceeds with available signals — NBA does not block the interaction. Graceful degradation is logged.
  - Signal assembly output is a ranked action list: NBA-1 (highest confidence, highest value), NBA-2, NBA-3. Only NBA-1 surfaces in the interaction unless NBA-1 is rejected or resolved.

- [ ] **REQ-2 — Confidence threshold calibration per action type**
  *Acceptance criteria:*
  - Each NBA action type has an independently calibrated confidence threshold before surfacing:
    - Outage credit offer: ≥0.70 confidence (low-risk, reversible, high-trust signal)
    - Billing adjustment offer: ≥0.80 confidence (moderate-risk, reversible with audit trail)
    - Retention offer: ≥0.90 confidence (high-risk, irreversible once accepted, $500+ per-customer value)
    - Upgrade offer: ≥0.75 confidence (low-risk, additive, low trust cost if wrong)
  - Actions below threshold are not surfaced. The interaction proceeds as a standard Aura resolution contact.
  - Threshold calibration is data-derived — each threshold is established from a minimum of 10,000 historical contacts of that action type before GA. Thresholds are not set by judgment alone.
  - *Gate: threshold calibration data must be validated by Data Science before REQ-2 enters engineering sprint planning.*

- [ ] **REQ-3 — Human-in-the-loop gate for retention actions**
  *Acceptance criteria:*
  - Retention offers — any action that involves a rate change, promotional extension, or service credit above $50 — require a review-and-approve step before execution, regardless of confidence score.
  - The review-and-approve surface displays: (a) the proposed offer in plain English, (b) the customer's propensity score and the signals driving it, (c) the estimated save value, (d) the confidence score and threshold for this action type.
  - An authorized agent or supervisor can approve, modify, or reject the proposed offer within the same session. The customer is not aware of the review step — hold or queue management is handled transparently.
  - Approved offers are logged with the approving agent ID, timestamp, and offer terms. Rejected offers are logged with rejection reason.
  - *Gate: this requirement applies to retention actions only in V1. Expansion to other action types is a V2 decision based on V1 accuracy data.*

- [ ] **REQ-4 — NBA action audit trail**
  *Acceptance criteria:*
  - Every NBA action surfaced — whether accepted, rejected, or not reached — is logged at the interaction level: session ID, customer ID, NBA action type, confidence score at time of surfacing, customer response (accepted/rejected/not reached), and outcome (measured at 30 days for retention, 7 days for credits).
  - Audit log is accessible to authorized product and operations teams for the prior 180 days.
  - Log is exportable for model retraining input.
  - Log cannot be edited post-session.
  - *Note: REQ-4 is prerequisite for REQ-2 threshold calibration to be maintainable — the audit log is what makes ongoing threshold adjustment evidence-based, not opinion-based.*

- [ ] **REQ-5 — Proactive outage credit surfacing**
  *Acceptance criteria:*
  - For customers contacting during or within 72 hours of a confirmed service event in their service area, the system surfaces an outage credit offer proactively — before the customer raises the issue — if the customer has not already received a credit for this event.
  - The offer is surfaced as a statement, not a question: "I can see there was a service interruption in your area on [date]. I've applied a $X credit to your account." Not: "Would you like a credit?"
  - Credit amount is calculated from the outage duration and the customer's service tier — not a flat amount.
  - If the customer was not affected (no service degradation detected on their line), the offer is not surfaced, regardless of area-level outage status.
  - Outage credit surfacing requires ≥0.70 confidence that the customer's line was affected. Below threshold: standard interaction, no proactive credit.

- [ ] **REQ-6 — Retention offer sequencing discipline**
  *Acceptance criteria:*
  - Retention offers are surfaced only after the primary contact reason is resolved or in active resolution. They are never surfaced as the opening action.
  - If the customer's primary contact reason is billing dispute, service failure, or complaint, retention offer sequencing requires the primary issue to reach resolution status before NBA-1 retention action is eligible to surface.
  - Retention offers are not surfaced if the customer has received a retention offer in the prior 90 days, regardless of propensity score.
  - The system logs the sequencing decision — primary issue resolution status at the time of NBA surfacing — for every interaction where retention was the NBA-1 action.

### Should Have (P1)

- [ ] **Propensity model refresh cadence** — Churn propensity, upgrade propensity, and outage credit eligibility models retrain on a weekly cadence using the prior 30 days of interaction and outcome data. Model version is logged against every NBA action for attribution. Drift detection alerts fire if model accuracy degrades >5% week-over-week.

- [ ] **Cross-channel NBA continuity** — If a customer contacts via IVR and transfers to chat, the NBA signal assembled at IVR session initialization is preserved and available to the chat agent. The NBA action is not re-surfaced if it was already offered in the IVR session. NBA-2 action becomes eligible if NBA-1 was resolved.

- [ ] **NBA action personalization by tenure and value segment** — Offer amounts (credit values, promotional lengths, discount depths) are calibrated by customer tenure and estimated lifetime value, not flat rates. A 10-year customer with Spectrum Mobile and broadband receives a materially different retention offer than a 2-year broadband-only customer with the same propensity score.

- [ ] **Agent assist NBA display** — For contacts that transfer to a human agent, the NBA signal and recommended action are surfaced in the agent assist interface with the confidence score and rationale. The agent sees the same NBA-1 action the system would have surfaced, with the option to execute, modify, or dismiss.

- [ ] **Post-interaction NBA outcome capture** — For accepted NBA actions, outcome is measured at 30 days (retention: did the customer churn? upgrade: is the new service active? credit: did the customer contact again within 30 days on the same issue?) and fed back to the propensity model as a training signal.

### Nice to Have (P2)

- [ ] **Proactive outbound NBA trigger** — For customers with a retention propensity score above a defined threshold who have not contacted in 60+ days, the system triggers an outbound notification (app push, email, or SMS based on channel preference) with a relevant offer. Requires separate legal and compliance review for outbound contact rules.

- [ ] **NBA A/B testing framework** — Product team can configure A/B tests on NBA action type, offer amount, and surfacing timing — with holdout controls — without an engineering sprint. Results visible in a product analytics dashboard within 48 hours of test completion.

- [ ] **Household-level NBA** — NBA signals incorporate household-level data: all Spectrum services across all lines at the address, total household value, and household-level churn risk. A customer contacting about a mobile line receives an NBA action informed by the household's broadband contract status.

---

## 6. Out of Scope — V1

| Out of Scope | Reason |
|---|---|
| **Outbound proactive contact (calls, SMS, email)** | V1 NBA is inbound-only — triggered by customer-initiated contact. Outbound proactive contact requires separate legal, compliance, and TCPA review. P2 consideration only. |
| **Automated retention execution without human review** | REQ-3 is non-negotiable for V1. Retention offers without a review-and-approve gate require accuracy baseline data that does not yet exist. V2 consideration after 6 months of V1 audit log data. |
| **Commercial / business account NBA** | Business accounts have different contract structures, decision-maker dynamics, and offer eligibility. V1 is residential only. Business NBA is a separate workstream. |
| **NBA for new customers (<90 days)** | Propensity models require minimum contact history and account state data. Customers under 90 days have insufficient signal for calibrated NBA actions. Excluded from V1 eligibility. |
| **Offer fulfillment UI changes** | NBA actions surface and log offers; fulfillment (credit application, rate change, service modification) uses existing backend integrations already live in Aura. No new fulfillment surfaces in V1. |
| **Third-party data integration (credit bureau, competitor signal)** | V1 NBA uses first-party Charter data only — account state, contact history, propensity models trained on Charter interactions. Third-party data integration requires separate privacy and legal review. |
| **Streaming or video retention offers** | TV and streaming package retention involves content licensing constraints and different margin profiles. V1 NBA covers broadband and mobile retention only. |

---

## 7. Open Questions

*Ranked by consequence if unanswered before build starts. Each question has an owner and a deadline.*

---

**Q1 — What is the minimum propensity model accuracy required before NBA retention actions are eligible to surface at scale — and who owns the validation methodology?**

REQ-2 requires confidence thresholds derived from 10,000+ historical contacts per action type. The retention threshold (≥0.90) is the highest and the most consequential — a miscalibrated retention model at 50M annual contacts is a financial exposure problem, not a tuning problem. Without a defined accuracy bar and a named Data Science owner for validation, REQ-2 has no build-complete criterion.

*Decisions needed:*
- Minimum model accuracy threshold per action type before GA (retention specifically)
- Validation methodology: holdout set construction, measurement window, success criteria
- Who owns threshold sign-off: Data Science alone or joint Data Science + Product + Finance

*Owner:* Data Science (lead) · Product · Finance
*Deadline:* **2026-05-06** — before REQ-2 enters engineering sprint planning

---

**Q2 — What is the legal and compliance framework for a retention offer surfaced and accepted through an automated system — and does it require agent disclosure to the customer?**

REQ-3 requires a human review-and-approve step for retention actions. The open question is what the customer must be told: does an accepted retention offer made through a system-assisted interaction require disclosure that the offer was generated by an AI system? Telecom retention offers are regulated in some states. A miscommunication or non-disclosure creates a dispute surface that the audit log alone doesn't resolve.

*Decisions needed:*
- Required customer disclosure language, if any, for AI-assisted retention offers
- State-by-state variance in retention offer regulations that affect V1 scope
- Whether the review-and-approve agent must verbally confirm offer terms with the customer or whether system-generated confirmation is sufficient

*Owner:* Legal (lead) · Compliance · Product
*Deadline:* **2026-05-06** — REQ-3 interaction design cannot be finalized without this

---

**Q3 — Does the proactive outage credit (REQ-5) require customer consent or notification before automatic application — and what is the billing system integration path?**

REQ-5 specifies that the outage credit is applied as a statement ("I've applied a $X credit") not a question. This requires real-time write access to the billing system from the NBA engine. Two unresolved dependencies: (1) does applying a credit without explicit customer request require a consent or notification step under any Charter regulatory obligation? (2) What is the billing system integration path — does Aura's existing tool contract cover write-access for credits, or is a new integration required?

*Decisions needed:*
- Legal/regulatory: is proactive credit application (without customer request) permissible without additional consent?
- Engineering: does existing Aura billing tool contract cover NBA-triggered credit writes, or is a new contract required?
- Operations: what is the customer-facing confirmation (statement vs. confirmation + receipt)?

*Owner:* Legal · Engineering (billing integration) · Product
*Deadline:* **2026-05-13** — REQ-5 cannot enter engineering without billing integration path confirmed

---

**Q4 — How does NBA interact with existing agent-owned retention workflows — and what is the change management plan for care operations?**

NBA surfaces retention actions to a review-and-approve agent (REQ-3). Currently, retention interactions are handled by a dedicated retention team with their own offer library, escalation paths, and performance metrics (save rate, offer cost). NBA changes the initiation point — the system identifies and proposes; the agent approves and executes. This is a workflow change for the retention team, not just a product feature. Without a change management plan, NBA actions will compete with existing retention workflows rather than augmenting them.

*Decisions needed:*
- Does NBA replace, supplement, or sit alongside the existing retention offer library?
- How are save rate metrics attributed when NBA initiates and an agent approves?
- What training is required for retention agents before V1 launch?

*Owner:* Care Operations (lead) · Product · HR/Training
*Deadline:* **2026-05-20** — change management plan required before V1 launch date is set

---

**Q5 — Should NBA V1 launch with a holdout experiment design — and who owns experiment governance?**

The primary business case metric (churn rate reduction in NBA cohort vs. matched control) requires a holdout experiment. A holdout means a defined percentage of eligible customers receive no NBA actions during the measurement period. This has a cost — customers in the holdout who would have been saved are not. The tradeoff is measurement credibility: without a holdout, NBA's impact on churn cannot be isolated from other variables. Executive alignment on experiment design, holdout size, and measurement window is required before V1 launch — not after.

*Decisions needed:*
- Holdout size: what % of eligible customers are in the control group? (Recommendation: 20%)
- Measurement window: 6 months minimum for churn signal to be reliable
- Experiment governance: who approves the holdout design and has authority to end it early if results are strong?

*Owner:* Product (lead) · Data Science · Finance · Executive sponsor
*Deadline:* **2026-05-20** — experiment design must be approved before V1 engineering begins

---

## 8. Confidence Level

| Assumption | Confidence | Rationale |
|---|---|---|
| The repeat contact rate (20%) represents a solvable NBA opportunity, not a customer behavior baseline | **Medium** | Post-Aura repeat contact rate of ~20% is significantly improved from 60% but the residual is uncharacterized. If the remaining 20% are genuinely new issues rather than unresolved prior issues, NBA addresses the wrong problem. Requires cohort analysis of repeat contact root causes before V1 investment is confirmed. |
| Propensity models trained on Charter contact history have sufficient signal quality for ≥0.90 retention confidence threshold | **Medium** | Charter has 250M annual contacts and rich account state data — the signal volume is sufficient. The gap is whether the models currently trained on this data have been validated at the 0.90 threshold specifically for retention. No validation study exists yet. This is the highest-consequence low-confidence assumption in the PRD. |
| Customers will accept proactively surfaced credits without perceiving it as intrusive | **Medium–High** | The "the bot doesn't know me" failure mode is well-documented. The inverse — the system proactively surfacing something valuable — is the intended correction. However, proactive credit surfacing before the customer raises the issue is a trust claim that can backfire if the offer feels scripted or mistimed. Qualitative testing required before GA. |
| Retention offer sequencing (primary issue resolved first) is sufficient to prevent customer perception of manipulation | **Medium** | REQ-6 enforces sequencing discipline. The risk is that customers who are aware of retention tactics may perceive the offer as scripted regardless of sequencing. No primary research has been done on customer perception of AI-assisted retention offers in telecom. |
| Care operations will adopt the review-and-approve workflow without resistance | **Low–Medium** | Retention teams have established workflows, performance metrics, and offer authority. NBA changes the initiation point but not the approval authority — agents still approve. The risk is that agents perceive NBA as reducing their discretion or as a performance measurement tool rather than an assist. Change management (Q4) is load-bearing. |
| The 50M retention contact volume provides sufficient training data for model calibration at ≥0.90 confidence | **High** | Volume is not the constraint. Data quality and label accuracy (did the customer actually churn? was the save attributable to the offer?) are the constraints. Assuming label quality is addressed, 50M contacts is more than sufficient for calibration. |
| NBA will not cannibalize existing self-service resolution rate | **Medium–High** | REQ-6 sequencing discipline and the guardrail metric (self-service resolution ≥80% on NBA-handled contacts) address this directly. The risk is that NBA action surfacing adds interaction length that tips marginal self-service contacts into agent transfers. Monitored via guardrail metric from day one. |
| Cox merger completion will expand the addressable NBA contact base to 40M+ customers without requiring NBA architecture changes | **Medium** | The architecture (signal assembly, confidence thresholds, audit log) is designed to scale. The risk is Cox-specific data model differences (billing systems, account state schema, contact history format) requiring integration work before NBA can operate on Cox accounts. Cox data model review is outside V1 scope but should begin in parallel. |

---

*Final PRD date: 2026-04-22*
*Next review: Pre-sprint planning — Q1 and Q2 must be resolved before engineering begins*
