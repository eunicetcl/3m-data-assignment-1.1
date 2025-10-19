# Video Game Sales Analysis

## Dataset

You will be working with the following dataset: [Video Game Sales](https://www.kaggle.com/datasets/gregorut/videogamesales?resource=download)

📦 **Dataset Download Instructions**
1. Download the dataset ZIP file from the above link.
2. After downloading: Unzip the file to access vgsales.csv. Note the full file path to vgsales.csv — you'll need it in the next step.

🔍 **Challenge: Load the Data into DuckDB**
Using DBeaver and your DuckDB connection, how would you load the vgsales.csv file into a table so you can begin querying it?

## Business Question
How can game developers and publishers optimize their strategy to maximize global sales by understanding the performance of different game genres, platforms, and publishers?

*To answer the above question, use the following SQL queries to explore the dataset and address the following questions:*

Which genres contribute the most to global sales?

SQL:
```
CREATE TABLE vgsales AS
SELECT * FROM read_csv_auto('/Users/eunicetan/Downloads/vgsales.csv');

SELECT * FROM vgsales;

SELECT Genre, SUM(Global_Sales) AS Total_Global_Sales
FROM vgsales
GROUP BY Genre
ORDER BY Total_Global_Sales DESC;
```
Findings:
```findings
Action - 1751.18

```
Which platforms generate the highest global sales?

SQL:
```sql
SELECT Platform, SUM(Global_Sales) AS Total_Global_Sales
FROM vgsales
GROUP BY Platform
ORDER BY Total_Global_Sales DESC;
```
Findings:
```findings
PS2 - 1255.64
```
Which publishers are the most successful in terms of global sales?

SQL:
```sql
SELECT 
    Publisher,
    SUM(Global_Sales) AS Total_Global_Sales
FROM vgsales
GROUP BY Publisher
ORDER BY Total_Global_Sales DESC;

```
Findings:
```findings
Nintendo : 1786.56
```
How does success vary across regions (North America, Europe, Japan, Others)?

SQL:
```sql
SELECT 'North America' AS Region, ROUND(SUM(NA_Sales), 0) AS Total_Sales,
  ROUND(SUM(NA_Sales) * 100.0 / SUM(Global_Sales), 0) AS Percent_of_Global_Sales
FROM vgsales
UNION ALL
SELECT
  'Europe', ROUND(SUM(EU_Sales), 0),
  ROUND(SUM(EU_Sales) * 100.0 / SUM(Global_Sales), 0)
FROM vgsales
UNION ALL
SELECT
  'Japan', ROUND(SUM(JP_Sales), 0),
  ROUND(SUM(JP_Sales) * 100.0 / SUM(Global_Sales), 0)
FROM vgsales
UNION ALL
SELECT
  'Others', ROUND(SUM(Other_Sales), 0),
  ROUND(SUM(Other_Sales) * 100.0 / SUM(Global_Sales), 0)
FROM vgsales;

```
Findings:
```findings
North America - 4393 (total sales), 49%
Europe - 2434, 27%
Japan - 1291, 14%
Others, 798, 9%
```
What are the trends over time in game sales by genre and platform?

SQL:
```sql
WITH
genre_totals AS (
  SELECT Year, Genre, SUM(Global_Sales) AS total_sales
  FROM vgsales
  WHERE Year IS NOT NULL
  GROUP BY Year, Genre
),
ranked_genres AS (
  SELECT
    Year,
    Genre,
    total_sales,
    RANK() OVER (PARTITION BY Year ORDER BY total_sales DESC) AS rnk
  FROM genre_totals
),
platform_totals AS (
  SELECT Year, Platform, SUM(Global_Sales) AS total_sales
  FROM vgsales
  WHERE Year IS NOT NULL
  GROUP BY Year, Platform
),
ranked_platforms AS (
  SELECT
    Year, -- pull out by year, plarform and total sales, then rank them
    Platform,
    total_sales,
    RANK() OVER (PARTITION BY Year ORDER BY total_sales DESC) AS rnk
  FROM platform_totals
)
SELECT
  g.Year,
  g.Genre AS Top_Genre,
  ROUND(g.total_sales, 2) AS Genre_Sales,
  p.Platform AS Top_Platform,
  ROUND(p.total_sales, 2) AS Platform_Sales
FROM ranked_genres g
JOIN ranked_platforms p USING (Year)
WHERE g.rnk = 1 AND p.rnk = 1
ORDER BY g.Year;
```
Findings:
```findings
sort by: column Year, top genre, genre sales, top platform, platform sales: 

1980,	Shooter,	7.07,	2600,	11.38
1981,	Action,	14.84,	2600,	35.77
1982,	Puzzle,	10.03,	2600,	28.86
```
Which platforms are most successful for specific genres?

SQL:
```sql
WITH genre_platform_totals AS ( -- name temp table as genre platform total
  SELECT
    Genre, -- pull genre, platform and total up the global sales 
    Platform,
    SUM(Global_Sales) AS Total_Sales
  FROM vgsales
  GROUP BY Genre, Platform
),
ranked AS ( -- name temp table as rank
  SELECT
    Genre,
    Platform,
    Total_Sales,
    RANK() OVER (PARTITION BY Genre ORDER BY Total_Sales DESC) AS rnk
  FROM genre_platform_totals
)
SELECT
  Genre,
  Platform AS Top_Platform,
  ROUND(Total_Sales, 2) AS Total_Global_Sales
FROM ranked
WHERE rnk = 1
ORDER BY Total_Global_Sales DESC;
```
Findings:
```findings
Action  - PS3 , 307.88
Sports - wii, 292.06
Shooter - X360, 278.55
```
## Deliverables:
- SQL Queries: Provide all the SQL queries you used to answer the business questions.
- Summary of Findings: For each question, summarise your key findings and recommendations based on your analysis.

## Submission

- Submit the GitHub URL of your assignment to NTU black board.
- Should you reference the work of your classmate(s) or online resources, give them credit by adding either the name of your classmate or URL.
