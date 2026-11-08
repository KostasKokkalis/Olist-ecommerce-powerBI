# Olist-ecommerce-powerBI

A hands-on Power BI project analyzing how shipping delays impact customer review ratings, using the public **Olist Brazil dataset** (around 99K orders from 2016 to 2018).

## 🎯 Project Goals & Business Projects

* How much does a delivery delay hurt a customer's review score?
* What is the current on-time delivery rate, and how long does an average order take to arrive?
* Where are most customers located, and which product categories drive the highest order volume?

## 🧹 Data Cleaning (Power Query)

* **Product Names:** Replaced underscores (`_`) with spaces and capitalized words so category names look clean (e.g., `bed_bath_table` turned into `Bed Bath Table`).
* **Delivery Status:** Created a simple condition to tag each order as `On Time`, `Late`, or `Non-Delivered` by comparing the actual delivery date with the estimated delivery date.
* **Missing Dates:** Removed blank delivery dates where needed so they wouldn't mess up average delivery time calculations.
* **Data Types:** Made sure date columns were set correctly to date/time format for slicer filtering.

## 📐 Data Model & DAX

### Main DAX Formulas used :

```dax

Total Orders = 
DISTINCTCOUNT(olist_orders_dataset[order_id])

//Counts unique orders so items in the same cart don't inflate the total

On-Time Delivery Rate = 
DIVIDE(
    CALCULATE([Total Orders], olist_orders_dataset[Delivery_Status] = "On Time"),
    [Total Orders],
    0
)

//Calculates the percentage of the orders delivered on time

Avg Delivery Time = 
AVERAGEX(
    FILTER(
        olist_orders_dataset, 
        NOT(ISBLANK(olist_orders_dataset[order_delivered_customer_date]))
    ),
    DATEDIFF(
        olist_orders_dataset[order_purchase_timestamp],
        olist_orders_dataset[order_delivered_customer_date],
        DAY
    )
  
//Calculates the average days from purchase date to delivery date (ignores the non-delivered ones)

Average Review Score = 
AVERAGE(olist_order_reviews_dataset[review_score])

//Calculates the average customer review rating of the orders

```

## 💡 What the Data Shows (Key Takeaways)

- Orders delivered on time get an average rating of 4.3, but when an order is late, it drops down to 2.3 (almost cut in half).
  - Insight - Delays negatively impact review scores and non-delivering usually equals ratings below 2/5.

- Orders are heavily concentrated to certain states as more than half (54%) of top-state orders go directly to São Paulo (SP).
  - Insight - Improving shipping times in this region alone would improve overall delivery metrics.

- Top Selling categories: Bed Bath Table, Health Beauty, and Sports Leisure make up the bulk of orders.
  - Insight - Inventory shortages in these categories carry the biggest risk.

## 🛠️ Tools Used

* **Power BI Desktop** (Visualizations & DAX)
* **Power Query** (Data Cleaning)
* **Dataset:** [Olist E-Commerce Dataset on Kaggle](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)
