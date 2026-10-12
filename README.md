# OmniDash — Autonomous Multi-Source Decision-Intelligence Engine

**Smart India Hackathon 2026 | Problem Statement ID: SIH26199**  
**Team Name:** Jet Brains  
**Category:** Software |  Miscellaneous

OmniDash is an AI-powered, natural-language data analytics assistant designed to help non-technical users retrieve insights from structured databases without manually writing SQL queries. It connects conversational AI with database tools to translate business questions into SQL, retrieve results, and present them in an understandable format.

The project addresses operational data silos, delays in manual reporting, and the challenges of accessing business information across sectors such as hospitality, retail, and financial services.

---

## 📌 Problem Statement

Organizations often store critical information across relational databases, spreadsheets, and disconnected systems. Business managers and administrators may depend on technical teams to write SQL queries, prepare reports, and generate insights.

This creates several challenges:

- **Operational Silos:** Business information is distributed across databases and spreadsheets.
- **Reporting Delays:** Routine data requests depend on developers and analysts.
- **SQL Complexity:** Non-technical users may struggle to retrieve information independently.
- **Data Integrity Risks:** AI-generated queries require appropriate validation and access controls.
- **Limited Accessibility:** Raw database records are not always easy to interpret for decision-making.

## 💡 Proposed Solution

OmniDash aims to bridge the gap between natural-language business questions and database analytics through an AI-assisted query workflow.

Users can ask questions in plain English, allowing the application to interpret their intent, use database schema information, request query execution through tools, and present the results.

The broader vision is a reusable decision-intelligence engine for tertiary sectors, including hospitality, retail, and banking and financial services (BFSI).

## ✨ Key Features

- **Natural-Language Querying:** Ask questions about available database information without manually writing SQL.
- **AI-Powered Text-to-SQL:** Uses Google Gemini to interpret questions and request appropriate database operations.
- **Schema-Aware Generation:** Retrieves PostgreSQL table and column metadata to provide relevant database context to the model.
- **MCP-Based Tool Integration:** Uses the Model Context Protocol to connect the AI workflow with database tools.
- **PostgreSQL Integration:** Retrieves structured information using SQL queries.
- **Interactive Results:** Displays natural-language responses, tabular data, and basic charts through Streamlit.
- **Conversation History:** Maintains chat messages during the application session.
- **CSV Data Ingestion:** Supports importing CSV data into PostgreSQL through a separate ingestion script.
- **Simulated Query Mode:** Provides a lightweight, heuristic-based alternative for a limited set of recognizable query patterns.
- **Query Traceability:** Supports inspection of SQL-related execution details in the application workflow.

> **Development scope:** The current prototype uses Streamlit, Google Gemini, MCP over standard input/output (stdio), and PostgreSQL. Features such as multi-database adapters, vector-based schema RAG, LangGraph orchestration, SQLGlot AST validation, DuckDB-based analytical execution, human approval workflows, and a separate React + FastAPI architecture are part of the proposed broader architecture unless implemented and verified separately.

## 🏗️ Current System Architecture

The current prototype follows this workflow:

```text
                ┌───────────────────────────┐
                │           User            │
                │ Natural-Language Question │
                └─────────────┬─────────────┘
                              │
                              ▼
                ┌───────────────────────────┐
                │       Streamlit UI        │
                │ Chat & Engine Selection   │
                └─────────────┬─────────────┘
                              │
                   ┌──────────┴──────────┐
                   ▼                     ▼
        ┌──────────────────┐  ┌──────────────────┐
        │    Live AI Mode  │  │  Simulated Mode  │
        │   Gemini + MCP   │  │ Heuristic Logic  │
        └─────────┬────────┘  └──────────────────┘
                  │
                  ▼
        ┌───────────────────────────┐
        │     MCP Client / Server   │
        │ Schema & Query Tools      │
        └─────────────┬─────────────┘
                      │
                      ▼
        ┌───────────────────────────┐
        │        PostgreSQL         │
        │ Schema & Structured Data  │
        └─────────────┬─────────────┘
                      │
                      ▼
        ┌───────────────────────────┐
        │   Results & Visualization │
        │ Tables, Answers, Charts   │
        └───────────────────────────┘
```

