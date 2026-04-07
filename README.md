# ParsCN: A Persian Dataset for Counter-Narrative Generation to Combat Online Hate Speech

<p align="center">
  <a href="https://doi.org/10.5281/zenodo.18266930"><img src="https://img.shields.io/badge/Dataset-Zenodo-blue?style=flat-square&logo=zenodo" alt="Dataset DOI"></a>
  <a href="https://github.com/zahrasafdari/ParsCN"><img src="https://img.shields.io/badge/Code-GitHub-black?style=flat-square&logo=github" alt="GitHub"></a>
  <a href="https://creativecommons.org/licenses/by/4.0/"><img src="https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey?style=flat-square" alt="License: CC BY 4.0"></a>
  <a href="https://icwsm.org/2026/"><img src="https://img.shields.io/badge/ICWSM-2026-orange?style=flat-square" alt="ICWSM 2026"></a>
</p>

> **Accepted at ICWSM 2026**
>
> *Zahra Safdari Fesaghandis (Bilkent University) · Suman Kalyan Maity (Missouri University of Science and Technology)*

---

## Overview

**ParsCN** is the first large-scale, carefully curated Persian dataset for counter-narrative generation targeting online hate speech. It contains **1,100 hate speech–counter-narrative pairs** spanning six target groups and six countering strategies, tailored to the socio-cultural context of Persian online discourse.

The dataset was built through a novel **multi-stage pipeline** combining expert human annotation, strategic translation, and few-shot LLM-augmented generation — each step followed by rigorous human curation — making it a rich, balanced, and culturally grounded resource for both training and evaluating counter-narrative systems in a low-resource language setting.

---

## Key Features

- **1,100 HS–CN pairs** covering 6 target groups and 6 counter-narrative strategies
- **Multi-source construction**: native Persian hate speech (PHATE), translated and culturally adapted instances (MultiTarget-CONAN, HateXplain)
- **LLM-augmented generation**: GPT-4o, Gemini 2.0 Flash, Claude 3.7 Sonnet, and Qwen3-235B-A22B with semantic retrieval-based few-shot prompting
- **Thorough evaluation**: both human and automatic metrics (BERTScore, Perplexity, Distinct-n, Toxicity)
- **Baseline benchmarks**: mBART and PersianMind evaluated on a held-out test set
- **FAIR-compliant**: persistent DOI, CC BY 4.0 license, standard CSV format

---

## Repository Contents

```
ParsCN/
├── ParsCN-Dataset.csv        # Full dataset (1,100 HS–CN pairs with annotations)
└── baseline-eval.ipynb       # Baseline model training and evaluation notebook
```

### Dataset Fields (`ParsCN-Dataset.csv`)

| Field | Description |
|---|---|
| `Hate_Speech` | Persian hate speech instance |
| `Counter_Narrative` | Corresponding Persian counter-narrative |
| `Target_Group` | One of: Religious, Racial, Gender, Political, Occupational, National |
| `Counter_Type` | One or more of the six counter-narrative types (see below) |
| `Annotator` | Source: Human, GPT-4o, Gemini, Claude, Qwen, or MT-CONAN |

---

## Dataset Details

### Target Groups

| Category | Description |
|---|---|
| Religious | Statements targeting religious groups (e.g., Islam, Judaism) |
| Racial | Speech discriminating against ethnic/racial groups (e.g., Azeris, Kurds) |
| Gender | Statements derogatory to men, women, or activists |
| Political | Expressions targeting government, politicians, or country laws |
| Occupational | Statements targeting professions (e.g., police, teachers) |
| National | Speech targeting national or immigrant groups (e.g., Afghans, Arabs) |

### Counter-Narrative Types

| Type | Purpose |
|---|---|
| Positive Response | Fosters empathy and inclusion |
| Counter Questions | Prompts reflection on biases |
| Denouncing | Rejects harmful rhetoric |
| Fact-based | Corrects misinformation with evidence |
| Warning of Consequences | Highlights negative outcomes |
| Contradiction | Exposes logical inconsistencies |

### Dataset Composition

| Source | Stage | Instances |
|---|---|---|
| PHATE (native Persian) | Stage 1 | 450 |
| MultiTarget-CONAN (translated) | Stage 1 | 150 |
| LLMs (GPT-4o, Gemini, Claude, Qwen) | Stage 2 | 500 |
| **Total** | | **1,100** |

---

## Dataset Creation Pipeline

The dataset was built in two stages:

