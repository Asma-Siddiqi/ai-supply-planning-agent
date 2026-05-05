# Product Requirements Document (PRD)
## AI Supply Planning Agent
**Version:** 1.0 | **Status:** Shipped | **PM:** Asma Siddiqi

---

## 1. Executive Summary

This PRD defines the requirements for an AI-powered Supply Planning Agent embedded within Fusion Cloud SCM. The agent replaces manual, batch-based demand review workflows with near-real-time AI-driven planning recommendations, grounded in live supply chain data via RAG architecture.

---

## 2. Problem Statement

### Current State (Before)
- Planning teams run weekly batch cycles — demand signals are 5–7 days stale
- Planners spend 60%+ of their time reviewing all exceptions manually
- No explainability on system recommendations — planners override 40% of AI outputs
- Supplier lead time changes not reflected in planning until next batch cycle
- Stockouts and excess inventory occurring simultaneously across SKU portfolio

### Impact
- Revenue loss from stockouts
- Working capital tied up in excess inventory
- Planner productivity low — high cognitive load, low-value work
- Low trust in AI/ML features already deployed in Fusion SCM

---

## 3. Goals & Success Metrics

### Primary Goals
1. Improve supply-demand match rate by reducing forecast-to-actual gaps
2. Reduce planning cycle time by automating exception pre-processing
3. Increase planner trust and adoption of AI recommendations

### Success Metrics (KPIs)

| KPI | Baseline | Target | Measurement Method |
|-----|----------|--------|--------------------|
| Supply-Demand Match Rate | Current baseline | +14% improvement | Measured over 90-day post-launch window |
| Planning Cycle Time | Current baseline | -20% reduction | Time from demand signal to approved plan |
| Planner Override Rate | 40% | <15% | Override logs in OCI Logging |
| AI Recommendation Adoption | 60% | >85% | Accepted / total recommendations ratio |
| Stockout Frequency | Baseline | Measurable reduction | SKU-level stockout tracking |
| Fill Rate % | Baseline | Improvement | Orders fulfilled on time / total orders |

---

## 4. Scope

### In Scope
- AI agent embedded in Oracle Fusion Supply Planning workbench (Redwood UI)
- RAG knowledge layer: historical demand, inventory, supplier lead times, seasonal patterns
- Explainability layer: reasoning trace, confidence scoring, data source citation
- Human-in-the-loop: planner review, accept/modify/override with reason codes
- Override feedback loop: reason codes captured as training signal
- Full audit trail: OCI Logging and Monitoring
- Short-term and long-term memory architecture
- Confidence-based routing: AI recommendation / flagged review / rule-based fallback

### Out of Scope (v1.0)
- Multi-agent collaboration (planned for v2.0)
- Voice interface
- External demand signal integration (e.g. POS data, social signals)
- Automated order execution without planner approval
- Fine-tuning of foundation LLMs (RAG-first approach adopted)

---

## 5. User Personas

### Persona 1 — Supply Chain Planner (Primary)
- **Name:** Maya, Senior Supply Planner
- **Goal:** Complete daily planning cycle efficiently with high-confidence decisions
- **Pain point:** Too many exceptions to review manually, can't trust AI black-box recommendations
- **Success:** Sees clear reasoning for every recommendation, acts on AI output with confidence

### Persona 2 — Supply Chain Manager (Secondary)
- **Name:** Rajesh, Supply Chain Operations Manager
- **Goal:** Visibility into team planning decisions and AI performance metrics
- **Pain point:** No aggregated view of AI adoption, recommendation accuracy, or override patterns
- **Success:** Dashboard showing AI adoption rate, supply-demand match trend, and exception resolution time

### Persona 3 — IT / Platform Admin (Tertiary)
- **Name:** Sanjay, Cloud Admin
- **Goal:** Configure and maintain AI agent without deep ML expertise
- **Pain point:** Complex AI setup requiring data science skills
- **Success:** Agent configured via Oracle AI Agent Studio UI — no code required

---

## 6. User Stories

*See user-stories.md for full backlog*

### Epic 1 — AI Recommendation Engine
- As a planner, I want AI-generated replenishment recommendations so I don't have to calculate manually
- As a planner, I want recommendations updated in near-real-time so I'm not working on stale data

