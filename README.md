# CORA-AI

### Explainable Agentic GraphRAG for Corporate Risk Intelligence

> **CORA-AI** is an explainable corporate intelligence and risk assessment system that combines heterogeneous corporate registration and financial data with a **Neo4j Knowledge Graph**, **GraphRAG**, and **LangGraph-based agents** to answer company-related questions and generate evidence-backed corporate risk assessments.

---

## 🚀 Overview

Corporate risk analysis often requires information from multiple sources such as company registration records, financial statements, business sectors, industries, and financial ratios.

Traditional retrieval systems can struggle to represent the relationships between these entities.

**CORA-AI** addresses this problem by transforming corporate and financial data into a connected knowledge graph and using an adaptive agentic architecture to determine how each user query should be handled.

The system can answer questions such as:

* Where is a company registered?
* What industry does a company operate in?
* What is the company's revenue?
* What is its total debt?
* What is its debt-to-equity ratio?
* What is the company's profit margin?
* What is the corporate risk level?
* Why was a company classified as high, medium, or low risk?
* How confident is the system in its assessment?

---

## 🎯 Key Features

### 🔹 1. Multi-Source Corporate Data Integration

CORA-AI combines:

* Corporate registration data
* Company identification information
* Registration status
* Registered state
* Industry information
* Financial data
* Revenue
* Net profit
* Total debt
* Debt-to-equity ratio
* Current ratio
* Return on equity
* Profit margins

The current prototype works with financial information collected for **50 companies**.

---

### 🔹 2. Knowledge Graph with Neo4j

Corporate information is represented as connected entities rather than isolated rows.

### Graph Schema

```text
                  ┌──────────────┐
                  │   Industry   │
                  └──────▲───────┘
                         │
                    OPERATES_IN
                         │
                         │
┌────────────┐     ┌─────┴──────┐     ┌──────────────┐
│   State    │◄────│  Company   │────►│FinancialRecord│
└────────────┘     └─────┬──────┘     └──────────────┘
       ▲             │           │
       │             │           │
REGISTERED_IN    HAS_FINANCIAL_RECORD
```

### Nodes

* `Company`
* `Industry`
* `State`
* `FinancialRecord`

### Relationships

```text
(Company)-[:OPERATES_IN]->(Industry)

(Company)-[:REGISTERED_IN]->(State)

(Company)-[:HAS_FINANCIAL_RECORD]->(FinancialRecord)
```

This allows CORA-AI to perform relationship-aware and multi-hop retrieval.

---

## 🤖 Agentic Architecture

CORA-AI uses **LangGraph** to implement an adaptive agent workflow.

```text
                         USER QUERY
                              │
                              ▼
                    ┌──────────────────┐
                    │  PLANNER AGENT   │
                    └────────┬─────────┘
                             │
              ┌──────────────┼──────────────┐
              │              │              │
              ▼              ▼              ▼
       ┌────────────┐ ┌────────────┐ ┌────────────┐
       │  COMPANY   │ │ FINANCIAL  │ │    RISK    │
       │   AGENT    │ │   AGENT    │ │   AGENT    │
       └─────┬──────┘ └─────┬──────┘ └─────┬──────┘
             │              │              │
             └──────────────┼──────────────┘
                            ▼
                    ┌──────────────┐
                    │   GraphRAG   │
                    └──────┬───────┘
                           ▼
                    ┌──────────────┐
                    │    Neo4j     │
                    │ Knowledge    │
                    │    Graph     │
                    └──────┬───────┘
                           ▼
                    Evidence-Based
                       Response
```

### Planner Agent

The Planner Agent classifies the user's query into one of three categories:

```text
COMPANY
FINANCIAL
RISK
```

The query is then automatically routed to the appropriate specialized agent.

---

## 🧠 GraphRAG

CORA-AI uses **GraphRAG** to combine the reasoning capabilities of an LLM with structured information stored in Neo4j.

The system uses:

```text
User Question
      ↓
LLM
      ↓
Cypher Query Generation
      ↓
Neo4j Knowledge Graph
      ↓
Retrieved Context
      ↓
LLM
      ↓
Natural Language Answer
```

A custom Cypher generation prompt is used to improve company-name matching.

For example, a user can ask:

```text
What is the debt of icic?
```

instead of needing to provide:

```text
What is the debt of ICICI Bank Limited?
```

The system performs case-insensitive partial matching using Cypher `CONTAINS` logic.

---

# ⚠️ Explainable Corporate Risk Assessment

One of the core components of CORA-AI is its explainable Risk Agent.

Instead of simply returning:

```text
Risk: HIGH
```

the system provides:

* Risk level
* Supporting evidence
* Data quality confidence
* Missing critical fields

### Risk Assessment Flow

```text
User Query
    ↓
Extract Company
    ↓
Retrieve Financial Data
    ↓
Check Data Quality
    ↓
Evaluate Risk Indicators
    ↓
Generate Evidence
    ↓
Calculate Confidence
    ↓
Risk Assessment
```

---

## 📊 Risk Classification

The current prototype uses the company's **Debt-to-Equity (D/E) ratio** as a primary risk indicator.

