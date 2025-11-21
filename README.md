Case Study: Gans

# Scooter Fleet Data Pipeline

## Project Overview
This project builds a data pipeline for a scooter fleet management company.   
integrates:
- Weather data from OpenWeatherMap API
- Flight data from AeroDataBox API
- Optional web-scraped data

The pipeline normalizes and stores all data in a **MySQL database**, enabling analytics, reporting, and future machine learning applications.

## Data Pipeline
1. **Fetch Data**  
   Python scripts call APIs and optionally scrape web data.
2. **Normalize & Clean Data**  
   Flatten JSON responses into pandas DataFrames.
3. **Store in MySQL**  
   Insert cleaned data into 'Population' `City`, `Airport`, `Flight`, and `Weather` tables.
4. **Analyze**  
   Query flights, monitor weather, and visualize trends.

