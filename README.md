# NYC Citi Bike: Rider Segments & Conversion Analytics Dashboard
## Live Interactive Dashboard
**[Click Here to View My Live Interactive Looker Studio Dashboard] (https://datastudio.google.com/reporting/d28aba4b-2880-4d21-a069-cfff3367337e)**
---
## 1. The Business Task (Ask Phase)
As a junior data analyst on the marketing team, the core objective of this project is to analyze historical ride records to identify how casual "Customers" and annul "Subscribers" utilize Citi Bike differntly.
Because annual members generate significantly more stable and profitable long-term revenue streams, these data insights will be used to design a targeted marketing strategy aimed at converting casual riders into annual subscribers.
---
## 2. Data Processing & Cleaning (Process Phase)
The analysis was performed utilizing **Google BigQuery (SQL)** to clean and aggregate millions of rows of public transit data. To ensure data integrity and remove system testing errors, the data was processed through the following constaints:
**Logical Validation:** Excluded trips where the recorded stop time preceded the start time (`stoptime > starttime`).
**False Start Removal:** Removed trips lasting under 60 seconds, which typically indicate docking station errors or immediate user cancellation.
**Outlier Filtering:** Filtered out outlier trips lasting longer than 24 hours (86,400 seconds), which typically represent stolen, or maintenance-bound bicycles.
**Feature Engineering:** Extracted temporal dimensions including `start_hour`, `day_of_week`, and created a conditional `data_type` variable segment "Weekdays" vs "Weekends."

### SQL Cleaning Script Snippet:
```sql
SELECT tripduration, starttime, stoptime, start_station_name, end_station_name, usertype,
  EXTRACT(HOUR FROM starttime) AS start_hour,
  EXTRACT(DAYOFWEEK FROM starttime) AS day_of_week,
  CASE 
    WHEN EXTRACT(DAYOFWEEK FROM starttime) IN (1, 7) THEN 'Weekend'
    ELSE 'Weekday'
  END AS day_type
FROM `bigquery-public-data.new_york_citibike.citibike_trips` 
WHERE stoptime > starttime
AND tripduration BETWEEN 60 AND 86400 
AND usertype IS NOT NULL;
```
---
## 3. Key Insights & Data Explorations (Analyze Phase)
**The commuter vs. Leisure Split:** Annual Subscribers exhibit sharp traffic spikes at 8:00 AM and 5:00 PM strictly on weekdays, proving their primary utility is for daily employment commuting. Casual customers display a smooth bell-curve volume trend that peaks heavily between 11:00 AM and 3:00 PM on weekends, signaling leisure and tourist utility.
**The Elasticity and Duration Gap:** Casual Customers keep bikes out significantly longer trip on average compared to Subscribers. This behavior exposes casual users to high overage fees due to unfamiliarity with standard timing thresholds.
**Geographic Hotspots:** Casual riders cluster heavily around public parks, landmarks, and waterfront stations. Subscribers focus their starting points tightly around urban mass transit hubs (e.g., Grand Central Station, Penn Station).
---
## 4. Strategic Business Recommendations (Act Phase)
Based on the behavioral gaps uncovered in the data, the following two strategic initiatives are recommended to optimize customer-to-subscriber conversions:
1. **Introduce a "Weekend Explorer" Pass:** Launch a targeted, lower-tier weekend subscription package. This acts as a frictionless pricing gateway to capture high-volume casual weekend leisure riders who are not yet ready to commit for a full year.
2. **Station-Specific Digital Marketing Kiosks:** Concentrate physical ad placement and digital screen real estate strictly on the top 10 casual-heavy park and waterfront stations during peek weekend hours, rather than diluting marketing budgets across city-wide campaigns.
A data analytics case study using Google BigQuery (SQL)  and Looker Studio