```text
D/E > 20
   ↓
HIGH RISK

5 < D/E ≤ 20
   ↓
MEDIUM RISK

D/E ≤ 5
   ↓
LOW RISK
```

The system also checks the company's registration status.

A non-active registration status can increase the assessment to **HIGH RISK**.

> These thresholds are implemented as prototype business rules and are not intended to represent a validated financial-risk model.

---

## 📈 Data Quality & Confidence

CORA-AI does not blindly trust incomplete data.

The Risk Agent begins with a confidence score of:

```text
100%
```

and applies penalties when critical information is missing.

Critical fields include:

* Revenue
* Net Profit
* Total Debt
* Debt-to-Equity

This produces a **Data Quality Confidence Score** alongside the risk assessment.

Example:

```text
-----------------------------------
CORPORATE RISK ASSESSMENT
-----------------------------------

Company: RELIANCE INDUSTRIES LIMITED

Risk Level: HIGH

Evidence:
Debt-to-Equity ratio is 36.653,
indicating high leverage.

Data Quality Confidence Score: 100%
-----------------------------------
```

This makes the output more transparent and easier for an analyst to review.

---

# 👤 Human-in-the-Loop

CORA-AI is designed with analyst oversight rather than treating AI output as an unquestionable decision.

The dashboard provides three actions:

```text
┌──────────────────────┐
│  Accept Assessment   │
└──────────────────────┘

┌──────────────────────┐
│ Request Further      │
│ Review               │
└──────────────────────┘

┌──────────────────────┐
│ Override / Reject    │
└──────────────────────┘
```

This enables a human analyst to review and act on AI-generated assessments.

---

# 🏗️ System Architecture

```text
                 ┌─────────────────────┐
                 │      User Query     │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │    Planner Agent    │
                 └──────────┬──────────┘
                            │
              ┌─────────────┼─────────────┐
              │             │             │
              ▼             ▼             ▼
        Company Agent  Financial Agent  Risk Agent
              │             │             │
              └─────────────┼─────────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │      GraphRAG       │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │  Cypher Generation  │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │        Neo4j        │
                 │   Knowledge Graph   │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │ Evidence + Context  │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │   Risk Assessment   │
                 │   + Confidence      │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │ Human Analyst / UI  │
                 └─────────────────────┘
```

---

# 🛠️ Technology Stack

| Category             | Technology                 |
| -------------------- | -------------------------- |
| Programming Language | Python                     |
| LLM                  | Groq-hosted `gpt-oss-120b` |
| LLM Framework        | LangChain                  |
| Agent Framework      | LangGraph                  |
| Knowledge Graph      | Neo4j                      |
| Graph Query Language | Cypher                     |
| Financial Data       | yfinance                   |
| Data Processing      | Pandas                     |
| Interface            | IPyWidgets                 |
| Environment          | Google Colab / Python      |

---

# 📂 Project Structure

```text
CORA-AI/
│
├── README.md
│
├── notebooks/
│   └── CORA_AI_Demo.ipynb
│
├── src/
│   ├── data_prep/
│   ├── utils/
│   ├── graph_rag.py
│   ├── agents.py
│   └── risk_engine.py
│
├── data/
│   ├── raw/
│   ├── processed/
│   └── final/
│
├── docs/
│   └── architecture.png
│
├── screenshots/
│   ├── dashboard.png
│   ├── knowledge_graph.png
│   └── risk_assessment.png
│
├── requirements.txt
├── .env.example
├── .gitignore
└── LICENSE
```

---

# 🔄 End-to-End Workflow

### Step 1 — Data Collection

Financial information is collected for selected companies using `yfinance`.

Corporate registration data is loaded from the corporate dataset.

---

### Step 2 — Data Preparation

The datasets are inspected and prepared for integration.

Important corporate attributes include:

```text
CIN
Company Name
Company Status
Registered State
Industrial Classification
Business Activity
```

Financial attributes include:

```text
Ticker
Revenue
Net Profit
Total Debt
Debt-to-Equity
Current Ratio
Return on Equity
Profit Margins
```

---

### Step 3 — Entity Integration

Corporate and financial records are associated with their corresponding companies.

Partial company-name matching is supported during graph querying.

---

### Step 4 — Knowledge Graph Construction

The processed data is converted into Neo4j nodes and relationships.

---

### Step 5 — GraphRAG

Natural-language questions are converted into Cypher queries.

The queries retrieve relevant information from the knowledge graph.

---

### Step 6 — Agent Routing

The Planner Agent determines whether the query is:

```text
COMPANY
FINANCIAL
RISK
```

and routes it accordingly.

---

### Step 7 — Risk Analysis

For risk queries, the Risk Agent:

1. Extracts the company name.
2. Retrieves financial information.
3. Checks data completeness.
4. Evaluates financial risk indicators.
5. Generates supporting evidence.
6. Calculates confidence.
7. Produces the final assessment.

---

### Step 8 — Human Review

The generated assessment can be:

```text
Accepted
Reviewed Further
Overridden / Rejected
```

by the analyst.

---

# 💡 Example Queries

### Company Information

```text
Where is TCS registered?
```

