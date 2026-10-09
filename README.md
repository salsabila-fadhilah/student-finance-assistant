# student-finance-assistant
# 🎓 Student Budget Checker & AI Finance Assistant

An interactive Python web application built using **Pandas**, **Google Gemini API**, and **Gradio** to help university students with part-time jobs analyze their weekly spending and receive tailored, data-grounded financial advice.

---

## 🎯 Target User Persona & Problem Statement

* **Target User:** Full-time university students earning irregular weekly income from part-time jobs.
* **Problem:** Managing variable paychecks against fixed costs (rent, textbooks) and discretionary spending (dining, social life) makes it difficult to maintain a positive cash flow.
* **Solution:** This application ingests transaction CSV files and weekly income figures to calculate spending totals, net balances, and category breakdowns. It then passes these numbers into a persona-driven AI coach powered by Google Gemini to offer customized budgeting recommendations.

---

## 🚀 Features

1. **Custom Financial Analytical Tool:**
   * Reads transaction CSV files using `pandas`.
   * Automatically calculates total income, total spending, and remaining net balance.
   * Group expenses by category (`Category` and `Amount` fields).
   * Robust input validation to catch missing files, non-numeric values, and negative income entries without crashing.

2. **Persona-Driven AI Assistant ("UniBudget Coach"):**
   * Driven by Google's Gemini Flash model (`gemini-flash-latest`).
   * Grounded directly in the user's uploaded weekly figures.
   * Encapsulated with strict system instructions to keep advice practical, encouraging, and focused on student life.

3. **Gradio Web Interface:**
   * Accessible directly inside Google Colab notebooks or via shareable browser links.
   * Simple two-column visual dashboard connecting analytical outputs directly to the AI prompt context.

4. **Automated Unit Test Suite:**
   * Includes 5 assertion-based test cases validating happy paths, missing columns, non-numeric values, and negative income inputs.

---

## 🛠️ Requirements & Setup Instructions

### Prerequisites
* Python 3.10+
* Google Colab Notebook environment
* A free Google Gemini API Key from [Google AI Studio](https://aistudio.google.com/)

### Libraries Used
* `pandas` — CSV parsing and data aggregation
* `google-genai` — Official Google GenAI SDK for Gemini API integration
* `gradio` — Interactive web application interface

### How to Run in Google Colab

1. Open `Student_Finance_Assistant.ipynb` in **Google Colab**.
2. Set up your secret API key:
   * Click the **Secrets (Key icon)** in Colab's left sidebar.
   * Add a new secret named `GEMINI_API_KEY`.
   * Paste your key into the Value field and toggle **Notebook access** to **ON**.
3. Run all cells sequentially (**Runtime > Run all**).
4. Launch the Gradio web application at the bottom of the notebook, upload `sample_student_transactions.csv`, enter your weekly income, and click **Analyze Finances**.

---

## 📊 Sample Input & Output

### Sample Input Data (`sample_student_transactions.csv`)
| Category | Amount ($) |
| :--- | :--- |
| Rent | 200.00 |
| Textbooks | 120.00 |
| Groceries | 85.50 |
| Coffee & Dining | 42.00 |
| Transport | 35.00 |

### Sample Output Summary
```text
--- WEEKLY FINANCIAL SUMMARY ---
Total Weekly Income: $400.00
Total Spending: $482.50
Net Balance: -$82.50

Category Breakdown:
 - Rent: $200.00
 - Textbooks: $120.00
 - Groceries: $85.50
 - Coffee & Dining: $42.00
 - Transport: $35.00
