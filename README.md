# CORA-AI
# An Explainable Adaptive Agentic GraphRAG Framework for Corporate Risk Assessment

> **Note:** This system is designed as an **AI Decision-Support Tool** for analysts, not as an autonomous financial decision-maker. 

## 📖 Abstract
Corporate risk assessment requires synthesizing fragmented information from heterogeneous sources, such as government registration portals and stock exchange financial databases. Traditional Relational Database Management Systems (RDBMS) struggle with multi-hop reasoning, while standard Large Language Models (LLMs) are prone to hallucinations and lack verifiable evidence. Furthermore, fixed multi-agent AI pipelines execute every available agent regardless of query complexity, leading to unnecessary latency and API costs. 

This project proposes an **Explainable Adaptive Agentic GraphRAG Framework**. It integrates Indian corporate registration data (MCA) and NSE/BSE financial data into a unified Neo4j Knowledge Graph. A Graph Retrieval-Augmented Generation (GraphRAG) layer retrieves structured evidence, which is dynamically routed to specialized AI agents by a Planner Agent. The framework provides evidence-based explainability and a mathematical, data-quality-aware confidence score distinct from the risk score itself.

---

## ✨ Key Research Contributions
1. **Adaptive Agent Selection:** A Planner Agent dynamically routes queries only to the necessary specialized agents (Company, Financial, Risk), reducing token usage and latency.
2. **Knowledge Graph Reasoning:** A unified Neo4j graph integrating corporate registry and financial relationships for multi-hop retrieval.
3. **Evidence-Based Explainability:** Risk assessments are accompanied by traceable evidence paths derived from retrieved graph data.
4. **Data Quality-Aware Confidence Estimation:** A confidence score calculated from measurable data-quality dimensions (completeness, consistency, freshness) rather than LLM self-reporting.
5. **Human-in-the-Loop Review:** A dashboard interface allowing analysts to inspect graph evidence and override AI decisions.

---

## 🏗️ System Architecture


MULTI-SOURCE CORPORATE DATA (MCA + NSE/BSE)
             │
             ▼
      Data Preprocessing & Schema Alignment
             │
             ▼
       Entity Resolution (RapidFuzz)
             │
             ▼
   Corporate Knowledge Graph (Neo4j AuraDB)
             │
             ▼
        GraphRAG Retrieval Layer
             │
             ▼
        Planner Agent (LangGraph)
             │
      ┌──────┼───────┐
      ▼      ▼       ▼
 Company  Financial  Risk/
  Agent     Agent    Reasoning Agent
      └──────┼───────┘
             ▼
       Risk Assessment Engine
             │
       ┌─────┴─────┐
       ▼           ▼
 Evidence      Data Quality
 Explanation    Confidence
       │           │
       └─────┬─────┘
             ▼
      Human Review Dashboard

---

## 🛠️ Technology Stack

| Component | Technology |
| :--- | :--- |
| **Programming** | Python, Google Colab |
| **Data Processing** | Pandas, NumPy |
| **Entity Matching** | RapidFuzz |
| **Knowledge Graph** | Neo4j AuraDB |
| **Graph Query Language** | Cypher |
| **Agent Orchestration** | LangGraph |
| **LLM Backend** | Google Gemini / OpenAI API |
| **Dashboard** | Streamlit |
| **Version Control** | Git + GitHub |

---

## 📂 Project Structure
corporate-risk-graphrag/
│
├── data/
│   ├── raw/                  # Original untouched datasets (MCA CSV, yfinance CSV)
│   ├── processed/            # Cleaned data and matched master dataset
│   └── final/                # Data ready for graph ingestion
│
├── notebooks/
│   ├── 01_eda_and_cleaning.ipynb    # Data inspection and cleaning
│   ├── 02_entity_resolution.ipynb   # Fuzzy matching and data fusion
│   ├── 03_neo4j_graph_build.ipynb   # Pushing data to Neo4j
│   └── 04_agentic_graphrag.ipynb    # LangGraph and LLM integration
│
├── src/
│   ├── data_prep/            # Python scripts for pipeline automation
│   └── utils/                # Helper functions
│
├── requirements.txt          # Python dependencies
└── README.md                 # Project documentation


## 🚀 Installation & Setup

Follow these steps to set up the project locally or in Google Colab.

### 1. Clone the Repository
```bash
git clone https://github.com/your-username/corporate-risk-graphrag.git
cd corporate-risk-graphrag
```

### 2. Install Dependencies
```bash
pip install -r requirements.txt
```
*(Required packages: `pandas`, `rapidfuzz`, `yfinance`, `neo4j`, `langchain`, `langgraph`, `google-generativeai`)*

### 3. Environment Variables
You will need to set up the following credentials:
* **Neo4j AuraDB:** Create a free instance at [Neo4j AuraDB](https://neo4j.com/product/auradb/). Get your `URI`, `Username`, and `Password`.
* **LLM API Key:** Get a key from [Google AI Studio](https://aistudio.google.com/app/apikey) for Gemini, or OpenAI.

---

## 📊 Datasets Used

1. **Indian Companies Registration Data:** Sourced from the Ministry of Corporate Affairs (MCA) via Kaggle. Contains ~2 million registered Indian companies (CIN, Status, State, Business Activity).
2. **NSE/BSE Financial Data:** Sourced via the `yfinance` Python library. Contains financial metrics (Revenue, Net Profit, Total Debt, Debt-to-Equity) for the top 50 NIFTY companies.

---

## 🧪 Experimental Evaluation

To validate the framework, the following experiments are conducted:

1. **Adaptive vs. Fixed Agents:** Comparing token usage, execution time, and cost between the dynamic Planner Agent and a static multi-agent pipeline.
2. **GraphRAG vs. Traditional RAG:** Evaluating retrieval precision and the ability to answer multi-hop relationship queries.
3. **Explainability Verification:** Ensuring generated risk assessments correctly cite retrieved graph evidence without unsupported claims.
4. **Confidence Calibration:** Artificially degrading data quality (removing values, staling data) to verify that the system's confidence score decreases appropriately.

---

## ⚠️ Limitations & Disclaimers

With the current two-dataset scope, this project **does not** claim:
* Autonomous loan approval capabilities.
* Guaranteed prediction of company bankruptcy.
* Detection of director/shareholder networks (requires additional ownership datasets).
* Real-time monitoring of every Indian company.

This system is intended strictly as an **explainable AI decision-support framework** for human analysts.

---

## 📄 License

This project is licensed under the MIT License - see the `LICENSE` file for details.
```