### How It Works

1. **Question Submission:** The user enters a business question through the Streamlit chat interface.
2. **Processing Mode:** The application processes the request using Live AI mode or the simulated engine.
3. **Schema Retrieval:** In Live AI mode, the application retrieves PostgreSQL schema metadata through an MCP tool.
4. **Context Preparation:** The schema information and conversation history are provided to Gemini.
5. **SQL Generation:** Gemini interprets the question and may request execution of a database query through a tool call.
6. **Query Execution:** The MCP server receives the requested SQL and executes it against PostgreSQL, subject to the implemented query checks and limits.
7. **Result Interpretation:** The returned data is supplied to the AI workflow for natural-language response generation.
8. **Visualization:** Streamlit can display the response, a dataframe preview, and a basic bar chart when the data is suitable.

The simulated engine follows a separate heuristic-based workflow and supports only a limited set of query patterns.

## 🛠️ Technology Stack

| Technology | Purpose |
|---|---|
| Python | Application logic and AI integration |
| Streamlit | Interactive chat interface and data visualization |
| Google Gemini SDK | Natural-language understanding and tool calling |
| PostgreSQL | Relational database and SQL execution |
| Model Context Protocol (MCP) | Communication between the AI application and database tools |
| FastMCP | MCP server implementation |
| asyncpg | Asynchronous PostgreSQL access |
| pandas | Data processing and CSV ingestion |
| SQLAlchemy | Database engine and CSV-to-PostgreSQL ingestion |
| psycopg2 | PostgreSQL driver used by the ingestion workflow |

## 🔐 Security and Responsible Data Access

Database access through an AI assistant must be designed carefully because generated SQL can be incorrect, expensive, or outside the intended scope.

The current prototype includes a basic query-prefix check, an outer row limit, and bounded result output. These measures provide basic restrictions but do not constitute complete SQL security.

Recommended safeguards for production deployment include:

- Using database credentials restricted to read-only access.
- Applying SQL parsing and validation against an explicit allowlist.
- Restricting access to approved tables and columns.
- Enforcing query timeouts and resource limits.
- Recording execution metadata for auditing.
- Avoiding the exposure of credentials and sensitive database records.

These safeguards should be implemented and tested before the system is used with production or sensitive organizational data.

## 🎯 Target Users and Use Cases

### Hospitality
Hotel managers can explore available reservation data, occupancy information, and operational performance, provided the relevant data exists in the connected database.

### Retail
Inventory and business teams can analyze product information, sales records, and other available retail metrics.

### Banking and Financial Services
Authorized personnel can explore permitted operational and reporting data, subject to appropriate access controls and data governance requirements.

### General Business Analytics
Managers and administrators can retrieve information from structured databases using natural-language questions rather than manually preparing SQL queries for every request.

## 🚀 Getting Started

### Prerequisites

Install or configure the following:

- Python 3.10 or a compatible version for the project's dependencies.
- PostgreSQL with an accessible database.
- A Google Gemini API key.
- Git and pip.

### 1. Clone the Repository

```bash
git clone https://github.com/Harsh-1324/OmniDash--Autonomous_Multi-Agent_Data_Analytics_Framework.git
cd OmniDash--Autonomous_Multi-Agent_Data_Analytics_Framework
```

### 2. Create a Virtual Environment

```bash
python -m venv venv
```

Activate it on Windows:

```bash
venv\Scripts\activate
```

Activate it on macOS or Linux:

```bash
source venv/bin/activate
```

### 3. Install Dependencies

If the repository contains a `requirements.txt` file:

```bash
pip install -r requirements.txt
```

Otherwise, install the dependencies listed in the project's actual dependency configuration.

### 4. Configure Environment Variables

Configure the Gemini API key and PostgreSQL connection settings according to the environment-variable names expected by the application.

Keep API keys, passwords, and other credentials out of source control. Do not commit your `.env` file.

