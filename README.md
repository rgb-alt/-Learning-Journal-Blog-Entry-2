SQL Learning Journal #2

Reflecting on what I learned using SQL for the Chinook database

------------------------------------------------------------

Section 1: What I Learned
I practiced SQL basics like SELECT, WHERE, and ORDER BY, plus aggregates (COUNT, SUM, AVG) to summarize data. I learned how JOIN connects tables, how subqueries add conditions, and how CTEs (WITH) simplify complex queries.  
Key insight: WHERE filters rows before grouping, while HAVING filters groups after aggregation.  
CTEs helped me compute totals (like revenue per customer) in a cleaner, more readable way.

------------------------------------------------------------

Section 2: A Query I Am Proud Of
WITH CustomerRevenue AS (
  SELECT
    c.CustomerId,
    c.FirstName,
    c.LastName,
    SUM(il.UnitPrice * il.Quantity) AS Revenue
  FROM customers c
  JOIN invoices i ON c.CustomerId = i.CustomerId
  JOIN invoice_items il ON i.InvoiceId = il.InvoiceId
  GROUP BY c.CustomerId, c.FirstName, c.LastName
)
SELECT FirstName, LastName, Revenue
FROM CustomerRevenue
WHERE Revenue > (SELECT AVG(Revenue) FROM CustomerRevenue)
ORDER BY Revenue DESC;

Explanation:
This query builds a temporary table of customer revenues, then selects only those above the average and sorts them by highest spend. It answers “Who are the top customers spending more than average?” and highlights VIPs for loyalty or special offers.

------------------------------------------------------------

Section 3: A Mistake or Struggle
Error: Forgot to group by non‑aggregated columns, causing a SQL error or wrong totals.  
Fix: Added GROUP BY CustomerId, FirstName, LastName to produce correct totals.  
Lesson: Always group by non‑aggregated columns, test queries step by step, and use stable keys like CustomerId.

------------------------------------------------------------

Section 4: Connecting to the Real World
E‑commerce app examples:
- Top products by revenue: Use JOIN + GROUP BY + SUM to rank products for stocking or promotions.  
- Customer churn: Use a CTE/subquery to find last order date and inactivity, then flag customers for win‑back campaigns.

Why SQL matters:
SQL is simple and readable, fast and reliable since databases optimize queries, and versatile because it works with BI tools, ETL, and dashboards.

------------------------------------------------------------

Section 5: Self‑Assessment
Confidence Table (Scale: 1 = lost, 3 = need notes/help, 5 = could teach it)

Topic                                1 2 3 4 5
-----------------------------------------------
Basic SELECT, WHERE, ORDER BY                  X
Aggregation (COUNT, SUM, AVG, GROUP BY, HAVING)        X
Joins (2 tables)                                         X
Joins (3 or more tables)                                 X
Subqueries                                     X
CTEs (WITH) and window functions                   X
CREATE, INSERT, UPDATE, DELETE             X

Reflection:
My strongest skills are basic SELECT/WHERE/ORDER BY and two‑table joins, which I can write quickly from practice. Aggregation is also a strength since I’ve used SUM and AVG correctly and understood WHERE vs. HAVING. My two weakest areas are CTEs and subqueries — I need more practice applying them effectively. To improve, I’ll continue testing queries step by step and focus on examples that use stable keys and clear partitions.

------------------------------------------------------------

Section 6: Goals and Next Steps
- Skill to strengthen: CTE — I’ll practice by rewriting queries.  
- New topic: Indexes — I want to learn how they speed up queries.  
- Goal: By next week, I’ll rewrite three join queries as CTEs and check them with EXPLAIN.
