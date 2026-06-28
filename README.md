# Sales Data Cleaning and Business Analysis Project

## 1. Problem Summary

The company has exported order-level sales data from multiple internal systems. The raw dataset contains issues such as inconsistent text formatting, date format problems, duplicate records, missing values, invalid discounts, calculation mismatches, and order status inconsistencies.

Task is to clean the dataset, validate the business rules, document all issues found, and create summary reports using Excel.

---

## 2. Dataset Description

| Column Name    | Description                                                 |
| -------------- | ----------------------------------------------------------- |
| order_id       | Unique identifier for each order                            |
| order_date     | Date when the order was placed                              |
| ship_date      | Date when the order was shipped                             |
| customer_id    | Unique identifier for each customer                         |
| customer_name  | Name of the customer                                        |
| segment        | Customer segment (Consumer, Corporate, Home Office, etc.)   |
| region         | Geographic region of the customer/order                     |
| state          | State where the customer is located                         |
| city           | City where the customer is located                          |
| category       | Main product category                                       |
| sub_category   | Product sub-category                                        |
| product_name   | Name of the product purchased                               |
| ship_mode      | Shipping method used for delivery                           |
| quantity       | Number of units purchased                                   |
| unit_price     | Price per unit of the product                               |
| discount       | Discount applied to the order                               |
| sales          | Total sales amount generated                                |
| cost           | Total cost associated with the product/order                |
| profit         | Profit earned (Sales − Cost)                                |
| payment_status | Status of payment (Paid, Pending, Failed, etc.)             |
| order_status   | Current order status (Completed, Cancelled, Returned, etc.) |

---

## 3. Tools Used

* Microsoft Excel
* Pivot Tables
* Excel Functions (IF, COUNTIF, SUM, AVERAGE, MONTH, YEAR, ROUND, etc.)
* Conditional Formatting
* Sorting and Filtering

---

## 4. Cleaning Actions Performed

### Missing Value Imputation

* Filled all blank region and ship_mode fields with the fallback string "Unknown".
* Replaced missing discount entries with 0 to keep financial columns clean.

### Data Standardization

* Stripped whitespace from string IDs and parsed all varied text fields in order_date and ship_date into clean datetime object formats.

### Discount Cleanup

* Built a programmatic cleaning loop to catch text symbols (like %), standardize whole numbers into decimals (turning 15 into 0.15), and force negative parameters to zero.

### Feature Engineering

Calculated 7 new columns to enhance data utility:

* cleaned_discount
* calculated_sales
* calculated_profit
* profit_margin
* shipping_delay_days
* order_month (formatted with numeric prefixes like "1 - January")
* order_year

---

## 5. Business Rules Applied

### Data Preservation (Rule 1)

The raw input file raw_orders.xlsx was completely left alone and preserved; all modifications were compiled into a separate workbench sheet.

### Discount Boundary Rule

Discounts were strictly bounded between a valid 0.0 (0%) and 1.0 (100%) range to safeguard downstream calculations.

### Calculated Metrics Formulas

```excel
=(Quantity * Unit_Price) * (1 - Cleaned_Discount)
```

```excel
Calculated_profit = calculated_sales - cost
```

```excel
Profit_margin = calculated_profit / calculated_sales
```

### Filter Rule for Summary Reports

Excluded non-completed transaction states (Cancelled, Failed, Refunded) from summaries to prevent false performance metrics.

---

## 6. Summary of Data Quality Issues Found

During the initial profiling of the dataset, several data quality issues were found across the columns:

### Missing Values

Blank fields discovered in critical categorical and numerical dimensions—specifically across:

* region (26 missing)
* ship_mode (22 missing)
* discount (18 missing)

### Duplicate Entries

* Identical row repetitions causing data volume inflation.
* Conflicting duplicate IDs where a single order_id held completely different operational details.

### Format & Value Inconsistencies

* Messy, unstandardized string percentages in the discount column.
* Negative discount values (-16 entries).

### Logical & Sequential Breaches

Transactions where the ship_date was logged chronologically earlier than the order_date.

### Downstream Calculation Mismatches

System-generated math errors where recorded sales and profit metrics diverged from their foundational formula equations.

### Order Status Vulnerabilities

Blended tracking records containing non-completed transaction states (Cancelled, Failed, and Refunded rows).

---

## 7. Summary of Final Pivot Reports

a. Sales by Region

b. Sales and Profit by Category and Sub-Category

c. Order Count by Ship-Mode

d. Profit Margin by Consumer Segment

e. Refunded/Cancelled/Failed Orders by Region

f. Monthly Sales Trend

---

## 8. Key Business Insights

### a. Regional Performance

* South Region generated the highest sales (~2.15M), followed closely by East and West.
* North Region recorded the lowest profit performance, contributing the largest overall losses despite comparable sales.

### b. Category & Sub-Category Analysis

* Furniture is the highest-selling category (~2.98M sales), followed by Technology and Office Supplies.
* Technology generated the highest losses, suggesting pricing, discounting, or cost issues.
* Among sub-categories, Chairs, Copiers, Phones, and Storage contribute significant sales volumes and should be prioritized for profitability analysis.

### c. Profitability Concerns

* All categories and customer segments show negative profit values, indicating a potential issue in pricing strategy, discount management, cost calculations, or profit formula logic.
* Further validation of the profit calculation is recommended before making strategic decisions.

### d. Shipping Preferences

* Standard Class is the most frequently used shipping mode (246 orders).
* Orders are relatively evenly distributed across First Class, Second Class, and Same Day shipping, suggesting diverse customer delivery preferences.
* Only a small percentage of orders have an unknown shipping method.

### e. Customer Segment Performance

* Consumer segment contributes the largest share of negative profit margin.
* Corporate, Home Office, and Small Business segments perform similarly, indicating no major profitability advantage from a particular segment.

### f. Order Status Trends

* Out of 932 total orders, 622 (67%) were completed successfully.
* 146 orders (16%) were cancelled and 164 orders (18%) were returned.
* The North Region has the highest number of cancelled orders, which may indicate fulfillment or customer satisfaction issues.

### g. Monthly Sales Trend (2025)

* Sales peaked in August (505K) and remained strong in September (432K).
* A decline is observed in the final quarter, with sales dropping sharply in November and December.
* The business appears to experience stronger performance during the middle of the year.

---

## 9. Assumptions & Limitations

### Assumptions Made

#### Cost Column Definitions

In the raw dataset, the cost field represents Total Cost for that line item rather than unit cost, based on the perfect matching of Sales - Cost = Profit across all mathematically valid rows.

#### Zero Discount Default

Missing discounts were assumed to be a placeholder for 0% rather than an uncompleted transaction, provided the other sales metrics held up as valid.

#### First Instance Validation

For exact carbon-copy rows, the first appearance was assumed to be the valid entry, while subsequent matches were assumed to be copy-paste system glitches.

### Limitations of Your Cleaning Process

#### Lack of Primary Source Verification

Because this cleaning process is handled programmatically post-export, there is no direct connection to the underlying inventory management database or retail CRM. This means we cannot automatically verify whether a mismatch in a customer's name or sales value is the absolute truth without a manual invoice lookup.

#### Blind Overwrite of Total Cost

Since the system assumes the cost column tracks total cost uniformly, any individual rows that might have actually flipped to "unit cost" during manual data entry could show up as false-positive profit calculation errors.

---

## 10. Screenshot Included

* raw_data_preview.png
* cleaned_data_preview.png
* pivot_summary_1.png
* pivot_summary_2.png
