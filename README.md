# LangGraph SQL Agent

A multi-step AI agent that answers natural language questions about a music store database using LangGraph, GPT-4o, and SQLite.

## What it does

You ask a question in plain English — the agent figures out which database tables to look at, writes a SQL query, validates it, executes it, and returns the answer.

**Example questions:**
- *"Which sales agent made the most in sales in 2009?"*
- *"How many invoices were created in 2009?"*
- *"What is the name of the artist with ID 3?"*

## Tech stack

- **[LangGraph](https://github.com/langchain-ai/langgraph)** — stateful multi-step agent framework
- **[GPT-4o](https://openai.com/gpt-4o)** — LLM for schema selection, SQL generation, and query validation
- **[LangChain](https://github.com/langchain-ai/langchain)** — SQL toolkit and prompt templates
- **[Chinook SQLite DB](https://github.com/lerocha/chinook-database)** — sample music store database (11 tables)
- **Jupyter Notebook** — interactive development environment

## How the agent works

The agent follows a fixed pipeline, then loops to self-correct:

```
List Tables → Get Schema → Generate SQL → Validate SQL → Execute SQL → Submit Answer
                                ↑                               |
                                └───────────── retry ───────────┘
```

| Node | What it does |
|---|---|
| `first_tool_call` | Kicks off the pipeline by listing all tables |
| `list_tables_tool` | Executes the list tables tool call |
| `model_get_schema` | LLM picks which tables are relevant to the question |
| `get_schema_tool` | Fetches the schema for selected tables |
| `query_gen` | LLM writes a SQL query, then submits the final answer after seeing results |
| `correct_query` | LLM validates and corrects the SQL before execution |
| `execute_query` | Runs the SQL against the Chinook database |

## Improvements made

The original notebook had several issues that were identified and fixed:

| Fix | Type | Problem solved |
|---|---|---|
| Fix 1 | Prompt | `model_get_schema` missed the `Customer` table for sales agent questions — added schema hint about `Customer.SupportRepId → Employee.EmployeeId` |
| Fix 2 | Prompt | `query_gen` called `SubmitFinalAnswer` in the same step as writing SQL, hallucinating answers without running a query |
| Fix 3 | Prompt | `query_gen` answered directly from schema sample data instead of executing SQL |
| Fix 4 | Code guard | Enforced SQL execution in code by checking `state["messages"]` for a prior `db_query_tool` result before allowing `SubmitFinalAnswer` |
| Fix 5 | Code guard | Added a max retry limit (`MAX_QUERY_RETRIES = 5`) to prevent infinite loops on repeated query errors |

## Setup

**Requirements:** Python 3.10+, Anaconda, OpenAI API key

```bash
# Create and activate conda environment
conda create -n genai python=3.11
conda activate genai

# Install dependencies
pip install langchain langchain-community langchain-openai langgraph pydantic langsmith
```

Open `sql_agent.ipynb` in Jupyter and run the cells in order. The Chinook database is downloaded automatically in Cell 3.

## About this project

The original notebook was sourced from the web (exact source unknown). This project was an exercise in understanding, debugging, and improving a LangGraph SQL agent with the help of **[Claude Code](https://claude.ai/code)** (Anthropic's AI coding assistant). The goal was to identify real failure modes in the agent and fix them systematically.

## Project status

- [x] Agent working end-to-end
- [x] 5 robustness fixes applied
- [ ] Ambiguous question handling
- [ ] LangSmith evaluation setup
- [ ] Migrate from GPT-4o to Claude (Anthropic SDK)
