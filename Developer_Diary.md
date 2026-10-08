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
