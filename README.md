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
NADA follows a Descriptive → Diagnostic → Predictive → Prescriptive analytical framework.

![image1](https://github.com/user-attachments/assets/c22dc71f-6724-4ef0-be66-70a326e1b10b)

1. **Data Preparation** — Integrated and cleaned socioeconomic data from DOSM/OpenDOSM and the Ministry of Education across Pahang's 11 districts.
2. **Exploratory & Statistical Analysis** — Analysed relationships between employment indicators and poverty to identify socioeconomic patterns.
3. **District Vulnerability Index (DVI)** — Developed a multidimensional index using labour, demographic, living-condition and price-pressure indicators.
4. **Nowcasting** — Used PCHIP interpolation and machine learning models to estimate household income and poverty during HIES gap years.
5. **Clustering** — Applied K-Means clustering to identify districts with similar vulnerability profiles.
6. **Dashboard Development** — Integrated the findings into an interactive Power BI decision-support dashboard for district-level monitoring and prioritisation.

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
