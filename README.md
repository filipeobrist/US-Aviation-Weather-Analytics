# Impact of Weather and Airport Volume on Flights

## 📋 Project Overview

This project explores the applicability of data analytics technologies to assess the impact of weather conditions and airport throughput on various flight characteristics in the US aviation sector. By constructing an ETL pipeline using open-source data, we built a data warehouse supported by a star schema to enhance decision support processes and provide insights into operational adversities in aviation.

**Course:** Data Analytics Technologies  
**Date:** May 10, 2026

## 👥 Authors

- Emanuel Pacheco (2024138898)
- Ivo Simões (2024160048)
- Filipe Obrist (2024170686)

## 🎯 Research Questions

Our analysis addresses six key questions:

1. **Diagnostic (DW):** Does extreme weather significantly impact travel time, flight cancellations, or delays in specific geographical locations?
2. **Diagnostic (DW):** Are flight delays related to daily passenger volumes recorded at TSA checkpoints?
3. **Descriptive (DW):** Does the number of flights or geographical location influence average ticket price?
4. **Descriptive & Diagnostic (DW):** How do ticket prices vary between "stormy" and "clear" quarters? Do high ticket prices influence cancellation rates?
5. **Predictive (DM):** Based on historical weather patterns and city travel numbers, what are the predicted ticket prices for the next quarter?
6. **Prescriptive (DM):** Which months and cities are recommended for travel to minimize weather-related delays and costs?

## 🏗️ Architecture

### Data Sources

| Source | Description |
|--------|-------------|
| **BTS Flight and Market Statistics** | Monthly domestic flight records (dates, durations, airlines, origins, destinations, times) and quarterly market data (average fares, passenger volumes) |
| **NOAA Daily Summaries** | Meteorological data including precipitation, temperatures, and snowfall from weather stations worldwide |

### ETL Pipeline

The ETL process was implemented using **Pentaho** with supporting Python interventions:

1. **Extract:** Data collected from BTS and NOAA sources (2022-2025)
2. **Transform:**
   - **Weather Cleaning:** Filter US stations, remove missing values, denormalize rows
   - **Flights Cleaning:** Exclude missing coordinates, rename attributes, split date/time components
   - **Market Traffic Cleaning:** Filter by period, select key variables, define data types
3. **Load:** PostgreSQL database hosted on DEI network VM

### Data Warehouse Schema

**Star Schema with 2 Fact Tables and 4 Dimension Tables:**

| Table Type | Table Name | Estimated Rows | Total Size |
|------------|------------|----------------|------------|
| Fact | flight | 20,360,852 | 3180 MB |
| Fact | market_traffic | 52,420 | 5232 kB |
| Dimension | weather | 1,718,053 | 163 MB |
| Dimension | time | 1,929,157 | 213 MB |
| Dimension | delay | 22,225 | 2032 kB |
| Dimension | airport | 196 | 64 kB |

**Total Data Size:** ~3,556.7 MB (68% reduction from initial 11GB)

### Data Integration

- **Performance Optimization:** Reduced load time from 32+ hours to 43 minutes (98% improvement) by using temporary PostgreSQL tables for index resolution
- **Spatial Join:** Linked flights to nearest weather stations using latitude/longitude coordinates
- **Bidirectional Mapping:** Market Traffic data available for both A→B and B→A routes

## 📊 OLAP Dashboard

An interactive 4-tab Tableau dashboard was developed:

1. **Weather Disruption Analysis** - Impact of weather on travel time, cancellations, and delays
2. **Fares, Delays & Passenger Volumes** - Correlation analysis between fares, delays, and passenger volumes
3. **Geographic Pricing & Forecast** - Geographical pricing analysis with time-series forecasting
4. **Travel Recommendations** - Optimal travel windows based on fares and delays

### Analytics Types

- **Descriptive:** Bar charts and geographic maps summarizing ticket prices
- **Diagnostic:** Correlation analysis between weather events and operational metrics
- **Predictive:** Time-series forecasting for future ticket prices
- **Prescriptive:** Recommendations for optimal travel times and locations

## 🔍 Key Findings

1. **Weather Impact:** While "Standard Weather" accounts for most flight volume, rare events like "Heavy Rain" and "Severe Rain" cause disproportionate spikes in delays and travel time. Disruptions are often localized to specific geographical hubs.

2. **Pricing vs. Performance:** "Stormy" quarters correlate with higher fares, but these prices don't consistently lead to higher cancellation rates, suggesting resilient market demand during operational friction.

3. **Predictive Limitations:** Limited temporal resolution of market data (quarterly averages) restricts forecasting precision—only 16 price observations per location over the study period.

4. **Big Data Bias:** The vast majority of standard flights overshadow rarer disruption events, making it complex to isolate every local behavior.

## 🛠️ Technologies Used

| Category | Technologies |
|----------|--------------|
| ETL | Pentaho, Python |
| Database | PostgreSQL, ONDA |
| Visualization | Tableau |
| Data Processing | SQL |

## 🚀 Prerequisites

- PostgreSQL 12+
- Pentaho Data Integration
- Tableau Desktop (for dashboard visualization)
- Python 3.8+ (for coordinate mapping)



## 📄 License

This project is developed for academic purposes as part of the Data Analytics Technologies course.

## 📚 References

1. Bureau of Transportation Statistics (BTS) - Flight and Market Statistics
2. National Oceanic and Atmospheric Administration (NOAA) - Daily Summaries
3. ONDA - Database design tool

---

*For detailed information, please refer to the [Final Report](src/Tad2526.pdf).*
