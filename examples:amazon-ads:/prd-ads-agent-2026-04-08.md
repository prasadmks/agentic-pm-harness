# PRD: Amazon Ads Agent

*Date: 2026-04-08*
*Status: Final Draft — Ready for Review*
*Author: Product — Amazon Ads*

---

## 1. Problem Statement

The mid-market Performance Marketing Manager ($50K–$500K/month Amazon ad spend) is running a multi-surface Amazon program — Sponsored Products, DSP, and increasingly Streaming TV — but making every strategic decision with structurally incomplete data: AMC incrementality analyses require SQL or a $1K–$3K agency engagement with a 1–2 week lag, so most teams skip them entirely; default last-touch attribution systematically undervalues DSP and Streaming TV, creating a budget-allocation bias the PM knows is wrong but cannot fix; and 15–20 hours per week disappear into manual bid management, cross-console reconciliation, and leadership deck rebuilding that produces no new insight. The cumulative result is a PM who is chronically under-informed on what their full Amazon program actually contributes, perpetually at risk of defending budget with data their CFO can see through, and unable to move upstream from operations to strategy. The moment to act is now: Google Ads Advisor (Gemini) is live across all US MCCs as of January 2026, Walmart Marty is entering full rollout H1 2026 targeting the same endemic advertiser segment, and Amazon's "crystal box" differentiation — explainable AI grounded in purchase-transaction data — is a real and defensible moat only if the product depth behind it is built before competitors close the capability gap.

---

## 2. User Persona

**Primary: Performance Marketing Manager — mid-market in-house**

| Attribute | Detail |
|---|---|
| Company size | $10M–$500M annual revenue |
| Team | 2–6 in-house; often supplemented by an agency partner |
| Ad spend | $50K–$500K/month on Amazon Ads |
| Ad products | Sponsored Products (core) · DSP (active or in pilot) · AMC (known, inaccessible) · Streaming TV (aspirational) |
| Current tool stack | Amazon Ads console + DSP console (separate logins) · Pacvue/Skai/Perpetua (bid automation) · Google Sheets/Looker (cross-channel reporting) · Agency partner (AMC SQL) |
| Reporting line | CMO or VP of E-commerce · weekly performance deck · quarterly business review |
| Core constraint | No SQL capability. No dedicated analyst. Required to produce CFO-grade attribution analysis with consumer-grade tooling. |

**Goals:** Maximize blended ROAS with full-funnel visibility. Prove DSP and Streaming TV contribution before the next QBR. Reduce weekly operational overhead without losing auditability. Access AMC-level analysis without adding headcount or agency cost.

**What they fear:** The CMO asks "what did DSP actually do for us this quarter?" and there's no credible answer. ACoS spikes and they find out 48 hours too late. DSP budget gets cut because last-touch ROAS made it look ineffective — and it actually worked.

---

## 3. Jobs to Be Done

**Functional job:**
Run a full-funnel Amazon Ads program with the data to optimize in real time, allocate budget confidently across ad types, and justify every line item to leadership with defensible, transaction-grounded evidence.

**Emotional job:**
Feel in control of a platform that evolves faster than any team can absorb. Reduce the pre-QBR anxiety of making $200K+ budget decisions with incomplete attribution data.

**Social job:**
Present as a sophisticated, full-stack Amazon advertiser. Producing AMC-level incrementality analysis signals program maturity, justifies headcount and budget, and closes the credibility gap with agencies and enterprise counterparts.

---

## 4. Success Metrics

**Primary metric**

| Metric | Baseline | Target | Timeframe |
|---|---|---|---|
| AMC weekly active users (WAU) among mid-market accounts running Sponsored Products + DSP | ~0% (NL AMC access launched Nov 2025; <4 months of availability data) | 25% WAU penetration among eligible accounts (40% stretch) | 6 months post-GA |

**Secondary metrics**