### 5. Prepare the Database

Create or configure a PostgreSQL database and ensure that the required tables and data are available.

If you are using the CSV ingestion script, review its configuration and target table before importing data. The current ingestion workflow uses table replacement behavior, so importing into an existing table may overwrite its contents.

### 6. Run the Application

From the project directory, run:

```bash
streamlit run app.py
```

Open the local URL printed by Streamlit in your browser.

**Note:** The exact environment-variable names, dependency versions, database setup, and startup requirements should match the files in the repository.

## 📊 Current Implementation and Future Scope

### Current Prototype

- Streamlit-based conversational interface.
- Google Gemini integration.
- PostgreSQL schema retrieval and query execution through MCP tools.
- Schema metadata supplied to the model as prompt context.
- Basic query restrictions and result limits.
- Dataframe and basic chart rendering.
- Separate CSV ingestion and simulated query workflows.

### Planned Enhancements

The following items represent the proposed direction and should not be interpreted as completed features unless implemented and tested.

- **Multi-Agent Orchestration:** Introduce specialized agents for query planning, SQL generation, validation, error correction, and reporting.
- **Advanced Schema Retrieval:** Add embeddings-based schema retrieval using a vector database such as ChromaDB or PGVector.
- **Stronger SQL Validation:** Introduce SQLGlot AST parsing, operation allowlists, and database-level read-only enforcement.
- **Deterministic Analytics:** Evaluate DuckDB for analytical processing and calculation verification.
- **Multi-Source Connectivity:** Develop secure adapters for additional databases and spreadsheet formats.
- **Improved Visualization:** Expand dashboard generation with more interactive charts and business KPIs.
- **Human-in-the-Loop Review:** Add approval workflows for sensitive or uncertain requests.
- **Frontend and Backend Separation:** Evaluate React for the frontend and FastAPI for a dedicated backend API.

## 📈 Expected Impact

OmniDash is designed to reduce the technical barrier to business analytics by allowing users to ask questions in natural language.

Expected benefits include:

- Faster access to business information.
- Reduced dependence on manually written SQL for routine analysis.
- More accessible data exploration for non-technical users.
- Improved visibility into query execution and results.
- A reusable foundation for analytics across different tertiary-sector use cases.

The SIH presentation identifies a target of under 10 seconds for query-to-dashboard latency, at least 70% autonomous repair success, and at least 95% join/query accuracy. These are **prototype target KPIs, not verified benchmark results**.

## 📚 Research and References

The SIH presentation identifies the following research and resources:

- [Multi-agentic Text-to-SQL with Guided Error Correction](https://arxiv.org/abs/2509.00581)
- [BIRD Benchmark](https://bird-bench.github.io/)
- [National Data & Analytics Platform (NDAP) — NITI Aayog](https://ndap.niti.gov.in/)

Additional references listed in the presentation:

- *A Query Engine for the Agents* — arXiv:2605.27785
- *CoDA: Agentic Systems for Collaborative Data Visualization* — arXiv:2510.03194

## 👥 Team

**Team Name:** Jet Brains  
**Team ID:** 133384  
**Event:** Smart India Hackathon 2026  
**Problem Statement ID:** SIH26199

**Project Contributors**
- Harsh Gupta
- Nakul
- Prajakta
- Janhvi

**Project Guide:** Mrs. B. I. Alte

## 🔗 Project Links

- **Project Resources and Video:** [Google Drive](https://drive.google.com/drive/u/6/folders/1ogtq1PeCkxsfEBUlrNfk2n_roijzbq4C?usp=sharing)

---

## 📄 License

Add a `LICENSE` file to the repository if you intend to distribute the project under an open-source license. Until a license is specified, reuse and redistribution permissions are not explicitly granted.

---

**OmniDash — Turning natural-language questions into accessible, data-driven insights.**

#SmartIndiaHackathon #SIH2026 #OmniDash #TextToSQL #GenerativeAI #MCP #PostgreSQL #DataAnalytics #DecisionIntelligence
