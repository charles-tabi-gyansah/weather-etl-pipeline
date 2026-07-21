# Weather Data ETL Pipeline

**Week 7 Internship Task — AnalystLab Africa**
Batch B (June 1 – August 1, 2026)

## Project Overview

This project builds a simple, end-to-end **ETL (Extract, Transform, Load) pipeline** in Python that collects real-time weather data from a public API, cleans and structures it, stores it for future analysis, and produces a short comparative analysis across multiple cities.

## Data Source

- **API:** [OpenWeather Current Weather Data API](https://openweathermap.org/api)
- **Cities collected:** Lagos, Accra, Nairobi, London
- **Fields retrieved:** City name, temperature, humidity, weather condition, wind speed, date/time of reading

## ETL Process

### 1. Extract
- Connected to the OpenWeather API using `requests`, authenticating with a personal API key.
- API key stored securely in a local `.env` file (excluded from version control via `.gitignore`) and loaded using `python-dotenv`, rather than hardcoding it in the source code.
- Looped through a list of 4 cities, sending one API request per city and collecting the raw JSON response for each.
- Included status-code checks (`200` = success) to confirm each request succeeded before proceeding.

### 2. Transform
- Parsed each city's raw JSON response and extracted only the relevant fields: city name, temperature, humidity, weather description, wind speed, and timestamp.
- Renamed fields into clear, consistent column names (e.g. `Temperature_C`, `Humidity_%`).
- Converted the API's raw Unix timestamp into a human-readable datetime using Python's `datetime` module.
- Assembled all cleaned records into a single structured Pandas DataFrame.
- Verified data types (numeric columns as `float64`/`int64`, timestamp as `datetime64`) and confirmed there were no missing values.

### 3. Load
- Saved the cleaned dataset in two formats:
  - **CSV file** (`weather_data_cleaned.csv`) — a portable, universally readable format.
  - **SQLite database** (`weather_data.db`, table `weather_data`) — a lightweight, queryable database format, demonstrating SQL-based storage.

### 4. Analysis
- Compared temperatures across all 4 cities and ranked them from hottest to coldest.
- Identified the hottest city, coldest city, and the city with the highest humidity using Pandas' `idxmax()`/`idxmin()`.
- Compared live weather conditions across cities.
- Visualized the temperature comparison using a Matplotlib bar chart.

## Tools Used

- **Python** — core language
- **Requests** — API calls
- **Pandas** — data transformation and structuring
- **python-dotenv** — secure API key management
- **SQLite3** — database storage
- **Matplotlib** — data visualization
- **Jupyter Notebook (Anaconda)** — development environment

## Key Findings

- **Hottest city:** Accra at 24.31°C
- **Coldest city:** Nairobi at 14.70°C — nearly a 10°C difference from Accra, despite both being African cities, reflecting the impact of elevation and regional climate.
- **Most humid city:** Lagos at 93% humidity, paired with overcast cloud conditions — consistent with its coastal, tropical climate.
- **Weather conditions varied noticeably** across all 4 cities at the time of collection: overcast clouds (Lagos), broken clouds (Accra and Nairobi), and clear sky (London).
- This dataset reflects a single point-in-time snapshot; running this pipeline on a schedule (e.g. daily or hourly) would enable genuine trend analysis over time rather than a one-off comparison.

## Project Files

| File | Description |
|---|---|
| `Weather_ETL_Pipeline.ipynb` | Full Jupyter Notebook — extraction, transformation, loading, and analysis |
| `weather_data_cleaned.csv` | Final cleaned dataset (CSV format) |
| `weather_data.db` | Final cleaned dataset (SQLite database) |
| `README.md` | This documentation file |
| `.gitignore` | Excludes the private `.env` file from version control |

## How to Run This Project

1. Clone this repository.
2. Create a free account at [openweathermap.org](https://openweathermap.org/api) and generate an API key.
3. Create a file named `API.env` in the project root containing:
   ```
   OPENWEATHER_API_KEY=your_api_key_here
   ```
4. Install dependencies:
   ```
   pip install pandas requests python-dotenv matplotlib
   ```
5. Open `Weather_ETL_Pipeline.ipynb` in Jupyter Notebook and run all cells.

---
*Submitted as part of the AnalystLab Africa Data Analyst Internship — Week 7: Data Pipelines & Automation.*
