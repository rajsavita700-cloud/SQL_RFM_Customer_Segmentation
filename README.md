**Executive Summary & Context**

Every customer interacts with a business differently. Treating a first-time buyer the same way as a long-time regular leads to wasted marketing budgets and missed revenue opportunities. This project leverages the **RFM (Recency, Frequency, Monetary)** framework in **MySQL** to segment e-commerce customers, uncover hidden retention risks, and identify high-value opportunities. By using customer segmentation, a wide customer base can be divided into distinct and useful groups according to actual behaviour rather than personal opinion.

The RFM (Recency, Frequency, Monetary) approach is one of the most effective means of achieving this; it looks at three basic measures, how recently a customer has made a purchase (known as Recency), how often they buy (Frequency) and how much money they spend (Monetary). Businesses are able to work out which customers contribute most to their bottom line, which ones are drifting away, and where to allocate their resources in order to achieve the best return on investment.

---

**The Business Problem:**

At first sight, our e-commerce performance appears to be in good shape since our top-line figures indicate strong total sales, these being mainly the result of a steady number of VIP and Loyal accounts. Nevertheless, focusing just on the revenue figures gives a misleading sense of security.

When we look deeper into our customer engagement metrics, critical operational risks emerge:

* **A high risk of customer churn:** A large number of our customers haven't placed an order for months. More than **27% of our customers** are classified as being "At Risk" or "Lost", indicating a considerable amount of potential lost revenue.
  
* **Overreliance at the top level:** Almost all of our VIP revenue comes from the UK (1,210 of the 1,293 VIPs), which means that our growth is heavily based on just one geographic area.
  
* **Stalled Pipeline:** Since less than 8% of our users are new customers, this shows that our marketing is not succeeding in turning new signups into repeat buyers.

---

**Steps taken for RFM Analysis:**

To diagnose these underlying issues, we structured an end-to-end SQL analysis on our transactional database using MySQL:

1. **Data Cleaning:** In order to clean the raw transaction records we used `WHERE` clauses to eliminate those cases with missing customer IDs, negative order quantities, zero unit prices, and cancelled invoices (specifically by using `WHERE invoice NOT LIKE 'C%'`).

2. **Combining the baseline RFM metrics:** We determined each customer's most recent purchase date, the total number of orders, and their net lifetime expenditure by using GROUP BY customer_id together with DATEDIFF() and the aggregate functions (MAX(), COUNT(DISTINCT), and SUM()).

3. **RFM Scoring using Window Functions:** We used the NTILE(5) window function with the dataset partitioned to assign customers a score on a scale from 1 to 5 for the Recency, Frequency, and Monetary parameters.

4. **Segment Allocation:** We divided the scores into separate behavioural categories such as **VIP**, **Loyal**, **At Risk**, **New Customer**, and **Lost** by means of conditional logic (`CASE WHEN`).

5. **Relevant Business Queries:** We carried out JOIN queries between our segmented table and the raw transaction tables (rfm_segments JOIN orders_clean) in order to examine specific product purchases and geographic distributions.

---

**Key Findings**

The deeper query results revealed a striking insight that directly explains customer churn:

* The case of the one item involved was that within our 'At Risk' group, **74,239 units** of the *Medium Ceramic Top Storage Jar* were purchased. This made it the best-selling item overall in the entire survey, outperforming all the top-selling products in the VIP segment. These high-value customers did not stop buying due to a lack of interest; rather, they ceased their purchases because a particular product that was in high demand went out of stock or experienced a decline in quality.
* **Geographic Imbalance:** Of the **1,293 VIPs**, **1,210 are in the United Kingdom**. Although there is strong order potential in international markets such as Ireland, the Netherlands, Germany, and France, the number of VIPs in these countries is very low.

---

**Strategic Recommendations & Action Plan**

* **Targeted Retention and Stock Security:** Start immediate win-back campaigns for customers who are at risk and have purchased high-volume core products such as the *Medium Ceramic Top Storage Jar* (which had 74,239 units sold in this category), while at the same time maintaining strict control of inventory and carrying out thorough quality checks on these important sources of revenue in order to avoid silent churn.

* **Segment-Specific Growth and Resource Allocation:** Direct resources towards converting 'Loyal' customers into 'VIPs' by offering incentives based on purchase frequency, provide 'Loyal' customers with a high level of service without giving them excessive discounts, and cut back on marketing expenditure regarding low-return one-time buyers ('Big Spender Gone').

* **Expanding the pipeline and examining the 'Others':** Targeted onboarding offers should be used to deal with the low rate of new customer acquisition (which is less than 8 per cent), and behavioural audits must be carried out on the 'Others' group (which makes up 15 per cent of all users) in order to identify any potential repeat buyers who require personalised engagement.

* **Macro Trend & Market Diagnostics:** Examine the underlying reasons for the high recency observed among "Lost" customers (~582 days since their last activity), determining if customer loss is due to the emergence of new market competitors or a shift on the part of consumers towards Q-Commerce, and at the same time expand the UK retention processes into those European markets that are currently underpenetrated (Germany, France).
