# ai-supply-planning-agent
# 🤖 AI Supply Planning Agent
### AI-Powered Agentic Supply Chain Planning | Fusion Cloud SCM

---

## 📌 Project Overview

An enterprise-grade AI agent integrated into Fusion Cloud Supply Chain Management (SCM) that uses Generative AI, RAG (Retrieval-Augmented Generation), and agentic orchestration to automate and optimise supply planning decisions across 200+ enterprise clients globally.

This project replaced weekly batch planning cycles with near-real-time AI-driven demand sensing — improving supply-demand match rates and reducing planning cycle time significantly.

---

## 🎯 Problem Statement

Enterprise supply chain planners were operating on stale demand signals. Weekly batch planning cycles meant decisions were based on last week's data — resulting in:

- Simultaneous stockouts and excess inventory across SKUs
- Planners spending 60%+ of their time reviewing exceptions manually
- Low trust in system recommendations — 40% override rate
- Slow response to supplier lead time changes and demand spikes

---

## 💡 Solution

An AI Supply Planning Agent built on O AI Agent Studio and OCI Generative AI Service, using a RAG architecture grounded in live Fusion SCM data.

### Architecture

```
┌─────────────────────────────────────────────────────┐
│           Oracl Fusion SCM UI      
│           Planner Workbench + Chat Interface         │
└──────────────────────┬──────────────────────────────┘
                       │
┌──────────────────────▼──────────────────────────────┐
│              Oracl AI Agent Studio                  │
│   Agent Config │ Guided Journey │ Tool Registry      │
└──────────────────────┬──────────────────────────────┘
                       │
┌──────────────────────▼──────────────────────────────┐
│           OCI Generative AI Agents Service           │
│     Cohere Command R+ │ Meta Llama 3 │ RAG Engine    │
└────────┬──────────────────────────────┬─────────────┘
         │                              │
┌────────▼────────┐           ┌─────────▼─────────────┐
│  OCI Vector     │           │  Oracl Integration    │
│  Search (RAG    │           │  Cloud (OIC)           │
│  Knowledge Base)│           │  API Orchestration     │
└────────┬────────┘           └─────────┬─────────────┘
         │                              │
┌────────▼──────────────────────────────▼─────────────┐
│              Oracl Fusion SCM Data Layer            │
│  Inventory │ Orders │ Suppliers │ Demand History     │
└─────────────────────────────────────────────────────┘
```

---

## 🔧 Technical Stack

| Layer | Technology |
|-------|-----------|
| AI Agent Framework | AI Agent Studio |
| LLM Models | Cohere Command R+, Meta Llama 3 (via OCI GenAI) |
| RAG / Knowledge Layer | OCI Vector Search, OCI Object Storage |
| Orchestration | O Integration Cloud (OIC) |
| UI Embedding | O Visual Builder Studio |
| Data Source | O Fusion Cloud SCM REST APIs |
| Logging & Audit | OCI Logging and Monitoring |
| Internal AI Tooling | Enterprise ChatGPT (GPT-4o via Azure OpenAI) |

---

## 🧠 How the Agent Works

### Step-by-Step Agent Loop

```
1. TRIGGER
   Planner initiates planning run or asks natural language question
   e.g. "What is the replenishment risk for SKU-1042 this week?"

2. INTENT PARSING
   LLM parses intent → identifies: SKU, time horizon, query type

3. RAG RETRIEVAL
   OCI Vector Search retrieves relevant context:
   - Last 90 days demand history for SKU-1042
   - Current on-hand inventory levels
   - Supplier lead time (current vs historical)
   - Safety stock policy
   - Seasonal demand patterns

4. REASONING (Chain-of-Thought)
   Agent reasons through the data:
   - Current stock: 8 days coverage
   - Supplier lead time: increased 7→10 days
   - Historical spike: +23% demand in this period
   - Stockout risk: Day 11 (within lead time window)

5. RECOMMENDATION GENERATION
   Output: Recommended order qty + timing
   + Reasoning trace (explainability)
   + Confidence score (0-100%)

6. HUMAN-IN-THE-LOOP
   Planner reviews recommendation + reasoning
   Accepts / Modifies / Overrides
   Override requires structured reason code → fed back as training signal
```

---

## 📊 Key Features

### 1. Explainability Layer
Every AI recommendation includes:
- **Data sources used** — which demand records, supplier data, inventory snapshots
- **Pattern detected** — what anomaly or trend triggered the recommendation
- **Confidence score** — how certain the agent is (High / Medium / Low)
- **Reasoning trace** — step-by-step logic chain in plain language

### 2. Confidence-Based Routing
| Confidence | Action |
|-----------|--------|
| > 80% | Standard recommendation — planner expected to act |
| 60–80% | Flagged — "Review recommended" |
| < 60% | Deferred to rule-based planning — no AI recommendation |

### 3. Memory Architecture
- **Short-term memory**: Conversation context window — planner preferences within session
- **Long-term memory**: Persisted in OCI Vector Search — historical demand patterns, past override decisions

### 4. Audit Trail
Full logging in OCI Logging and Monitoring:
- Query received
- Data retrieved (RAG sources)
- Recommendation generated
- Confidence score assigned
- Planner action (accept / override)
- Override reason code
- Outcome (was the recommendation correct?)

---

## 📈 Outcomes

| Metric | Before | After | Improvement |
|--------|--------|-------|-------------|
| Supply-Demand Match Rate | Baseline | +14% | ✅ |
| Planning Cycle Time | Baseline | -20% | ✅ |
| Planner Override Rate | 40% | <15% | ✅ |
| Stockout Frequency | Baseline | Significant reduction | ✅ |
| Exception Review Time | Manual / Full review | AI pre-sorted | ✅ |

---

## 🗂️ Repository Structure

```
├── README.md                          # This file
├── docs/
│   ├── PRD.md                         # Product Requirements Document
│   ├── user-stories.md                # Full user story backlog
│   ├── architecture-diagram.md        # System architecture details
│   └── agent-configuration-guide.md  # How the agent was configured
├── agent-config/
│   ├── agent-setup-steps.md           # Oracle AI Agent Studio setup
│   ├── knowledge-base-schema.md       # RAG knowledge layer design
│   └── confidence-threshold-logic.md # Scoring framework
├── metrics/
│   └── kpi-tracking-template.md      # KPI measurement framework
└── retrospective/
    └── lessons-learned.md             # What worked, what didn't
```

---

## 👤 Role

**Senior Product Manager** — Oracle Fusion Cloud SCM
- Owned end-to-end product vision, design, and delivery of the AI agent
- Defined RAG knowledge layer architecture and data ingestion strategy
- Designed explainability framework and confidence scoring logic
- Led UAT with enterprise supply chain planning teams
- Tracked and reported value realization post-launch

---

## 📎 Related Certifications
- Oracle AI Agent Studio Applications Associate
- Oracle AI in Fusion Supply Chain
- PMP — Project Management Institute