**Stage 1 — Manual Annotation (600 pairs):** Three native Persian annotators with backgrounds in hate speech research wrote and curated counter-narratives for instances drawn from PHATE and translated examples from MultiTarget-CONAN. All pairs were annotated with target group and counter-narrative type labels.

**Stage 2 — LLM-Augmented Generation (500 pairs):** New hate speech instances were sourced from PHATE, MT-CONAN, and HateXplain. A `paraphrase-multilingual-mpnet-base-v2` SentenceTransformer model retrieved the top-10 semantically similar Stage 1 pairs for each new instance, which were then used as few-shot examples to prompt four LLMs. All 500 generated counter-narratives were manually reviewed and curated before inclusion.

---

## Baseline Evaluation

The `baseline-eval.ipynb` notebook reproduces the paper's baseline experiments and includes:

- **mBART** fine-tuning on MT-CONAN for Persian counter-narrative generation
- **PersianMind** prompted directly in Persian
- Automatic metrics: BLEU, ROUGE-L, METEOR, BERTScore F1, Perplexity, Toxicity
- Human evaluation: Relevance, Effectiveness, Fluency, Tone Appropriateness, Cultural Relevance

### Main Evaluation Results

**Human Evaluation (1–5 scale):**

| Source | Relevance | Effectiveness | Fluency | Tone Appropriateness |
|---|---|---|---|---|
| Human | **4.231** | **4.206** | **4.919** | **4.787** |
| GPT-4o | 4.144 | 4.113 | 4.719 | 4.693 |
| Claude | 4.132 | 4.094 | 4.825 | 4.669 |
| Gemini | 4.112 | 3.938 | **4.807** | 4.556 |
| Qwen | 4.150 | 4.006 | 4.500 | 4.738 |
| MT-CONAN | 4.025 | 3.756 | 4.756 | 4.507 |

**Automatic Evaluation:**

| Source | Perplexity ↓ | BERTScore F1 ↑ | Distinct-2 ↑ | Toxicity ↓ |
|---|---|---|---|---|
| Gemini | 39.15 | 0.662 | 0.754 | 0.037 |
| Claude | 49.37 | 0.663 | 0.782 | 0.053 |
| Qwen | 61.70 | 0.669 | 0.740 | 0.078 |
| GPT-4o | 99.60 | 0.666 | 0.873 | 0.088 |
| MT-CONAN | 100.42 | **0.709** | **0.892** | 0.106 |
| Human | 158.45 | 0.635 | 0.879 | 0.061 |

---

## Installation

Install all required dependencies with:

```bash
pip install transformers==4.41.2 datasets==2.20.0 pandas==2.2.2 torch==2.2.2 bert-score==0.3.13 nltk==3.8.1
```

---

## Usage

### Load the Dataset

```python
import pandas as pd

df = pd.read_csv('ParsCN-Dataset.csv')
print(df.head())
print(f"Total pairs: {len(df)}")
print(f"Target groups: {df['Target_Group'].unique()}")
```

### Run the Baseline Notebook

Open `baseline-eval.ipynb` in Jupyter or Google Colab. The notebook is self-contained and walks through:

1. mBART fine-tuning on MT-CONAN data
2. PersianMind counter-narrative generation
3. Automatic evaluation (Perplexity, BERTScore, Distinct-n)
4. Toxicity evaluation using the Glot500 classifier

---

## Citation

If you use ParsCN in your research, please cite:

```bibtex
@inproceedings{safdari2026parscn,
  title     = {{ParsCN}: A Persian Dataset for Counter-Narrative Generation to Combat Online Hate Speech},
  author    = {Safdari Fesaghandis, Zahra and Maity, Suman Kalyan},
  booktitle = {Proceedings of the International AAAI Conference on Web and Social Media (ICWSM)},
  year      = {2026}
}
```

---

## License

The **dataset** and **code** are released under the [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/) license. You are free to share and adapt the material for any purpose, provided appropriate credit is given.

---

## Contact

- **Zahra Safdari Fesaghandis** — Bilkent University, Ankara, Turkey · zahra.safdari@bilkent.edu.tr
- **Suman Kalyan Maity** — Missouri University of Science and Technology, Rolla, MO, USA · smaity@mst.edu

For questions about the dataset or code, please open a [GitHub Issue](https://github.com/zahrasafdari/ParsCN/issues).

---

## Acknowledgements

We thank the native Persian annotators for their careful work, and gratefully acknowledge the creators of [PHATE](https://aclanthology.org/2024.aaai-main), [MultiTarget-CONAN](https://arxiv.org/abs/2107.08720), and [HateXplain](https://arxiv.org/abs/2012.10289) for making their datasets publicly available.
