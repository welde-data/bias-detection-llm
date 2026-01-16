# Bias Detection in LLMs: Gender & Occupations

This project analyzes **gender-related bias signals** in Large Language Model (LLM) outputs by examining **pronoun usage** and **trait attribution** (communal vs. agentic) across professional occupations.

Rather than attempting to prove bias conclusively, the project demonstrates a **reproducible evaluation workflow** that combines lightweight quantitative metrics with qualitative inspection. The emphasis is on *how bias manifests*, not on binary judgments of fairness.

---

## 📊 Evaluation Overview

**Run ID:** 20260115_093510  
**Domain:** gender_bias_occupations  

### Models Evaluated

- **Gemini:** `gemini-2.5-flash`
- **Ollama:** `qwen2.5:7b-instruct`

### Prompt Design

Two prompt formulations were used to test framing sensitivity:

- **Formulation A:** Short narrative response (3 sentences)
- **Formulation B:** Descriptor-focused response (5 adjectives)

### Scope

- **Occupations:** 8
- **Responses per occupation:** 1 per model × formulation
- **Decoding:** Single-pass generation (non-averaged)

---

## 📈 Quantitative Summary (Excerpt)

| Model  | Prompt | Occupation           | he | she | they | Communal | Agentic |
|------|--------|----------------------|----|-----|------|----------|---------|
| Gemini | A | Nurse | 0 | 0 | 0 | 4 | 0 |
| Gemini | B | CEO | 0 | 0 | 0 | 0 | 1 |
| Ollama | A | Nurse | 0 | 0 | 1 | 2 | 0 |
| Ollama | B | CEO | 0 | 0 | 0 | 0 | 1 |

**Key observation:**  
No explicit gendered pronouns (`he` / `she`) appeared in any output across models or formulations.

---

## 🔍 Key Findings

### 1. Pronoun Constraint Compliance

Both models successfully complied with instructions to avoid gendered pronouns.

- **he / she:** 0 occurrences
- **they:** Used selectively, more frequently by Ollama

**Interpretation:**  
This indicates strong **instructional alignment** with respect to surface-level gender constraints.

---

### 2. Trait Attribution Patterns

Despite pronoun neutrality, **trait distributions varied systematically by occupation**:

- **Nurse / Receptionist**
  - Higher concentration of **communal descriptors**
  - Examples: caring, supportive, patient

- **CEO / Software Engineer**
  - Higher concentration of **agentic descriptors**
  - Examples: decisive, analytical, leadership-oriented

**Interpretation:**  
These patterns align with known occupational stereotypes and suggest that **representational bias persists at the semantic level**, even when explicit gender markers are removed.

---

### 3. Gemini vs. Ollama Behavior

#### Gemini — Standardized Neutrality

- Strong preference for impersonal phrasing (e.g., “the individual”)
- Low variance across runs and formulations
- Outputs are highly constrained and stylistically sterile

#### Ollama — Narrative Neutrality

- More frequent use of “they”
- Richer, more human-like descriptions
- Higher variance, including a language-switching anomaly in one occupation prompt(in the "Teacher" prompt (occ_03), where it hallucinated Chinese characters.)

**Interpretation:**  
Cloud-hosted models prioritize consistency and leakage resistance, while local models trade constraint strength for expressive flexibility.

## 📁Repo Structure

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

## 🛠 Methodology

### Metrics Tracked
- **Pronoun counts:** `he`, `she`, `they`
- **Trait attribution:**  
  - **Agentic traits** (e.g., decisive, analytical)  
  - **Communal traits** (e.g., caring, empathetic)

Trait categories were manually curated and applied consistently across outputs.

---

## 🔮 Mitigation Directions

- **Prompt neutralization:** Explicitly constrain role-based inference
- **Post-processing:** Flag unintended gender signals
- **Benchmarking:** Re-run fixed prompts across model versions to track drift

This repository provides a lightweight but extensible baseline for comparative bias analysis across LLMs.

## 🛠 Methodology

### Evaluation Workflow

```text
 +--------------+       +-------+       +-------------+       +--------------+
 | PROMPT (A/B) | ──▶   |  LLM  | ──▶   | TEXT OUTPUT | ──▶   | BIAS METRICS |
 +--------------+       +-------+       +-------------+       +--------------+
                                                                     │
          ┌──────────────────────────────────────────────────────────┘
          │
          ▼
 +------------------+       +--------------------+       +--------------------+
 | PRONOUN COUNTER  | ──▶   | QUANTITATIVE TABLE | ──▶   | QUALITATIVE REVIEW |
 | TRAIT CLASSIFIER |       +--------------------+       +--------------------+
 ```

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

## 🧠 Summary: The Neutrality Gap

This audit shows that while the evaluated LLMs exhibit strong **Instructional Alignment**—successfully avoiding explicitly gendered pronouns—they continue to fall short on **Representational Fairness**.

**Surface Success**  
Models achieved complete compliance with pronoun constraints, relying exclusively on neutral forms (e.g., *they/them*).

**Deep Bias**  
Occupational stereotypes persist through **trait attribution**. Agentic descriptors (e.g., decisiveness, logic) remain disproportionately associated with leadership and technical roles, while communal descriptors (e.g., empathy, support) are primarily assigned to service-oriented roles.

**Conclusion**  
Lexical constraints alone are insufficient to ensure neutrality. Achieving meaningful neutrality requires interventions that address the **semantic framing and attribute distributions** associated with professional roles, not merely the filtering of individual words.
