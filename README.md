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
   
## Data Pipeline Flow Diagram Placeholder
   
+----------------+      +----------------+      +----------------+      +----------------+
| OpenWeatherMap |      | AeroDataBox API|      | Optional Web   |      | Python Scripts |
|    API         |      |    API         |      | Scraping       |      | (Data Fetching)|
+--------+-------+      +--------+-------+      +--------+-------+      +--------+-------+
         |                        |                        |                       |
         +------------------------+------------------------+-----------------------+
                                         |
                                         v
                                 +----------------+
                                 | Data Cleaning  |
                                 | & Normalization|
                                 |   (pandas)    |
                                 +--------+-------+
                                          |
                                          v
                                 +----------------+
                                 |    MySQL  |
                                 |   Database     |
                                 +--------+-------+
                                          |
                                          v
                                 +----------------+
                                 |  Analytics /   |
                                 |  Dashboard /   |
                                 |     ML         |
                                 +----------------+

## SQL Table Schema (MySQL)

CREATE TABLE City (
    city_id INT PRIMARY KEY,
    name VARCHAR(100),
    country VARCHAR(50),
    latitude DECIMAL(9,6),
    longitude DECIMAL(9,6)
);

CREATE TABLE Airport (
    ICAO VARCHAR(10) PRIMARY KEY,
    city_id INT,
    name VARCHAR(100),
    type VARCHAR(50),
    FOREIGN KEY (city_id) REFERENCES City(city_id)
);

CREATE TABLE Flight (
    flight_id INT PRIMARY KEY AUTO_INCREMENT,
    ICAO VARCHAR(10),
    flight_number VARCHAR(20),
    airline VARCHAR(100),
    arrival_time DATETIME,
    departure_airport VARCHAR(10),
    FOREIGN KEY (ICAO) REFERENCES Airport(ICAO)
);

CREATE TABLE Weather (
    weather_id INT PRIMARY KEY AUTO_INCREMENT,
    city_id INT,
    temp DECIMAL(5,2),
    description VARCHAR(100),
    timestamp DATETIME,
    FOREIGN KEY (city_id) REFERENCES City(city_id)
);
