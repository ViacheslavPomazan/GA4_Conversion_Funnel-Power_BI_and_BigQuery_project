# GA4 Conversion Funnel | Power BI & BigQuery Project 🚀

## Project Overview
The primary goal is to visualize the user journey from the 'session_start' to the final 'purchase' through such stages as 'view_item', 'add_to_cart', 'begin_checkout', 'add_shipping_info', 'add_payment_info', identifying where potential customers drop off and how different user segments behave throughout the funnel. Data is sourced from the `ga4_obfuscated_sample_ecommerce` public dataset, containing 4M+ rows of session data from November 1, 2020, to January 31, 2021.

## Key Objectives: 🎯
* <b>Funnel Visualization:</b> Map the progression between key events: session_start → view_item → add_to_cart → begin_checkout → add_shipping_info → add_payment_info → purchase.
* <b>User Segmentation:</b> Analyze how conversion rates vary across different dimensions such as Traffic Source, Device Category, and Geographic Location.
* <b>Behavioral Relationships:</b> Discover correlations between micro-conversions (e.g., viewing a product) and the final conversion rate (CR) to optimize the marketing budget.

## Tech Stack: ⚒️
* <b>Data Source:</b> Google Analytics 4 (BigQuery Public Dataset): `bigquery-public-data.ga4_obfuscated_sample_ecommerce`
* <b>Data Extraction:</b> Advanced SQL query in BigQuery to unnest event parameters and flatten the hierarchical GA4 structure.
* <b>BI Tool:</b> Power BI for creating an interactive dashboard with DAX measures to calculate conversion steps and drop-off rates.

## Gallery
1. 
![Funnel](image/Conversion_Funnel1.png)

2.
![Matrixl](image/Matrix1.png)

3.
![Traffic](image/Traffic_and_Journey1.png)

## Technical Highlights: ⭐
🔹 <b>SQL Query</b> performs the following functions:
   *  <b>Data Flattening:</b> Unnested GA4 event parameters to transform hierarchical JSON-like data into a flat relational structure.
   *  <b>Data Quality & Cleaning:</b> Filtered 7 key funnel events and handled session inconsistencies (removed sessions with multiple or missing session_start).
   *  <b>Scale:</b> Successfully processed a final table of 867K+ rows for Power BI visualization.

🔹 <b>Data Modelling:</b>

![model](image/ga1_funnel_model1.png)

     


 

