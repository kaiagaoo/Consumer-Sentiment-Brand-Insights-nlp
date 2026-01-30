# Amazon Fashion Consumer Insights

**Authors:** Kaia Gao, Fuguan Obrien, & Pei Zheng <br>
**Course:** COMPSS 211: Advanced Computing, University of California, Berkeley <br>
**Date:** December 12, 2025

[View the Full Presentation (PDF)](Final_presentation.pdf)

---

## Project Overview

This project develops a comprehensive, multi-layered analytical pipeline to transform large-scale Amazon Fashion reviews into interpretable signals for strategic planning. By analyzing rich, unstructured text, we aim to understand how consumer sentiment fluctuates, what themes dominate the conversation, and which specific product attributes drive satisfaction.

The analysis is divided into three complementary components:
1.  **Seasonal Trend Analysis:** Investigating temporal shifts in sentiment and demand.
2.  **Topic Modeling:** Uncovering dominant themes and concerns using BERTopic.
3.  **Aspect-Based Sentiment Analysis (ABSA):** Extracting fine-grained opinions on features like "size" and "material" to determine rating drivers.

## Data

We utilized the **Amazon Fashion Reviews (2023)** dataset, selecting a sample from **2020–2023** to align with computing capacity.

* **Sample Size:** ~831,000 reviews (downsampled from 2.5M).
* **Data Content:** Rich textual reviews, star ratings, timestamps, and product metadata.
* **Access:** [Download the CSV version of the data here](https://drive.google.com/file/d/1_pOWVAUB7qOKym1NOYRj_QV5kTJy5Xvw/view?usp=sharing).

## Methodology

### 1. Seasonal Trend Analysis
* **Goal:** Understand temporal changes in consumer behavior and sentiment.
* **Methods:**
    * Categorized reviews by season (Winter, Spring, Summer, Fall).
    * Applied **TF-IDF** to identify critical season-specific words.
    * Utilized **Gemini-2.5-flash** (LLM) for nuanced sentiment analysis and trend annotation on sampled data.

### 2. Topic Modeling
* **Goal:** Identify dominant themes and rank them by user satisfaction.
* **Pipeline:**
    * **Embeddings:** `all-MiniLM-L6-v2` sentence transformers.
    * **Clustering:** Dimensionality reduction followed by **BERTopic** to extract coherent topics.
    * **Ranking:** Calculated average star ratings per topic to identify "positive" vs. "negative" clusters.

### 3. Aspect-Based Sentiment Analysis (ABSA)
* **Goal:** Pinpoint fine-grained sentiment toward specific attributes (e.g., Fit, Material, Delivery).
* **Pipeline:**
    * **Extraction:** Used **PyABSA** to discover raw aspect terms.
    * **Taxonomy:** Applied UMAP clustering to organize terms into high-level categories (Taxonomy Construction).
    * **Annotation:** Implemented high-precision annotation using **Gemini-2.0-Flash** on 831K reviews with batching and rate-limiting strategies.
    * **Prediction:** Used **XGBoost Regression** to determine which aspects most strongly influence star ratings and review helpfulness.

## Key Findings

### Seasonal Dynamics
* **Positive Seasons:** Fall and Spring see the highest positive sentiment, likely due to comfortable, transitional clothing.
* **Negative Seasons:** Summer has the highest negative sentiment, driven by functional complaints regarding breathability and fit.
* **Pain Points:** "Fit" and "Sizing" are consistent issues across all seasons.

### Topic Analysis
* **Highest Rated (Avg ~4.8):** Topics related to **Gifts** (emotional value) and **Quality/Price Fit** ("great deal").
* **Lowest Rated (Avg ~1.1):** Topics dominated by words like "Junk," "Waste," and "Garbage," reflecting quality failures and misleading descriptions.

### Aspect Importance
* **Rating Drivers:** **Material** and **Size** are the most critical predictors of star ratings, outweighing style or shipping.
* **Overall Sentiment:** Broad emotional expressions (e.g., "Great product") often override specific attributes in determining the final rating.
* **Helpfulness:** Reviews containing detailed information on size and material are perceived as more helpful by other users.


## License & Acknowledgements
This project was completed for the Master’s of Computational Social Science program at UC Berkeley under the supervision of Dr. Van Nuenen.


Repository Structure
--------------------
```
project-root/
├── .devcontainer/
│   └── devcontainer.json
│
├── .vscode/
│   └── settings.json
│
├── data/
│   └── .DS_Store
│
├── scripts/
│   └── checkpoints/
│       ├── ATEPC_ENGLISH_CHECKPOINT/
│       │   ├── fast_lcf_atepc.args.txt
│       │   ├── fast_lcf_atepc.config
│       │   └── fast_lcf_atepc.tokenizer
│       │
│       └── ATEPC_MULTILINGUAL_CHECKPOINT/
│           ├── fast_lcf_atepc.args.txt
│           ├── fast_lcf_atepc.config
│           └── fast_lcf_atepc.tokenizer
│
├── notebooks/
│   ├── .gitkeep
│   ├── Pei_NLP_Analysis.ipynb
│   ├── colab_absa_llm.ipynb
│   ├── data_cleaning.ipynb
│   ├── llm_absa_analysis.ipynb
│   ├── seasonal_analysis.ipynb
│   ├── subset_absa_analysis.ipynb
│   ├── subset_absa_extraction.ipynb
│   ├── winter_analysis.ipynb
│   └── checkpoints.json
│
├── src/
│   └── fashion/
│       ├── preprocessing.py
│       ├── __init__.py
│       └── .DS_Store
│
├── .gitignore
├── README.md
├── environment.yml
└── pyproject.toml
```
