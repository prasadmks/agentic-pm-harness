# Competitive Landscape: Agentic AI in Telecom Customer Experience

## Overview
Telecom operators face a common structural problem: high contact volume, high cost-per-contact, and customer satisfaction scores that inversely correlate with how often customers have to call. The market response has been a decade of IVR and chatbot investment that optimized for deflection — reducing agent contacts — rather than resolution. The shift to agentic AI platforms reframes the problem: resolution rate, not deflection rate, is the correct optimization target. Charter's Aura platform is among the earliest production deployments of a true agentic resolution platform at this scale (32M+ customers). Competitors are at varying stages of the same transition — from chatbot improvement to agentic architecture.

## Key competitors

**Comcast (Xfinity)** — The largest US cable operator, with a comparable digital service investment profile. Xfinity Assistant handles a significant share of inbound contacts across chat and voice. Comcast's advantage is scale and early investment in conversational AI; its gap vs. Aura is resolution architecture — Xfinity Assistant is optimized for deflection and self-service content surfacing, not multi-step task execution with full account context pre-injection. Comcast has not publicly disclosed agentic platform architecture or resolution-first metrics.

**AT&T** — Deployed a virtual assistant (AT&T Automated System) across voice and chat at significant scale. AT&T's digital service investment has been inconsistent post-WarnerMedia divestiture; their IVR architecture is aging and their NLU capability trails Charter's Google CCAI stack. AT&T's advantage is wireless customer base scale; their gap is cross-channel session continuity and proactive engagement capability.

**T-Mobile** — The strongest digital CX investment among wireless-first operators. T-Mobile's "Team of Experts" model is human-first by design — they have invested in routing quality over AI resolution. AI is used for augmentation (agent assist) rather than autonomous resolution. T-Mobile's gap vs. Aura is autonomous resolution capability; their strength is agent experience design and customer satisfaction on human-handled contacts.

**ASAPP** — Enterprise conversational AI vendor with deployments in telecom (including Charter as a partner evaluation). ASAPP's strength is generative AI for agent assist and real-time coaching; their gap is autonomous resolution — ASAPP is primarily a human augmentation platform, not an autonomous agent. Relevant as a vendor in Charter's ecosystem evaluation, not a direct competitor.

**Google CCAI (as a platform)** — The underlying stack Charter uses (Dialogflow CX, Agent Assist, NGA, Vertex AI). Google is both a vendor and a potential competitive threat as they move toward end-to-end CCaaS. Google's advantage is model quality and platform integration; their gap is domain-specific resolution architecture — the Know/Act/Trust framework, skills architecture, and eval harness are Charter IP, not Google's.

## Comparison table

| Dimension | Charter Aura | Comcast Xfinity | AT&T Virtual Assistant | T-Mobile | ASAPP |
|---|---|---|---|---|---|
| **Primary optimization target** | First Contact Resolution | Deflection rate | Deflection rate | Human routing quality | Agent augmentation |
| **Pre-session context injection** | Yes — before first utterance | No — query-triggered | No — query-triggered | N/A (human-first) | Partial — agent-facing |
| **Cross-channel continuity** | Yes — session object persists | Limited | Limited | Yes (human handoff) | No |
| **Autonomous task execution** | Yes — with confidence thresholds | Limited | Limited | No | No |
| **Proactive engagement** | Yes — outage credits, retention offers | Limited | Limited | Limited | No |
| **Eval framework** | HHH + LLM-as-a-Judge + golden dataset | Not disclosed | Not disclosed | Human QA | Human QA |
| **Human-in-the-loop design** | Yes — review-and-approve for high-risk actions | Not disclosed | Not disclosed | By design | Yes |
| **Skills architecture** | Yes — reusable modules across channels | Not disclosed | Not disclosed | N/A | No |
| **Underlying AI stack** | Google CCAI + Vertex AI + BigQuery | Proprietary + vendors | Proprietary | Proprietary | ASAPP proprietary |
| **Production scale** | 32M+ customers, 250M annual contacts | 32M+ customers | 100M+ subscribers | 113M subscribers | Enterprise deployments |

## Positioning summary

**Where Aura leads:**
- Resolution-first architecture — the only platform with documented pre-session context injection, cross-channel session continuity, and confidence-threshold-calibrated autonomous execution at this scale
- Eval rigor — HHH framework, LLM-as-a-Judge, and 89% human-judge agreement on golden dataset is ahead of public disclosures from any comparable deployment
- Proactive engagement — surfacing credits and offers before the customer asks is a structural capability gap vs. all named competitors

**Where competitors have advantages:**
- AT&T and T-Mobile: wireless-native customer relationships with mobile as the primary service (Charter's mobile is growing but cable-anchored)
- T-Mobile: human CX design excellence — "Team of Experts" NPS outperforms cable operators on human-handled contacts
- ASAPP: agent assist and real-time coaching for complex contacts that require human judgment

**Fastest-growing moat:**
The Cox merger (pending) would make Charter the largest US cable operator — adding Cox's 6M+ customers and contact volume to Aura's platform. At 40M+ customers and 300M+ annual contacts, the eval dataset, model quality, and skills architecture compound into a resolution platform that no single-operator deployment can match on training data volume and domain coverage.
