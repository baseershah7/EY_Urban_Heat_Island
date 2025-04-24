# Urban Heat Island (UHI) Prediction Model - NYC Bronx & Manhattan

## 🏆 3rd Place Solution on EY Urban Heat Island Competition Leaderboard

This repository contains my solution for modeling and predicting the Urban Heat Island (UHI) effect in New York City's Bronx and Manhattan regions. The model achieved a leaderboard score of 0.9837, securing 3rd place in the competition and earning recognition as a global semi-finalist by the organizers.

## Project Overview

The Urban Heat Island effect refers to the phenomenon where urban areas experience significantly higher temperatures than surrounding rural areas due to human activities and urban infrastructure like buildings concrete absorbing more heat compared to the rural part where the vegetation and trees ratio is much higher. This project leverages multiple satellite and urban datasets to create a robust predictive model of UHI intensity.

## Datasets

The solution integrates a diverse range of datasets to capture the complex factors driving urban heat:

- **Satellite Data**: Landsat thermal bands and Sentinel-2 imagery
- **Urban Infrastructure**: NYC Building Footprints, Tree Census, Traffic data
- **Environmental Factors**: Elevation points, forestry data, weather measurements
- **Network Analysis**: OSMNX street networks for connectivity metrics

| **Dataset Name**                  | **Approach**                                                                                     | **Features Extracted**                                                                 | **URL**                                                                 |
|------------------------------------|--------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| Landsat Satellite Data            | Scaled thermal bands (LWIR11) to Celsius; weighted mosaic for temporal generalization           | Surface temperature (LWIR11), thermal variation metrics, temporal stability indices.  | [Microsoft Planetary Computer](https://planetarycomputer.microsoft.com) |
| Sentinel-2 Satellite Data         | Resampled to 30m (bilinear) to align with Landsat; derived vegetation indices and surface reflectivity with weighted mosaic | EVI, Albedo, vegetation stress indices, spectral anomaly scores.                      | [Microsoft Planetary Computer](https://planetarycomputer.microsoft.com) |
| NYC Tree Census (2015)            | Aggregated tree metrics with normal and annular buffers (500m–15,000m)                          | Tree count, avg_tree_health, tree_size, ring features, agg features (min, max, mean, std) | [NYC Open Data](https://data.cityofnewyork.us/)                      |
| NYC Building Footprints (KML/SHP) | Spatial joins across normal and annular buffers (500m–15,000m)                                  | Building count/height/area, ground elevation (SHP only), ring features, agg features  | [SHP File](https://nycmaps-nyc.hub.arcgis.com/datasets/nyc::building/about)<br>[KML File](https://challenge.ey.com/challenges/the-2025-ey-open-science-ai-and-data-challenge-cooling-urban-heat-islands-external-participants/data-description)        |
| NYC Traffic 2022                  | Aggregated traffic volume/count across normal and annular buffers (500m–15,000m)                | Traffic volume, traffic count, ring features, agg features                            | [NYC Open Data](https://data.cityofnewyork.us/)                      |
| OSMNX Street Networks             | Network analysis and features derived using OSMnx library                                        | Distance to water/park, street length, road/node density, area types                  | [OSMNX Docs](https://osmnx.readthedocs.io)                              |
| NYC Planimetric Database: Elevation Points | Slope/elevation analysis with normal and annular buffers (500m–15,000m)                          | Elevation, elev range, slope, agg features                                            | [Elevation_NYC](https://data.cityofnewyork.us/)                          |
| Weather Data                      | Direct sensor measurements from local weather stations                                           | Air temperature, humidity, wind speed, solar flux, WindDir                            | [NY Mesonet](https://www.nysmesonet.org)                                |
| Forestry Data                     | Forestry data analysis across normal and ring buffers (500m–15,000m)                             | Mean tree DBH, forestry count, risk score, ring features, species details             | [NYC Open Data](https://data.cityofnewyork.us/)                               |
## Methodology

### Data Processing
- Generated 1,700+ features from multiple datasets
- Created spatial buffer analyses (500m-15,000m) to capture geographic relationships
- Implemented temporal aggregation techniques to ensure model stability
- Null values in buffer-based features were imputed with 0 to reflect absence of structures or vegetation

## Key Factors

1. **Satellite Data Resampling**
   - Implemented bilinear resampling of 10m Sentinel-2 data to 30m resolution
   - This technique effectively removed noise while preserving important spatial patterns (similar to an autoencoder)
   - Compared with other resampling methods, bilinear resampling yielded significant CV and LB improvements

2. **Weather Data Temporal Alignment**
   - Solved critical temporal misalignment between training data (timeframes 3-4) and weather data (timeframes 6-20)
   - Developed linear time compression algorithm to map weather data to training timeframes
   - Validated multiple window sizes to optimize temporal matching
   - Mapped closest temporal weather measurements to training observations

3. **Spatial Imputation for Test Data**
   - Addressed missing time data in the test set using innovative spatial imputation techniques
   - Applied nearest neighbor spatial joins to impute weather data values
   - Created a two-step imputation process: temporal alignment for training data, followed by spatial imputation for test data

4. **Advanced Buffer Analysis**
   - Implemented both standard and annular (ring) buffers at multiple scales (500m-15,000m)
   - This approach captured spatial relationships at different distances without requiring explicit coordinates
   - Generated powerful features for urban heat drivers at increasing distances from measurement points

### Feature Engineering
- Developed specialized features capturing building-vegetation interactions
- Created thermal mass indicators and vertical density metrics
- Engineered elevation gradients and building height dispersion ratios
- Both non-linear and domain-friendly features created for diverse impact.
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
├── ey_Landsat_train.ipynb       # Landsat data mapped with study coordinates to get them model ready.
├── ey_Sentinel2_train.ipynb     # Sentinel-2 data mapped with study coorinates to get them model ready.
├── landsat_lst.ipynb            # Landsat-8 prerprocessing and extractions
├── sentinel2.ipynb              # Sentinel-2 processing and extractions
├── ey_external_data.ipynb       # External datasets integration(elevation, tree_cencus, forestry, traffic)
├── ey_extra_data.ipynb          # Additional data sources processing(osmnx)
├── ey_geographical.ipynb        # Geographical features engineering
├── ey_weather_data.ipynb        # Weather data processing
├── ey_feature_selection.ipynb   # Feature selection implementation
├── ey_hyperparameter_tuning.ipynb  # Model hyperparameter optimization
├── ey_ensembling.ipynb          # Model ensemble creation with oofs
├── ey_final_boss.ipynb          # Final single model solution implementation
├── README.md
└── requirements.txt
```

## Usage

1. Clone this repository
2. Install requirements: `pip install -r requirements.txt`
3. Run the notebooks in order to reproduce the solution:
   - Data Extraction from satellite notebooks and mapping them to coordinates.
   - Extracting additional datasets from the complimentary notebooks.
   - Train models with `single_model.ipynb` or `ensemble_model.ipynb` with the combined dataset.

## Acknowledgements

Thanks to EY for organizing this competition and providing the opportunity to work on such an important environmental challenge addressing urban heat islands, which have significant impacts on public health, energy consumption, and quality of life in cities worldwide.
