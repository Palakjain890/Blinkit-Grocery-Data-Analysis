# 🛒 Blinkit Sales Data Analysis Using SQL & Power BI

This project presents an end-to-end analysis of Blinkit grocery store sales data using **SQL for data transformation and aggregation**, and **Power BI** for interactive visualization. It highlights key performance indicators (KPIs), patterns across products and outlets, and data cleaning operations to improve analytical accuracy.

---

## 🎯 Objective

The primary objective is to:
- Clean and preprocess sales data using SQL
- Generate KPIs such as total sales, item count, and average ratings
- Segment performance by product type, outlet characteristics, and fat content
- Visualize key insights through an interactive dashboard built in Power BI

---

## 🧹 Data Cleaning (SQL)

Before performing any analysis, inconsistent values in the `Item_Fat_Content` column were standardized:

```sql
UPDATE blinkit_data
SET Item_Fat_Content = 
  CASE 
    WHEN Item_Fat_Content IN ('LF', 'low_fat') THEN 'Low Fat'
    WHEN Item_Fat_Content = 'reg' THEN 'Regular'
    ELSE Item_Fat_Content
  END;

---

## 📊 KPI Metrics Using SQL

### 🔹 Overall Business Metrics

```sql
-- Total Sales (in millions)
SELECT CAST(SUM(Sales)/1000000 AS DECIMAL(10,2)) AS total_sales_millions FROM blinkit_data;

-- Average Sales
SELECT CAST(AVG(Sales) AS DECIMAL(10,1)) AS Avg_Sales FROM blinkit_data;

-- Total Number of Items
SELECT COUNT(*) AS No_Of_Items FROM blinkit_data;

-- Average Rating
SELECT CAST(AVG(Rating) AS DECIMAL(10,2)) AS Avg_Rating FROM blinkit_data;
```

---

## 📈 Segmented Analysis Using SQL

### 1️⃣ Metrics by Item Fat Content

```sql
SELECT Item_Fat_Content, 
       CAST(SUM(Sales)/1000 AS DECIMAL(10,2)) AS Total_Sales_Thousands,
       CAST(AVG(Sales) AS DECIMAL(10,1)) AS Avg_Sales,
       COUNT(*) AS No_Of_Items,
       CAST(AVG(Rating) AS DECIMAL(10,2)) AS Avg_Rating
FROM blinkit_data
GROUP BY Item_Fat_Content
ORDER BY Total_Sales_Thousands DESC;
```

### 2️⃣ Metrics by Item Type (Top 5)

```sql
SELECT TOP 5 Item_Type,
       CAST(SUM(Sales)/1000 AS DECIMAL(10,2)) AS Total_Sales,
       CAST(AVG(Sales) AS DECIMAL(10,1)) AS Avg_Sales,
       COUNT(*) AS No_Of_Items,
       CAST(AVG(Rating) AS DECIMAL(10,2)) AS Avg_Rating
FROM blinkit_data
GROUP BY Item_Type
ORDER BY Total_Sales DESC;
```

### 3️⃣ Pivot: Sales by Fat Content & Outlet Type

```sql
SELECT Outlet_Location_Type,
       ISNULL([Low Fat], 0) AS Low_Fat,
       ISNULL([Regular], 0) AS Regular
FROM (
  SELECT Outlet_Location_Type, Item_Fat_Content,
         CAST(SUM(Sales) AS DECIMAL(10,2)) AS Total_Sales
  FROM blinkit_data
  GROUP BY Outlet_Location_Type, Item_Fat_Content
) AS SourceTable
PIVOT (
  SUM(Total_Sales)
  FOR Item_Fat_Content IN ([Low Fat], [Regular])
) AS PivotTable;
```

### 4️⃣ Metrics by Outlet Establishment Year

```sql
SELECT Outlet_Establishment_Year,
       CAST(SUM(Sales) AS DECIMAL(10,2)) AS Total_Sales,
       CAST(AVG(Sales) AS DECIMAL(10,1)) AS Avg_Sales,
       COUNT(*) AS No_Of_Items,
       CAST(AVG(Rating) AS DECIMAL(10,2)) AS Avg_Rating
FROM blinkit_data
GROUP BY Outlet_Establishment_Year
ORDER BY Total_Sales DESC;
```

### 5️⃣ Percentage of Sales by Outlet Size

```sql
SELECT Outlet_Size,
       CAST(SUM(Sales) AS DECIMAL(10,2)) AS Total_Sales,
       CAST((SUM(Sales) * 100.0 / SUM(SUM(Sales)) OVER()) AS DECIMAL(10,2)) AS Sales_Percentage
FROM blinkit_data
GROUP BY Outlet_Size
ORDER BY Total_Sales DESC;
```

### 6️⃣ Metrics by Outlet Location (Year = 2020)

```sql
SELECT Outlet_Location_Type,
       CAST(SUM(Sales) AS DECIMAL(10,2)) AS Total_Sales,
       CAST((SUM(Sales) * 100.0 / SUM(SUM(Sales)) OVER()) AS DECIMAL(10,2)) AS Sales_Percentage,
       CAST(AVG(Sales) AS DECIMAL(10,1)) AS Avg_Sales,
       COUNT(*) AS No_Of_Items,
       CAST(AVG(Rating) AS DECIMAL(10,2)) AS Avg_Rating
FROM blinkit_data
WHERE Outlet_Establishment_Year = 2020
GROUP BY Outlet_Location_Type
ORDER BY Total_Sales DESC;
```

### 7️⃣ Metrics by Outlet Type

```sql
SELECT Outlet_Type,
       CAST(SUM(Sales) AS DECIMAL(10,2)) AS Total_Sales,
       CAST((SUM(Sales) * 100.0 / SUM(SUM(Sales)) OVER()) AS DECIMAL(10,2)) AS Sales_Percentage,
       CAST(AVG(Sales) AS DECIMAL(10,1)) AS Avg_Sales,
       COUNT(*) AS No_Of_Items,
       CAST(AVG(Rating) AS DECIMAL(10,2)) AS Avg_Rating
FROM blinkit_data
GROUP BY Outlet_Type
ORDER BY Total_Sales DESC;
```

📄 **Full SQL script and outputs available in:** `Blinkit Analysis Using SQL.pdf`

---

## 📊 Power BI Dashboard

The Power BI dashboard (`Blinkit Dashboard.pbix`) includes:
- Total and average sales
- Sales by item type and fat content
- Breakdown by outlet type, location, and size
- Slicers for interactive filtering
- Establishment year-based trends

💡 **How to view:**
1. Download the `.pbix` file
2. Open it in [Power BI Desktop](https://powerbi.microsoft.com/)
3. Explore the filters and visuals interactively

---

## 🔮 Future Work

- Implement time-series forecasting models using Python or Power BI’s predictive tools
- Add promotional, seasonal, or customer demographic data for deeper segmentation
- Publish the dashboard online with auto-refresh via Power BI Service
- Connect directly to a SQL database for real-time reporting
 
---

**Thanks for exploring my Blinkit Sales Analysis project!**

