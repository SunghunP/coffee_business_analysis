# Half of This Coffee Shop's Customers Only Buy One Thing. Here's How I'd Fix That.

![Dashboard of four charts: net sales by day, net sales by hour, total revenue by item, and percent of revenue by category](coffee%20one/coffeeshopdash.png)

*Dashboard built from the PivotTables.*

I analyzed four weeks of sales from a coffee shop to practice pivot tables and SQL. The data is a practice dataset modeled on a Square POS export (2,418 transactions, $19,936 in sales), not a real business.

## Result

Mornings and weekdays drive the business. Sales peak at 8 AM, Wednesday is the busiest day, and coffee brings in 47% of revenue. The SQL queries matched the pivots.

![PivotTable of percent of net sales by category](coffee%20one/c3_categories.png)

## Business Findings

Half of all orders are a single item, averaging $4.70 versus $11.80 for bigger orders.

7–9 AM is about 46% of sales, and after 3 PM is only about 11%.

![PivotTable of net sales by hour](coffee%20one/c4_hour.png)

A weekday brings in about 1.7 times the sales of a weekend day.

![PivotTable of net sales by day of the week](coffee%20one/c4_day.png)

The Latte is the top seller at $2,924.

For Here and To Go have about the same average ticket, but To Go orders drop on weekends.

![PivotTable of sales, transactions, and average ticket by dining option](coffee%20one/c5_dining.png)

## Recommendations

- **Slow hours:** offer a drink-and-pastry bundle from 3 PM to close and suggest it to single-item customers. If 10% took it, sales would rise roughly 3%.

## Step 1: Objective

I focused on three questions:

- Which items and categories earned the most?
- What were the busiest and slowest days and hours?
- How do For Here and To Go orders compare?

## Step 2: Pivot Tables

Each row in the data is one item, not one order, so I added a helper column to count real transactions. The first pivot gives an average ticket of $8.24 and 1.74 items per order.

![PivotTable with total net sales, quantity, and transactions, plus average ticket and items per transaction](coffee%20one/c2.png)

## Step 3: Rebuilding the Charts in SQL

Each dashboard chart maps to one short query. I added two computed columns, `DayName` and `Hour`, to a copy of the table.

For net sales by day, sorting on the name gives alphabetical order, so I sorted on the weekday number. SQL Server counts Sunday as day 1, so I set `DATEFIRST` to 1 to put Monday first.

```sql
SET DATEFIRST 1;

SELECT
	s.[DayName],
	SUM(s.Net_Sales) AS Total_Sales
FROM dbo.sales_clean s
GROUP BY s.[DayName]
ORDER BY MAX(DATEPART(WEEKDAY, [Date]));
```

Net sales by hour is the same pattern, ordered by the hour.

```sql
SELECT
	s.[Hour],
	SUM(s.Net_Sales) AS Total_Revenue
FROM dbo.sales_clean s
GROUP BY s.[Hour]
ORDER BY s.[Hour];
```

Revenue by item sorts the summed sales from high to low.

```sql
SELECT
	Item,
	SUM(Net_Sales) AS Total_Revenue
FROM dbo.sales_clean
GROUP BY Item
ORDER BY SUM(Net_Sales) DESC;
```

For the category share, a window function gives the grand total without a second query. The empty `OVER ()` treats all the categories as one window.

```sql
SELECT
	Category,
	SUM(Net_Sales) AS Sales,
	SUM(Net_Sales) / SUM(SUM(Net_Sales)) OVER () * 100 AS Percentage
FROM dbo.sales_clean
GROUP BY Category
ORDER BY Sales DESC;
```

*Files and notes are on my GitHub.*