| Metric | Baseline | Target | Timeframe | Notes |
|---|---|---|---|---|
| First-session AMC completion rate | N/A — new feature | ≥60% of first-time Ads Agent sessions result in a completed AMC analysis (query submitted → interpretable result returned) | 30 days post-GA | Activation metric; AMC WAU is meaningless if first sessions fail |
| Recommendation acceptance rate | No prior data | ≥55% of bid and targeting recommendations accepted | 90 days post-GA | Must be paired with outcome-positive acceptance rate to detect blind acceptance |
| Recommendation accuracy rate | No prior data | ≥80% of accepted recommendations achieve projected outcome within ±20% over projected timeframe | 6 months post-GA | Measures whether accepted trust is warranted, not just given |
| Time-to-first-AMC-insight | 1–2 weeks (agency SQL baseline) | <5 minutes end-to-end (p95 latency) | At launch | Binary test: either it works in <5 min or it doesn't |
| DSP budget retention at QBR cycle | Internal baseline required (% of multi-product accounts that maintain or grow DSP budget QoQ) | ≥10% higher retention rate in Ads Agent cohort vs. matched control | 2 QBR cycles (~6 months) post-GA | Requires holdout experiment; cannot be a 90-day metric |

**Guardrail metrics** *(must not worsen)*

| Metric | Threshold |
|---|---|
| Third-party tool (Pacvue/Skai/Perpetua) API call volume per account | Neutral — no statistically significant increase in churn among Ads Agent users |
| Advertiser support ticket volume (attribution-related) | No increase above pre-MTA-default baseline in the 60 days post-launch |

---

## 5. Requirements

### Must Have (P0)

- [ ] **REQ-1 — NL → AMC SQL for 5 core analysis types**
  *Acceptance criteria:*
  - Given a natural-language question matching one of the 5 supported analysis types (DSP incrementality, path-to-conversion, audience overlap between DSP and Sponsored, new-to-brand rate by channel, frequency impact on conversion rate), Ads Agent generates and executes an AMC query without the advertiser writing or viewing SQL.
  - Results are returned as a plain-English summary plus a downloadable data table within 5 minutes of query submission (p95).
  - Results match analyst-run output on the same dataset within ±5% for all numeric fields.
  - If the query cannot be completed (data insufficiency, AMC provisioning error, model uncertainty), the advertiser receives an explicit failure message with a plain-English reason — never a silent failure or a confidently wrong result.
  - AMC provisioning state is checked before the query runs; if the account does not have active AMC access, the advertiser is shown a one-step provisioning path before the query is attempted.

- [ ] **REQ-2 — Pre-built AMC templates (5 templates, one-click execution)**
  *Acceptance criteria:*
  - Five analysis templates are available, each invocable by a natural-language use case description (advertiser does not need to know the template name).
  - Templates cover: (1) DSP incrementality proof, (2) path-to-conversion for new-to-brand customers, (3) audience overlap between DSP and Sponsored, (4) frequency impact on conversion rate, (5) new-to-brand % by channel.
  - Each template executes with a single confirmation action — no date range, filter, or configuration input required from the advertiser in V1.
  - Results return in plain English with a downloadable data table.
  - Templates surface in Ads Agent when an advertiser has been running Sponsored Products + DSP for ≥30 days and has not yet run any AMC analysis, as a contextual onboarding prompt.

- [ ] **REQ-3 — Multi-touch attribution as default view for multi-product accounts**
  *Acceptance criteria:*
  - For all accounts actively running ≥2 Amazon ad product types (e.g., Sponsored Products + DSP), Campaign Manager defaults to multi-touch attribution in all performance dashboard views.
  - Last-touch attribution remains available via a dropdown or toggle — it is not removed.
  - When an advertiser switches to the last-touch view, a persistent contextual banner reads: "You're viewing last-touch attribution. For accounts running both Sponsored Ads and DSP, last-touch typically under-credits upper-funnel channels. Switch to multi-touch for a more complete picture."
  - Single-product accounts (Sponsored Products only) are not affected — last-touch remains their default.
  - A one-time in-console notification informs affected multi-product advertisers of the attribution default change before it takes effect, with a link to documentation explaining multi-touch vs. last-touch.
  - *Gate: this requirement does not enter engineering until Finance and Legal sign-off is confirmed (see Open Questions, Q3).*

