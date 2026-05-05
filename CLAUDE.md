# Project: LangGraph SQL Agent

## What this project is
A LangGraph-based multi-step SQL agent that answers natural language 
questions about the Chinook SQLite database using GPT-4o.

## Environment
- Conda environment: genai
- Python location: C:\Users\riswa\anaconda3\envs\genai\python.exe
- Launch Jupyter: activate genai, then jupyter notebook

## Key files
- sql_agent.ipynb — the main notebook
- Chinook.db — SQLite music store database
- session_summary.txt — running log of work done

## Project plan (in progress)
1. ✅ Understand the code (Direction 4) — complete
2. ✅ Fix broken cell — Cell 15 converted to markdown, Cell 14 is the working version
3. Improve the agent (in progress)
   - ✅ Fix 1: model_get_schema missing Customer table
   - ✅ Fix 2: query_gen hallucinating — calling SubmitFinalAnswer with SQL
   - ✅ Fix 3: query_gen skipping SQL, answering from schema sample (prompt)
   - ✅ Fix 4: Programmatic guard — enforce SQL execution in code
   - [ ] Max retry limit — prevent infinite loops on repeated errors
   - [ ] Ambiguous question handling — clarify terms like "total" before querying
4. Run & test end-to-end in genai environment
   - [ ] LangSmith evaluation setup (Cell 16 currently inactive)
5. Push to GitHub

On hold: Migrate from GPT-4o to Claude (Anthropic SDK)

## Known issues
- None currently

## Preferences
- Walk through code cell by cell when exploring
- Save progress to session_summary.txt with date timestamps
