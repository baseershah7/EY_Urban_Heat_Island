Urban Heat Island (UHI) Prediction Model - NYC Bronx & Manhattan
🏆 3rd Place Solution on EY Urban Heat Island Competition Leaderboard
This repository contains my solution for modeling and predicting the Urban Heat Island (UHI) effect in New York City's Bronx and Manhattan regions. The model achieved a leaderboard score of 0.9837, securing 3rd place in the competition and earning recognition as a global semi-finalist.
Project Overview
The Urban Heat Island effect refers to the phenomenon where urban areas experience significantly higher temperatures than surrounding rural areas due to human activities and urban infrastructure. This project leverages multiple satellite and urban datasets to create a robust predictive model of UHI intensity.
Datasets
The solution integrates a diverse range of datasets to capture the complex factors driving urban heat:

Satellite Data: Landsat thermal bands and Sentinel-2 imagery
Urban Infrastructure: NYC Building Footprints, Tree Census, Traffic data
Environmental Factors: Elevation points, forestry data, weather measurements
Network Analysis: OSMNX street networks for connectivity metrics

Methodology
Data Processing

Generated 1,700+ features from multiple datasets
Created spatial buffer analyses (500m-15,000m) to capture geographic relationships
Implemented temporal aggregation techniques to ensure model stability

Feature Engineering

Developed specialized features capturing building-vegetation interactions
Created thermal mass indicators and vertical density metrics
Engineered elevation gradients and building height dispersion ratios

Model Architecture

Single Model: Extra Trees Regressor optimized with Optuna (CV score: 0.9824)
Ensemble Solution: 27 diverse Extra Trees Regressors with polynomial kernel Ridge meta-learner
Implemented 10-fold Out-of-Fold (OOF) validation framework to ensure generalizability

Results
The final ensemble model achieved:

Leaderboard score: 0.9837
Competition ranking: 3rd place
Recognition: Global Semi-Finalist

Key Insights
The model revealed important relationships between urban heat intensity and:

Vegetation coverage (particularly EVI from Sentinel-2)
Building density and height profiles
Street network configuration
Tree canopy characteristics

Repository Structure
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
Usage
[Instructions for running the model, any dependencies, etc.]
Acknowledgements
Thanks to EY for organizing this competition and providing the opportunity to work on such an important environmental challenge.
