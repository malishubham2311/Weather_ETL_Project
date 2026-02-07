# Weather Analytics ETL & Dashboard

A comprehensive weather data analytics platform that extracts meteorological data from the Open-Meteo API, processes it through a Dagster ETL pipeline, stores it in PostgreSQL, and visualizes insights through an interactive Streamlit dashboard.

## Project Overview

This project implements a complete data engineering workflow for weather analytics, focusing on three key domains:
- **Solar Analytics**: Solar radiation, temperature, cloud coverage data
- **Agricultural Insights**: Soil conditions, moisture levels, evapotranspiration data
- **Marine & Wind Analysis**: Wind speed, wave conditions, and marine weather patterns

The system continuously ingests historical and real-time weather data from Open-Meteo, processes it through Dagster assets, and serves interactive dashboards for data exploration and analysis.

## Architecture

```
├── dags/                           # Dagster job definitions (future)
├── src/                            # Source code modules
├── weather_etl_project/
│   ├── postgres.py                # ETL pipeline with Dagster assets
│   ├── dashboard/                 # Streamlit dashboard application
│   │   ├── app.py                # Main dashboard entry point
│   │   ├── utils.py              # Utility functions for data loading
│   │   └── pages/                # Multi-page dashboard views
│   │       ├── Overview.py
│   │       ├── Agricultural_Insights.py
│   │       ├── Correlations.py
│   │       ├── Map.py
│   │       ├── Solar_Analytics.py
│   │       ├── Temporal_Analysis.py
│   │       └── Wind_Marine.py
│   ├── dagster_home/             # Dagster configuration & storage
│   └── database_conn_test.py     # Database connection testing
└── requirements.txt               # Python dependencies
```

## Data Pipeline

### Data Source
- **Open-Meteo API**: Free weather data API providing historical and forecast data
- **Location**: Dublin, Ireland (Lat: 53.34, Lon: -6.26)
- **Date Range**: 2023-01-01 to 2023-12-31

### Pipeline Stages

1. **Raw Data Ingestion** (`openmeteo_raw`)
   - Fetch data from Open-Meteo API
   - Store raw API responses

2. **Data Transformation** (`openmeteo_tables`)
   - Clean and normalize data
   - Create structured datasets for each domain (solar, agricultural, marine)

3. **Data Loading** (`postgres_load`)
   - Load processed data into PostgreSQL database
   - Load processed data into MongoDB Atlas
   - Create fact and dimension tables
   - Both databases are populated in parallel



## Getting Started

### Prerequisites

- Python 3.8+
- PostgreSQL database
- MongoDB Atlas cluster (free tier available)
- pip or conda

### Installation

1. **Clone the repository**:
   ```bash
   cd "c:\Users\nisha\Desktop\Data-Analysis\analytics programming\Project\MAIN"
   ```

2. **Create virtual environment** (optional but recommended):
   ```bash
   python -m venv venv
   venv\Scripts\activate
   ```

3. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

4. **Configure environment variables**:
   Create a `.env` file in `weather_etl_project/` with the following variables:
   ```
   # PostgreSQL
   PG_HOST=127.0.0.1
   PG_PORT=5432
   PG_USER=your_pg_user
   PG_PASSWORD=your_pg_password
   PG_DB=weather_demo
   
   # MongoDB Atlas (optional but recommended)
   MONGO_URL=mongodb+srv://username:password@cluster.mongodb.net/?appName=YourApp
   MONGO_DB=weather_analytics
   
   # Dagster
   DAGSTER_HOME=/path/to/dagster_home
   ```

5. **Set up PostgreSQL database**:
   ```bash
   createdb weather_demo
   ```

6. **Set up MongoDB Atlas** (optional):
   - Create a free cluster at [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
   - Create a database user with read/write permissions
   - Get your connection string and add it to `.env` as `MONGO_URL`

### Running the Project

#### Option 1: Dashboard Only
```bash
cd weather_etl_project/dashboard
streamlit run app.py
```
Access the dashboard at `http://localhost:8501`

#### Option 2: Full ETL Pipeline
```bash
cd weather_etl_project
dagster dev -f postgres.py
```
Access Dagster UI at `http://localhost:3000`

Then in another terminal:
```bash
cd weather_etl_project/dashboard
streamlit run app.py
```

##  Dependencies

### Core ETL
- **dagster** (1.6.0): Orchestration framework
- **sqlalchemy** (2.0.23): ORM for database operations
- **psycopg2-binary** (2.9.9): PostgreSQL adapter- **pymongo** (4.6.0): MongoDB driver for Atlas
### Dashboard
- **streamlit** (1.30.0): Web framework
- **plotly** (5.18.0): Interactive visualizations

### Data Processing
- **pandas** (2.1.4): Data manipulation
- **numpy** (1.26.2): Numerical computing
- **scikit-learn** (1.3.2): Machine learning utilities
- **statsmodels** (0.14.1): Statistical analysis

### Utilities
- **requests** (2.31.0): HTTP library for API calls
- **python-dotenv** (1.0.0): Environment variable management

## Key Features

**Automated ETL Pipeline**: Dagster-orchestrated data ingestion and transformation  
**Dual Database Support**: PostgreSQL + MongoDB Atlas for flexible data access  
**Multi-Domain Analytics**: Solar, agricultural, and marine weather insights  
**Interactive Dashboard**: Real-time data visualization with Streamlit  
**Statistical Analysis**: Correlations, trends, and forecasting capabilities  
**API Integration**: Direct integration with Open-Meteo weather API  
**Modular Design**: Reusable assets and utilities for extensibility  
**Data Quality Checks**: Automatic imputation, outlier detection, and validation  

##  Development

### Project Structure
- `postgres.py`: Main ETL pipeline with Dagster assets
- `dashboard/app.py`: Dashboard home page
- `dashboard/utils.py`: Database connection and data loading utilities
- `database_conn_test.py`: Connection testing utilities

### Adding New Analyses
1. Create a new page in `dashboard/pages/`
2. Import utilities from `dashboard/utils.py`
3. Use Plotly for visualizations
4. Add navigation in sidebar

### Extending the Pipeline
1. Add new assets to `postgres.py`
2. Define data sources and transformations
3. Configure outputs in Dagster
4. Test with `database_conn_test.py`


### Dashboard Configuration
Modify `dashboard/app.py`:
- Page layout and styling
- Filter options
- Default date ranges
- Metric calculations

## Troubleshooting

### Database Connection Issues
```bash
python weather_etl_project/database_conn_test.py
```

### Dashboard Won't Start
- Check if all dependencies are installed: `pip install -r requirements.txt`
- Verify database is running
- Check `.env` file configuration

### ETL Pipeline Errors
- Review logs in `dagster_home/logs/`
- Check API connectivity to Open-Meteo
- Verify database permissions

## Data Schema

### Fact Table
"time"
"temperature_2m"
"precipitation"
"cloud_cover"
"cloud_cover_low"
"shortwave_radiation"
"direct_radiation"
"diffuse_radiation"
"wind_speed_10m"
"month_name"
"hour"
"day_of_week"
"is_weekend"
"rain"
"et0_fao_evapotranspiration"
"vapor_pressure_deficit"
"pressure_msl"
"surface_pressure"
"temperature_2m_marine"
"wind_speed_10m_marine"
"wind_speed_100m"
"wind_direction_100m"
"wind_gusts_10m"

