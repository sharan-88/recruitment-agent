# 🧠 Candidate Screening Workflow with LangGraph

This project implements an AI-powered candidate screening pipeline using [LangGraph](https://docs.langchain.com/langgraph/) and [LangChain](https://docs.langchain.com/). It uses a conversational language model to analyze a job application and route the candidate based on experience level and skill match.

---

## 📌 Features

- ✅ Categorizes candidates as **entry-level**, **mid-level**, or **senior-level**
- ✅ Assesses skill match (e.g., `"Match"` or `"NoMatch"`) based on job description
- ✅ Routes candidates automatically to:
  - Schedule HR interview
  - Escalate to recruiter
  - Reject application

---

## 🧩 Workflow Overview

```text
     ┌──────────────┐
     │  __start__   │
     └─────┬────────┘
           ↓
 ┌───────────────────────┐
 │ categorize_experience │
 └─────────┬─────────────┘
           ↓
    ┌──────────────┐
    │ assess_skills│
    └────┬──┬──┬────┘
         │  │  │
         ↓  ↓  ↓
  schedule  ↑  reject
    HR     escalate
 interview  to recruiter


---

## 🧠 Core Logic

### 🔹 `categorize_experience(state)`
Uses an LLM to classify the candidate’s **experience level** into one of the following categories:

- `"entry-level"`
- `"mid-level"`
- `"senior-level"`

---

### 🔹 `assess_skills(state)`
Uses an LLM to determine whether the candidate’s skillset matches the job role (e.g., Python Developer). The response is expected to be:

- `"Match"` — The candidate has the required skills.
- `"NoMatch"` — The candidate lacks the required skills.

---

### 🔹 `routing_finction(state)`
This function uses the results from the previous steps to determine what happens next. The logic is:

- If skill is `"Match"` → route to `schedule_hr_interview`
- If experience is `"senior-level"` and skill is `"NoMatch"` → route to `esculate_to_recruiter`
- Otherwise → route to `reject_application`

