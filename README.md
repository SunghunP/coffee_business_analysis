# Coffee Shop Sales Analysis

This project analyzes four weeks of point-of-sale (POS) data from a coffee shop to understand what drives revenue and customer traffic patterns using Excel and SQL. The data is a practice dataset modeled on a Square POS export (about 3,600 rows, 2,418 transactions, $19,936 in sales), not a real business.

I built the analysis in Excel PivotTables first, then rebuilt the four dashboard charts in T-SQL to check the numbers. The analysis focuses on aggregations, window functions, and data visualization to turn raw sales data into business recommendations.

![Dashboard](coffee%20one/coffeeshopdash.png)

## Key Findings

- Latte was the top-selling item ($2,924)
- Coffee brought in 47% of revenue
- Wednesday was the busiest day, and a weekday brought in about 1.7 times the sales of a weekend day
- 8 AM was the peak sales hour, with 7–9 AM making up about 46% of sales
- After 3 PM was the slowest period, at about 11% of sales
- Half of all orders were a single item, averaging $4.70 versus $11.80 for larger orders

## Recommendations

- Offer a drink-and-pastry bundle from 3 PM to close and suggest it to single-item customers. If 10% took it, sales would rise roughly 3%

## Tools & Skills

Tools: SQL (T-SQL), Excel (PivotTables & Charts)

Skills: Data aggregation, window functions, revenue analysis, business analysis, data visualization

## Files

- `coffee_shop_sales.xlsx`: the source data and PivotTables
- `Article.md`: the write-up
