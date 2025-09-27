 plsql-windows-function-UMUBYEYI-GAHAMANYI-Madeleine
PL/SQL window function assignment auca 26/09/2025 
## Automotive Retail business
## STEP1:PROBLEM DEFINITION
### Business context
 ### company type:
 small company that sells and buys car.
### department:
sales and procurement. 
### industry:
Automotive retail(business of buying and selling cars and related services to end customers).
### Data Challenge 
in the business of buying and selling cars right now does not clearly to know which cars sells faster and give me more profit.decisions are often made based only on guesswork,this is cause of some cars to stay long time in the garage and its lose value,while others sells quickly.without good data, its hard to decide the best cars to buy and the right price to sells.
### Expected outcomes
Identify which car models sells the faster in the market,recommend the best price to buy cars and the best price to sell them,reduce the time cars stay unsold and increase total profit.

## STEP2:Success criteria  
### Top 5 cars per region/quater-RANK()
identify the 5 most sold vehicles in each region in each region every quater,its helps to understand which cars are most popular and plan stock efficient.
### Running monthly sales totals-SUM() OVER()
Determine calculate the total sales for each month across all vehicles,the purpose of this its track overall business performance and revenue trend.
### Month over month growth-LAG() LEAD()
Compare this month's sales with the previous month's for each cars, measure sales growth or decline and spot trends in cars.
### Customer quartiles
group customers into four categories based on their purchase amounts and product nedeed.
### 3 month moving averages
calculate the average sales of each car over the last three months.

## STEP3: Database schema
| Table         | Purpose             | Key Columns                                                                 | Example Row                                |
|---------------|-------------------|----------------------------------------------------------------------------|-------------------------------------------|
| customers     | Store customer info | `customer_id (PK)`, `name`, `region`                                       | `1001, Bebeto, USA`                  |
| products      | Store vehicle catalog | `product_id (PK)`, `name`, `category`,'price'                                     | `2001, Range Rover, SUV`             |
| transactions  | Record sales       | `transaction_id (PK)`, `customer_id (FK)`, `product_id (FK)`, `sale_date`, `amount` | `3001, 1001, 2001, 2025-01-15, 25,000,000` |
| sellers       | Store seller info  | `seller_id (PK)`, `name`, `contact`, `region`                              | `4001, Mutagoma, +241788888888, Kigali` |
## ER Diagram

<img width="1898" height="892" alt="image" src="https://github.com/user-attachments/assets/92686099-b352-447e-b681-9f86d143f80f" />

## STEP4:Window functions implementation
## ranking
## query
### // Window function ranking products by products_price per products_category
### SELECT 
    customer_id,
    customer_name,
    customer_region,
    ROW_NUMBER() OVER (PARTITION BY customer_region ORDER BY customer_name DESC) AS row_num,
    RANK() OVER (PARTITION BY customer_region ORDER BY customer_name DESC) AS rank_,
    DENSE_RANK() OVER (PARTITION BY customer_region ORDER BY customer_name DESC) AS dense_rank_,
    PERCENT_RANK() OVER (PARTITION BY customer_region ORDER BY customer_name DESC) AS percent_rank
FROM customer;
## Screenshot
<img width="1600" height="765" alt="image" src="https://github.com/user-attachments/assets/5e9b1720-9b6e-486e-b643-190330001322" />

## Aggregate

## query

### // window function for calculating aggregate running_avg_products_price for each customer odering products

### SELECT 
    t.transactions_id, 
    p.products_id,
    c.customer_id,
    p.products_name,
    t.sales_date,
    AVG(p.products_price) OVER (
        PARTITION BY c.customer_id 
        ORDER BY p.products_price 
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
    ) AS running_avg_products_price
FROM products p
JOIN transactions t ON t.products_id = p.products_id
JOIN customer c ON t.customer_id = c.customer_id
ORDER BY c.customer_id, t.sales_date;

