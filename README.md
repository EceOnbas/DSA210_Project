# DSA210_Project
DSA210 Spring'26 Individual Project
# Global Music Diffusion Analysis  
**How language, audio features, and structure shape the international spread of songs**

## Data Sources
*Note: Due to file size constraints, raw data files are not included in this repository.*
- `charts.csv`: Historical Spotify charts (via Dhruvil Dave on Kaggle). (https://www.kaggle.com/datasets/dhruvildave/spotify-charts/data)
- `Spotify_Dataset_V3.csv`: Audio features for Spotify tracks (via Bruno Alarcon on Kaggle). (https://www.kaggle.com/datasets/brunoalarcon123/top-200-spotify-songs-dataset)
- `ling_web.dta`: Linguistic proximity data (Melitz & Toubal).(https://www.cepii.fr/CEPII/en/bdd_modele/bdd_modele_item.asp?id=19)

---

## What this project asks

The goal of this project is to understand:

> **What determines how fast a song spreads from its home country to the rest of the world?**

To answer this, I track when a song first appears in its origin country and how long it takes to appear elsewhere.

This delay is defined as:

> **Migration Lag (days)**

Across the dataset, the average migration lag is about **36 days**, but there is substantial variation around this.

---

## A key issue in the data

Before modeling, there is an important structural problem:

- **71.7% of observations are right-censored**

This means that for most songs, we do not observe the full diffusion process — they are still spreading when the dataset ends.

This has a direct implication:

> A standard regression (OLS) would treat these as slow-moving songs, even though their final spread is unobserved.

Because of this, OLS estimates are biased. To address it, the main analysis uses a **Cox proportional hazards model**, which is designed for time-to-event data with censoring.

---

## Main result: linguistic barriers

The central variable in the analysis is the **Barrier Score**, which captures linguistic distance between countries.

From the Cox model:

- Hazard ratio = **0.554**
- 95% CI: **[0.532, 0.576]**
- p < **0.001**

Interpretation:

> Holding everything else constant, an increase in linguistic barrier reduces the instantaneous probability of a song spreading by about **45%**.

In practical terms:

- Songs crossing large language differences take **substantially longer** to spread  
- This is one of the dominant constraints in global music diffusion  

---

## Role of audio features

Audio characteristics do matter, but their effects are smaller and more uneven.

### Positive effects

- **Speechiness (HR = 2.03)**  
  Songs with more spoken or lyrical content spread roughly **twice as fast**

- **Acousticness (HR = 1.85)**  
  More acoustic songs spread about **85% faster**

- **Energy (HR = 1.19)**  
  Higher energy is associated with a **moderate (~19%) increase** in speed

- **Valence (HR = 1.13)**  
  More positive songs spread slightly faster

### Negative effects

- **Danceability (HR = 0.74)**  
  More danceable songs spread about **26% slower**

- **Instrumentalness (HR = 0.55)**  
  Instrumental tracks spread about **45% slower**

- **Loudness (HR = 0.17)**  
  Initially appears to strongly slow diffusion (see below)

---

## Why model choice matters (Mixed Effects)

To control for artist-level differences, a mixed model is estimated with:

- **598 artists**
- **~300,000 observations**

Key result:

- **Loudness becomes insignificant (p = 0.088)**

Interpretation:

> The loudness effect observed earlier was a false positive driven by artist-level clustering.

All other main variables, including Barrier Score, remain highly significant.

---

## Interaction: can strong songs overcome language?

An interaction term is included:

- Energy × Barrier

Results:

- Cox model: not significant (p = 0.924)  
- Mixed model: significant and negative (coef = -1.132)

Interpretation:

> There is no evidence that high-energy songs overcome language barriers. In fact, under more realistic modeling, they may perform worse when barriers are high.

---

## Model performance

### Survival model

- **C-index = 0.607**

Indicates moderate predictive ability, typical for complex social processes.

### Machine learning validation

- Random Forest accuracy: **0.747** (vs. 0.517 baseline)
- XGBoost ROC-AUC: **0.938**
- Cross-validation: **0.932 ± 0.003**

Interpretation:

> The identified patterns generalize well and support strong predictive performance.

### Regression performance

- MAE ≈ **19 days**
- R² = **0.641**

The model explains about **64% of the variation** in diffusion time.

---

## Scenario-based insights

### K-Pop

- Global average lag: **36.0 days**
- K-Pop average lag: **64.5 days**
- Barrier Score: **1.0**

Interpretation:

> K-Pop spreads significantly slower than average due to maximum linguistic barriers, despite strong global fandom.

---

### Latin America (shared language)

- p < **0.001**
- Effect ≈ **3.1 days faster**

Interpretation:

> Shared language leads to consistently faster diffusion.

---

### Nordic countries

- p < **0.001**
- Effect ≈ **5.2 days faster**

Interpretation:

> Cultural and linguistic proximity further accelerates diffusion.

---

## Network structure

The diffusion process follows structured pathways rather than random spread.

### Receiving hubs (PageRank)

- Switzerland: **0.0418**
- Austria: **0.0254**
- Sweden: **0.0237**

### Relay countries (Betweenness)

- Morocco: **0.158**
- Italy: **0.073**
- Austria: **0.071**

Interpretation:

> Some countries act as hubs or bridges, facilitating global music flow across regions.

---

## Diagnostics

- VIF:
  - Energy: **31.7**
  - Energy × Barrier: **43.1**

Indicates multicollinearity (expected with interaction terms).

Handled using:

> **Penalized Cox model (λ = 0.1)**

---

## Limitations

- Moderate model fit (C-index = 0.607)  
- Observational data (no strict causality)  
- Simplified linguistic barrier measure  
- Counterfactual results depend on model assumptions  

---

## Conclusion

Across all models and robustness checks, the same conclusion emerges:

> Linguistic barriers are a central constraint on global music diffusion.

Audio features influence performance, but they do not override structural factors like language and cultural proximity.

---

## AI Documentation & Methodology

As per academic integrity guidelines, the use of **Gemini (Google AI)** and Claude in this project is documented below. AI was used as a collaborative tool for technical implementation and data cleaning optimization.

### Specific Prompts Used:
1. *"Write a Python script to filter 3 million rows from charts.csv for the year 2021 and merge it with a Spotify audio features dataset using song titles and artist names."*
2. *"How can I use the `pycountry` library to convert country names to ISO3 codes, and how do I handle manual exceptions like 'Turkey' or 'USA'?"*
3. *"Construct a Multiple Linear Regression model using `statsmodels` to predict `Migration_Lag` based on `Barrier_Score`, `Energy`, `Valence`, `Speechiness`, `Acousticness`, and `Instrumentalness`."*
4. *"Create a regional scenario analysis script to compare the migration speed of K-Pop in Asia vs. Spanish music in Latin America."*
5. *"Generate a Seaborn heatmap to visualize the correlation between musical components and global spread speed."*
6. *"Optimize a Python script to perform memory-efficient merging of 2021 Spotify Charts data with a linguistic distance dataset (`ling_web.dta`) using ISO3 country codes."*
7.  *"Implement a **Cox Proportional Hazards (Cox PH)** model using the `lifelines` library to analyze song migration speed, including 95% confidence intervals and hazard ratios."*
8.   "Construct a **Mixed Linear Model (MixedLM)** with `statsmodels` to account for artist-level random effects while controlling for linguistic barriers and musical features."*
9.    *"Create an advanced Machine Learning pipeline using **XGBoost** for predicting 'Fast Hits' (lag < 14 days) and integrate **SHAP (SHapley Additive exPlanations)** to visualize feature importance and directional influence."*
10. *"Develop a **Network Analysis** script using `networkx` to calculate PageRank and Betweenness Centrality for identifying global music relay hubs."*
11. *"Refactor a complex 3x3 Matplotlib dashboard to fix subplot overlapping, handle 1D array errors in Kaplan-Meier plotting, and ensure SHAP summary plots are rendered in separate figures for clarity."*

### Human Oversight:
While Gemini assisted with the coding syntax, the **research design, hypothesis formulation, and interpretation of the cases** were developed independently by the author.

---

## 🛠️ Reproduction Guide

Follow these steps to replicate the analysis pipeline locally on your machine.

### 1. Prerequisites & Environment Setup
Make sure you have **Python 3.9+** installed. Clone this repository and navigate to its root directory:

```bash
git clone [https://github.com/EceOnbas/DSA210_Project.git](https://github.com/EceOnbas/DSA210_Project.git)
cd DSA210_Project

# Create a virtual environment
python -m venv venv

# Activate the environment
# On Windows:
venv\Scripts\activate

# On macOS/Linux:
source venv/bin/activate

pip install -r requirements.txt

jupyter notebook Data_Analysis_Project_Milestone2.ipynb

