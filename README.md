# CAPSTONE-PROJECT_SQL
A SQL project on a fictional online coffee store called Monday Coffee that wants to expand into physical stores but needs to make data-driven decisions based on some data.

Monday Coffee
Business Expansion Analysis


                                                  PROBLEM STATEMENT
Monday Coffee wants to expand from an online shop into a real, physical café locations. However, opening physical stores is expensive and introduces new fixed costs like monthly rent, which varies by city. Expanding without a plan risks opening stores in the wrong cities and losing money. This project uses SQL data analysis to find out the safest, most profitable cities to launch our first physical locations.
OBJECTIVES OF THE PROJECT
The core objectives of this project is to transition from an e-commerce model to a successful brick-and-mortar retail presence. By leveraging SQL data analysis, the business will minimize expansion risks and optimize store performance through three main objectives:
•	Identify top cities by ranking geographic locations through total revenue and order volume to select the best cities for physical stores.
•	Locate areas with the highest concentration of active, unique customers to guarantee immediate local demand.
•	To identify top selling coffee blends and products by region to optimize inventory for each new storefront.

                                                 DATABASE OVERVIEW
The database consists of a relational schema named retail containing four interconnected tables that track historical online coffee sales, customer profiles, product details, and city-level market data.
TABLE BREAKDOWNS
1.	Retail.sales(Transaction log): This is the core fact table. It records the individual details of every customer purchase.
•	Sales_id (integer/primary key): The unique invoice number for each transaction.
•	Sale_date (integer/foreign key): The exact calendar day the coffee order was placed.
•	Customer_id(integer/foreign key): Identifies which customer placed the order.
•	Product_id (integer/foreign key) : Identifies the exact coffee product purchased.
•	Total (integer): The total checkout amount paid by the customer.
2.	Retail.customers (Customer profiles): This table stores biographical information about Monday Coffee’s digital user base.
•	Customer_id (integer/primary key): The unique identification number of each client.
•	Customer_name (text/VARCHAR): The full name of the registered customer.
•	City_id (integer/foreign key): Connects the customer to their primary residential city.
3.	Retail.products (Menu Catalog): This table acts as master product index, containing the baseline menu configuration.
•	Product_id (intger/primary key): The unique identification number for each beverage or item.
•	Product_name (text/VARCHAR): The commercial name of the item.
•	Price(integer): The standard baseline catalog for the item.
4.	Retail.city (Market dynamics): This table provides the essential geographic context and overhead cost data needed for expansion modeling.
•	City_id (integer/primary key): The unique identification number for each city market.
•	City_name (text/VARCHAR): The name of the city.
•	Estimated_rent (integer): The fixed monthly overhead cost of leasing a physical storefront in that city.
•	Population (integer): The total population count used to estimate the available coffee drinking market size.

