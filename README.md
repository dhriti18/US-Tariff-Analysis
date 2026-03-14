# US Tariffs Analysis: Machine Learning for Trade Policy Risk Assessment

Quantifying the impact of US tariff policies on international trade flows using machine learning to identify country-level risk exposure.

## Project Overview

This project analyzes 204 countries to assess trade vulnerability under US tariff policies. Using regression, classification, and clustering models, it identifies key indicators of economic risk and provides a systematic framework for portfolio risk management.

**Built for:** Asset management risk assessment and geopolitical analysis

## Key Results

| Metric | Value | Description |
|--------|-------|-------------|
| **Classification Accuracy** | 73% | Cross-validated performance on risk tier prediction |
| **High Risk Detection** | 100% | Perfect precision and recall on most critical category |
| **Clustering Validation** | 0.686 | Silhouette score confirming natural risk groupings |
| **Top Predictor** | Export_Import_Ratio (63.5%) | Trade balance emerged as strongest risk indicator |

## Key Findings

### 1. Regression Models Failed - Data Structure Issue

Both Decision Tree and Random Forest regression models achieved negative median R² (-0.25), performing worse than baseline predictions.

**Root cause:** Extreme concentration - 5 countries (China, EU, Mexico, Vietnam, Taiwan) represent 75% of total US trade deficit. With only 204 countries split into cross-validation folds, models trained on smaller economies couldn't generalize when major economies appeared in test sets.

### 2. Classification Succeeded - Risk Tiers Approach

Random Forest Classifier achieved 73% accuracy by predicting High/Medium/Low risk categories rather than exact deficit amounts.

**Why it worked:** 
- 51 High Risk examples (vs. only 5 extreme deficits in regression)
- Model learns characteristics of risk, not exact dollar amounts
- More robust to outlier magnitude

### 3. Three Key Sensitivity Indicators

Feature importance analysis identified:

1. **Export_Import_Ratio (63.5%)** - Trade balance vulnerability  
   *Countries importing 3x more than exporting face structural risk*

2. **Tariff_Gap (19.5%)** - Policy asymmetry pressure  
   *Gap >30% between US tariff threats and country retaliation indicates elevated exposure*

3. **Tariff_Proposed (15.7%)** - Absolute tariff level  
   *Baseline tariff rate provides additional context*

### 4. Unsupervised Validation

K-Means clustering independently discovered the same risk patterns:
- **100% pure High Risk cluster** (44/44 countries correctly grouped)
- 59% overall alignment with supervised categories
- Confirms risk tiers reflect genuine data structure, not arbitrary classification

## Dataset

**Source:** US trade data (2024) covering 204 countries

**Features:**
- US trade deficit with each country
- Proposed US tariff rates
- Expected country retaliation rates
- Bilateral trade volumes
- Population data

**Engineered Features:**
- Tariff_Gap (Proposed tariff - Expected response)
- Export_Import_Ratio (US Exports / US Imports)
- Composite risk score (for categorization)

## Methodology

### 1. Data Preprocessing
- Cleaned percentage symbols and numeric formatting
- Handled missing values (32 countries without population data)
- Capped extreme outliers (Export_Import_Ratio >10)

### 2. Exploratory Analysis
- Correlation heatmap (identified weak linear relationships, max r=0.31)
- Distribution analysis (revealed concentration in 5 countries)
- Feature engineering (created policy asymmetry metrics)

### 3. Risk Categorization
Initial approach (deficit quartiles) → 58% accuracy  
Revised approach (composite score: -Deficit/100k + Tariff_Gap/10) → 73% accuracy

### 4. Model Development

**Regression Models (Failed):**
- Decision Tree: Median R² = -0.106
- Random Forest: Median R² = -0.251
- Issue: High variance, catastrophic performance on folds with China/EU

**Classification Model (Successful):**
- Random Forest Classifier: 73% accuracy
- Perfect High Risk identification (100% precision, 100% recall)
- Stable cross-validation (std = 0.026)

