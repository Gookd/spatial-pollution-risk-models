# Spatial Pollution Risk Models — PM10 France Métropolitaine 2025

![Python](https://img.shields.io/badge/Python-3.11-blue)
![License](https://img.shields.io/badge/License-All%20Rights%20Reserved-red)
![Status](https://img.shields.io/badge/Status-In%20Progress-orange)

## Overview

This project applies **spatial statistics and geostatistical methods** to model and predict PM10 particulate matter concentrations across metropolitan France using 2025 monitoring data. The goal is to demonstrate that spatial approaches outperform standard non-spatial regression by explicitly accounting for the spatial autocorrelation structure of air pollution data.

According to Santé Publique France, approximately **40,000 deaths per year** are attributable to fine particle exposure in France, representing around 6.7% of total annual mortality. With a mean observed concentration of **15.35 µg/m³** across 319 stations — just above the WHO 2021 annual guideline of 15 µg/m³ — this project provides tools to identify at-risk areas and predict pollution levels in unmonitored zones.

---

## Methods

### 1. Exploratory Spatial Analysis
- Directional variogram analysis (0°, 45°, 90°, 135°) to test for anisotropy
- Moran's I test on raw PM10 values and OLS residuals to confirm spatial autocorrelation

### 2. Kriging Interpolation
- **Universal Kriging** with linear drift to account for non-stationary mean
- Anisotropy parameters: `anisotropy_scaling=1.5`, `anisotropy_angle=0`
- Optimal lag number: `nlags=43` (determined by the rule `lag_size × nlags ≈ dist_max / 2`)
- Model selection: spherical, gaussian, exponential compared via leave-one-out cross-validation
- **Normality of kriging residuals** verified via Shapiro-Wilk test (W=0.997, p=0.841)

### 3. Spatial Regression
- **OLS** with spatial diagnostics (`spat_diag=True`)
- **SAR** (Spatial Autoregressive Model) — spatial lag on dependent variable
- **SEM** (Spatial Error Model) — spatial autocorrelation in residuals
- Model selection via robust Lagrange Multiplier tests → **SEM preferred**
- Spatial weight matrix: KNN with k=8 neighbors

### 4. Risk Assessment
- Probability of exceeding WHO threshold: $P(Z(x) > 15) = 1 - \Phi\left(\frac{15 - \hat{Z}(x)}{\sigma(x)}\right)$
- Spatial Value at Risk (VaR) at 90%, 95%, 99%

---

## Data

| Source | Variable | API |
|---|---|---|
| ATMO France | PM10 concentrations (annual mean 2025) | — |
| INSEE | Commune population | `geo.api.gouv.fr` |
| Open-Elevation | Station altitude | `api.open-elevation.com` |
| Open-Meteo | Wind, precipitation, temperature (2025) | `archive-api.open-meteo.com` |
| GéoRisques (BRGM) | Number of ICPE within 5km radius | `georisques.gouv.fr` |

---

## Key Results

### Moran's I
| Test | I | p-value |
|---|---|---|
| Raw PM10 | 0.232 | 0.001 |
| OLS residuals | 0.275 | 0.001 |

### Kriging (Universal, spherical, nlags=43)
| Model | RMSE | MAE |
|---|---|---|
| Spherical | 3.117 | 2.258 |
| Gaussian | 3.143 | 2.277 |
| Exponential | 3.114 | 2.271 |

### Spatial Regression
| Model | AIC | Log-likelihood |
|---|---|---|
| OLS | 1380.9 | -677.5 |
| SAR | 1347.8 | -659.9 |
| **SEM** | **1323.8** | **-648.9** |

### Risk Assessment
| Spatial VaR | PM10 (µg/m³) |
|---|---|
| 90% | 15.94 |
| 95% | 16.28 |
| 99% | 16.85 |

50.8% of the grid exceeds the WHO annual threshold of 15 µg/m³.

---

## Interactive Map

An interactive map of PM10 concentrations by monitoring station is available here:

👉 [Carte interactive PM10 — France Métropolitaine 2025](https://raw.githack.com/Gookd/spatial-pollution-risk-models/main/code/carte_pm10.html)

---



## References

- Cressie, N. (1993). *Statistics for Spatial Data*. Wiley.
- Deutsch, C.V. & Journel, A.G. (1998). *GSLIB: Geostatistical Software Library*. Oxford University Press.
- ESRI. *Choosing a lag size — ArcGIS Geostatistical Analyst*.
- Pacific Northwest National Laboratory. *Kriging Variogram*. VSP Documentation.
- Santé Publique France. *Exposition de la population française à la pollution de l'air*.
- WHO (2021). *Air Quality Guidelines: Global Update 2021*.