# Urban Heat Island (UHI) Prediction Model - NYC Bronx & Manhattan

## 🏆 3rd Place Solution on EY Urban Heat Island Competition Leaderboard

This repository contains my solution for modeling and predicting the Urban Heat Island (UHI) effect in New York City's Bronx and Manhattan regions. The model achieved a leaderboard score of 0.9837, securing 3rd place in the competition and earning recognition as a global semi-finalist.

## Project Overview

The Urban Heat Island effect refers to the phenomenon where urban areas experience significantly higher temperatures than surrounding rural areas due to human activities and urban infrastructure. This project leverages multiple satellite and urban datasets to create a robust predictive model of UHI intensity.

## Datasets

The solution integrates a diverse range of datasets to capture the complex factors driving urban heat:

- **Satellite Data**: Landsat thermal bands and Sentinel-2 imagery
- **Urban Infrastructure**: NYC Building Footprints, Tree Census, Traffic data
- **Environmental Factors**: Elevation points, forestry data, weather measurements
- **Network Analysis**: OSMNX street networks for connectivity metrics

| Dataset Name | Features Extracted | Source |
|-------------|-------------------|--------|
| Landsat Satellite Data | Surface temperature (LWIR11), thermal variation metrics, temporal stability indices | Microsoft Planetary Computer |
| Sentinel-2 Satellite Data | EVI, Albedo, vegetation stress indices, spectral anomaly scores | Microsoft Planetary Computer |
| NYC Tree Census (2015) | Tree count, avg_tree_health, tree_size, ring features | NYC Open Data |
| NYC Building Footprints | Building count/height/area, ground elevation, ring features | NYC Open Data |
| NYC Traffic 2022 | Traffic volume, traffic count, ring features | NYC Open Data |
| OSMNX Street Networks | Distance to water/park, street length, road/node density | OSMNX Library |
| NYC Planimetric Database | Elevation, elevation range, slope | NYC Open Data |
| Weather Data | Air temperature, humidity, wind speed, solar flux, wind direction | NY Mesonet |
| Forestry Data | Mean tree DBH, forestry count, risk score, species details | Center for Open Science |

## Methodology

### Data Processing
- Generated 1,700+ features from multiple datasets
- Created spatial buffer analyses (500m-15,000m) to capture geographic relationships
- Implemented temporal aggregation techniques to ensure model stability
- Null values in buffer-based features were imputed with 0 to reflect absence of structures or vegetation

### Feature Engineering
- Developed specialized features capturing building-vegetation interactions
- Created thermal mass indicators and vertical density metrics
- Engineered elevation gradients and building height dispersion ratios
- Used RFECV to select optimal features (60 for single model, ~220 for ensemble)

### Model Architecture

#### Single Model Approach
- Extra Trees Regressor optimized with Optuna
- Parameters: `ExtraTreesRegressor(n_estimators=200, max_depth=55)`
- CV/LB score: 0.9824

#### Ensemble Solution
- 27 diverse Extra Trees Regressors:
  - 24 ETRs trained on unscaled Dataset A (220 features)
  - 3 ETRs trained on Standard-Scaled Dataset B (221 features)
- Meta-learner: Ridge regression with polynomial kernel
  - Parameters: `KernelRidge(alpha=0.00001, kernel='poly', degree=2, coef0=0.25)`
- 10-fold Out-of-Fold (OOF) validation framework
- Leaderboard score: 0.9837

## Results

The final ensemble model achieved:
- Leaderboard score: 0.9837
- Competition ranking: 3rd place
- Recognition: Global Semi-Finalist

## Key Insights

The model revealed important relationships between urban heat intensity and:
- Vegetation coverage (particularly EVI from Sentinel-2)
- Building density and height profiles
- Street network configuration
- Tree canopy characteristics

## Repository Structure

```
├── notebooks/
│   ├── data_preprocessing.ipynb
│   ├── feature_engineering.ipynb
│   ├── single_model.ipynb
│   └── ensemble_model.ipynb
├── models/
│   ├── base_models/
│   └── meta_model/
├── data/
│   └── processed/
├── README.md
└── requirements.txt
```

## Usage

1. Clone this repository
2. Install requirements: `pip install -r requirements.txt`
3. Run the notebooks in order to reproduce the solution:
   - First process the data with `data_preprocessing.ipynb`
   - Then engineer features with `feature_engineering.ipynb`
   - Train models with `single_model.ipynb` or `ensemble_model.ipynb`

## Acknowledgements

Thanks to EY for organizing this competition and providing the opportunity to work on such an important environmental challenge addressing urban heat islands, which have significant impacts on public health, energy consumption, and quality of life in cities worldwide.