### Epic 2 — Explainability
- As a planner, I want to see why the AI made a recommendation so I can trust and act on it
- As a planner, I want a confidence score on every recommendation so I know when to scrutinise further

### Epic 3 — Human-in-the-Loop
- As a planner, I want to accept, modify, or override recommendations easily within my workflow
- As a planner, I want my override reasons captured so the system improves over time

### Epic 4 — Audit & Governance
- As a compliance officer, I want a full audit trail of every AI recommendation and planner action
- As a manager, I want to track AI adoption rate and recommendation accuracy over time

---

## 7. Functional Requirements

### FR-01: RAG Knowledge Layer
- System shall ingest supply chain data from Fusion SCM into OCI Vector Search
- Data types: demand history (rolling 24 months), inventory levels, supplier lead times, safety stock policies, seasonal indices
- Refresh frequency: near-real-time via OIC data pipeline

### FR-02: AI Recommendation Generation
- Agent shall generate replenishment recommendations for flagged SKUs
- Each recommendation shall include: recommended quantity, recommended order date, risk level
- Recommendations shall be generated using chain-of-thought reasoning before final output

### FR-03: Explainability Output
- Every recommendation shall display: data sources used, pattern detected, confidence score (0–100%)
- Confidence routing: >80% standard, 60–80% flagged, <60% rule-based fallback

### FR-04: Human-in-the-Loop Interface
- Planner shall be able to Accept / Modify / Override each recommendation within the Fusion SCM UI
- Override shall require selection of structured reason code from predefined list
- All planner actions shall be logged with timestamp and user ID

### FR-05: Audit Trail
- All agent actions logged in OCI Logging: query, retrieval sources, recommendation, confidence, planner action
- Audit logs retained for minimum 12 months
- Logs queryable by SKU, date range, planner, confidence level

### FR-06: Memory
- Short-term: agent shall maintain planner preferences within active session
- Long-term: historical demand patterns and override decisions persisted in OCI Vector Search

---

## 8. Non-Functional Requirements

| Requirement | Specification |
|-------------|--------------|
| Latency | Recommendation generated within 3 seconds of query |
| Availability | 99.9% uptime aligned to Fusion SLA |
| Security | OCI IAM role-based access, no PII in agent logs |
| Scalability | Support 200+ enterprise clients, concurrent planning sessions |
| Compliance | Full audit trail for compliance readiness |

---

## 9. Technical Architecture Summary

See README.md for full architecture diagram.

**Key decisions:**
1. **RAG over fine-tuning** — supply chain data changes daily; RAG allows knowledge base updates without retraining
2. **Confidence-based routing** — protects planners from low-quality AI output; builds trust progressively
3. **Human-in-the-loop mandatory in v1.0** — no automated order execution; planner approval required for all actions
4. **OIC orchestration** — ensures data consistency across Fusion SCM modules before agent query

---

## 10. Risks

| Risk | Probability | Impact | Mitigation |
|------|------------|--------|------------|
| Low planner adoption | High | High | Explainability layer, change management, training sessions |
| RAG retrieval latency >3s | Medium | Medium | OCI Vector Search optimisation, query chunking |
| Hallucination on low-data SKUs | Medium | High | Confidence threshold routing to rule-based fallback |
| Data quality issues in knowledge base | Medium | High | Data validation pipeline before ingestion |
| Scope creep — requests for auto-execution | High | Medium | Explicit out-of-scope documentation, stakeholder sign-off |

---

## 11. Go-to-Market / Rollout Plan

| Phase | Scope | Timeline |
|-------|-------|----------|
| Phase 1 — Pilot | 10 enterprise clients, high-volume SKUs only | Q1 |
| Phase 2 — Expansion | 50 clients, full SKU portfolio | Q2 |
| Phase 3 — Full Rollout | 200+ clients | Q3 |
| Phase 4 — v2.0 Planning | Multi-agent, voice interface | Q4 |

**Go-live criteria:**
- AI recommendation accuracy validated over 4-week parallel run
- Planner UAT sign-off from 3 pilot clients
- Audit trail validated by compliance team
- Rollback plan documented and tested