- [ ] **REQ-4 — Human-in-the-loop approval gate for all recommendations**
  *Acceptance criteria:*
  - No campaign change (bid adjustment, budget reallocation, targeting update, audience modification) executes without an explicit advertiser confirmation action.
  - Every recommendation displays all four of the following before the confirmation action is presented: (a) the specific proposed change in plain English ("Reduce bid on 'wireless headphones' from $1.40 to $0.95"), (b) the data rationale citing the specific metric and timeframe ("ACoS: 47% over last 30 days vs. your 28% target — 214 clicks, 6 conversions"), (c) the projected outcome ("Estimated ACoS reduction to ~30% within 14 days"), (d) a confidence level (High / Medium / Low) with a one-line definition of what each level means.
  - Advertiser can reject any recommendation with one action; the rejection is logged.
  - There is no "accept all" or batch-approval action in V1.
  - *Gate: confidence level language must be reviewed by Legal before implementation (see Open Questions, Q4).*

- [ ] **REQ-5 — Recommendation explainability grounded in account data**
  *Acceptance criteria:*
  - The data rationale displayed with every recommendation (REQ-4b) cites the specific account-level data inputs — not category benchmarks or generalized best practices — unless the account lacks sufficient data, in which case the source is explicitly labeled ("Based on category benchmark — your account data is insufficient for a personalized recommendation").
  - Explanations use plain English only. No technical jargon (no "bid landscape elasticity," "p-value," "confidence interval") in advertiser-facing copy.
  - An advertiser can copy the recommendation rationale text directly and use it verbatim in a stakeholder communication without editing for clarity.
  - Every recommendation rationale is logged in the account-level audit log (see REQ below) with timestamp, inputs, proposed change, and outcome.

- [ ] **REQ-6 — Recommendation audit log**
  *Acceptance criteria:*
  - All Ads Agent recommendations are logged at the account level: timestamp, recommendation type, specific proposed change, data rationale displayed, advertiser action (accepted/rejected), and outcome (measured 14 days after acceptance for bid/budget changes).
  - Audit log is accessible to the advertiser in-console for the prior 90 days.
  - Log is exportable as CSV.
  - Log cannot be edited or deleted by the advertiser or by Amazon account teams.
  - *Note: REQ-6 is prerequisite for REQ-4 and REQ-5 to be credible — the audit log is what makes "crystal box" auditable, not just claimed.*

### Should Have (P1)

- [ ] **Proactive AMC incrementality trigger** — For accounts running Sponsored Products + DSP concurrently for ≥30 days with sufficient data density (minimum thresholds: TBD by Data Science), Ads Agent automatically surfaces an incrementality analysis via an in-console notification without the advertiser initiating a query. Notification includes a plain-English summary (incremental revenue, incremental ROAS, % of conversions that would not have occurred without DSP) and a one-click CTA to explore the full analysis. Fires once per 90-day period per account.

- [ ] **Cross-surface context in a single conversation** — Ads Agent maintains read context across Sponsored Products, DSP, and AMC data within a single conversation session. Advertiser can ask questions spanning ad types ("what's my blended ROAS across Sponsored and DSP for Home & Kitchen this quarter?") and receive a single response without switching views. Session context persists for ≥60 minutes of inactivity.

- [ ] **DSP campaign structure from media plan upload** — Advertiser uploads a media plan document (PDF or XLSX); Ads Agent parses targeting parameters, flight dates, and budget allocations and proposes a DSP campaign structure for review and one-click activation.

