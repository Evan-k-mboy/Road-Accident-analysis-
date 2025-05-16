# Road-Accident-analysis
## Problem Statement
- Understanding the causes and conditions of road accidents is vital for improving traffic safety worldwide. This analysis aims to identify trends based on location, time, weather, road conditions, and human behavior using structured SQL queries.
 The cleaned dataset includes:
- Accident ID, Date & Time
- Location (City, Country), Latitude & Longitude
- Weather & Road Conditions
- Number of Vehicles Involved, Casualties
- Cause of Accident
- 
  ## SQL QUERIES
  1. ## Total number of accidents
     ``SELECT COUNT(*) AS total_accidents FROM accidents;
     ``
     
     2. ## Accidents that occurred in Mumbai
`` SELECT * FROM accidents WHERE Location LIKE '%Mumbai%';
``

  3. ### Total casualties reported in all accidents
     `` SELECT SUM(Casualties) AS total_casualties FROM accidents;
     ``
## Average casualties per accident by road condition
`` SELECT Road_Condition, ROUND(AVG(Casualties), 2) AS avg_casualties
FROM accidents
GROUP BY Road_Condition
ORDER BY avg_casualties DESC;
``
  ## Monthly trend in accident frequency
`` SELECT MONTH(STR_TO_DATE(Date, '%d/%m/%Y')) AS month, COUNT(*) AS accidents
FROM accidents
GROUP BY month
ORDER BY accidents DESC;
``
### Accidents with more than 5 casualties
`` SELECT *
FROM accidents
WHERE Casualties > 5
ORDER BY Casualties DESC;
``
### Top 3 accident-prone cities by total casualties
`` SELECT Location, SUM(Casualties) AS total_casualties
FROM accidents
GROUP BY Location
ORDER BY total_casualties DESC
LIMIT 3;
``
### Accident ranks by casualties within each city
`` SELECT Accident_ID, Location, Casualties,
       RANK() OVER (PARTITION BY Location ORDER BY Casualties DESC) AS rank_in_city
FROM accidents;
``
### locations with above-average accident counts
`` WITH city_accidents AS (
  SELECT Location, COUNT(*) AS accident_count
  FROM accidents
  GROUP BY Location
  ),
average_count AS (
  SELECT AVG(accident_count) AS avg_accidents FROM city_accidents
)
),
SELECT ca.Location, ca.accident_count
FROM city_accidents ca, average_count ac
WHERE ca.accident_count > ac.avg_accidents
ORDER BY ca.accident_count DESC;
``
## Accidents where weather was Fog and casualties exceeded average
`` WITH avg_casualties AS (
  SELECT AVG(Casualties) AS avg_val FROM accidents
)
SELECT *
FROM accidents, avg_casualties
WHERE Weather_Condition = 'Fog' AND Casualties > avg_val;
``
## Tools
- SQL – MySQL

