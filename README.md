# FTEC5660 Individual Project  
## Reproducibility and Agentic Extension Study of UNBench Task 2

This repository contains the reproducibility study and methodological extension
for **UNBench Task 2 – Representatives Voting Simulation**, completed for **FTEC5660: Agentic AI for Business and FinTech**.

The project reproduces the evaluation protocol of Task 2 and investigates
whether structured reasoning (a minimal agentic decomposition) improves
decision robustness under severe class imbalance.

---

## Original Paper

**Benchmarking LLMs for Political Science: A United Nations Perspective**  
Yueqing Liang et al.  
AAAI 2026 (Oral)  
https://arxiv.org/abs/2502.14122

---

## Project Overview

Task 2 evaluates whether a large language model can simulate country-specific
voting behavior (`Y`, `N`, `A`) conditioned on UNSC draft resolutions.

This project includes:

- **Baseline:** Direct single-step vote prediction
- **Modification:** Structured two-stage reasoning  
  (State → Reasoning → Action)

The modification introduces an intermediate reasoning step but remains
single-pass and non-adaptive (no memory or feedback loop).

---

## Experimental Setup

- Model: `gpt-4o-mini`
- Decoding: deterministic (`temperature = 0`)
- Subset size: 50 draft resolutions
- Random seed fixed for reproducibility
- Draft text truncated to first 2000 characters
- Evaluation metrics:
  - Accuracy
  - Balanced Accuracy
  - Macro-F1

To ensure deterministic reproducibility:

Subset selection is performed using a fixed random seed (RANDOM_SEED = 42) to  ensure deterministic reproducibility.

All predictions and labels are cached in task2_subset_results.csv.

This allows metric verification without re-running API calls.

---

## Results (50-draft subset)

| Setting                  | Accuracy | Balanced Accuracy | Macro-F1 |
|--------------------------|----------|------------------|----------|
| Baseline (single-shot)   | 0.7453   | 0.6895           | 0.3348   |
| Modification (multi-step)| 0.9573   | 0.6639           | 0.4215   |

Key observation:

- Structured reasoning increases raw Accuracy
- Balanced Accuracy decreases due to minority-class collapse
- Majority-class confidence amplification occurs under imbalance

---

## Repository Structure
```
UNBench-Task2-Reproduction/
│
├── run_task2_final_submission.ipynb # Main reproduction + modification notebook
├── task2_subset_results.csv         # Cached predictions and labels
├── README.md                        # Dataset description and usage guide
└── report.pdf                       # Final project report
```

---

## How to Reproduce

1. Clone the repository:

   ```bash
   git clone <your_repository_url>
   cd UNBench-Task2-Reproduction
   ```

2. Install dependencies:
   
   ```bash
   pip install pandas numpy scikit-learn matplotlib tqdm openai python-dotenv
   ```

3. Create a `.env` file in the project root:

   OPENAI_API_KEY=your_api_key_here

   The notebook loads the key using `python-dotenv`.

4. Run run_task2_final_submission.ipynb
