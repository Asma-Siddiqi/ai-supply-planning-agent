# User Stories — Oracle AI Supply Planning Agent

---

## EPIC 1: AI Recommendation Engine

### US-001
**As a** supply chain planner,
**I want** AI-generated replenishment recommendations for at-risk SKUs,
**So that** I can focus my attention on exceptions that need human judgment rather than reviewing every SKU manually.

**Acceptance Criteria:**
- Agent generates recommendations for all SKUs flagged as at-risk within current planning horizon
- Recommendations include: SKU ID, recommended order quantity, recommended order date, supplier
- Recommendations visible within Supply Planning workbench without navigating away
- Response time: recommendation generated within 3 seconds of query

**Priority:** P0 | **Estimate:** 13 points

---

### US-002
**As a** supply chain planner,
**I want** demand signals updated in near-real-time rather than weekly batch,
**So that** my planning decisions are based on current market conditions not last week's data.

**Acceptance Criteria:**
- Demand data refreshed via OIC pipeline with maximum 4-hour lag from source
- Planning recommendations reflect latest open orders, inventory movements, and supplier confirmations
- Planner can see data freshness timestamp on each recommendation

**Priority:** P0 | **Estimate:** 8 points

---

### US-003
**As a** supply chain planner,
**I want** to ask the agent natural language questions about specific SKUs,
**So that** I can quickly investigate planning scenarios without running manual reports.

**Acceptance Criteria:**
- Agent chat interface embedded in Supply Planning Redwood UI
- Agent understands queries such as: "What is the replenishment risk for SKU-1042?", "Which SKUs are at stockout risk this week?", "Why is SKU-0789 flagged?"
- Agent responds in plain language with structured recommendation

**Priority:** P1 | **Estimate:** 8 points

---

## EPIC 2: Explainability & Transparency

### US-004
**As a** supply chain planner,
**I want** to see the reasoning behind every AI recommendation,
**So that** I can trust the output and make an informed decision to accept or override.

**Acceptance Criteria:**
- Every recommendation displays: data sources used, pattern detected, risk assessment, recommended action
- Reasoning shown in plain language — no technical jargon
- Planner can expand/collapse reasoning detail
- Example: "Flagged because supplier lead time increased from 7 to 10 days (confirmed 3 days ago). Historical demand shows +23% spike in this calendar period. Current stock covers 8 days. Stockout risk in 11 days — within lead time window."

**Priority:** P0 | **Estimate:** 8 points

---

### US-005
**As a** supply chain planner,
**I want** a confidence score on every recommendation,
**So that** I know when to act immediately vs when to apply additional scrutiny.

**Acceptance Criteria:**
- Confidence score displayed as percentage (0–100%) on every recommendation
- Visual indicator: Green (>80%), Amber (60–80%), Red (<60%)
- Recommendations below 60% confidence automatically deferred to rule-based planning with explanation
- Tooltip explains what the confidence score means

**Priority:** P0 | **Estimate:** 5 points

---

### US-006
**As a** supply chain planner,
**I want** to see which data sources the agent used to generate its recommendation,
**So that** I can verify the quality of the underlying data.

**Acceptance Criteria:**
- Every recommendation cites data sources: demand records used, inventory snapshot date, supplier lead time source
- Planner can click through to source data in Fusion SCM
- If data source has quality issues, agent flags it in the recommendation

**Priority:** P1 | **Estimate:** 5 points

---

## EPIC 3: Human-in-the-Loop

### US-007
**As a** supply chain planner,
**I want** to accept, modify, or override AI recommendations within my existing workflow,
**So that** I don't need to switch between systems to act on AI output.

**Acceptance Criteria:**
- Accept / Modify / Override buttons visible on every recommendation within Fusion SCM UI
- Accept: recommendation logged and submitted to planning system automatically
- Modify: planner can edit quantity or date, submit modified version
- Override: planner must select a reason code before overriding

**Priority:** P0 | **Estimate:** 8 points

---

### US-008
**As a** supply chain planner,
**I want** to provide a reason when I override an AI recommendation,
**So that** the system can learn from my decisions and improve over time.

**Acceptance Criteria:**
- Override requires mandatory selection from reason code list:
  - Incorrect demand signal
  - Supplier relationship factor not in system
  - Business rule exception
  - Data quality issue
  - Other (free text)
- Override reason logged with recommendation ID, planner ID, timestamp
- Override patterns reviewed monthly by PM for model improvement

**Priority:** P0 | **Estimate:** 3 points

---

### US-009
**As a** supply chain planner,
**I want** the agent to remember my preferences within a planning session,
**So that** I don't have to repeat context with every query.

**Acceptance Criteria:**
- If planner specifies "focus on high-velocity SKUs only", agent applies this filter to all subsequent queries in session
- Session context maintained for duration of browser session
- Context cleared when planner logs out or closes session

**Priority:** P1 | **Estimate:** 5 points

---

## EPIC 4: Audit, Governance & Compliance

### US-010
**As a** compliance officer,
**I want** a full audit trail of every AI recommendation and planner action,
**So that** we can demonstrate responsible AI use and meet SOX compliance requirements.

**Acceptance Criteria:**
- Every agent interaction logged: query, retrieval sources, recommendation, confidence, planner action, outcome
- Logs retained for minimum 12 months in OCI Logging
- Logs queryable by: SKU, date range, planner ID, confidence level, action type
- Log export available in CSV format for audit purposes

**Priority:** P0 | **Estimate:** 8 points

---

### US-011
**As a** supply chain manager,
**I want** a dashboard showing AI adoption metrics and recommendation accuracy,
**So that** I can track the value delivered by the AI agent and identify improvement areas.

**Acceptance Criteria:**
- Dashboard shows: AI adoption rate (accepted/total), override rate by reason, confidence distribution, supply-demand match rate trend
- Data refreshed daily
- Filterable by time period, planner, SKU category
- Export to Excel/CSV

**Priority:** P1 | **Estimate:** 8 points

---

### US-012
**As an** IT/Platform admin,
**I want** to configure and update the agent knowledge base without engineering support,
**So that** planning policy updates are reflected in AI recommendations without a development cycle.

**Acceptance Criteria:**
- Admin can upload new documents (PDF, TXT) to knowledge base via Oracle AI Agent Studio UI
- Updated documents processed and embedded within 2 hours
- Version history maintained for knowledge base documents
- Rollback available if new documents degrade recommendation quality

**Priority:** P1 | **Estimate:** 5 points

---

## EPIC 5: Performance & Reliability

### US-013
**As a** supply chain planner,
**I want** the agent to respond within 3 seconds,
**So that** it doesn't slow down my planning workflow.

**Acceptance Criteria:**
- P95 response time ≤ 3 seconds for recommendation generation
- P99 response time ≤ 5 seconds
- Graceful degradation: if AI unavailable, rule-based planning activates automatically
- Planner notified if AI fallback is active

**Priority:** P0 | **Estimate:** 5 points

---

## Story Map Summary

| Epic | Stories | Total Points |
|------|---------|-------------|
| AI Recommendation Engine | US-001 to US-003 | 29 |
| Explainability | US-004 to US-006 | 18 |
| Human-in-the-Loop | US-007 to US-009 | 16 |
| Audit & Governance | US-010 to US-012 | 21 |
| Performance | US-013 | 5 |
| **Total** | **13 stories** | **89 points** |
