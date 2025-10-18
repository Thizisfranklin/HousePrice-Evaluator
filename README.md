# House Prices — Data Preprocessing & Feature Engineering

## Overview
This project applies a full **data-science pipeline** to a small house-prices dataset. The goal is to **clean and enrich messy real-estate data** so it supports reliable analytics and, ultimately, price prediction. Following a structured workflow—duplicates → missing values → outliers → normalization/encoding → feature engineering → multivariate analysis—I show practical preprocessing skills and how they translate into **business-ready** insights for property valuation.

---

## Dataset
- **Rows:** 110 residential properties  
- **Columns:** `ID`, `Size(sqft)`, `Bedrooms`, `Badhrooms` *(renamed → `Bathrooms`)*, `Location`, `House_Type`, `Year_Built`, `Date_Sold`, `Price`  
- **Issues identified:**
  - **Missing values:** Size (12), Bedrooms (8), Bathrooms (20), Year_Built (11), Price (5)
  - **Typo:** `Badhrooms` → `Bathrooms`
  - **Unrealistic values:** negative/zero square footage, extremely large sizes (~500,000 sqft), implausible construction years (> 2025)
  - **Small sample:** 110 rows → careful regularization and validation needed

---

## Data Cleaning & Preprocessing
**1) Duplicates**  
Checked entire dataset; **no duplicate rows** detected.

**2) Missing values**  
- Numeric (`Size(sqft)`, `Bedrooms`, `Bathrooms`, `Year_Built`, `Price`): **median imputation** after fixing invalid entries (e.g., negative sqft and >2025 `Year_Built` set to `NaN` first).  
- Categorical (`Location`, `House_Type`): **mode imputation**.

**3) Outliers**  
- **Z-score** to flag >3σ extremes.  
- **Winsorization** to cap tails at the 1st/99th percentiles (e.g., extreme sqft and a ~\$75M price capped to reduce distortion).

**4) Normalization & scaling**  
- **Standardization (z-score):** `Size(sqft)`, `Price`  
- **Mean normalization:** `Bedrooms`, `Bathrooms`  
- **Min–Max scaling:** `Year_Built` → [0, 1]

**5) Encoding**  
- **Label encoding:** `Location` (neighborhoods → integers)  
- **One-hot encoding:** `House_Type` (detached, semi-detached, townhouse, …)

---

## Feature Engineering
Created signal-rich variables to stabilize scale effects and capture structure:
- **House Age:** `Age = 2025 - Year_Built` (older homes may sell for less due to maintenance)
- **Price per SqFt:** `PricePerSqFt = Price / Size(sqft)` (size-normalized pricing)
- **Bath-to-Bed Ratio:** `BathBedRatio = Bathrooms / Bedrooms` (occupancy suitability)

All engineered features were appropriately scaled and included in the analysis.

---

## Multivariate Analysis
- **Correlation heatmap:** moderate positive link between `Bathrooms` and `Price`; `Size(sqft)`/`Bedrooms` weaker after outlier treatment.  
- **Pairplot:** larger homes with more bathrooms **tend** to command higher prices, though variance remains high.

> *Figures (optional in repo):* `house_correlation_heatmap.png`, `house_pairplot.png`

---

## Snapshot

<img width="926" height="832" alt="image" src="https://github.com/user-attachments/assets/85f6cfb9-b295-4b6c-87c5-0b07276f55ee" />

---

## Tools & Tech
**Python 3.10** • **pandas** • **NumPy** • **scikit-learn** (imputation, scaling, encoding, models) • **seaborn** & **matplotlib** (EDA/plots) • **Jupyter Notebook** (experiments) • **Git/GitHub** (versioning)

---

## Business Value
A structured preprocessing pipeline transforms a messy spreadsheet into a **trustworthy foundation** for:
- More accurate valuations (pricing guidance for agents/sellers)
- Buyer decision tools (e.g., best value by **PricePerSqFt**)
- Location/type trend analysis for investment and planning

---

## Limitations & Future Work
- **Small-N:** 110 rows → add more data across locations/time; perform **cross-validation**  
- **Imputation uncertainty:** explore **multiple imputation (MICE)** with sensitivity checks  
- **Models:** try robust ensembles (GBM/XGBoost), systematic hyper-parameter tuning  
- **Richer features:** amenities, schools, transit, crime, seasonality; spatial encodings (geo-features)

---

## Getting Started
1) **Clone** the repo and create a Python env  
2) **Place** `house_prices-1.csv` in `data/` (or repo root)  
3) **Open** the notebook (`Lab_2_Osualaaham.ipynb`) or run your pipeline script

```bash
python -m venv .venv && source .venv/bin/activate      # or conda
pip install -U pandas numpy scikit-learn seaborn matplotlib jupyter
jupyter lab  # open Lab_2_Osualaaham.ipynb
