<!DOCTYPE html>
  <h1>SQL Learning Journal #2</h1>
  <p><em>Reflecting on what I learned using SQL for the Chinook database</em></p>
  <hr>

  <h2>Section 1: What I Learned</h2>
  <p>I worked through a lot of SQL basics and some advanced parts too. I practiced SELECT to pull specific columns, WHERE to filter rows, ORDER BY to sort results, and aggregate functions like COUNT, SUM, and AVG to summarize data. I also learned how to connect tables with JOIN, and how subqueries and CTEs (WITH clauses) make queries more powerful and easier to read.</p>
  <p>I explored SQL fundamentals and some advanced features. I used SELECT to choose columns, WHERE to filter, ORDER BY to sort, and aggregates like COUNT, SUM, and AVG to summarize. I learned how JOIN connects tables, while subqueries add conditions, and CTEs (WITH clauses) simplify complex queries by breaking them into clear steps.</p>
  <p>One thing that finally made sense to me is the difference between WHERE and HAVING. WHERE filters rows before grouping, while HAVING filters groups after aggregation. Another useful distinction is between JOIN and a subquery. An INNER JOIN combines related rows across tables.</p>
  <p>CTEs were also a big help. Instead of stacking messy subqueries, I can compute something like total revenue per customer in a CTE, then reference it in the main query. It feels cleaner and easier to follow.</p>

  <hr>
  <h2>Section 2: A Query I Am Proud Of</h2>
  <pre><code>WITH CustomerRevenue AS (
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
WHERE Revenue &gt; (SELECT AVG(Revenue) FROM CustomerRevenue)
ORDER BY Revenue DESC;
</code></pre>
  <p><strong>Explanation:</strong> This query builds a temporary table of customer revenues, then selects only those above the average and sorts them by highest spend. It answers “Who are the top customers spending more than average?”. It because it neatly combines joins, aggregation, a CTE, and a subquery for clear insights.</p>

  <hr>
  <h2>Section 3: A Mistake or Struggle</h2>
  <pre><code>SELECT 
    c.FirstName,
    c.LastName,
    SUM(il.UnitPrice * il.Quantity) AS Revenue
FROM 
    customers c
JOIN 
    invoices i ON c.CustomerId = i.CustomerId
JOIN 
    invoice_items il ON i.InvoiceId = il.InvoiceId;
</code></pre>
  <p><strong>Error:</strong> I forgot to group by non‑aggregated columns, which caused either a SQL error or one wrong total row.</p>
  <p><strong>Fix:</strong> I added GROUP BY CustomerId, FirstName, LastName to produce correct totals per customer.</p>
  <p><strong>Lesson:</strong> Always check which columns need grouping, test with small queries, and group by stable keys like CustomerId to avoid mistakes.</p>

  <hr>
  <h2>Section 4: Connecting to the Real World</h2>
  <h3>E‑commerce app</h3>
  <p><strong>Question 1 — Which products made the most revenue last quarter?</strong></p>
  <ul>
    <li>Tables: orders, order_items, products</li>
    <li>Query type: JOIN + GROUP BY aggregation (SUM(order_items.price * quantity)) + ORDER BY DESC</li>
    <li>Why: ranks products by revenue so we know what to stock, promote, or discount.</li>
  </ul>
  <p><strong>Question 2 — Which customers are likely to churn this month?</strong></p>
  <ul>
    <li>Tables: customers, orders, sessions (or subscriptions)</li>
    <li>Query type: CTE/subquery to get last_order_date and order_count, then filter with LEFT JOIN/WHERE for inactivity</li>
    <li>Why: flags customers with declining activity so we can target win‑back campaigns.</li>
  </ul>
  <h3>Why SQL still matters</h3>
  <p>SQL is simple and readable because you can state what you want without detailing how to get it, fast and reliable since databases optimize queries and keep transactions safe, and versatile as it works seamlessly with BI tools, ETL, and dashboards.</p>

  <hr>
  <h2>Section 5: Self-Assessment</h2>
  <p><strong>Confidence Table (Scale: 1 = lost, 3 = need notes/help, 5 = could teach it)</strong></p>
  <table border="1" cellpadding="5" cellspacing="0">
    <tr>
      <th>Topic</th><th>1</th><th>2</th><th>3</th><th>4</th><th>5</th>
    </tr>
    <tr><td>Basic SELECT, WHERE, ORDER BY</td><td></td><td></td><td></td><td></td><td>X</td></tr>
    <tr><td>Aggregation (COUNT, SUM, AVG, GROUP BY, HAVING)</td><td></td><td></td><td></td><td>X</td><td></td></tr>
    <tr><td>Joins (2 tables)</td><td></td><td></td><td></td><td></td><td>X</td></tr>
    <tr><td>Joins (3 or more tables)</td><td></td><td></td><td>X</td><td></td><td></td></tr>
    <tr><td>Subqueries</td><td></td><td></td><td>X</td><td></td><td></td></tr>
    <tr><td>CTEs (WITH) and window functions</td><td></td><td></td><td>X</td><td></td><td></td></tr>
    <tr><td>CREATE, INSERT, UPDATE, DELETE</td><td>X</td><td></td><td></td><td></td><td></td></tr>
  </table>
  <p><strong>Reflection:</strong> My strongest skills are basic SELECT/WHERE/ORDER BY and two‑table joins, which I can write quickly from practice. Aggregation is also a strength since I’ve used SUM and AVG correctly and understood WHERE vs. HAVING. My two weakest areas are CTEs and subqueries — I still need more practice applying them effectively. To improve, I’ll continue testing queries step by step and focus on examples that use stable keys and clear partitions.</p>

  <hr>
  <h2>Section 6: Goals and Next Steps</h2>
  <ul>
    <li><strong>Skill to strengthen:</strong> CTE — I’ll practice by rewriting queries.</li>
    <li><strong>New topic:</strong> Indexes — I want to learn how they speed up queries.</li>
    <li><strong>Goal:</strong> By next week, I’ll rewrite three join queries as CTEs and check them with EXPLAIN.</li>
  </ul>
</body>
</html>
