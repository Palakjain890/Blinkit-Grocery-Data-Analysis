# 🛒 Blinkit Sales Data Analysis Using SQL & Power BI

This repository showcases a comprehensive analysis of Blinkit grocery store sales using **SQL for data wrangling** and **Power BI for data visualization**. The project demonstrates how to clean data, generate key metrics (KPIs), and extract actionable insights using SQL, followed by visual storytelling through interactive dashboards.

---

## 📌 Project Objective

The goal of this project is to perform **end-to-end sales analysis** of Blinkit data by:
- Cleaning categorical variables using SQL.
- Generating business-relevant KPIs across product and outlet dimensions.
- Building an interactive Power BI dashboard for decision-makers.

---

## 🗃️ Dataset Overview

- **Table Name:** `blinkit_data`
- **Total Rows:** (varies)
- **Key Fields Used:**
  - `Item_Fat_Content`, `Item_Type`, `Sales`, `Rating`
  - `Outlet_Location_Type`, `Outlet_Establishment_Year`, `Outlet_Size`, `Outlet_Type`

---

## 🧹 Data Cleaning (SQL)

### ✅ Standardizing Item Fat Content

```sql
UPDATE blinkit_data
SET Item_Fat_Content = 
  CASE 
    WHEN Item_Fat_Content IN ('LF', 'low_fat') THEN 'Low Fat'
    WHEN Item_Fat_Content = 'reg' THEN 'Regular'
    ELSE Item_Fat_Content
  END;

SELECT DISTINCT Item_Fat_Content FROM blinkit_data;
-- Output: Low Fat, Regular
📈 SQL Analysis – KPIs and Trends
🔹 Overall Metrics

SELECT CAST(SUM(Sales)/1000000 AS DECIMAL(10,2)) AS total_sales_millions FROM blinkit_data;
SELECT CAST(AVG(Sales) AS DECIMAL(10,1)) AS Avg_Sales FROM blinkit_data;
SELECT COUNT(*) AS No_Of_Items FROM blinkit_data;
SELECT CAST(AVG(Rating) AS DECIMAL(10,2)) AS Avg_Rating FROM blinkit_data;
🔹 Group-wise Insights
By Item_Fat_Content

By Item_Type (with Top 5 Products)

By Outlet_Location_Type (Pivoted by Fat Content)

By Outlet_Establishment_Year

By Outlet_Size (with % Sales)

By Outlet_Location_Type (for year 2020)

By Outlet_Type

All SQL queries used are available in the Blinkit Analysis Using SQL.pdf file.

📊 Power BI Dashboard
An interactive dashboard was created using Power BI (Blinkit Dashboard.pbix) which includes:

Total sales and average sales cards

Visuals filtered by item type, outlet type, and fat content

Bar and pie charts to explore sales distribution

Drilldowns by year and location

💡 To view the dashboard:

Download and open Blinkit Dashboard.pbix in Power BI Desktop.

Explore the slicers, filters, and visuals for dynamic insights.

🔮 Future Enhancements
Integrate forecasting models using Python/Power BI for predictive analytics.

Expand dashboard with customer segmentation and time-series trends.

Automate data refresh using Power BI Service + SQL database connection.
