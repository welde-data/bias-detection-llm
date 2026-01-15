# Bias Detection in LLMs: Gender & Occupations

This project investigates potential **gender-related bias signals** in Large Language Model (LLM) outputs by analyzing **pronoun usage** and **trait attribution** (communal vs. agentic) across a set of professional occupations.

The goal is not to prove the presence or absence of bias definitively, but to demonstrate a **reproducible evaluation workflow** that combines quantitative indicators with qualitative analysis.

---

## 📊 Latest Run Summary

- **Date:** 2026-01-15  
- **Models Tested:**  
  - Gemini (`gemini-2.5-flash`)  
  - Ollama (`qwen2.5:7b-instruct`)  
- **Sample Size:** 32 analysis rows  
- **Domain:** Gender Bias in Occupations  

---

## 📈 Key Findings

### 1. Pronoun Usage and Neutrality

Both Gemini and Ollama consistently used **gender-neutral pronouns** when instructed to avoid explicit gender references.

- **Total “they / their / them” usage:** 40 instances  
- **Total “he / she” usage:** 0 instances  

**Interpretation:**  
The models complied with the prompt constraint by defaulting to singular *they/their*. This indicates successful avoidance of explicit gendered language in this experimental setup.

> Important: Pronoun neutrality alone does **not** guarantee absence of bias. Bias may still appear implicitly through framing, descriptors, or role assumptions.

---

### 2. Trait Attribution (Communal vs. Agentic)

Despite neutral pronoun usage, **trait attribution patterns varied by occupation**:

- **Nurse / Teacher**  
  - Higher **communal** descriptors  
  - Examples: *caring, supportive, empathetic, patient*

- **CEO / Mechanic**  
  - Higher **agentic** descriptors  
  - Examples: *decisive, analytical, technical, leadership-oriented*

**Interpretation:**  
These patterns align with well-documented occupational stereotypes in social science literature. While subtle, they suggest that bias can surface through **descriptor choice**, even when gendered language is avoided.

---

## Repo Structure

```text
bias-detection-llm/
├── .env
├── .env.example
├── .gitignore
├── Bias_Detection.ipynb
├── README.md
├── requirements.txt
├── prompts/
│   └── prompts_v1.json
└── outputs/
    ├── report.md
    └── responses_raw.csv
```

## 🚀 Getting Started

### 1) Install dependencies

Create and activate a virtual environment (recommended), then install requirements.

```bash
python -m pip install -r requirements.txt
```

### 2) Setup Environment

Copy the example environment file and configure your API key:

```bash
cp .env.example .env
```

Create a .env file which should include:

```text
GEMINI_API_KEY=YOUR_GEMINI_KEY
GEMINI_MODEL=gemini-2.5-flash(or your preferred model)

OLLAMA_MODEL=qwen2.5:7b-instruct(or your preferred model)
OLLAMA_CHAT_URL=http://localhost:11434/api/chat
``````

### 3) Run Analysis

Open the notebook:

- `Bias_Detection.ipynb`

Run all cells from top to bottom.  
Once execution completes, results are automatically generated in:

- `outputs/report.md`

 ## 🧠 Takeaway

This run demonstrates that:

- Explicit gender bias (via pronouns) can be mitigated through careful prompt design.
- Implicit bias may persist through **semantic framing** and **trait emphasis**.
- Combining **quantitative signals** with **qualitative review** is necessary for meaningful bias evaluation.

This repository provides a lightweight but extensible framework for such analyses across models, prompts, and domains.

### 🛠 Methodology

Two prompt formulations (**A** and **B**) are used to evaluate whether the **framing of a question** influences model behavior and potential bias.

### Metrics Tracked

- **Pronoun counts**  
  Frequency of:
  - `he`
  - `she`
  - `they`

- **Lexical analysis**  
  Categorization of descriptive adjectives into:
  - **Agentic** (traditionally masculine-coded traits)
  - **Communal** (traditionally feminine-coded traits)
