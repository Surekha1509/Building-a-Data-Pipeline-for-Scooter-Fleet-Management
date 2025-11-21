Case Study: Gans

# Scooter Fleet Data Pipeline

## Project Overview
This project builds a data pipeline for a scooter fleet management company.   
integrates:
- Weather data from OpenWeatherMap API- provides temperature, humidity, wind, and conditions.
- Flight data from AeroDataBox API - tracks flights arriving/departing near cities with scooter fleets.
- Optional web-scraped data-e.g., traffic, events, or local conditions affecting scooters.

The pipeline normalizes and stores all data in a **MySQL database**, enabling analytics, reporting, and future machine learning applications.

## Data Pipeline
**Fetch Data**
Python scripts call APIs and optionally scrape web data.
Raw data is collected in JSON or other formats.
 
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
                                 +----------------+

  3. **Store in MySQL**  
   Insert cleaned data into 'Population' `City`, `Airport`, `Flight`, and `Weather` tables.
                       
## SQL Table Schema (MySQL)
# MySQL database: connecting with python

CREATE TABLE cities (
    city_id INT PRIMARY KEY AUTO_INCREMENT,
    city_name VARCHAR(100) NOT NULL,
    country_code CHAR(2) NOT NULL,
    latitude DECIMAL(10,6) NOT NULL,
    longitude DECIMAL(10,6) NOT NULL
);

CREATE TABLE populations (
    population_id INT PRIMARY KEY AUTO_INCREMENT,
    city_id INT NOT NULL,
    population DECIMAL(10,2),
    year YEAR NOT NULL,
    FOREIGN KEY (city_id) REFERENCES cities(city_id)
);

CREATE TABLE weather (
    weather_id INT PRIMARY KEY AUTO_INCREMENT,
    city_id INT NOT NULL,
    timestamp_weather DATETIME NOT NULL,
    temperature DECIMAL(5,2),
    humidity INT,
    wind_speed DECIMAL(5,2),
    condition VARCHAR(100),
    FOREIGN KEY (city_id) REFERENCES cities(city_id)
);

CREATE TABLE flights (
    flight_id INT PRIMARY KEY AUTO_INCREMENT,
    city_id INT NOT NULL,
    timestamp_flight DATETIME NOT NULL,
    flights_arriving INT,
    flights_departing INT,
    airport_code VARCHAR(10),
    FOREIGN KEY (city_id) REFERENCES cities(city_id)
);


2. **Normalize & Clean Data**  
   Flatten JSON responses into pandas DataFrames.
   
   Use pandas to flatten JSON responses.
Standardize columns, handle missing data, correct types.
Ensure latitude, longitude, timestamps, and IDs are consistent.

4. **Analyze**  
   Query flights, monitor weather, and visualize trends.
   
Analytics & Reporting
Query MySQL to monitor scooter fleet needs:
eather impact on usage.
Flight schedules affecting demand near airports.
Enable dashboards for visualization or predictive modeling.
