# Sales-and-Operational-Performance-Dashboard
The primary goal of this project was to establish a reliable, automated data workflow to clean and standardize messy source data, calculate key performance indicators (KPIs), and visualize operational performance across sales, customer service, and logistics (SLA). The final dashboard allows stakeholders to quickly identify performance bottlenecks, such as areas with low customer satisfaction or high delivery failure rates.

---

## 🛠 Tools
- **Microsoft Excel**
  - Power Query (Get & Transform Data)
  - Excel Formulas (e.g., XLOOKUP, COUNTIFS, COUNTA, IF, OR, ISBLANK)
  - PivotTables and Slicers
  - VBA Macro (for dashboard refresh button)
  - Charts
 
---

## 📂 About the dataset
**The dataset used in this project:** [Customer Satisfaction and Demographics Dataset](https://www.kaggle.com/datasets/noir1112/customer-satisfaction-and-demographics-dataset)

**Dataset Columns:**
- Customer ID: Unique identifier for each customer. Contains 52 missing values.
- Product Category: The category of the product purchased (e.g., Electronics, Books, Home Goods). Contains 59 missing values.
- Satisfaction Score: A score from 1 to 12 representing the customer's satisfaction with their purchase. Contains 52 missing values.
- Days for Delivery: Number of days taken for the product to be delivered. Contains 65 missing values.
- Customer Service Interaction: Whether the customer had an interaction with customer service (Yes/No). Contains 205 missing values.
- Purchase Amount: The total amount spent by the customer on the purchase. Contains 51 missing values.
- Customer Age: The age of the customer at the time of purchase. Contains 40 missing values.
- Gender: The gender of the customer (Male, Female, Other). Contains 212 missing values.

**Assumptions:** The original dataset lacked explicit Service Level Agreement (SLA) target delivery times. To create a meaningful performance metric for our delivery operations, I established a set of hypothetical SLA targets for the analysis. These targets were not pulled from official policy but were based on best-guess business judgment reflecting what we believe is a reasonable delivery timeframe for each product category. This allows the dashboard to function as a simulated performance benchmark, highlighting where we are likely failing or succeeding against a desirable standard.

This table contains synthetic, assumed targets for two key metrics: Delivery SLA (Days) and Late Threshold, which serve as the baseline standards for judging efficiency.

| Product Category    | Delivery SLA (Days) | Late Threshold  |
|:--------------------|:--------------------|:----------------|
| Books               |10                   |14               |
| Electronics         | 5                   | 7               |
| Clothings           | 7                   | 10              |
| Home Goods          | 7                   | 10              |


- Delivery SLA (Days): Represents the maximum acceptable delivery time (in days) that the company aims to meet for an order in that specific category.
- Late Threshold: Defines the tolerance level. An order is officially flagged as "SLA fail : late delivery" if the actual delivery time is greater than the Delivery SLA (Days).


---
## Excel Dashboard
🔎 **Download the raw file for the full Excel Dashboard and file here:** [Excel file & Dashboard](https://github.com/JennyHoang01/Sales-and-Operational-Performance-Dashboard/blob/main/Sales%26Operational_Performance_Dashboard.xlsm)

### Data Cleaning in Power Query
The following transformations were applied to the raw data:
- Promoted the first row as headers and explicitly set appropriate column data types.
- Applied Trim Text to remove unwanted leading/trailing spaces and standardised key columns to UPPERCASE.
- Replaced empty ID cells with "GUEST" to track non-logged-in transactions. Replaced empty Product Category cells with "UNCATEGORISED".
- Filtered the Purchase Amount_Financial KPI column to only include values greater than 0 to remove negative transactions error.
- Removed duplicate rows based on the Customer ID to ensure each transaction is counted only once.
- Created new columns for grouping and status tracking: Customer Age Group (based on age), and new status flags for Delivery Status (nulls → "delivery status missing") and Feedback Status (nulls → "FEEDBACK MISSING").

### KPI and Key Metrics Calculation using Excel Formulas
Advanced formulas were used to create the core metrics for the dashboard:
- The SLA_Target Days column was populated using XLOOKUP to dynamically retrieve the target days for the product's category.
- The SLA_Met_Status column was calculated using a nested IF and OR formula:
    - SLA fail : late delivery - If Days for Delivery is greater than SLA_Target Days.
    - SLA met - If Days for Delivery is less than or equal to SLA_Target Days.
    - delivery time missing / no SLA - If data is missing or no SLA is applicable.
    - The column was color-coded using Conditional Formatting (Fail=Red, Met=Green).
- Rate Calculations for dashboard using COUNTIFS and COUNTA:
  - Non-Straight Through Rate: Percentage of transactions requiring human intervention (Customer Service Interaction_Clean = "YES") / Total Transactions.
  - At-Risk Transaction Rate: Percentage of transactions with a low satisfaction score <= 5 AND a delivery failure (SLA fail : late delivery) / Total Transactions.
  - Percentage of Feedback Missing: Percentage of transactions where the Satisfaction Feedback_Status is "FEEDBACK MISSING" / Total Transactions.

![Dashboard](https://github.com/JennyHoang01/Sales-and-Operational-Performance-Dashboard/blob/main/Sales%26OperationalDashboard.png)

---

## 🎯 Insights and Business Recommendation
- **The Books category is the biggest operational drag. It has the Lowest Satisfaction (6.82) and a high average delivery time (5.04 days).** <br>
💡Recommendation: Conduct an immediate root cause analysis for the Books category:
   - Logistics: Identify ways to streamline inventory or fulfillment process for Books to reduce delivery days
   - Product & Service: Check for frequent issues with product quality and order accuracy for this category
- **41% Non-Straight Through Rate (NSTR)** : A transaction is "Non-Straight Through" if it requires human intervention like a customer service interaction. This high rate suggests that nearly half of all transactions encounter an issue requiring manual support, leading to high operational costs and potentially customer frustration. <br>
💡Recommendation: Focus on automation and self-service.
    - Analyze interaction drivers: Check the transactions that triggered a "YES" for customer service interaction and track whether it is due to order tracking, billing errors, or product information.
    - Implement self-service tools: Improve the clarity of order tracking pages, create comprehensive FAQs, and use AI-powered chatbots to handle routine inquiries. The goal should be to push the NSTR below 20%.
- **15% Late Delivery is linked to 3% At-Risk Transactions, where customers experienced low satisfaction score and late delivery** <br>
💡Recommendation: Review carrier performance and fulfillment centers bottlenecks to have more reliable delivery times.

