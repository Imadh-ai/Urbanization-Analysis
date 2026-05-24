<div align="center">

<img src="https://img.shields.io/badge/Sri%20Lanka-Kurunegala%20District-cc0000?style=for-the-badge&logo=google-maps&logoColor=white"/>
<img src="https://img.shields.io/badge/Landsat%208%2F9-USGS%20Collection%202-1a73e8?style=for-the-badge&logo=satellite&logoColor=white"/>
<img src="https://img.shields.io/badge/Python-3.8+-3776ab?style=for-the-badge&logo=python&logoColor=white"/>
<img src="https://img.shields.io/badge/Period-2015–2025-2d7d46?style=for-the-badge"/>

<br/><br/>

# 🛰️ Urbanization Monitoring
## Kurunegala District, Sri Lanka

> **A satellite-powered study of a decade of urban growth** — processing Landsat imagery into actionable spatial intelligence for sustainable planning.

<br/>

```
📡 Landsat 8/9  →  🐍 Python Pipeline  →  🗺️ NDVI/NDBI Maps  →  📈 Forecasts  →  🏙️ Growth Zones
```

<br/>

[![Dataset](https://img.shields.io/badge/Google%20Drive-Dataset-4285f4?style=flat-square&logo=googledrive&logoColor=white)](https://drive.google.com/drive/folders/1Q8ly3e7yM_PeuMtXb4gMqR2w7YqzlXyI?usp=drive_link)
[![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)]()
[![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=flat-square)]()

</div>

---

## 📋 Table of Contents

| # | Section |
|---|---------|
| 1 | [📖 Project Overview](#-project-overview) |
| 2 | [🗂️ Repository Structure](#️-repository-structure) |
| 3 | [⚙️ Pipeline Workflow](#️-pipeline-workflow) |
| 4 | [📥 Step 1 — Dataset Download & Arrangement](#-step-1--dataset-download--arrangement) |
| 5 | [🗺️ Step 2 — AOI Creation](#️-step-2--aoi-creation) |
| 6 | [🏙️ Step 3 — Urbanization Processing](#️-step-3--urbanization-processing) |
| 7 | [📊 Step 4 — Prediction Model Comparison](#-step-4--prediction-model-comparison) |
| 8 | [🌱 Step 5 — Potential Growth Analysis](#-step-5--potential-growth-analysis) |
| 9 | [📈 Key Results](#-key-results) |
| 10 | [🛠️ Installation](#️-installation) |
| 11 | [👥 Team](#-team) |
| 12 | [📚 References](#-references) |

---

## 📖 Project Overview

This project monitors **urban expansion in Kurunegala District** — a major economic and administrative hub in Sri Lanka's North Western Province — using **Landsat 8 & 9 multispectral satellite imagery** spanning 2015 to 2025.

### Why Kurunegala?

| Factor | Detail |
|--------|--------|
| 📍 Location | North Western Province, Sri Lanka |
| 👥 Population | ~1.7 million |
| 🌾 Economy | Agriculture, trade, transportation, education |
| 🌡️ Climate | Tropical monsoon with seasonal variability |
| 📈 Growth | Rapidly urbanizing secondary city |

### What We Built

A fully automated **Python-based geospatial pipeline** that:

- 🛰️ Ingests raw Landsat Level-2 surface reflectance bands
- ✂️ Clips imagery to district boundary
- 🧮 Computes NDVI and NDBI spectral indices per scene
- 🏙️ Classifies urban vs. non-urban pixels
- 📉 Trains Prophet, ARIMA, and Random Forest forecasting models
- 🗺️ Generates potential growth zone maps via weighted spatial overlay
- 📊 Outputs maps, CSVs, charts, and GeoTIFFs

---

## 🗂️ Repository Structure

```
urbanization-kurunegala/
│
├── 📁 AOI/
│   ├── gadm41_LKA.gpkg                  # Sri Lanka GADM admin boundaries
│   └── Kurunegala_District_AOI.geojson  # Extracted district AOI
│
├── 📁 Rearrangedats/                     # Organized Landsat scenes
│   ├── 2015/
│   │   └── 20150203/
│   │       ├── LC08_..._SR_B2.TIF
│   │       ├── LC08_..._SR_B4.TIF
│   │       └── LC08_..._SR_B5.TIF
│   ├── 2016/ ... 2020/
│
├── 📁 URB_outputs/                       # Urbanization outputs
│   ├── NDVI_YYYYMMDD.tif
│   ├── NDBI_YYYYMMDD.tif
│   ├── Urban_YYYYMMDD.tif
│   ├── Urbanization_Stats_2015_2020.csv
│   └── *.png (trend plots)
│
├── 📁 prediction_output/                 # Model forecast outputs
│   ├── Model_Comparison_Results.csv
│   ├── Future_Predictions_2021_2025.csv
│   └── *.png (forecast plots)
│
├── 📁 Future_Growth/                     # Spatial growth potential
│   ├── potential_score.tif
│   ├── potential_zones.tif
│   ├── proximity_score.tif
│   ├── terrain_suitability.tif
│   └── potential_growth_analysis.png
│
├── 📄 01_aoi_creation.py
├── 📄 02_urbanization_processing.py
├── 📄 03_prediction_models.py
├── 📄 04_potential_growth.py
└── 📄 README.md
```

---

## ⚙️ Pipeline Workflow

```
┌─────────────────────────────────────────────────────────────────────┐
│                    COMPLETE PIPELINE OVERVIEW                        │
├──────────────┬──────────────┬──────────────┬──────────────┬─────────┤
│   STEP 1     │   STEP 2     │   STEP 3     │   STEP 4     │ STEP 5  │
│              │              │              │              │         │
│  📥 Dataset  │  🗺️ AOI      │  🏙️ Urban    │  📊 Predict  │ 🌱 Growth│
│  Download &  │  Creation    │  Processing  │  Models      │ Zones   │
│  Arrange     │              │              │              │         │
│              │              │              │              │         │
│ USGS EarthEx │ GADM → Filter│ Band loading │ Prophet      │ Distance│
│ porer → sort │ Kurunegala → │ → NDVI/NDBI  │ ARIMA        │ + NDVI  │
│ by year/date │ Export GeoJSON│ → Urban mask │ RandomForest │ overlay │
└──────────────┴──────────────┴──────────────┴──────────────┴─────────┘
         ↓              ↓              ↓              ↓           ↓
    Landsat TIFs    AOI polygon    GeoTIFF maps   Forecasts   Zone maps
```

---

## 📥 Step 1 — Dataset Download & Arrangement

### Data Source
All imagery was sourced from **[USGS EarthExplorer](https://earthexplorer.usgs.gov/)** — free to access with a registered account.

| Parameter | Value |
|-----------|-------|
| Satellite | Landsat 8 OLI (2015–2021), Landsat 9 OLI-2 (2022–2025) |
| Product Level | Collection 2 Level 2 Surface Reflectance |
| Spatial Resolution | 30 metres per pixel |
| Cloud Cover Filter | < 20% |
| Temporal Coverage | 2015 – 2025 |
| Revisit Cycle | 16 days |

### Spectral Bands Used

| Band | Wavelength (µm) | Purpose |
|------|----------------|---------|
| Band 2 – Blue | 0.450–0.515 | Water detection |
| Band 3 – Green | 0.525–0.600 | Vegetation discrimination |
| Band 4 – Red | 0.630–0.680 | Vegetation & soil analysis |
| **Band 5 – NIR** | **0.845–0.885** | **NDVI computation** |
| **Band 6 – SWIR1** | **1.560–1.660** | **NDBI / urban mapping** |

### Folder Structure Required

After downloading, arrange scenes into this structure before running any scripts:

```
Rearrangedats/
  └── 2015/
      └── 20150203/
          ├── LC08_L2SP_141055_20150203_..._SR_B2.TIF
          ├── LC08_L2SP_141055_20150203_..._SR_B3.TIF
          ├── LC08_L2SP_141055_20150203_..._SR_B4.TIF
          ├── LC08_L2SP_141055_20150203_..._SR_B5.TIF
          └── LC08_L2SP_141055_20150203_..._SR_B6.TIF
  └── 2016/ (same pattern)
  └── 2017/
  ...
  └── 2025/
```

> 💡 **Tip:** Scene filenames follow Landsat convention `LC08_L2SP_YYYYMMDD_*.TIF` — the date is auto-parsed by the processing scripts.

---

## 🗺️ Step 2 — AOI Creation

**Script:** `01_aoi_creation.py`

Extracts the Kurunegala District administrative boundary from the GADM Sri Lanka dataset and exports it as a GeoJSON for use in all subsequent raster clipping operations.

```python
# Load GADM Level-2 file
gdf = gpd.read_file("AOI/gadm41_LKA.gpkg", layer="ADM_ADM_2")

# Filter to Kurunegala province
mask = gdf["NAME_1"].str.lower().str.contains("kurunegala", na=False)
aoi  = gdf[mask].to_crs("EPSG:4326")

# Export
aoi.to_file("AOI/Kurunegala_District_AOI.geojson", driver="GeoJSON")
```

**Output:** `AOI/Kurunegala_District_AOI.geojson`

| Attribute | Value |
|-----------|-------|
| CRS | WGS84 (EPSG:4326) |
| Format | GeoJSON |
| Source | GADM v4.1 |
| Coverage | All administrative divisions of Kurunegala District |

---

## 🏙️ Step 3 — Urbanization Processing

**Script:** `02_urbanization_processing.py`

The core processing loop — iterates over every Landsat scene, clips bands to the AOI, computes spectral indices, classifies urban pixels, and exports results.

### Surface Reflectance Conversion

All Landsat Collection 2 Level-2 digital numbers are scaled to surface reflectance:

```
Surface Reflectance = (DN × 0.0000275) − 0.2
```

### Spectral Indices

**NDVI** — Normalized Difference Vegetation Index

```
NDVI = (NIR − Red) / (NIR + Red + ε)
```

| NDVI Range | Interpretation |
|------------|---------------|
| −1.0 → 0.0 | Water / impervious surfaces |
| 0.0 → 0.3 | Sparse vegetation / bare soil |
| 0.3 → 0.6 | Agricultural land |
| 0.6 → 1.0 | Dense forest / healthy vegetation |

**NDBI** — Normalized Difference Built-up Index

```
NDBI = (SWIR − NIR) / (SWIR + NIR + ε)
```

> Pixels with **NDBI > 0.1** are classified as **urban**.

### Urban Area Calculation

```
Urban Area (km²) = Urban Pixel Count × 900 m² ÷ 1,000,000
```
*(Each Landsat pixel covers 30m × 30m = 900 m²)*

### Per-Scene Outputs

For each processed Landsat scene (date-stamped):

| File | Description |
|------|-------------|
| `NDVI_YYYYMMDD.tif` | Vegetation index raster |
| `NDBI_YYYYMMDD.tif` | Built-up index raster |
| `Urban_YYYYMMDD.tif` | Binary urban classification |
| `Urbanization_Stats_2015_2020.csv` | Urban area (km²) per scene |
| `Urbanization_Trend_2015_2020.png` | Monthly trend plot |
| `Yearly_Urbanization_Trend_2015_2020.png` | Annual mean ± range |
| `UrbanChange_YYYYMMDD_YYYYMMDD.tif` | Change detection raster |

---

## 📊 Step 4 — Prediction Model Comparison

**Script:** `03_prediction_models.py`

Three forecasting models are trained on 2015–2020 urban area time-series data and evaluated against a held-out test set (last 15% of observations).

### Models

| Model | Type | Key Strengths |
|-------|------|--------------|
| **Prophet** | Time Series | Seasonal decomposition; handles outliers & missing data |
| **ARIMA(1,1,1)** | Statistical | Robust for smooth short-term trends |
| **Random Forest** | Machine Learning | Captures non-linear temporal patterns |

### Evaluation Metrics

| Metric | Description |
|--------|-------------|
| **MAE** | Mean Absolute Error — average magnitude of error |
| **RMSE** | Root Mean Square Error — penalises large errors |
| **R²** | Variance explained by the model |

> 🏆 **ARIMA** demonstrated the most stable performance for short-term urban extent prediction on this dataset.

### Future Forecast (2021–2025)

After model selection, Prophet is retrained on the full 2015–2020 dataset and extended 60 months into the future:

```python
future_model = Prophet(yearly_seasonality=True, changepoint_prior_scale=0.3)
future_model.fit(prophet_full_df)
future_df = future_model.make_future_dataframe(periods=60, freq="MS")
future_forecast = future_model.predict(future_df)
```

### K-Means Temporal Clustering

Urban area observations are also clustered into 3 temporal regimes using K-Means to identify distinct growth phases across the study period.

### Outputs

| File | Description |
|------|-------------|
| `Model_Comparison_Results_2015_2020.csv` | MAE / RMSE / R² for all models |
| `Model_Forecast_Comparison_2015_2020.png` | Side-by-side forecast plot |
| `Future_Predictions_2021_2025.csv` | Prophet forecast with confidence intervals |
| `Future_Predictions_2021_2025.png` | Forecast chart with uncertainty bands |
| `KMeans_Temporal_Clusters_2015_2020.png` | Cluster scatter plot |

---

## 🌱 Step 5 — Potential Growth Analysis

**Script:** `04_potential_growth.py`

Identifies where future urban expansion is most likely to occur using a **weighted spatial overlay** of proximity and terrain suitability.

### Scoring Formula

```
Potential Score = (0.4 × Proximity Score) + (0.6 × Terrain Suitability)
```

| Factor | Weight | Derivation |
|--------|--------|-----------|
| **Proximity Score** | 40% | Inverse Euclidean distance from existing urban pixels |
| **Terrain Suitability** | 60% | Inverse NDVI — low vegetation = higher suitability |

### Development Zone Classification

| Zone | Label | Recommended Action |
|------|-------|--------------------|
| **Zone 5** | 🔴 Very High Potential | Immediate planned development + infrastructure |
| **Zone 4** | 🟠 High Potential | Medium-term controlled development |
| **Zone 3** | 🟡 Medium Potential | Long-term planning + environmental assessment |
| **Zone 2** | 🟢 Low Potential | Green buffer preservation |
| **Zone 1** | 🔵 Very Low Potential | Conservation priority; development not recommended |

### Outputs

| File | Description |
|------|-------------|
| `potential_score.tif` | Continuous development potential surface (0–1) |
| `potential_zones.tif` | Classified 5-zone development suitability map |
| `proximity_score.tif` | Distance-from-urban score raster |
| `terrain_suitability.tif` | NDVI-derived terrain suitability raster |
| `potential_growth_analysis.png` | 6-panel summary figure |

---

## 📈 Key Results

### Urban Growth Summary (2015–2025)

- 📈 **Continuous and measurable urban expansion** documented throughout the decade
- 🏙️ Growth concentrated around **Kurunegala City** and major **transportation corridors**
- 🌾 **Agricultural land** on urban peripheries was the most common conversion target
- 🌳 **Protected forest areas** maintained consistently high NDVI throughout the period

### Dominant Expansion Patterns

| Pattern | Description |
|---------|-------------|
| **Edge Expansion** | Most common — new growth contiguous with existing built-up areas |
| **Corridor Development** | Linear growth along Colombo–Kurunegala and Kurunegala–Puttalam routes |
| **Limited Sprawl** | Moderate dispersed peri-urban development; no major leapfrog growth |

### Model Performance

ARIMA showed the best performance for short-term forecasting on this dataset. Prophet provided the most informative seasonal decomposition and was used for long-range projections to 2025.

---

## 🛠️ Installation

### Key Configuration (edit at top of each script)

```python
LANDSAT_DIR = Path("/path/to/Rearrangedats")
AOI_PATH    = Path("/path/to/AOI/Kurunegala_District_AOI.geojson")
OUTPUT_DIR  = Path("/path/to/outputs")
YEARS       = range(2015, 2026)
```

### Dependencies

| Library | Purpose |
|---------|---------|
| `rasterio` | Raster I/O and masking |
| `geopandas` | Vector/AOI processing |
| `numpy` | Array computation |
| `pandas` | Tabular statistics |
| `scikit-learn` | K-Means & Random Forest |
| `prophet` | Time series forecasting |
| `statsmodels` | ARIMA modelling |
| `matplotlib` / `plotly` | Visualization |

---


## 📚 References

1. Xu, H. (2008). A new index for delineating built-up land features in satellite imagery. *International Journal of Remote Sensing*, 29(14), 4269–4276.
2. Zha, Y., Gao, J., & Ni, S. (2003). Use of normalized difference built-up index in automatically mapping urban areas from TM imagery. *International Journal of Remote Sensing*, 24(3), 583–594.
3. Rouse, J. W., et al. (1974). Monitoring vegetation systems in the Great Plains with ERTS. *NASA Goddard Space Flight Center*.
4. U.S. Geological Survey. Landsat Collection 2 Level 2 Science Products. https://www.usgs.gov/landsat-missions
5. GADM Database of Global Administrative Areas. (2023). https://gadm.org
6. Department of Census and Statistics Sri Lanka. (2022). Population and Housing Census Data.
7. Urban Development Authority Sri Lanka. (2021). National Physical Planning Policy and Plan 2050.

---

<div align="center">

**📡 Built with open-source tools · Powered by USGS Landsat · For sustainable urban planning in Sri Lanka**

<img src="https://img.shields.io/badge/Made%20with-Python-3776ab?style=flat-square&logo=python&logoColor=white"/>
<img src="https://img.shields.io/badge/Data-USGS%20Landsat-1a73e8?style=flat-square"/>
<img src="https://img.shields.io/badge/GIS-Rasterio%20%7C%20GeoPandas-2d7d46?style=flat-square"/>

</div>