## Screenshot
<img width="1600" height="733" alt="image" src="https://github.com/user-attachments/assets/d07c24f2-f2fb-40d6-80a9-5c402e86793b" />

## Navigate

## query

### // navigate window function can prove if product_price is improve or decline

### SELECT 

    t.transactions_id, 
    p.products_id,
    c.customer_id,
    p.products_name,
    p.products_price,
    TO_CHAR(t.sales_date, 'DD-MON-YYYY') AS sales_date,
    LAG(p.products_price) OVER (
        PARTITION BY c.customer_id 
        ORDER BY t.sales_date
    ) AS prev_products_price,
    LEAD(p.products_price) OVER (
        PARTITION BY c.customer_id 
        ORDER BY t.sales_date
    ) AS next_products_price
FROM (SELECT

    3001 AS transactions_id, 2001 AS products_id, 1001 AS customer_id, 'Range Rover' AS products_name, 700000000 AS products_price, TO_DATE('15-JAN-2025','DD-MM-YYYY') AS sales_date FROM dual
    UNION ALL
    SELECT 3002, 2002, 1001, 'Toyota', 100000000, TO_DATE('28-FEB-2025','DD-MM-YYYY') FROM dual
    UNION ALL
    SELECT 3003, 2003, 1001, 'Carina', 90000000, TO_DATE('30-DEC-2025','DD-MM-YYYY') FROM dual 
    UNION ALL
    SELECT 3004, 2004, 1004, 'Lambogin' 400000000, TO_DATE('25-OCT-2025','DD-MM-YYYY') FROM dual
) t
JOIN customer c ON t.customer_id = c.customer_id
JOIN products p ON t.products_id = p.products_id
ORDER BY c.customer_id, t.sales_date;

## Screenshot

<img width="1600" height="695" alt="image" src="https://github.com/user-attachments/assets/301493eb-d537-4afa-b9b0-ca2829d7e071" />

## Distribution

## query

## find average use to calculate quantile and cume 

### SELECT 

  sub.customer_id,
  
  sub.avg_products_price,
  
  NTILE(4) OVER (ORDER BY sub.avg_products_price DESC) AS products_price_quantile,
  
  CUME_DIST() OVER (ORDER BY sub.avg_products_price DESC) AS products_price_cume_dist
  
FROM (
  SELECT
  
    t.customer_id,
    
    AVG(p.products_price) AS avg_products_price
    
  FROM
    transactions t
    
  JOIN
    products p ON t.products_id = p.products_id
    
  GROUP BY
    t.customer_id
    
) sub

ORDER BY 

  sub.avg_products_price DESC;
  
  ## Screenshot
  <img width="1600" height="763" alt="image" src="https://github.com/user-attachments/assets/4601b01c-bf36-43b9-aed4-f1c622632d4e" />


## STEP5: Result analysis

### Descriptive analysis(what happened):
in business of buying and selling cars requires market analysing  to understand demand and profitable niches,identifying and acquiring inventory through auctions or private sellers we need to prepare vehicle details and minor repairs, accurate and honest pricing for any customer we have,effective marketing and sales using online platform and personal connection and hindling legal and administrative tasks like title,bills of sales and financing option and my business is scalable for full time opertions.
### diagnostic analysis(why):
in business of buying and selling cars operation involves comprehensive assessment using SWOT analysis is strength,weakness,opportunities and threats use need use this SWOT to analyse identify area  like market conditions,competitions,operational efficiency,financial management and technological adoption.in this diagnostic analysis include setting clear objective finally implementing action s to improve performance and achieve goals.
###  Perspective analysis(what next):
buying and selling cars business determine rewarding venture,but requires a thorough understanding pf market trends,keen negotiation skills and we have strong financial displine ensures profitability.success hinges on sourcing undervalued vehicles,estimating repair cost accurately to maintain profity margins,preparings the cars professionally and building trust with buyers through transparency and acknowledge of the cars you offer.and use online platforms for marketing and careful research into consumer demand like the shift from diesel to hybrid or another electronic cars these are critical for our success for the future.



