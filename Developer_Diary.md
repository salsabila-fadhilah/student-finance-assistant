Markdown
# Developer's Diary: Student Finance Assistant

**Developer:** Salsabila Nur Fadhilah  
**Project:** Student Budget Checker & AI Finance Assistant  
**Target User Persona:** Full-time university student working a part-time job.

---

## Week 1: Problem Definition, Persona Framing & AI Setup
**Date:** 1st October 2026

### 1. Goal
To establish a well-framed financial problem in business/user terms, set up the project's version control infrastructure, and configure the Google Gemini API securely.

### 2. AI Collaboration & Prompts
* **Prompt Given to AI:**  
  > "What are the most common financial management challenges faced by full-time university students with irregular part-time income, and how can a simple python application help them?"
* **AI Output Received:**  
  > The AI suggested focusing on unpredictable weekly earnings, high discretionary spending on eating out, and difficulty managing semester textbook and transport costs.

### 3. Decisions & Implementation Details
* **What I Kept & Why:** I adopted the target persona of a **full-time university student earning part-time income**. This persona provides clear boundaries for my custom tool (focusing on weekly net cash flow rather than complex multi-year investment tracking).
* **What I Rejected & Why:** The AI initially recommended building an automated subscription-cancellation bot. I rejected this idea because it required complex third-party browser automation and banking API authentication that goes far beyond the core scope of this unit.
* **Security & Environment Setup:** Created a free API key in Google AI Studio under the `Default Gemini Project`. I explicitly stored the key inside Google Colab's **Secrets (Key icon)** tab as `GEMINI_API_KEY` rather than hardcoding it in python code or committing it to GitHub.

---
## Week 2: Custom Analytical Tool & Pandas Logic
**Date:** 6th October 2026

### 1. Objective
Design a custom Python function (`analyze_student_finances`) that ingests a transaction CSV, aggregates category totals using `pandas`, and returns a formatted budget summary.

### 2. AI Collaboration & Prompts Used
* **Prompt Submitted to AI:**  
  > *"Write a Python function using pandas that takes a CSV file containing 'Category' and 'Amount' columns and a weekly income numeric input, then returns total spending, remaining balance, and aggregated spending by category."*
* **AI Output Summary:**  
  The AI provided code utilizing `pd.read_csv()` and `df.groupby('Category')['Amount'].sum()`.

### 3. Key Design Decisions & Reflections
* **What I Kept:** Kept `pandas` for grouping transaction categories and converting the result into a native Python dictionary (`.to_dict()`) for custom text formatting.
* **What I Modified & Refined:** 
  1. Added input validation to check if `weekly_income < 0` or if mandatory columns (`Category`, `Amount`) were missing from the uploaded file.
  2. Wrapped data conversions in `try-except ValueError` and `try-except Exception` blocks to return clear error text rather than allowing unhandled crashes.
* **Approach Rejected:** The AI suggested using `df.fillna(0)` to auto-fill missing values quietly. I rejected this because silent modification of financial data can disguise user logging errors; raising explicit feedback is far safer.

---

## Week 3: Model Troubleshooting, Resilient AI Grounding & UI Integration
**Date:** 12th October 2026

### 1. Objective
Connect the custom financial output to Google's Gemini API, build a Gradio web interface, write assertion unit tests, and resolve live API server errors.

### 2. AI Collaboration & Prompts Used
* **Prompt Submitted to AI:**  
  > *"How do I build a Gradio app that passes the calculated text output from my finance tool into the prompt sent to Google Gemini so the AI answers questions based on my actual budget numbers, and how do I handle 503 server demand errors?"*
* **AI Output Summary:**  
  The AI provided code combining the system persona, the financial summary text string, and the user's query into a single prompt string sent to `client.models.generate_content()`, along with a basic try-except error structure.

### 3. Key Design Decisions & Implementation Details
* **Persona & Guardrails (R1):** Defined the system persona as `"UniBudget Coach"`—an encouraging financial counselor for university students. I added explicit guardrail instructions to ensure that if the user asks off-topic or non-financial questions, the model politely redirects them back to student budgeting.
* **Data Grounding (R2):** Passed the generated summary string (`financial_context`) directly into the Gemini prompt template so that response advice is grounded in the user's explicit weekly numbers rather than generic financial advice.
* **Interface Design (R4):** Built a two-column Gradio layout (`gr.Blocks`) separating financial data analysis on the top/left from the interactive AI assistant chat on the bottom/right.
* **What I Kept vs. Modified:** I kept the core Gradio block structure recommended by the AI, but I heavily modified the AI call wrapper to handle live server errors gracefully instead of crashing the interface.
* **Approach Rejected:** The AI initially suggested using a permanent streaming chat history component (`gr.ChatInterface`). I rejected this because our application is designed around static, uploaded weekly budget summaries, making clean text box inputs and outputs much more reliable and easier for a non-programmer student to use.

### 4. Bugs Found & Fixed (Evidence of Debugging for R5)
* **Bug 1 (404 NOT_FOUND):** Calling `gemini-2.5-flash` caused a `404 NOT_FOUND` error because Google retired that specific identifier.  
  * *Fix:* Updated the API request string to use the official `gemini-flash-latest` alias, ensuring automatic routing to active free-tier Flash models.
* **Bug 2 (503 UNAVAILABLE):** During live testing, Google's free API endpoint returned `503 UNAVAILABLE` due to server capacity limits.  
  * *Fix:* Updated `ask_finance_assistant` to include a candidate model list (`gemini-flash-latest`, `gemini-2.0-flash`, `gemini-1.5-flash`) with a `time.sleep(2)` retry loop so temporary server spikes don't crash the user interface.
