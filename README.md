# Same Reality, Different Words
## Semantic Divergence in Colombian Labor Market Discourse

> *"The ILO and Colombian media are talking about the same workers — but not in the same language."*

**Author:** Paula Andrea Jaramillo Galeano  
**Program:** MSc Data Science for Society and Business (Online) — Constructor University Bremen  
**Course:** Text Analysis and Natural Language Processing  
**Date:** May 2026

---

## Project Overview

This project applies a seven-step computational NLP pipeline to detect, measure, and classify semantic divergence across three discourse registers describing Colombian labor informality:

| Register | Style | Label |
|---|---|---|
| **Institutional** | DANE / ILO statistical reports | 0 |
| **Popular Media** | Colombian newspaper narrative | 1 |
| **Critical Media** | Investigative journalism (La Silla Vacía style) | 2 |

The corpus consists of **180 synthetic texts** generated with Claude 3.5 Sonnet, equally distributed across registers. Analysis follows the *text-as-data* paradigm, treating lexical choices as measurable signals of discourse framing.

### Key Findings

- **Classifier accuracy:** 72.2% CV (±4.7%) — more than double the 33.3% random baseline
- **Popular Media F1:** 0.909 — the most semantically distinct register
- **Unexpected finding:** Institutional ↔ Critical Media centroid similarity = **0.741** — critical journalism adopts institutional vocabulary to document institutional failure, not to invent new language
- **All three hypotheses supported:** H1 (classifier performance), H2 (lexical divergence), H3 (boundary zone convergence)

---

## Repository Structure

```
same-reality-different-words/
│
├── README.md                          ← You are here
│
├── scripts/                         ← Run in order (Step 1 → Step 7)
│   ├── step1_preprocessing.ipynb
│   ├── step2_pos_tagging.ipynb
│   ├── step3_domain_dictionary.ipynb
│   ├── step4_tfidf_divergence.ipynb
│   ├── step5_cosine_similarity.ipynb
│   ├── step6_logistic_regression.ipynb
│   └── step7_final_visualization.ipynb
│
├── data/                              ← Pipeline inputs and intermediate outputs
│   ├── corpus_raw.csv                 ← Original 180 AI-generated texts
│   ├── corpus_preprocessed.csv        ← Step 1 output
│   ├── corpus_pos.csv                 ← Step 2 output
│   ├── corpus_domain.csv              ← Step 3 output
│   ├── cosine_similarity_results.csv  ← Step 5 output
│   └── classification_results.csv     ← Step 6 output
│
├── outputs/                           ← Model artifacts and figures
│   ├── domain_dictionary.json         ← 6-cluster, 115-term domain vocabulary
│   ├── tfidf_matrix.npz               ← Sparse TF-IDF matrix (180 × 171)
│   ├── tfidf_feature_names.json       ← Feature names for TF-IDF matrix
│   ├── tfidf_profiles_by_register.csv ← Mean TF-IDF weights per register
│   ├── misclassified_docs.csv         ← Step 6: 9 misclassified documents
│   └── figures/
│       ├── fig1_corpus_overview.png
│       ├── fig2_pos_profile.png
│       ├── fig3_domain_coverage.png
│       ├── fig4_tfidf_divergence.png
│       ├── fig5_cosine_similarity.png
│       ├── fig6_classification.png
│       └── fig7_master_dashboard.png
│
└── report/
    ├── NLP_ProjectReport_Jaramillo_v4.html     ← Full report (rendered)
    ├── NLP_ProjectReport_Jaramillo_v4.qmd      ← Quarto source
    ├── NLP_ExecutiveSummary_Jaramillo.docx     ← 1-page executive summary
    ├── references.bib                           ← APA-7 bibliography
    ├── apa.csl                                  ← Citation style
    └── styles.css                               ← Report stylesheet
```

---

## Pipeline

Each notebook is self-contained and produces a validated output file that serves as the direct input to the next step.

