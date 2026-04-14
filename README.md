# DSA210_Project
DSA210 Spring'26 Individual Project
# Spotify Music Migration Analysis (2021)

This project investigates whether a song's international success stems primarily from its **linguistic accessibility** or its **technical audio characteristics** (Energy, Valence, etc.).

## Data Sources
*Note: Due to file size constraints, raw data files are not included in this repository.*
- `charts.csv`: Historical Spotify charts (via Dhruvil Dave on Kaggle). (https://www.kaggle.com/datasets/dhruvildave/spotify-charts/data)
- `Spotify_Dataset_V3.csv`: Audio features for Spotify tracks (via Bruno Alarcon on Kaggle). (https://www.kaggle.com/datasets/brunoalarcon123/top-200-spotify-songs-dataset)
- `ling_web.dta`: Linguistic proximity data (Melitz & Toubal).(https://www.cepii.fr/CEPII/en/bdd_modele/bdd_modele_item.asp?id=19)

## Key Findings

### 1. Statistical Impact of Audio Features (OLS Regression)
The multiple regression analysis ($R^2=0.02, p < 0.001$) reveals that musical characteristics are stronger predictors of migration speed than linguistic barriers:
- **Energy & Acousticness:** High-energy tracks ($\beta \approx -27.77$) and acoustic elements ($\beta \approx -22.31$) are the most potent accelerators, significantly reducing the migration lag.
- **Valence (Happiness):** Positive and happy songs ($\beta \approx -16.75$) travel faster across borders, suggesting that "musical emotion" acts as a universal language.
- **Loudness & Speechiness:** Higher loudness and speech density (likely reflecting the 2021 Rap/Hip-Hop trend) also correlate with faster international spread.

### 2. The Linguistic Barrier & The "Diaspora" Cheat Code
- **Linguistic Lag:** On average, songs migrating to "Distant Language" regions face an additional **9.12-day delay** compared to "Similar Language" regions.
- **The Diaspora Exception:** A case study on **Turkey (TUR)** shows near-instantaneous migration (1-day lag) to **Germany (DEU)** and **Austria (AUT)**. This proves that high-density immigrant populations can completely neutralize linguistic barriers.

### 3. Regional Scenario Analysis
- **Nordic Sync:** Represents the fastest migration speed (**6.02 days**), likely driven by high digital platform integration and cultural homogeneity.
- **Latin Highway:** Confirms that shared language facilitates a rapid "linguistic corridor" (**26.76 days**), significantly faster than the global average.
- **K-Pop Paradox:** Demonstrates that despite extreme linguistic distance (**109.59 days lag**), high technical energy ($0.738$) eventually allows cultural products to penetrate distant markets.

## AI Documentation & Methodology

As per academic integrity guidelines, the use of **Gemini (Google AI)** in this project is documented below. AI was used as a collaborative tool for technical implementation and data cleaning optimization.

### Specific Prompts Used:
1. *"Write a Python script to filter 3 million rows from charts.csv for the year 2021 and merge it with a Spotify audio features dataset using song titles and artist names."*
2. *"How can I use the `pycountry` library to convert country names to ISO3 codes, and how do I handle manual exceptions like 'Turkey' or 'USA'?"*
3. *"Construct a Multiple Linear Regression model using `statsmodels` to predict `Migration_Lag` based on `Barrier_Score`, `Energy`, `Valence`, `Speechiness`, `Acousticness`, and `Instrumentalness`."*
4. *"Create a regional scenario analysis script to compare the migration speed of K-Pop in Asia vs. Spanish music in Latin America."*
5. *"Generate a Seaborn heatmap to visualize the correlation between musical components and global spread speed."*

### Human Oversight:
While Gemini assisted with the coding syntax, the **research design, hypothesis formulation (Linguistic Barrier vs. Technical Signature), and interpretation of the cases** were developed independently by the author.
