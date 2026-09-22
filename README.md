```markdown
# ✈️ SkyWeather Analytics: Impact of Weather & Airport Volume on US Aviation

> An end-to-end Data Warehousing, ETL, and OLAP analytics platform evaluating how extreme meteorological events and passenger throughput impact US domestic flight delays, cancellations, and ticket pricing.

---

## 👥 Authors

* **Emanuel Pacheco** (2024138898)
* **Ivo Simões** (2024160048)
* **Filipe Obrist** (2024170686)

**Course:** Data Analytics Technologies (*Tecnologias de Análise de Dados*)  
**Institution:** Universidade de Coimbra — Department of Informatics Engineering (DEI-UC)  
**Date:** May 2026

---

## 📌 Project Overview

Despite modern logistical tools, US aviation remains vulnerable to environmental disruptions and capacity constraints. **SkyWeather Analytics** integrates multi-year operational flight records (2022–2025) with NOAA meteorological data, TSA passenger throughput, and quarterly domestic ticket fares to provide decision support across the aviation sector.

### Key Questions Addressed
1. **Weather Impact:** Do severe weather events (precipitation, snow, temperature extremes) cause localized flight delays or cancellations? (Diagnostic)
2. **Passenger Volume vs. Delays:** How do passenger surges at TSA checkpoints correlate with operational delays? (Diagnostic)
3. **Geographic Pricing:** How does geographical location influence average ticket fares across market routes? (Descriptive)
4. **Stormy Quarters:** How do quarterly ticket prices vary during high-storm periods, and do higher prices impact cancellation rates? (Descriptive & Diagnostic)
5. **Price Forecasting:** What are the predicted ticket prices for upcoming quarters based on historical weather and volume trends? (Predictive)
6. **Travel Recommendations:** Which cities and months present the lowest risk of delays and high ticket costs? (Prescriptive)

---

## 🛠️ Tech Stack & Tools

* **Data Extraction & Spatial Preprocessing:** Python (spatial coordinates join via latitude/longitude mapping).
* **ETL Orchestration:** Pentaho Data Integration (PDI).
* **Data Warehouse:** PostgreSQL (Hosted on DEI virtual environment).
* **Dimensional Modeling:** Star Schema designed via ONDA3.
* **OLAP & Visualization:** Tableau Desktop (4-tab interactive decision dashboard).

---

## 🏗️ Data Warehouse Architecture (Star Schema)

The data warehouse consolidates over **20 Million raw flight records**. The raw dataset (~11 GB) was cleaned, transformed, and reduced by **~68%** down to **3.55 GB**.


```
            +-----------------+
            |     Weather     |
            +-----------------+
                     |
                     v (Arrival / Departure)


+-----------------+     +-----------------+     +-----------------+
|     Airport     |---->|     FLIGHT      |<---|      Delay      |
+-----------------+     |  (Fact Table)   |     +-----------------+
|   |              +-----------------+
|   |                       |
|   |                       v (Time Foreign Keys)
|   |              +-----------------+
|   +------------->|      Time       |
|                  +-----------------+
|                           ^
v                           |
+---------------------------------+
|         MARKET TRAFFIC          |
|          (Fact Table)           |
+---------------------------------+

```

### Fact & Dimension Tables Overview

| Table | Type | Estimated Rows | Total Size | Description |
| :--- | :--- | :--- | :--- | :--- |
| **`flight`** | Fact | ~20,360,852 | 3,180 MB | Individual flight metrics (distance, delays, cancellations, diversion). |
| **`market_traffic`** | Fact | ~52,420 | 5.2 MB | Quarterly fare averages and daily passenger volume per route. |
| **`time_`** | Dimension | ~1,929,157 | 213 MB | High-granularity timestamps (Year, Quarter, Month, Day, Hour, Min). |
| **`weather`** | Dimension | ~1,718,053 | 163 MB | Daily precipitation, snow, and min/max temperatures. |
| **`delay`** | Dimension | ~22,225 | 2.0 MB | Delay cause categorization and duration metrics. |
| **`airport`** | Dimension | 196 | 64 kB | Geographical metadata (City, State, Lat/Long coordinates). |

---

## ⚡ ETL Pipeline & Performance Optimization


```

[ Raw Data Sources ] ---> [ Python Spatial Join ] ---> [ Pentaho Cleaning Transformations ] ---> [ PostgreSQL Staging & Bulk Insertion ]
(BTS, NOAA, TSA, Fares)   (Nearest Station Link)       (Null Fixes, Code Mapping)              (SQL Index Resolution)

```

### 98% Performance Improvement
* **Initial Pentaho-Only Execution:** Inserting and indexing 1 year of data took over 8 hours (projected **32+ hours** for 4 years).
* **Optimization:** Shifted indexing resolution and heavy joins from row-by-row Pentaho transformations to **PostgreSQL SQL temporary staging queries**.
* **Result:** Total pipeline run time dropped to **43 minutes** (a **98% reduction** in execution time).

---

## 📊 Tableau OLAP Dashboards

The Tableau interface features 4 specialized analytical views:

1. **Weather Disruption Analysis:** Examines the impact of rain, snow, and extreme temperatures on flight delays and cancellation rates across US hubs.
2. **Fares, Delays & Passenger Volumes:** Explores relationships between ticket pricing, TSA passenger counts, and cancellation rates.
3. **Geographic Pricing & Forecast:** Geographic map view of fares by state/city paired with time-series forecasting models for quarterly ticket pricing.
4. **Travel Recommendations:** Interactive prescriptive tool filtering low-cost windows and minimal delay probabilities for travelers.

---

## 📁 Datasets Used

* **BTS Domestic Flight Records:** Bureau of Transportation Statistics flight schedules, delays, and cancellation reasons.
* **NOAA Daily Summaries:** Meteorological records across US airport-adjacent weather stations.
* **TSA Checkpoint Travel Numbers:** Daily passenger screening counts.
* **US DOT Average Fares:** Quarterly city-pair origin and destination fare statistics.


