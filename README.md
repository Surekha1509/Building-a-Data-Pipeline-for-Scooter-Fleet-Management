Phase 1:
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

phase 2: 
🛴 Gans Data Engineering Project
💼 Case Study: Gans — E-Scooter Data Pipeline
Welcome to the Gans Data Engineering Project, a case study designed to simulate a real-world data engineering challenge.
Gans is a growing e-scooter sharing startup aiming to optimize scooter distribution in major cities worldwide.

This repository contains the complete ETL (Extract, Transform, Load) pipeline for collecting, cleaning, and storing data from multiple external sources — such as APIs and web scraping — and automating the process through Google Cloud Functions.

🚀 Project Overview
The project’s main goal is to help Gans anticipate scooter movements and optimize placement across different cities.
To do that, we collect and merge data from multiple external sources that influence scooter usage, such as:

🌦 Weather conditions (via OpenWeather API)
🛫 Airport and flight activity (influence of tourist arrivals)
🏙 City demographics (population, density, etc.)
All of this data is processed using Python and pandas, and stored in a MySQL database hosted on Google Cloud.

🧩 Architecture
+-------------------+
|  External Sources |
|-------------------|
| Wikipedia (cities)|
| OpenWeather API   |
| Flight APIs       |
+--------+----------+
         |
         v
+-------------------+
|  Data Collection  |
| (requests, BS4)   |
+--------+----------+
         |
         v
+-------------------+
|   Data Cleaning   |
| (pandas, utils)   |
+--------+----------+
         |
         v
+-------------------+
|  Cloud Storage    |
|  (MySQL on GCP)   |
+--------+----------+
         |
         v
+-------------------+
|  Automation Layer |
| (Cloud Functions) |
+-------------------+

🌍 Why the Cloud?
Migrating the database to the cloud allows:


⚙️ Setup & Installation
1️⃣ Clone this repository
git clone https://github.com/
cd gans-data-engineering
2️⃣ Install dependencies
pip install -r requirements.txt
3️⃣ Set up your environment variables
Go to utils.py and write your informations.

RAPID_API_KEY = "your_api_key"
WEATHER_API_KEY = "your_api_key"
schema = "your_db_schema_name"
host = "your_gcp_ip_address"
password = "your_gcp_password"

4️⃣ Run locally
python cities.py
python population.py
python airport.py
python request.py

5️⃣ Deploy to Google Cloud
Make sure your main.py contains a properly defined entry point: @functions_framework.http def main(request): # your ETL logic here return 'Data successfully added.'

🧩 Key Learnings
The difference between web scraping and API-based data collection
How to build a structured ETL pipeline
Using Python and pandas to interact with a MySQL Cloud database
Deploying and automating scripts with Google Cloud Functions
Debugging best practices using Flask before deployment
Understanding that there are always multiple ways to achieve a task — and choosing the most efficient and reliable one
