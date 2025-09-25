# plsql-windows-function-UMUBYEYI-GAHAMANYI-Madeleine
PL/SQL window function assignment auca 26/09/2025 
## STEP1:PROBLEM DEFINITION
### Business context
### company type small company that sells and buys car.
-department:sales and procurement. 
-industry:Automotive retail.
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
group customers into four categories based on their purchase amounts
### 3 month moving averages
calculate the average sales of each vehicle over the last three months

## STEP3: Database schema

| Table          Purpose               | Key Columns                                                                      | Example Row                                |
| -------------  --------------------- | -------------------------------------------------------------------------------- | ------------------------------------------ |
| customers      Store customer info   | `customer_id (PK)`, name, region                                                 | `1001, Bebeto mutagoma, USA`               |
| products     | Store vehicle catalog | `product_id (PK)`, name, category                                                | `2001, Range Rover, SUV`                   |
| transactions | Record sales          | `transaction_id (PK)`, `customer_id (FK)`, `product_id (FK)`, sale\_date, amount | `3001, 1001, 2001, 2025-01-15, 25,000,000` |

## ER Diagram

<img width="1052" height="660" alt="image" src="https://github.com/user-attachments/assets/4fc5981b-708f-4c23-8173-f995442df61f" />

## STEP5: Result analysis

### Descriptive analysis(what happened):
in business of buying and selling cars requires market analysing  to understand demand and profitable niches,identifying and acquiring inventory through auctions or private sellers we need to prepare vehicle details and minor repairs, accurate and honest pricing for any customer we have,effective marketing and sales using online platform and personal connection and hindling legal and administrative tasks like title,bills of sales and financing option and my business is scalable for full time opertions.
### diagnostic analysis(why):
in business of buying and selling cars operation involves comprehensive assessment using SWOT analysis is strength,weakness,opportunities and threats use need use this SWOT to analyse identify area  like market conditions,competitions,operational efficiency,financial management and technological adoption.in this diagnostic analysis include setting clear objective finally implementing action s to improve performance and achieve goals.