**Validation:**
- K-Means Clustering: 3 clusters, silhouette = 0.686
- PCA visualization: 99.5% variance in 2 dimensions

## Business Application

### Portfolio Risk Framework

**High Risk Countries (Perfect Detection):**
- Characteristics: Export_Ratio <0.5 AND Tariff_Gap >30%
- Action: Reduce position sizing by 50%
- Examples: China, Vietnam (correctly identified 100% of the time)

**Medium/Low Risk Countries:**
- Standard position sizing rules
- Monthly monitoring for threshold changes

**Quantitative Thresholds:**
```
IF Export_Import_Ratio < 0.5 AND Tariff_Gap > 30%:
    Risk = High → Reduce exposure
ELIF Tariff_Gap < 15% AND Export_Import_Ratio > 1.0:
    Risk = Low → Standard allocation
ELSE:
    Risk = Medium → Monitor closely
```

## Technologies

**Core:**
- Python 3.11
- Jupyter Notebook

**Libraries:**
- scikit-learn 1.3+ (RandomForestClassifier, KMeans, PCA)
- pandas 2.0+
- numpy 1.24+
- plotly 5.14+ (interactive visualizations)

**Models:**
- Decision Tree Regressor
- Random Forest Regressor
- Random Forest Classifier (n_estimators=300, max_depth=12)
- K-Means Clustering (k=3)
- PCA (dimensionality reduction)

## Project Structure
```
US-Tariff-Analysis/
├── USTariffsAnalysis.ipynb          # Main analysis notebook
├── data/
│   └── Tariff_Calculations_plus_Population.csv
├── visualizations/                   # (Generated from notebook)
│   ├── correlation_heatmap.png
│   ├── distribution_dashboard.png
│   ├── confusion_matrix.png
│   ├── feature_importance.png
│   └── pca_clusters.png
├── README.md
└── requirements.txt
```

## Installation & Usage

### Setup
```bash
# Clone repository
git clone https://github.com/yourusername/US-Tariff-Analysis.git
cd US-Tariff-Analysis

# Install dependencies
pip install -r requirements.txt

# Launch notebook
jupyter notebook USTariffsAnalysis.ipynb
```

### Requirements
```txt
pandas>=2.0.0
numpy>=1.24.0
scikit-learn>=1.3.0
plotly>=5.14.0
jupyter>=1.0.0
```

### Running the Analysis

1. Open `USTariffsAnalysis.ipynb`
2. Run all cells sequentially (Cell → Run All)
3. Analysis completes in ~2-3 minutes
4. Interactive visualizations will render inline

## Results Reproducibility

All models use `random_state=42` for reproducibility. Running the notebook will produce identical results:
- Same train/test splits
- Same cross-validation folds
- Same cluster assignments

## Visualizations

**Included:**
- Correlation heatmap (49 feature correlations)
- 4-panel distribution dashboard
- Confusion matrix (classification performance)
- Feature importance bar charts
- PCA cluster visualization (2D projection)
- Risk category distribution (pie charts)

## Lessons Learned

### What Worked
- Iterative categorization (improved accuracy from 58% → 73%)
- Composite risk scoring (aligned labels with features)
- Unsupervised validation (confirmed patterns were real)

### What Didn't Work
- Regression on concentrated data (negative R²)
- Deficit-only categorization (misaligned with features)

### Key Insight
**Label quality matters as much as model architecture.** The jump from 58% to 73% accuracy came entirely from better risk categorization, not from model tuning.

## Future Enhancements

- [ ] Time-series analysis (tariff policy changes over time)
- [ ] Additional features (GDP, industry composition, trade agreements)
- [ ] Deep learning approach (neural networks for pattern detection)
- [ ] Real-time dashboard (live tariff policy monitoring)
- [ ] Sector-level analysis (industry-specific vulnerability)


**Last Updated:** March 2025  
**Status:** Complete ✓  
**Notebook Runtime:** ~2-3 minutes
