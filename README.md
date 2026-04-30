# LLM‑based Emergency Triage Ethics Evaluation

**Assessing Safety, Fairness, and Transparency of Large Language Models in Emergency Triage**

[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## Overview

This project addresses an emerging ethical concern: **can large language models be safely and fairly used for emergency triage decisions?**  
We constructed a small but structured benchmark of 45 real‑world clinical vignettes (covering emergency, non‑emergency, and self‑care levels) and generated **3 demographic variants** (age, sex, insurance status) for 10 representative cases to systematically evaluate two commercial LLMs on triage tasks.

Key evaluation dimensions:
- **Safety (non‑maleficence)** – How often do models under‑triage clearly high‑risk cases or over‑triage low‑risk cases?
- **Fairness (justice)** – For identical clinical presentations, do demographic changes systematically alter triage recommendations in ways that could disadvantage certain groups?
- **Transparency & accountability** – Are the model’s confidence scores and reasoning trustworthy? How should responsibility be distributed between clinicians, hospitals, and model developers?

## Research Questions

1. **Safety & non‑maleficence** – Compared to guideline‑based triage levels, what are the rates of under‑triage (missing an emergency) and over‑triage (unnecessary escalation)?
2. **Justice & fairness** – For the same clinical description, do changes in age, sex, or insurance status cause systematic differences in LLM triage suggestions?
3. **Transparency & automation bias** – Could seemingly confident LLM outputs create over‑trust in practice? How can governance frameworks mitigate such risks?

##  Dataset

| Dataset | Source | Cases | Gold label distribution | Description |
|---------|--------|-------|-------------------------|-------------|
| `triage_vignettes_base.csv` | MedAsk Triage Bench (Semigran et al.) | 45 | em: 15, ne: 15, sc: 15 | Original vignettes, no demographic info |
| `triage_vignettes_full.csv` | Our variant generation | 40 | em: 16, ne: 12, sc: 12 | 10 selected cases, each with 3 variants (age/sex/insurance changed) |

> **em** = Emergency **ne** = Non‑emergency **sc** = Self‑care

##  Models evaluated

| Model | API endpoint | Version |
|-------|--------------|---------|
| DeepSeek | `deepseek-chat` | latest |
| Kimi (Moonshot) | `moonshot-v1-8k` | v1 |

Each model was run under **two information conditions**:
- `text_only` – only the clinical description
- `text_plus_demo` – clinical description + age, sex, insurance

##  Key results summary

| Model | Condition | Accuracy | Over‑triage rate | Variant consistency | Key findings |
|-------|-----------|----------|------------------|---------------------|---------------|
| DeepSeek | text_only | 62.5% | 37.5% | 90% | Significant gender bias (female accuracy 53% vs male 69%) |
| DeepSeek | text_plus_demo | 72.5% | 27.5% | 80% | Accuracy improved with demographics, but consistency dropped |
| Kimi | text_only | 69.2% | 30.8% | 100% | No under‑triage, perfect consistency |
| Kimi | text_plus_demo | 65.7% | 34.3% | 100% | Accuracy slightly decreased with added demographics |

**Ethical insights**:
- **No under‑triage** observed for either model – a positive safety signal.
- **Over‑triage** rates were high (up to 37.5%), which could waste medical resources.
- DeepSeek exhibited **gender bias**: lower accuracy on female cases (53%) than on male cases (69%) when only clinical text was used.
- Adding demographic information improved DeepSeek’s overall accuracy but introduced **inconsistency** across variants (e.g., back pain and yeast infection cases), suggesting the model may over‑rely on sensitive attributes.

##  Reproduction steps

### 1. Clone the repository
```bash
git clone https://github.com/yourusername/llm-triage-ethics.git
cd llm-triage-ethics
