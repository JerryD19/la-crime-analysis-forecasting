# 🚓 Los Angeles Crime Patterns & Forecasting

Exploratory analysis and time-series forecasting of **897,000 crime records** (29 features) from the Los Angeles Police Department's open data, 2020 to early 2024. The analysis focuses on battery and simple assault, the most common violent-crime category. It looks at **when, where and how** incidents happen, then forecasts monthly volumes with **SARIMA** to support police staffing and resource planning.

**Tools:** Python · pandas · Matplotlib · Seaborn · statsmodels (seasonal decomposition, SARIMA) · Folium (mapping) · TextBlob & WordCloud (text analysis)

📓 **Code:** [`notebooks/la_crime_analysis.ipynb`](notebooks/la_crime_analysis.ipynb), the full analysis with outputs

**Data:** [Crime Data from 2020 to Present, LA Open Data](https://data.lacity.org/Public-Safety/Crime-Data-from-2020-to-Present/2nrs-mtv8)

---

## Key findings

| Finding | Detail |
|---|---|
| 📈 **Rising trend** | Battery / simple assault reports rose from 16,000 in 2021 to **18,900 in 2023** |
| ☀️ **Summer peak** | **July** is the worst month (6,364 incidents) and **February** the quietest (5,176), a 23% swing (2020–23 combined) |
| 🕒 **Afternoon peak** | Incidents peak at **3–6 pm**, are lowest around **5 am**, and spike on **weekend late nights** |
| 📍 **Hotspots** | **Central** division leads (6,429 incidents), followed by **77th Street** (4,273) |
| 🏠 **Where it happens** | The top locations are single-family homes, streets and multi-unit housing |
| ✊ **Weapon** | "Strong-arm" (hands, fists, feet) in **62,565** incidents, more than 10× the next category |
| ⏱️ **Reporting delay** | Victims take **2.6 days** on average to report, so there's room for earlier reporting |

## When does it happen?

The hour × weekday heatmap shows weekday afternoons and **weekend late nights** as the riskiest windows. This is directly useful for scheduling patrol shifts.

![Heatmap of incidents by hour and day of week](images/heatmap_hour_by_day.png)

<p float="left">
  <img src="images/monthly_seasonality.png" width="49%" alt="Monthly seasonality" />
  <img src="images/incidents_by_hour.png" width="49%" alt="Incidents by hour of day" />
</p>

## Where does it happen?

![Incidents by LAPD area](images/incidents_by_area.png)

Incidents were also mapped with Folium marker clusters to show street-level hotspots:

<img src="images/incident_map.png" width="600" alt="Folium map of incident clusters">

## Forecasting with SARIMA

The monthly series was split into **trend** and **seasonal** components, then a SARIMA model was trained on the first 80% of months and tested on the last 20%.

<p float="left">
  <img src="images/trend_component.png" width="49%" alt="Trend component" />
  <img src="images/seasonal_component.png" width="49%" alt="Seasonal component" />
</p>

The model is SARIMA(1,1,1)(1,1,1,12), trained on 2020 to early 2023 and tested on the remaining months. The forecast (red) tracks the mid-2023 summer surge and the autumn decline in the held-out data (orange). **Test RMSE ≈ 103 incidents per month**, about 6–7% of a typical month's volume.

![SARIMA forecast vs actual](images/sarima_forecast.png)

## Text analysis of crime descriptions

A word cloud of incident descriptions shows simple assault, vandalism, intimate-partner violence and theft as the dominant themes. Sentiment scoring confirmed that most police descriptions are written in a neutral tone.

<img src="images/word_cloud.png" width="520" alt="Word cloud of crime descriptions">

---

## Ethics

Crime data contains sensitive information about victims. The analysis uses only the anonymised public release, reports aggregates rather than individuals, and treats demographic patterns with care. They describe reported incidents, not underlying risk, and can reflect biases in policing and reporting.

## What I'd do next

- Compare SARIMA with Prophet and gradient-boosted models, and report MAE/MAPE alongside MSE so errors are easy to interpret.
- Build an interactive Power BI or Streamlit dashboard so area commanders can filter by division, hour and crime type.
- Add external drivers (temperature, holidays, major events) to explain the summer peak.

## Repository structure

```
├── notebooks/la_crime_analysis.ipynb   # Full analysis: cleaning, EDA, mapping, decomposition, SARIMA, text analysis
├── images/                             # Figures used in this README
├── data/                               # Download instructions (data not included)
└── requirements.txt
```

**To run:** `pip install -r requirements.txt`, download the CSV (see `data/README.md`) into `notebooks/`, then run the notebook.

---

*Built for the Data Visualisation module of my MSc Big Data Analytics, University of Derby, 2024.*