- [ ] **Proactive anomaly alerts with recommended action** — Ads Agent monitors active campaigns and surfaces alerts for: ACoS spikes >20% vs. 7-day average, budget burn rate projections exhausting budget before flight end, CTR drops >30% over 48 hours. Each alert includes the anomaly, probable cause, and a specific recommended action subject to REQ-4 approval.

- [ ] **Conversational follow-up within session** — After any recommendation or analysis result, the advertiser can ask follow-up questions in natural language and receive contextually aware responses within the same session. Ads Agent does not require the advertiser to re-establish context between turns.

### Nice to Have (P2)

- [ ] **Weekly cross-channel performance narrative ("CMO update")** — Ads Agent generates a weekly plain-English performance summary (blended ROAS, spend vs. plan, channel contribution, top anomaly, recommended talking point) available every Monday. Advertiser can edit before sending. Exportable as plain text or PDF.

- [ ] **Category benchmark integration** — Ads Agent surfaces anonymized category-level benchmarks for ROAS, ACoS, and CPC on request, with the advertiser's account data shown in context.

- [ ] **Budget reallocation recommendation across ad types** — Based on marginal ROAS data across Sponsored Products, DSP, and Streaming TV, Ads Agent proposes budget shifts with projected impact. Three-option framing: status quo, Sponsored-heavy, DSP-heavy.

- [ ] **Multi-account view for agency users** — Ads Agent supports switching context across multiple advertiser accounts in a single session; cross-account comparative queries supported.

---

## 6. Out of Scope — V1

The following are explicitly not in scope for V1. Any request to include them during build should be escalated to Product with a scope-change rationale, not accommodated silently.

| Out of Scope | Reason |
|---|---|
| **Creative generation** | Covered by Creative Agent (separate product). Ads Agent does not generate ad copy, images, or video assets. |
| **Automated execution without human approval** | Human-in-the-loop approval (REQ-4) is non-negotiable for V1. Rules-based auto-execution ("approve all bid changes under $X") requires recommendation accuracy baseline data that doesn't exist yet. V2 consideration only. |
| **Streaming TV data in cross-surface context** | Streaming TV data model (CPM, GRP, completion rate) diverges from Sponsored/DSP (CPC, CPA, ROAS); mid-market PMMs rarely run Streaming TV yet. Cross-surface context ships Sponsored + DSP + AMC in V1; Streaming TV added in V1.1. |
| **Off-Amazon channel management** | Amazon Attribution (tracking Google, Meta, email → Amazon sales) is a separate product. Ads Agent operates on Amazon-native ad surfaces only. |
| **Multi-account management** | Agency use case (10–50 accounts in one session) is P1. V1 is single-account only. |
| **AMC audience creation and activation** | Ads Agent queries AMC data; it does not build activation segments or push audiences to DSP campaigns. Audience building remains a manual workflow in V1. |
| **Billing, payment, and account financial management** | Payment methods, invoice disputes, credit adjustments, spending limit increases. |
| **Non-US markets** | V1 is US-only. Canada and UK follow based on AMC data availability and NL model quality validation in non-English markets. |

---

## 7. Open Questions

*Ranked by consequence if unanswered before build starts. Each question has an owner and a deadline — questions without a resolved owner are blocked before engineering begins.*

---

**Q1 — What accuracy bar does NL → AMC SQL need to reach before GA, and what is the error handling design when the model returns a wrong result?**

The PRD requires results to "match analyst output." That is not a testable specification. Without a defined accuracy threshold, there is no build-complete criterion for REQ-1. Without an error handling design, a confidently wrong incrementality result gets surfaced to a PM who uses it to defend DSP budget — and the "crystal box" positioning collapses in a single interaction.

*Decisions needed:*
- Minimum accuracy threshold per analysis type (e.g., ≥95% numeric result match vs. analyst baseline on the same dataset)
- Whether advertisers can optionally view the generated SQL
- What the PM sees when the model has low confidence or the query fails

