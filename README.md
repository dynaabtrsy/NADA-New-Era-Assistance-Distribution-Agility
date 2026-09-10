# NADA-New-Era-Assistance-Distribution-Agility
## Overview

NADA (New-Era Assistance Distribution Agility) is a data-driven socioeconomic decision-support project developed for the DAX Challenge 2026 — _Integrating Data and AI for a Sustainable Pahang_.

The project addresses a key limitation in socioeconomic planning: low unemployment does not necessarily indicate low household poverty. Using district-level data from DOSM/OpenDOSM and the Ministry of Education, NADA analyses socioeconomic vulnerability across all 11 administrative districts in Pahang, Malaysia.

The project combines statistical analysis, a District Vulnerability Index (DVI), machine-learning-based nowcasting, clustering, and an interactive Power BI dashboard to help policymakers identify vulnerable districts and understand the underlying drivers of vulnerability between official HIES release years.

The interactive Power BI dashboard can be viewed [here](https://app.powerbi.com/view?r=eyJrIjoiMTBhMGQ5MjMtY2QwMC00NDkwLTg1N2UtNDY5YmVjNDVhYTkxIiwidCI6IjE4Y2U3NmY2LTk5ZjQtNDU3Zi05ZjYyLWFjZDY1ZDliOTc3NyIsImMiOjEwfQ%3D%3D).
If you have any trouble opening the dashboard, please do not hesitate to contact me at dynaabtrsy@gmail.com.


**Key Technologies:**
- Python — Data cleaning, EDA, statistical analysis, modelling & clustering
- Pandas / NumPy — Data processing and feature engineering
- Scikit-learn — Machine learning and clustering
- SciPy / Statsmodels — Statistical testing and regression
- Power BI / DAX — Interactive dashboard and decision support
- Supabase PostgreSQL — Structured data layer
- DOSM OpenDOSM & Ministry of Education — Primary data sources

## Table of Contents
- [Overview](#Overview)
- [Executive Summary](#Executive-Summary) 
- [Methodology](#Methodology)
- [Key Findings](#Key-Findings)
- [Recommendations](#Recommendations)
- [Limitations](#Limitations)
- [Project Impact](#Project-Impact)
- [Dashboard](#Dashboard)

## Executive Summary
NADA investigates whether unemployment can be used as a reliable proxy for household poverty across Pahang's 11 districts. Analysis of 33 district-year observations from HIES release years found virtually no relationship between unemployment and absolute poverty (Pearson r = −0.020, p = 0.910). In contrast, Labour Force Participation Rate (LFPR) showed a significant negative relationship with poverty (r = −0.564, p = 0.0006).

This revealed a potential "working-poor" pattern, particularly in Lipis and Maran, where relatively low unemployment coexists with comparatively high poverty and lower labour-force participation.

To address this gap, NADA developed a District Vulnerability Index (DVI) using four dimensions:
1. Labour-market vulnerability
2. Household economic pressure
3. Living-condition vulnerability
4. Macroeconomic price pressure  

The DVI demonstrated a significant relationship with actual poverty (r ≈ 0.575, p < 0.001), providing evidence that a multidimensional indicator can serve as an interim vulnerability signal between HIES releases.

A nowcasting framework was then developed to estimate household income and poverty during non-survey years. Median household income achieved R² = 0.662 and MAPE = 6.31%, while the poverty model achieved R² = 0.533 and MAPE = 48.07%. Therefore, income estimates provide stronger predictive performance, while poverty estimates are more appropriate as directional early-warning signals.

Finally, K-Means clustering, validated using Ward hierarchical clustering with an Adjusted Rand Index of 1.000, identified three vulnerability typologies:
- Labour/Household-driven
- Living-condition-driven
- Compound labour + living-condition

The resulting insights were integrated into a three-page Power BI dashboard designed to support district prioritisation, vulnerability diagnosis, and inter-survey socioeconomic monitoring.

## Methodology
**Pipeline Project Methodology**
![image1](https://github.com/user-attachments/assets/4b5ab6d0-0ca0-4f1b-9b41-0b1a9a1bfb0e)

### **Phase 1 — Data Preparation**

Collect socioeconomic datasets from DOSM/OpenDOSM and the Ministry of Education, covering Pahang's 11 administrative districts.

The datasets are cleaned and standardised by:
- Standardising district names and year formats
- Converting variables into consistent numeric formats and units
- Checking duplicates and invalid values
- Assessing missing observations
- Aligning datasets to a common district-year structure

A master district-year panel is then created by merging the cleaned datasets using `district` and `year` as the common keys.

![image](https://github.com/user-attachments/assets/fcdf6d94-4c4d-4919-9a6e-a2abab4c7d20)

### **Phase 2 — Employment vs Poverty Analysis**

Test whether unemployment can reliably represent household poverty.
1. Match employment indicators with HIES poverty observations.
2. Examine the relationship between unemployment, LFPR and poverty.
![image](https://github.com/user-attachments/assets/63f9f2e0-e7ea-4c06-8a02-a8e7bdfbece3)

3. Apply correlation analysis and OLS regression.
4. Use district-level bootstrap resampling to assess uncertainty.
5. Identify districts with low unemployment but high poverty as potential working-poor areas.
![image](https://github.com/user-attachments/assets/293279e9-306b-4824-954e-c057796eb13e)

### **Phase 3 — District Vulnerability Index (DVI)**

Construct a multidimensional District Vulnerability Index using four dimensions:
- Labour-market vulnerability
- Household economic vulnerability
- Living-condition vulnerability
- Price pressure

Indicators are transformed so that higher values consistently represent greater vulnerability and are then normalised using min-max scaling.

The four dimension scores are aggregated using equal weighting:
$`
DVI=(L+H+C+P)/4
`$	​

### **Phase 4 — DVI Validation & Driver Analysis**

Validate whether the DVI reflects actual socioeconomic hardship.

1. Compare DVI scores against observed HIES poverty.
2. Evaluate the correlation and rank agreement between DVI and poverty.
3. Use PCA as a sensitivity check against the equal-weighted approach.
4. Examine individual dimensions to identify the main vulnerability drivers for each district.

### **Phase 5 — Vulnerability Clustering**

Group districts according to their vulnerability profiles, rather than only their overall severity.

1. Calculate each district's average vulnerability profile.
2. Apply K-Means clustering.
3. Determine an appropriate number of clusters using silhouette analysis.
4. Validate the resulting clusters using Ward hierarchical clustering.
5. Calculate the Adjusted Rand Index (ARI) to assess agreement between methods.
6. Assign interpretable vulnerability typologies to the resulting clusters.

### **Phase 6 — Nowcasting Framework**

Address the information gap between HIES release years.
The nowcasting process consists of two stages:

**Historical completion**  
- Retain observed HIES values for 2019, 2022 and 2024.
- Use PCHIP interpolation to estimate missing historical years.
- Flag interpolated observations separately from official observations.

**Future nowcasting**  
- Train models using the completed historical panel.
- Generate estimates for 2025–2027 using projected socioeconomic predictors.

![image](https://github.com/user-attachments/assets/c28306c2-55ae-43c5-a999-a79183f16b92)

### **Phase 7 — Resampling & Uncertainty Quantification** 

Quantify uncertainty around the nowcasted values using:
- 5,000 district-cluster bootstrap samples
- 5,000 Monte Carlo simulations
- 95% prediction intervals

This allows the dashboard to present not only point estimates but also the uncertainty associated with future projections.

### **Phase 8 — Model Diagnostics & Cross-Validation**  
Evaluate candidate predictive models using Leave-One-Out Cross-Validation (LOOCV).
Candidate models include:
- OLS Regression
- Ridge Regression
- Depth-3 Regression Tree
- Decision Tree

Model performance is compared using MAE, RMSE, MAPE and R². Residual diagnostics are also performed to assess model behaviour.

The selected models are:
- Ridge Regression → Poverty nowcasting
- Depth-3 Regression Tree → Median income nowcasting

### **Phase 9 — Database & Analytical Output Layer

Store the cleaned datasets and analytical outputs in Supabase PostgreSQL to provide a central data layer for the dashboard.

The pipeline connects:
```
Raw Data
   ↓
Cleaned Master Panel
   ↓
DVI & Analytical Outputs
   ↓
Nowcasting & Clustering Results
   ↓
Supabase PostgreSQL
```

This creates a structured and reproducible data layer for downstream visualisation and reporting.

### **Phase 10 — Dashboard & Reporting**

Develop an interactive Power BI dashboard to translate the analytical outputs into decision-support insights.

The dashboard consists of:
- **Executive Overview & Labour Mismatch** — employment–poverty relationship and district overview
- **District Vulnerability & Typology** — DVI rankings, vulnerability drivers and district clusters
- **Economic Nowcasting & Uncertainty** — income and poverty estimates with prediction intervals

The final outputs allow users to identify where vulnerability is highest, what drives it, and which districts share similar intervention needs.

NADA Dashboard link: https://app.powerbi.com/view?r=eyJrIjoiMTBhMGQ5MjMtY2QwMC00NDkwLTg1N2UtNDY5YmVjNDVhYTkxIiwidCI6IjE4Y2U3NmY2LTk5ZjQtNDU3Zi05ZjYyLWFjZDY1ZDliOTc3NyIsImMiOjEwfQ%3D%3D 

## Key Findings
### **1. Unemployment is not a reliable standalone measure of poverty**  

Unemployment showed almost no statistically defensible relationship with poverty
```
Pearson r = −0.020, p = 0.910
```

This suggests that unemployment-only targeting may overlook economically vulnerable districts.

### **2. LFPR provides a stronger socioeconomic signal**  

LFPR demonstrated a significant negative relationship with poverty:
```
Pearson r = −0.564, p = 0.0006
```

Higher labour-force participation was associated with lower poverty.

### **3. Lipis and Maran demonstrate the working-poor pattern**  

Both districts recorded relatively low unemployment but comparatively high poverty and lower LFPR.
This demonstrates why employment status alone may conceal household economic insecurity.

### **4. Maran, Lipis and Jerantut show high DVI vulnerability**  
These districts recorded the highest average DVI scores, with Maran and Lipis consistently ranking near the top from 2021 onwards.

### **5. Income nowcasting is more reliable than poverty nowcasting**
```
Median income achieved:
R² = 0.662 | MAPE = 6.31%

compared with:
Poverty: R² = 0.533 | MAPE = 48.07%
```

Therefore, income estimates can be used with greater confidence, while poverty estimates should primarily support early-warning monitoring.

### **6. Lipis has a distinctive compound vulnerability**  

Clustering identified Lipis as the only district exhibiting elevated labour and living-condition vulnerability simultaneously, which is not fully visible from its overall DVI ranking alone.

## Recommendations
### **1. Move beyond unemployment-only targeting**  

District-level aid and development decisions should incorporate DVI, LFPR and other socioeconomic indicators rather than relying solely on unemployment rates.

### **2. Prioritise persistently vulnerable districts**  

Maran, Lipis, Bera, Jerantut and Rompin should receive closer monitoring and socioeconomic assessment based on their vulnerability profiles and historical patterns.

### **3. Match interventions to vulnerability type**  

Different districts require different responses.  
For example:
  + Labour/household-driven vulnerability → employment and income-support interventions
  + Living-condition vulnerability → infrastructure and basic-service improvements
  + Compound vulnerability → integrated interventions addressing multiple dimensions

### **4. Use nowcasting as an early-warning system**  

Nowcasted poverty and income should help policymakers monitor conditions between HIES releases, but should not be used as automatic triggers for funding or treated as official statistics.

### **5. Continuously validate and recalibrate the models**

As new HIES observations become available, actual outcomes should be compared against previous nowcasts to improve model calibration and reliability.

## Limitations
- **Limited HIES observations:** Only three district-level HIES release years were available for validation.
- **Small analytical sample:** The predictive models are based on a relatively small panel of district-year observations.
- **Interpolation risk:** Three of the six historical nowcasting years were PCHIP-interpolated, which may introduce circularity and optimistic model performance.
- **District aggregation:** District-level data may mask substantial variation between households within the same district.
- **State-level inflation:** Pahang-wide CPI was applied uniformly because district-level CPI data were unavailable.
- **Proxy indicators:** Variables such as school enrolment do not directly measure household welfare.
- **Indicator coverage:** Health, social protection, agriculture dependence and physical hazard exposure were not included due to data availability limitations.
- **District-specific screening:** Different numbers of indicators survived the screening process for each district, particularly affecting Maran and Kuantan.
- **Future uncertainty:** 2025–2027 prediction intervals cannot yet be empirically validated because actual outcomes are unavailable.
- **Forecast assumptions:** Future predictors were extrapolated using linear trends and therefore cannot fully account for unexpected economic shocks, policy changes or turning points.
- **Geographic scope:** Findings are specific to Pahang's 11 administrative districts and should be re-estimated before applying the methodology elsewhere.

> **Important:** NADA is an interim decision-support and early-warning system, not an official poverty measurement system. Nowcasted values should not be interpreted as official DOSM statistics.

## Project Impact
The project contributes by:
- Challenging the assumption that low unemployment automatically indicates low poverty
- Providing a multidimensional vulnerability measure
- Filling information gaps between HIES release years
- Identifying different types of district vulnerability
- Supporting evidence-based resource prioritisation
- Providing an interactive interface for non-technical decision-makers
Ultimately, NADA shifts the question from:
> "Which districts have the highest unemployment?"  

to:

> "Which districts are most vulnerable, why are they vulnerable, and what type of intervention should be prioritised?"  