METHODOLOGY (Questions & SQL Answers)
This section outlines the step-by-step approach used to process, manage, and analyze the dataset to achieve the project’s objectives:
•	Creation of schemas for the four tables containing the data. The schema is named ‘retail’.
•	Creation of the four tables called retail.products, retail.sales, retail.customers, retail.city.
The following are the approach to which every question was answered:
Question 1: Coffee Consumer Estimate 
Assuming 25% of each city's population drinks coffee, calculate the estimated number of coffee consumers (in millions) per city. Order results from highest to lowest.
•	Answer: With this question, we are assuming that one quarter of the city’s population drinks coffee. So from my query, I selected the city_name column and rounded the population of each city to two decimal place, multiplied it by 0.25 which is 25% and divided it by 1,000,000 to get the results in millions and finally I used the ORDER BY clause to order the results from the city with the highest coffee consuming to the lowest assuming each city consumes coffee by 25% and I gave it alias.
QUESTION 2: What is the total revenue generated from coffee sales across all cities during the last quarter of 2023 (October-December)? Show results per city, ordered by revenue descending.
•	Answer: From this query, I used an inner join to connect the tables with common columns to be able to derive the total revenue of customers from each city. The I used WHERE clause to filter the revenue made during the last quarter of 2023, I used GROUP BY to split the total sum calculations by city rather combining everything into a single total.
QUESTION 3:Question 3: Sales Volume by Product 
How many units of each coffee product have been sold in total? Rank products from best-selling to least-selling.
•	From my query, I used an inner join to connect the sales and product tables with the product_id which is a common column between the two tables. I used COUNT to count each individual line item in the sales table. GROUP BY is used to give us the individual distinct coffee option. ORDER BY is used to order the total_units_sold from the highest units sold to the lowest units of coffee sold.
Question 4: Average Sales per Customer by City What is the average total sales amount per unique customer in each city? Include total revenue and customer count alongside the average. Order by total revenue descending.
•	From this query, I used a JOIN here because I need info from other tables to be able to get my results. I used DISTINCT here to ensure that a customer purchases are  only counted once as a unique person. I also used type casting in this query to change my data type from integer to unique because my total columns store whole numbers so dividing an integer by another integer can cause the database to cut off decimals which might lead to some inaccuracies. In my JOIN I used aliases and GROUP BY city_name because the question asked to find each unique customer in each city and we ORDERED BY total_revenue DESC to sort from the highest revenue to the lowest so we get to see which city has the highest total revenue and which has the lowest.
Question 5: Current Customers vs. Estimated Coffee Consumers 
For each city, show both the estimated coffee-drinking population (25% of city population, in millions) and the actual number of unique customers from the sales data. Use a CTE.
•	From this query, I defined my CTE as customer_count which runs behind the scenes to create a temporary summary list of how many unique customer Monday Coffee has in each city_id. I also used the formula by taking 25% of the population and dividing it by 1,000,000 to display the potential market size in millions. I used COALESCE, which is a data-cleaning function to return zero instead of null in case Monday Coffee online sales in specific city shows a NULL. Lastly, I used a LEFT JOIN to join the customer_count and the city_id to ensure that every single city in the expansion shows up regardless of whether they have an actual online customer(s) yet.
Question 6: Top 3 Products per City
What are the top 3 best-selling coffee products in each city, based on number of orders? Use a window function to rank products within each city.
•	As we have used in previous queries, we use FROM and JOIN to get access to the other tables since we need to connect the tables to be able to answer the question. We use JOIN statements to link the retail.sales to the retail.customer table, the retail.customer table to their respective cities and sales for the specific products they bought. I used GROUP BY to collapse the number of sales or orders into distinct rows for each unique product sold in a specific city. I also used DENSE_RANK() to rank the products in such a way that id two products in a city have the same orders, they both get Rank 1 and the next highest gets ranked 2. I used PARTITION BY to isolate each city into a group and ORDER BY DESC to sort the products from the highest to the lowest. And finally, I used WHERE to filter the 3 best-selling coffee products for every city group.
Question 7: Unique Customers per City
How many unique customers in each city have made at least one coffee purchase? Order by customer count descending.
•	From this query, I was asked to number of unique coffee buying customers in each city with this, I have to find the count distinct customer IDs and group them by each city. I used COUNT(DISTINCT s.customer_id) to ensure that every customer who bought coffee 10 times are counted once as a unique customer for each city. Also, I used JOIN to connect the tables. I used GROUP BY c.city_name to separate the aggregate customer count for each individual city. Finally, I used ORDER BY... DESC to place or filter the cities with the highest customer base at the top to the lowest
Question 8: Average Sale vs. Average Rent per Customer
For each city, compare the average sale amount per customer against the average rent cost per customer (estimated_rent divided by number of customers). This helps evaluate cost efficiency.
•	This question asked to compare the average sale amount per customer and the average rent cost per customer. So I had to calculate the average sale revenue per customer and the average cost of rent per customer. I divided (SUM(s.total)) which is the total of all revenue generated from sales by the total count of unique customers in each city (COUNT(DISTINCT s.customer_id)), and also, to get theavg_rent_per_customer. I had to divide the fixed cost of rent for a store in that city by the total count of unique customers in each city. I had to change the data type by using type casting ::numeric to avoid making inaccurate calculations because by dividing two integers, if there is a remainder it can be thrown away which will lead to inaccuracies.
Question 9: Month-on-Month Sales Growth 
Calculate the month-on-month percentage change in total sales for each city. Use a window function (LAG) to compare each month's sales to the previous month. Show only rows where a prior month exists.
•	From my query, I followed a step-by-step structured pipeline using CTE. I first created theMonthlySales CTE where used DATE_TRUNC to chop each day or time off every transaction and makes it into the first day of the month for all the months. I also used SUM(s.total) to add up all the individual transactions in the each month and I aliased it as current_month_sales, and I then used GROUP BY to group these monthly sales and their cities respectively. I also created the LaggedSales CTE, this CTE is for displaying the change in sale by using the LAG()window function to create the previous monthly sales from the current monthly sales which helps us seethe change or increment or reduction in sales per month. I used PARTITION BY city_name to ensure that when the database is looking at Ahmedabad, it only grabs previous sales numbers from it. When it finishes it resets to another city. I also used ORDER BY sales_month to ensure that the LAG() function is sorted chronologically and actually grabs previous month it is supposed to and not a random one. I changed the data type using type casting to numeric just because in dividing both integers, the decimal places might get lost or thrown away which would not reflect the true picture I am trying to show so I had to change it for nothing to get lost. Finally, for WHERE previous_month_sales IS NOT NULL, I used this because the first month January has no records in any city and there is no prior history and because of that the LAG() function would return a blank NULL value so this will drop the blank NULL value so that I would have a clean timeline.
Question 10: Market Potential Summary
Produce a full market potential table for each city, showing: total revenue, estimated rent, total customers, estimated coffee consumers (millions), average sale per customer, and average rent per customer. Order by total revenue descending.
•	This question aske for the full market potential table for decision-making as to the most viable city markets to open physical brick and mortar stores. From the question, I was asked for some growth metrics like the total revenue which I obtained by summing up the total in the sales table and we are grouping everything by the cities. The estimated rent was obtained by dividing the rent estimates for each city by the unique customer count. I did same with the average sale per customer. I divided the total sales for each city by the unique customer count. I obtained the estimated coffee consumers by the population of each city by 25% which is a standard estimation for most coffee drinkers in Asia like India and China which are countries that consume more tea than coffee. I used JOIN and FROM to connect the tables and I GROUP BY city_name, population and estimated rent to collapse the rows by uniquely grouping these rows into buckets and I ORDER BY total revenue DESC to arrange the completed table in such a way the Monday Coffee biggest and most profitable markets are displayed at the very top.
BONUS TASK: DESIGN YOUR OWN QUESTIONS/* QUESTION 1:Geographic Profitability Analysis 
As a an analyst I want to find out how the cities/ location affect product sourcing and pricing and impacts Monday Coffee gross profit margin.
•	From the results, the profit margin column shows all zeros. The variance in the avg retail price across cities like Indore(599.96) and Kolkata(599.51) have the highest average transaction values. And Lucknow(552.53) and Nagpur(553.56) have the lowest. Because the pricing is completely flat(s.total = p.price), this variance proves that customers in Indore and Kolkata are naturally ordering the highest priced menu items. Customers in Lucknow and Nagpur prefer cheaper, basic menu items. Monday Coffee should prioritize building physical stores in Indore and Kolkata because their customer bases are already trained to by premium items.
QUESTION 2: How many average transactions must a physical store in each city execute per month to cover for its fixed rent costs.
•	From this query, I am looking to find out the average transactions a physical store must have in each city per month to cover the fixed rent costs. I used AVG(s.total) to aggregate all the historical sales to calculate the average order value for each city. This tells us what a standard order ticket brings in.c.estimated_rent pulls the fixed monthly rent for each city from the retail.city table. To get the monthly orders to break even I divided the estimated_rent by the average order value. This in Lucknow if rent is 9000 rupees and avg_order value is 552 rupees the store must sell 17 orders monthly to pay the rent. The CEIL function is a mathematical logic function which means ceiling. It rounds any decimal up to the nearest whole integer. To obtain the daily orders to break even I divided the monthly target by 30. Then I used the ORDER BY function to sort the results from the lowest daily transactions to the highest so also from daily orders to break even we need to sell one cup of coffee daily to be able to pay a rent of 9000 rupees in the city of Lucknow.
QUESTION 3: Which cities have the highest rate of repeat buyers, and where does Monday Coffee possess the most loyal customer base to anchor a physical store.
•	From this question, we are using a FROM subquery to count how many orders each individual customer has made. It then aggregates that data by city to find the percentage of Loyal Repeat Customers versus One time shoppers. For the inner query I grouped the data by both city and the customer ID. It counts exactly how many times each single human being checked out. For the outer query I used CASE WHEN statement to assign 1 to repeat customers and 0 to one time customers. I added the summation metric to add up all the 1s for repeat buyers and 0 for one time buyers. I also divided the number of repeat customers by the total number of unique customers in each city to obtain the customer retention rate pct which is a good metric for measuring customer loyalty. I sorted the customer retention using ORDER BY DESC so that the city with the most loyal customers will be at the very top. But from my results all the customer retention rate pct. are the same for all cities, so since Monday Coffee has a perfect 100% loyalty score the percentage column is no longer tell us which city is the best. Instead we shift to total active customer’s column from there we see that in Jaipur we have 69 customers and in Delhi we have 68 customers these two cities possess massive clusters of 100% loyal customers. In Hyderabad and Indore which have 21 and 21 customers respectively do not possess massive clusters of 100% loyal customers.

RESULTS
The following are the results that was obtained after analyzing the data. Here are some screenshots of the data output using SQL:

SQL CONCEPTS USED
The following are the SQL concepts that was used:
1.	Basic Aggregation
•	SUM () & COUNT (): To calculate total revenue and total orders for each city.
•	COUNT (DISTINCT): To find the exact number of unique active customers per region.
•	AVG (): To determine the Average Order Value(AOV) for pricing strategies.
2.	Data Filtering & Grouping
•	WHERE: To filter out canceled or returned orders before calculating metrics.
•	GROUP BY: To aggregate sales, customer, and product data by specific cities.
3.	Combining Tables & Handling Missing Data
•	INNER JOIN: To link the sales, customers, products, and city tables together using primary and foreign keys.
•	LEFT JOIN:  Used to merge tables while preserving all records from the primary table, ensuring that customers and cities with zero sales were not accidentally deleted from the analysis.
•	COALESCE: Used to handle NULL values by replacing empty fields with a default value.
4.	Advanced Ranking & Sorting
•	ORDER BY: To sort the cities from the highest to the lowest revenue and pull up the top 3 locations. 
•	DENSE_RANK () (Window Function): To rank the most popular coffee blends within each individual city.
•	PARTITION BY: Used to calculate metrics separately for each individual city without collapsing rows.
•	LAG (): Used to look back at previous rows to analyze month-over-month sales growth.
5.	Conditional Logic
•	CASE WHEN: Used to create conditional business logic, such as categorizing customers into loyal customers based on purchased history.
6.	Data Structuring & Readability
•	CTE: Used to break complex, multi-step queries into organized, readable blocks.
•	SUBQUERY: Used to run nested calculations.
KEY INSIGHTS & RECOMMENDATION
Based on the collective insights from all the SQL analysis, here are the data-driven expansion strategy and the three specific
cities recommended for Monday Coffee's first physical brick-and-mortar stores.

1.	PUNE: Pune has high volume growth market. It boasts a massive established online presence with 2,135 total historical orders.
Despite this large volume, its estimated retail rent sits at an incredibly assessible 12,150. Its Average Order Value is a healthy 
551.44.
JUSTIFICATION: According to the Rent Breakeven query, Pune requires a mere 22 monthly orders or exactly one order per day to cover its
rent. Because Pune has a massive, highly active digital customer base, it represents an extremely safe market where consumer demand will instantly
overwhelm the low fixed rent barriers.

2.  JAIPUR: Jaipur holds the absolute highest concentration of active core regulars in the database, with 69 total active customers (who have 100% repeat-buyer retention rate).
It has generated a robust 1,377 historical orders.
JUSTIFICATION: Like Pune, Jaipur's low real estate overhead ensures it only needs 1 order per day to break even on rent. Physical coffee shops thrive on 'regulars' who build cafe 
visits into their morning habits. Jaipur gives Monday Coffee the largest pre-screened foundation of daily loyalists, eliminating expensive local market costs.

3.  INDORE: Indore possesses the absolute highest Average Order Value (is a core business metric that tracks the average amount of money a customer spends every time they place an order)
in the entire database at 599.96 per transaction. It also maintains a perfect 100% retention rate among 21 core customers.
JUSTIFICATION: While Indore has fewer overall active unique customers than Jaipur or Delhi, the customers it does have spent the most money per order. They naturally select premium, high-
margin items from the menu. Coupled with low estimated rent that requires only 1 order per day to clear, Indore yields the most profitable unit economics per transaction.

Additional information, while tier 1 metropolitan cities like Bangalore might seem attractive due to their size, the data warns against launching there because:
Bangalore's estimated rent spikes to 29,700, which instantly doubles the operational risk by requiring a higher volume of 2 orders per day just to break even on real estate.
Also, to maximize success, Monday Coffee should use low-risk markets (Pune and Jaipur) to perfect their physical store operations before taking on the expensive real estate risk
of major metro cities.

RECOMMENDATIONS
From the database, the flat history query proved that Monday Coffee currently charges a rigid, universal menu price across all cities. While an online app can sustain this, a physical store cannot.
Leadership must implement localized pricing markups (such as 10%-15% physical store premium) in high-rent districts to safeguard profit margins.
Lastly, the month-on-month sales growth query exposed dramatic swings in consumer demand, with sales surging by 90% during autumn/holiday months and 
dropping sharply in winter. Physical stores must be designed with flexible staffing and high seasonal seating capacity to maximize cash flow during these peak revenue seasons.
LIMITATIONS
Here are some of the limitations of the project:
•	Online purchasing habits do not perfectly predict foot traffic. Customers who buy coffee beans online might not regularly visit a physical café.
•	The analysis relies strictly on sales revenue. It does not account for physical store overhead like local employee wages, utilizes, or supply chain logistics.
•	The current analysis uses a snapshot of historical data and does not automatically account for real-time market changes, economic inflation, or sudden shifts in consumer trends.

FUTURE WORKS
The following are some future works of the project:
•	Combine the SQL database with GIS (Geospatial Information Systems) data to analyze foot traffic patterns, pedestrian traffic, and competitor proximity near potential storefronts.
•	Connect SQL queries to a Business Intelligence tool like Tableau or Power BI to create real-time, interactive dashboards for stakeholders to monitor ongoing regional sales trend.
•	Expand the database schema to include operating expenses(OpEx) and capital expenditures (CapEx) to calculate the true Net Profit Margin and accurate Return on Investment (ROI) timelines for each physical store.