*Owner:* Data Science (accuracy bar + validation methodology) · Product (error UX) · Legal (what obligation exists when incorrect data informs a business decision)
*Deadline:* **2026-04-25** — before REQ-1 enters engineering sprint planning

---

**Q2 — What is the agency channel's actual posture toward Ads Agent AMC automation — and does it change the launch strategy?**

The mid-market target segment is majority agency-managed. Agencies currently charge $1K–$3K per AMC SQL analysis. Ads Agent's NL → AMC capability directly removes a billable service. Agency response (advocacy vs. adverse recommendation) requires a different launch strategy, go-to-market model, and agency incentive design. This cannot be inferred — it requires direct research.

*Decisions needed:*
- Agency posture: advocate, neutral, or adverse?
- Is the launch strategy agency-first, direct-to-advertiser, or tiered?
- Does multi-account support (currently P1) need to move to P0 to make agencies Ads Agent power users rather than bystanders?

*Owner:* Partner Development (lead) · BD · Product Marketing
*Deadline:* **2026-05-02** — before launch strategy is finalized; 8 weeks before target engineering start

---

**Q3 — Does switching the MTA default require Finance and Legal sign-off — and can we get it on the build timeline?**

REQ-3 is the highest-impact, lowest-engineering-effort P0 item. It is also the one with the highest organizational blast radius: changing the attribution default in Campaign Manager will shift Amazon-reported ROAS numbers for every multi-product account and may generate a support spike from advertisers who interpret the change as a performance decline. If Finance cannot sign off on the revenue-reporting implications, REQ-3 does not ship — and the entire attribution narrative (DSP budget defense, "crystal box" credibility) loses its structural foundation.

*Decisions needed:*
- Finance sign-off: does MTA default affect Amazon Ads internal or external performance reporting? (Go/no-go gate for REQ-3)
- Communications plan: advertiser notification before the default change takes effect
- Migration approach: immediate cutover vs. opt-in period vs. phased rollout by account tier

*Owner:* Product (decision lead) · Finance · Legal · Product Marketing (communications plan)
*Deadline:* **2026-04-25** — REQ-3 cannot enter engineering without this resolved; parallel track to Q1

---

**Q4 — What is Amazon's legal framework for an Ads Agent recommendation that causes measurable campaign harm?**

REQ-4 and REQ-5 require confidence indicators ("High / Medium / Low") displayed on every recommendation and a full audit log. These create documented evidence chains. Legal review is needed to determine: whether confidence level language creates implied warranty; what disclaimer copy is required; whether the human-in-the-loop approval gate is legally sufficient for enterprise accounts with brand safety and compliance obligations; and whether a more explicit waiver is required before enterprise accounts deploy Ads Agent recommendations on live campaigns above a threshold spend.

*Decisions needed:*
- Approved confidence indicator language and any required disclaimer copy
- Whether enterprise accounts need a separate waiver or terms acknowledgment before first use
- What the audit log must and must not contain (factual record only vs. disclaimer-appended entries)

*Owner:* Legal (lead) · Product (UX implementation) · Enterprise Account Team (enterprise-specific requirements)
*Deadline:* **2026-05-02** — REQ-4 and REQ-5 UX design cannot be finalized without legal position

---

**Q5 — Should Amazon commission independent third-party benchmarking of Ads Agent ROI claims before GA — and what is the strategy if we don't?**

Amazon's AI performance results (Performance+: 51% lower acquisition cost; Ads Agent beta: 16% lower CPA) are self-reported. The IAB AI Transparency and Disclosure Framework is active. The market research is unambiguous: independent benchmarking will happen, budget allocations will shift based on results, and the first mover advantage belongs to the platform that commissions the validation rather than responding to it. The "crystal box" positioning specifically invites scrutiny — shipping without independent verification of the performance claims creates a structural contradiction between the positioning and the evidence behind it.

