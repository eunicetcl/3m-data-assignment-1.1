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

```
Findings:
```findings

```
What are the trends over time in game sales by genre and platform?

SQL:
```sql

```
Findings:
```findings

```
Which platforms are most successful for specific genres?

SQL:
```sql

```
Findings:
```findings

```
## Deliverables:
- SQL Queries: Provide all the SQL queries you used to answer the business questions.
- Summary of Findings: For each question, summarise your key findings and recommendations based on your analysis.

## Submission

- Submit the GitHub URL of your assignment to NTU black board.
- Should you reference the work of your classmate(s) or online resources, give them credit by adding either the name of your classmate or URL.
