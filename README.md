# GA4 Conversion Funnel | Power BI & BigQuery Project 🚀

## Project Overview
The primary goal is to visualize the user journey from the 'session_start' to the final 'purchase' through such stages as 'view_item', 'add_to_cart', 'begin_checkout', 'add_shipping_info', 'add_payment_info', identifying where potential customers drop off and how different user segments behave throughout the funnel. Project visualization was developed in two versions: Power BI Report and Tableau Dashboard. Data is sourced from the `ga4_obfuscated_sample_ecommerce` public dataset, containing 4M+ rows of session data from November 1, 2020, to January 31, 2021.

## Key Objectives: 🎯
* <b>Funnel Visualization:</b> Map the progression between key events: session_start → view_item → add_to_cart → begin_checkout → add_shipping_info → add_payment_info → purchase.
* <b>User Segmentation:</b> Analyze how conversion rates vary across different dimensions such as Traffic Source, Device Category, and Geographic Location.
* <b>Behavioral Relationships:</b> Discover correlations between micro-conversions (e.g., viewing a product) and the final conversion rate (CR) to optimize the marketing budget.
* <b>Event Timestamping:</b> Define and calculate timestamps for each step in the conversion funnel.

## Tech Stack: ⚒️
* <b>Data Source:</b> Google Analytics 4 (BigQuery Public Dataset): `bigquery-public-data.ga4_obfuscated_sample_ecommerce`
* <b>Data Extraction:</b> Advanced SQL query in BigQuery.
* <b>Visualization & Modeling:</b> Power BI and Tableu for creating an interactive dashboards. 
* <b>Metrics Calculation:</b> Developed business logic using DAX in Power BI and a combination of LOD expressions and Calculated Fields in Tableau.

## Gallery
1. 
![Funnel](image/Conversion_Funnel1.png)

2.
![Matrixl](image/Matrix1.png)

3.
![Traffic](image/Traffic_and_Journey1.png)


👉 [Link to Tableau Public Dashboard](https://public.tableau.com/views/GA4ConversionFunnelProject/DashboardConversion2?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)

## Technical Highlights: ⭐
🔹 <b>SQL Query</b> performs the following functions:
   *  <b>Data Flattening:</b> Unnested GA4 event parameters to transform hierarchical JSON-like data into a flat relational structure.
   *  <b>Data Quality & Cleaning:</b> Filtered 7 key funnel events and handled session inconsistencies (removed sessions with multiple or missing session_start).
   *  <b>Scale:</b> Successfully processed a final table of 867K+ rows for Power BI visualization.

<details>
<summary><b>👉 Code querying data from BigQuery is here</b></summary>

```sql
WITH init AS (
  SELECT timestamp_micros(event_timestamp) AS event_timestamp,
      (SELECT value.int_value FROM UNNEST(event_params) WHERE key = 'ga_session_id') AS session_id ,
          user_pseudo_id || '+' ||
          CAST((SELECT value.int_value FROM UNNEST(event_params) WHERE key = 'ga_session_id') AS string) AS user_session_id, 
      event_name,
      REGEXP_EXTRACT(
                (SELECT value.string_value FROM UNNEST(event_params) WHERE key = 'page_location'), 
                            r'(?:\w+:\/\/)?[^\/]+\/(.*)') AS landing_page_location,
      traffic_source.source AS sourse, traffic_source.medium AS medium, traffic_source.name AS campaign,
      geo.country AS country, device.category AS device_category, 
      device.operating_system AS operating_system,
      device.language AS device_language
  FROM `bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_*`
  WHERE event_name IN ('session_start', 'view_item', 'add_to_cart', 'begin_checkout',
                        'add_shipping_info', 'add_payment_info', 'purchase')
  ),
rank1 as (
  SELECT *, 
     ROW_NUMBER() OVER (PARTITION BY user_session_id ORDER BY event_timestamp ASC ) AS rn
  FROM init
  WHERE event_name = 'session_start'
  ),
init_filtered AS (
  SELECT event_timestamp,
        session_id, user_session_id,
        event_name,
        landing_page_location,
        sourse, medium, campaign,
        country, device_category,
        operating_system, device_language
  FROM rank1
  WHERE rn = 1
  UNION ALL
  SELECT *
  FROM init
  WHERE event_name <> 'session_start' --877 538
  ),
start_table as (
  SELECT user_session_id
  FROM init_filtered
  WHERE event_name = 'session_start'
  )
SELECT i.event_timestamp,
        i.session_id, st.user_session_id,
        i.event_name,
        i.landing_page_location,
        i.sourse, i.medium, i.campaign,
        i.country, i.device_category,
        i.operating_system, i.device_language
FROM start_table st LEFT JOIN init_filtered i 
ON st.user_session_id = i.user_session_id
```
</details>
 
🔹 <b>Data Modelling:</b>

![model](image/ga1_funnel_model1.png)

     


 
## Feedback and Collaboration 🙌

If you have any feedback regarding the data modeling, DAX formulas, or visualization choices, please open an issue or reach out to me directly. I'm also open to collaboration and welcome any contributions that could enhance the report's functionalities!

