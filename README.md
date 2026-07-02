
# Hotel Staffing Optimisation using Random Forest Regression

## Overview
This project addresses the operational challenge of hotel overstaffing and understaffing. By building a predictive data pipeline, the system forecasts daily staffing demands for a property in the Glasnevin area. 

The model predicts requirements by combining and aligning historical weather data, localized Dublin events, and past hotel occupancy records. This allows management to optimize shift distribution, reduce payroll waste, and maintain consistent guest service levels[cite: 1].

## Key Analytics & Engineering Steps

### 1. Environmental Data Processing (Met Éireann)
* Sourced and aggregated three years (2015–2017) of historical daily weather logs from Met Éireann[cite: 1].
* Isolated critical variables: rainfall, minimum temperature, and maximum temperature[cite: 1].
* Engineered a categorical feature (`weather_quality`) to map raw metrics into business-friendly conditions: *Good/Warm*, *Dry/Cool*, *Poor/Wet*, and *Moderate/Changeable*[cite: 1].

### 2. Localized Event Integration
* Sourced a localized dataset mapping major Dublin events near Glasnevin from 2015 to 2017[cite: 1].
* Normalized date formats to match the weather logs[cite: 1].
* Assigned numerical impact weights to events (`event_impact`): `0` for routine days, `1` for minor disruptions, and `2` for major area draws (e.g., stadium concerts)[cite: 1].

### 3. Hotel Guest & Room Occupancy Processing
* Sourced an extensive raw booking dataset (over 119,000 reservation logs) from Kaggle[cite: 1].
* Filtered data to isolate a single city-center property and dropped incomplete records[cite: 1].
* Consolidated adults and children metrics to calculate real, nightly guest volume[cite: 1].
* Developed a **custom date-expansion algorithm** using `pandas.Timedelta`[cite: 1]. This logic unpacks single booking ranges (check-in date + length of stay) into discrete nightly rows to calculate active daily on-site occupancy, while factoring out cancellations[cite: 1].

### 4. Pipeline Merging & Temporal Alignment
* Consolidated the transformed weather, event, and occupancy tables on their shared `date` keys[cite: 1].
* Identified and resolved an asynchronous timeline issue across the datasets by truncating the analysis strictly to the overlapping window: **July 1st, 2015 to August 31st, 2017** (resulting in 793 clean, filled, and aligned data points)[cite: 1].

### 5. Exploratory Data Analysis (EDA)
* Identified clear seasonal trends, including low-volume winter baselines, localized spikes around Christmas, and sustained high occupancy from April through October[cite: 1].
* Verified weekly occupancy rhythms, showing peak traffic on Fridays and Saturdays, with steady corporate volume on mid-week days[cite: 1].

## Tech Stack & Dependencies
* **Core Language:** Python 3
* **Environment:** Jupyter Notebook (`.ipynb`)
* **Libraries Used:** 
  * `pandas`, `numpy` — Data parsing, cleaning, and table operations[cite: 1]
  * `matplotlib`, `seaborn`, `plotly` — Statistical plotting and interactive heatmaps[cite: 1]
  * `statsmodels` — Trend and baseline evaluation[cite: 1]
  * `scikit-learn` — Model training, train/test splitting, and evaluation metrics[cite: 1]

## Repository Structure
* `Staffing.ipynb` — The primary notebook containing data cleaning, merging, EDA, and model testing[cite: 1].
* `DataSets/` — Local storage for raw CSV source files[cite: 1].

## Next Steps & Operational Improvements
If expanding this prototype for a production environment, future steps include:
1. Migrating code from a monolithic Jupyter Notebook into modular, testable `.py` scripts.
2. Converting the Random Forest model into a live microservice via an API endpoint.
3. Building an interactive supervisor dashboard using Streamlit or PowerBI so hotel managers can view upcoming roster recommendations visually.