```text
What industry does Reliance operate in?
```

```text
What is the status of ICICI Bank?
```

### Financial Information

```text
What is the total debt of HDFC?
```

```text
What is the revenue of Reliance?
```

```text
What is the debt-to-equity ratio of TCS?
```

### Risk Analysis

```text
Assess the risk of Reliance.
```

```text
What is the corporate risk of TCS?
```

```text
Evaluate the financial risk of ICICI.
```

---

# 🧪 Demonstration

For example:

```text
User:
Assess the risk of Reliance
```

CORA-AI routes the query:

```text
User Query
     ↓
Planner Agent
     ↓
RISK Agent
     ↓
Neo4j
     ↓
Financial Data
     ↓
Risk Evaluation
```

The system can produce:

```text
Company: RELIANCE INDUSTRIES LIMITED

Risk Level: HIGH

Evidence:
Debt-to-Equity ratio is 36.653,
indicating high leverage.

Confidence: 100%
```

---

# 🔍 Why GraphRAG?

Traditional keyword or document retrieval treats information primarily as separate pieces of text.

Corporate risk analysis is inherently relational.

For example:

```text
Company
   │
   ├── registered in → State
   │
   ├── operates in → Industry
   │
   └── has → Financial Record
                    │
                    ├── Revenue
                    ├── Profit
                    ├── Debt
                    └── Debt-to-Equity
```

A knowledge graph explicitly represents these relationships, allowing the system to perform structured and multi-hop retrieval.

---

# 🎯 Project Objectives

CORA-AI aims to:

* Integrate heterogeneous corporate and financial datasets.
* Represent corporate information as a connected knowledge graph.
* Enable natural-language querying over structured corporate information.
* Use GraphRAG for relationship-aware retrieval.
* Dynamically route user queries using an agentic architecture.
* Generate explainable corporate risk assessments.
* Provide confidence information based on data completeness.
* Keep human analysts in the decision loop.

---

# 🧠 Key Engineering Contributions

### 1. Adaptive Query Routing

Instead of sending every query through the same pipeline, CORA-AI uses a Planner Agent to select the appropriate specialized agent.

### 2. Relationship-Aware Retrieval

Neo4j allows corporate, geographic, industry, and financial relationships to be queried together.

### 3. Robust Company Name Matching

Partial and case-insensitive company names are supported to make natural-language queries more user-friendly.

### 4. Explainable Risk Assessment

The Risk Agent provides evidence instead of returning only a risk label.

### 5. Data Quality-Aware Confidence

Missing critical financial information affects the reported confidence of the assessment.

### 6. Human-in-the-Loop Decision Support

Analysts can accept, review, or override AI-generated assessments.

---

# ⚠️ Limitations

The current version is a research/prototype implementation.

Important limitations include:

* The financial dataset is limited to the companies included in the current data collection pipeline.
* Risk classification is currently rule-based.
* The D/E thresholds are prototype business rules and require domain validation.
* Financial information may contain missing values.
* The system should not be treated as a replacement for professional financial or investment analysis.
* Production deployment would require stronger authentication, secrets management, monitoring, validation, and data governance.

---

# 🔮 Future Improvements

Potential extensions include:

* Real-time financial data ingestion.
* Historical financial trend analysis.
* Additional financial risk indicators.
* Machine-learning-based risk prediction.
* Industry-specific risk thresholds.
* News and regulatory-event integration.
* Temporal knowledge graphs.
* More specialized agents.
* Automated report generation.
* Risk trend visualization.
* Production-grade web application.
* Model evaluation and benchmark datasets.
* Retrieval accuracy and agent-routing evaluation.
* Audit trails for analyst decisions.

---

# 📊 Current Prototype

The current implementation demonstrates:

```text
50 Companies
      ↓
Corporate + Financial Data
      ↓
Neo4j Knowledge Graph
      ↓
GraphRAG
      ↓
LangGraph Agentic Routing
      ↓
Company / Financial / Risk Agents
      ↓
Explainable Risk Assessment
      ↓
Confidence Score
      ↓
Human-in-the-Loop Review
```

---

# 🔐 Security

**Never commit API keys, database passwords, or other credentials to GitHub.**

Use environment variables or platform secret management.

Example:

```bash
GROQ_API_KEY=your_api_key
NEO4J_URI=your_neo4j_uri
NEO4J_USERNAME=your_username
NEO4J_PASSWORD=your_password
NEO4J_DATABASE=your_database
```

A `.env.example` file can be included in the repository without exposing actual credentials.

---

# 📌 Disclaimer

CORA-AI is an academic/research-oriented prototype for corporate intelligence and risk-analysis experimentation.

The generated risk assessments are decision-support outputs and should not be interpreted as professional financial, investment, legal, or credit advice.

---

# 👨‍💻 Project

**CORA-AI — Corporate Risk Intelligence using Agentic GraphRAG**

Built with:

**Python • LangChain • LangGraph • Neo4j • GraphRAG • Groq • Pandas • yfinance**

---

## ⭐ If you find this project interesting

Consider starring the repository and exploring the notebook to see the complete data-to-risk-analysis workflow.