*Decisions needed:*
- Executive decision: proactive independent benchmarking (yes/no) and timeline
- If yes: firm selection, methodology, cohort design, and disclosure plan
- If no: communications strategy for when independent benchmarking from a third party surfaces publicly

*Owner:* Product Marketing (lead — strategic decision on verification) · Finance (study budget) · Executive alignment required
*Deadline:* **2026-05-09** — before launch positioning is finalized; affects success metric design and launch narrative

---

## 8. Confidence Level

*Rating our confidence in the major assumptions underlying this PRD. Low confidence = assumption needs validation before significant build investment.*

| Assumption | Confidence | Rationale |
|---|---|---|
| The SQL barrier is the primary blocker to AMC adoption (not "I don't know what to ask") | **Medium** | User research identifies SQL as the blocker; however, this is based on behavioral inference (agency delegation, low AMC usage) not direct interview evidence. If the real blocker is motivation or question-formulation, NL query alone doesn't solve it — templates and proactive triggers become load-bearing. Validate in early beta via first-session observation. |
| Mid-market PMMs want proactive analysis, not just reactive query | **Medium** | The synthesis strongly supports this, but "proactive AI surfacing insights I didn't ask for" is a trust claim that can backfire if early recommendations are wrong or off-target. No direct test has been run. Validate via beta A/B: proactive trigger group vs. query-only group — measure trust rating and action rate. |
| Changing the MTA default will reduce DSP budget cuts at QBR | **Medium–High** | The causal chain is logical and well-supported: last-touch understates DSP → MTA shows truer contribution → PM has better data for QBR defense → DSP budget survives. However, the assumption is that finance and CMO counterparts will accept MTA as more credible than last-touch. If they're anchored to last-touch benchmarks, changing the default view doesn't change the budget outcome. |
| Agency partners will not actively work against Ads Agent adoption | **Low** | No primary research has been done. The displacement risk is real (AMC SQL is a billable service). The consequence of this assumption being wrong is material: mid-market distribution is majority agency-managed. This is the lowest-confidence assumption with the highest adoption consequence. Requires immediate research. |
| Amazon's NL → AMC SQL will reach the accuracy bar required for advertiser trust | **Medium** | The technical capability exists (demonstrated in beta). The gap is that no accuracy bar has been defined, no systematic validation has been run against analyst baselines, and no error handling model exists. "It worked in demo" is not "it will reliably work at scale for the 5 core analysis types." Needs Data Science to define and test before GA. |
| The "crystal box" positioning will be a durable competitive differentiator | **Medium** | The market research confirms the positioning is resonant and timely (IAB transparency framework, competitor black-box backlash). The risk: "crystal box" only holds if explainability is genuinely granular and performance claims are independently verifiable. If either pillar is weaker in practice than in positioning, the differentiation is fragile. Independent benchmarking (Q5) is the key validation. |
| Mid-market PMMs, not agencies or SMBs, are the right primary target for V1 | **High** | The research triangulation here is strong: AMC was a category unlock specifically for this segment (enterprise has analysts, SMB doesn't run DSP), the attribution defense problem is most acute for them, and the dollar consequence of getting attribution wrong is highest at $50K–$500K/month spend. This is the one assumption with consistent support across user research, competitive analysis, and market data. |
| Walmart Marty will not reach full cross-surface coverage (Sponsored + DSP + analytics) before Ads Agent V1 ships | **Medium** | Marty is Sponsored Search only as of Q1 2026. Full rollout is H1 2026. DSP and analytics expansion is unannounced. The assumption is that Amazon can ship V1 before Marty becomes a full-stack competitor. Timeline risk is real if V1 slips. Monitor Marty capability announcements quarterly. |

---

*Final PRD date: 2026-04-08*
*Next review: Pre-sprint planning — all Q1–Q3 open questions must be resolved before engineering begins*