```
Raw corpus (180 texts)
    │
    ▼
Step 1 — Preprocessing          spaCy en_core_web_sm
    │  Tokenization, lemmatization, stopword removal
    │  → corpus_preprocessed.csv
    ▼
Step 2 — POS Tagging            spaCy en_core_web_sm
    │  NOUN / VERB / ADJ / PROPN / NUM ratios per document
    │  → corpus_pos.csv
    ▼
Step 3 — Domain Dictionary      Custom vocabulary (6 clusters, 115 terms)
    │  Filter to labor-economics domain vocabulary
    │  → corpus_domain.csv + domain_dictionary.json
    ▼
Step 4 — TF-IDF Divergence      scikit-learn TfidfVectorizer
    │  ngram_range=(1,2), min_df=2, max_df=0.95, sublinear_tf=True
    │  → tfidf_matrix.npz (180 × 171 features)
    ▼
Step 5 — Cosine Similarity      sklearn.metrics.pairwise
    │  Centroid similarity + document-level intra/inter gap
    │  → cosine_similarity_results.csv
    ▼
Step 6 — Logistic Regression    scikit-learn LogisticRegression
    │  Stratified 80/20 split + 5-fold CV | random_state=42
    │  → classification_results.csv + misclassified_docs.csv
    ▼
Step 7 — Visualization          matplotlib + seaborn
       Master dashboard (fig1–fig7)
       → figures/
```

---

## Requirements

### Python version
Python 3.9+

### Install dependencies

```bash
pip install -r requirements.txt
```

### `requirements.txt`

```
spacy==3.7.4
scikit-learn==1.4.0
pandas==2.2.0
numpy==1.26.4
matplotlib==3.8.2
seaborn==0.13.2
scipy==1.12.0
jupyter==1.0.0
```

### spaCy language model

```bash
python -m spacy download en_core_web_sm
```

---

## How to Run

1. Clone the repository:
```bash
git clone https://github.com/<your-username>/same-reality-different-words.git
cd same-reality-different-words
```

2. Install dependencies:
```bash
pip install -r requirements.txt
python -m spacy download en_core_web_sm
```

3. Run notebooks in order:
```bash
jupyter notebook
```
Open and execute each notebook from `step1_preprocessing.ipynb` through `step7_final_visualization.ipynb`. Each notebook reads from the previous step's output — **do not skip steps**.

4. To render the full report (requires [Quarto](https://quarto.org/)):
```bash
cd report/
quarto render NLP_ProjectReport_Jaramillo_v4.qmd --to html
```

---

## AI Use Declaration

This project used AI assistance within the module's 25% policy limit:

| Component | Tool | Role |
|---|---|---|
| Corpus generation | Claude 3.5 Sonnet (`claude-3-5-sonnet-20241022`) | Generated all 180 synthetic texts via structured prompts. Full prompt templates documented in [Appendix A of the report](report/NLP_ProjectReport_Jaramillo_v4.html). Prompt IDs: A001–A060 (Institutional), B001–B060 (Popular Media), C001–C060 (Critical Media). Generation date: April 2026. |
| Code review | GitHub Copilot | Reviewed and debugged Python code in Steps 1–7. No code generated wholesale by AI. |
| Report scaffolding | Claude Sonnet 4.6 | Assisted with initial section structure and paragraph drafting. All interpretations, analytical decisions, hypotheses, and conclusions are the author's own. |

**Estimated total AI contribution: ~20% of project output.**

---

## References

- Biber, D. (1995). *Dimensions of register variation: A cross-linguistic comparison*. Cambridge University Press.
- Grimmer, J., & Stewart, B. M. (2013). Text as data: The promise and pitfalls of automatic content analysis methods for political texts. *Political Analysis*, *21*(3), 267–297.
- Honnibal, M., Montani, I., Van Landeghem, S., & Boyd, A. (2020). *spaCy: Industrial-strength natural language processing in Python* [Computer software]. https://doi.org/10.5281/zenodo.1212303
- ILO. (2023). *World employment and social outlook: Trends 2023*. International Labour Organization.
- Pedregosa, F., et al. (2011). Scikit-learn: Machine learning in Python. *Journal of Machine Learning Research*, *12*, 2825–2830.

---

## License

This repository is submitted as academic coursework. All code and analysis are the original work of the author unless otherwise stated in the AI use declaration above.
