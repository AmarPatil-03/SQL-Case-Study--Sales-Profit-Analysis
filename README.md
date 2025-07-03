# 📈 SQL Case Study: Sales & Profit Dashboard

📘 Overview
This project showcases a SQL-based case study focused on analyzing sales, marketing, and profit data across multiple U.S. states. It demonstrates how structured queries can uncover business insights from raw transactional and product data.


# 🎯 Objective
The primary goal is to:

- Evaluate product performance

- Measure marketing effectiveness

- Track sales and profit by region

- Use SQL techniques for real-world data problems



# 🧾 Dataset Description
- The project uses three relational tables:

Table Name	Description
fact	Contains 4,200+ sales records with metrics like sales, profit, COGS, and marketing
product	Holds product details including name, type, and category
location	Includes geographic info such as state, market, and area code



# 🛠️ Tools & Skills Used
SQL (MySQL)

- Joins, Subqueries, Aggregations

- Window Functions (DENSE_RANK)

- Stored Procedures & Functions

- Case Statements

- Data Cleanup & Transformation

  

# 🔍 Key Insights & Queries
✅ Total number of states in the dataset

✅ Count of “Regular” type products

✅ Total marketing spend by product ID

✅ Minimum & maximum values for sales and COGS

✅ State-wise profit and sales breakdown

✅ Sales ranking without gaps using DENSE_RANK

✅ Simulated 5% sales growth projections

✅ Stored procedures to filter product types dynamically



# 🔬 Advanced Queries & Analytics
This section includes complex SQL queries that go beyond basic reporting and help uncover deeper insights from the data.

📊 1. Forecasting Sales with 5% Growth
sql
Copy
Edit
SELECT ProductId, Sales, Sales * 1.05 AS IncreasedSales
FROM fact;
Insight: Projects future sales performance by simulating a 5% growth scenario.

🏅 2. Sales Ranking Without Gaps
sql
Copy
Edit
SELECT Sales, DENSE_RANK() OVER (ORDER BY Sales DESC) AS SalesRank
FROM fact;
Insight: Assigns sales ranks to each transaction, using DENSE_RANK to avoid gaps in the ranking.

🗺️ 3. State-Wise Sales and Profit
sql
Copy
Edit
SELECT l.state, SUM(f.sales) AS TotalSales, SUM(f.profit) AS TotalProfit
FROM fact f
JOIN location l ON f.`Area Code` = l.`Area Code`
GROUP BY l.state;
Insight: Aggregates financial performance metrics by U.S. state.

🛍️ 4. Top Profitable Product
sql
Copy
Edit
SELECT f.ProductId, p.`Product Type`, f.Profit
FROM fact f
JOIN product p ON f.ProductId = p.ProductId
WHERE f.Profit = (SELECT MAX(Profit) FROM fact);
Insight: Identifies the single most profitable product in the dataset.



# ⚙️ Stored Procedures & Functions
Reusable SQL logic was created to streamline query execution and improve flexibility.

🔧 Stored Procedure: Get Products by Type
sql
Copy
Edit
DELIMITER //
CREATE PROCEDURE GetProductsByType(IN productType VARCHAR(50))
BEGIN
    SELECT * FROM product WHERE `Product Type` = productType;
END //
DELIMITER ;

CALL GetProductsByType('Beverage');
Use Case: Dynamically fetch product records based on a specific product type input by the user.


# 🧠 User-Defined Function: List Products by Type
sql
Copy
Edit
DELIMITER //
CREATE FUNCTION GetProductByType(pType VARCHAR(50)) RETURNS VARCHAR(255)
DETERMINISTIC
BEGIN
    DECLARE result VARCHAR(255);
    SELECT GROUP_CONCAT(Product) INTO result FROM product WHERE `Product Type` = pType;
    RETURN result;
END //
DELIMITER ;

SELECT GetProductByType('Beverage');
