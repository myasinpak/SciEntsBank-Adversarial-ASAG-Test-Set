# SciEntsBank Adversarial ASAG Test Set

**LLM-generated, human-validated incorrect answers for evaluating the adversarial robustness of automatic short answer grading (ASAG) systems.**

## Overview

This repository contains an **evaluation-only adversarial test set** built from the SciEntsBank *Unseen Answers* (UA) test questions and reference answers used in the SemEval-2013 Student Response Analysis task.

- **1,034 adversarial incorrect answers** covering **126 questions**
- Language: **English**; domain: science education
- Generation: **GPT-5.6 Sol**, medium reasoning effort
- Labels: **all answers were human-validated as incorrect**; there is no numeric label column
- Intended use: adversarial robustness evaluation of ASAG systems, especially the rate at which incorrect adversarial answers are incorrectly accepted as correct
- Version: **1.0.0** (initial prepared release)

**Important:** These are synthetic, student-like adversarial answers, not authentic student submissions. This CSV does not contain original SciEntsBank student responses. The questions and reference answers originate from SciEntsBank; see [ATTRIBUTION.md](ATTRIBUTION.md) and [LICENSE](LICENSE).

## Files

| Path | Purpose |
| --- | --- |
| `data/adversarial_test.csv` | Public evaluation dataset (1,034 rows) |
| `ATTRIBUTION.md` | Acknowledgment of the SciEntsBank / SemEval source data |
| `LICENSE` | Dataset license information (CC BY-SA 3.0) |

## Dataset columns

| Column | Meaning |
| --- | --- |
| `question_id` | Identifier for the question **within this released dataset** (e.g., `Q1`); not necessarily the original SciEntsBank question identifier |
| `answer_id` | Stable unique identifier for an adversarial answer in release v1.0.0 (e.g., `A0001`) |
| `question` | Original SciEntsBank question text |
| `reference_answer` | Reference answer associated with that question |
| `adversarial_answer` | LLM-generated answer judged incorrect through human evaluation |


## Construction summary

1. **Generation:** Two independent GPT-5.6 Sol generation rounds (up to 15 candidates per question-reference pair per round) yielded **2,816 candidates for 135 question-reference pairs**. Candidates were designed to resemble plausible but incorrect student answers while preserving much of the reference wording/concepts.
2. **LLM-based quality validation:** Seven criteria (incorrectness; unambiguity; internal consistency; naturalness/plausibility; relevance; grounding in the reference; consistency with question conditions) retained **2,322 candidates for 132 questions**.
3. **Semantic deduplication:** all-MiniLM-L6-v2 similarity screening (cosine similarity >= 0.95), LLM verification, and connected-component grouping reduced the pool to **1,884 candidates for 132 questions**.
4. **Human evaluation:** Two independent computer-science academic raters assessed four criteria (incorrectness; unambiguity; naturalness/plausibility; consistency with the question). Disagreements and uncertain judgments were resolved by joint review. **1,347 valid candidates (126 questions)** remained; **537 candidates** were rejected.
5. **Question-level selection:** Up to **10 answers per question** were kept (simple random sampling when needed). **313** valid candidates were not included due to the cap, resulting in **1,034 answers across 126 questions**.

The target ASAG models' predictions were **not used** to generate, validate, deduplicate, or select the adversarial answers. Generation did not enforce a numerical reference-similarity threshold; the cosine similarity threshold above was used for **deduplication screening only**.

## Suggested evaluation

For each answer, obtain a model prediction of **correct** or **incorrect**. A successful adversarial case is an incorrect answer predicted as correct.

- **Micro-ASR:** number of adversarial answers predicted *correct* / 1,034.
- **Macro-ASR:** average of the per-question attack success rates across 126 questions.

This test set contains **only incorrect answers**. It cannot, by itself, measure ordinary binary classification accuracy, recall on correct answers, or overall grading performance. Use the original SciEntsBank test data separately for such evaluations. Avoid using this test set for model training if you intend to report clean held-out robustness results on it.

## Quick start

```python
import csv

with open('data/adversarial_test.csv', encoding='utf-8', newline='') as f:
    rows = list(csv.DictReader(f))

print(len(rows))                 # 1034
print(rows[0]['answer_id'])     # A0001
print(rows[0]['adversarial_answer'])

# The ground-truth label for every row is the string 'incorrect'.
```

Check the data using:

```bash
python scripts/validate_dataset.py
```

## Data source and license

Original SciEntsBank / SemEval-2013 Task 7 data are credited in [ATTRIBUTION.md](ATTRIBUTION.md). The source dataset is distributed as **Creative Commons Attribution-ShareAlike 3.0 Unported (CC BY-SA 3.0)**; this dataset release uses that same license to preserve source attribution and ShareAlike obligations. See [LICENSE](LICENSE). Source: https://github.com/myrosia/semeval-2013-task7

## How to cite
If you use this dataset in your research, please cite:

The original SciEntsBank dataset: Cite the original SemEval-2013 Task 7 publication. Bibliographic details are provided in ATTRIBUTION.md.

A research article describing the construction and evaluation of this adversarial test set is currently being prepared. Its bibliographic information will be added here once published.

## Limitations

The dataset is restricted to **English science questions drawn from SciEntsBank UA**, consists only of **human-validated incorrect answers**, and reflects one adversarial generation design. Results may not generalize to other question types, subjects, languages, or model architectures. The set is not a comprehensive catalogue of attack types and is not evidence that any particular attack succeeds against every grader.

## Versioning

Initial dataset release: **v1.0.0**. Keep `question_id` and `answer_id` stable when issuing corrections or subsequent versions. Use GitHub releases for versioned records; Zenodo can archive public GitHub releases and assign a DOI.
