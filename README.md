# Customer Segmentation with RFM Analysis | Power BI

An end-to-end RFM (Recency, Frequency, Monetary) analysis of AdventureWorks internet sales customers. The project segments 18,484 customers and presents the results in a four-page interactive Power BI dashboard.

---

## Project Overview

RFM analysis scores customers on three behaviours:

- **Recency (R):** How recently did the customer last purchase?
- **Frequency (F):** How often do they purchase?
- **Monetary (M):** How much do they spend?

Combining these scores groups customers into segments, so a business can decide who to reward, who to nurture, and who to win back.

This was a Week 5 cohort project. The goal was to research RFM analysis, apply it to a real dataset, build a dashboard, and understand the analysis behind the visuals.

## Business Questions

1. Who are the most valuable customers, and where does revenue come from?
2. How do recency, frequency, and monetary value differ across customers?
3. Which customers are at risk of churning or already lost?
4. Do customer characteristics, such as income group and number of children, relate to purchasing behaviour?

## Dataset

**AdventureWorksDW.xlsx**, using these tables:

| Table | Purpose |
|---|---|
| FactInternetSales | Sales transactions |
| DimCustomer | Customer attributes |
| DimProduct | Product details |
| DimSalesTerritory | Sales regions |
| DimGeography | Customer locations |
| DimDate | Date dimension |

## Tools Used

- **Power BI** for data modelling and dashboard design
- **DAX** for RFM scores and segmentation logic
- **Excel** as the data source

## Methodology

1. **Data modelling:** Loaded the tables into Power BI and related them in a star schema, with FactInternetSales as the fact table.
2. **RFM metrics:** Calculated recency, frequency, and monetary value for each customer.
3. **Scoring:** Scored each metric from 1 to 5, where 5 is best. For recency, 5 means the customer bought very recently and 1 means they have not bought in a long time. Recency uses fixed recency ranges rather than quintiles, which keeps the thresholds simple to explain.
4. **Segmentation:** Assigned each customer to a segment using the rules below.

## Segment Definitions

Each customer is placed in exactly one segment. The rules are checked in the order shown below, and a customer takes the first segment they qualify for.

| Order | Segment | Rule | What it means |
|---|---|---|---|
| 1 | Lost Customers | R_Score = 1 | Have not purchased in a very long time. Most likely gone. |
| 2 | Fading Customers | R_Score = 2 | Their activity is slowing down. They are at risk of being lost if no action is taken. |
| 3 | New Customers | R_Score = 5 and F_Score <= 2 | Bought very recently but only a few times. They are new or still early in their relationship with the business. |
| 4 | Best Customers | R_Score >= 4 and (F_Score >= 4 or M_Score >= 4) | Bought recently and either buy often or spend a lot. The most valuable group. |
| 5 | Regular Customers | All other customers | Active customers with average engagement. They do not meet the criteria of any other segment. |

### Why the segments are defined this way

- **Recency comes first.** A customer who has stopped buying is not a good customer today, however much they spent in the past. That is why Lost and Fading are decided by recency alone.
- **Best Customers need recent activity plus strong value.** Requiring an R_Score of 4 or 5 makes sure they are still active. Requiring a high frequency score or a high monetary score captures both frequent buyers and big spenders.
- **New Customers are separated from Best Customers.** Someone who just made their first few purchases has not yet proven long-term value, so they get their own segment and their own strategy.
- **Regular Customers are the middle group.** This includes customers with a middle recency score (R_Score = 3), and recent customers (R_Score = 4 or 5) who do not meet the New or Best conditions.

## Segment Distribution

| Segment | Customers | Share of Customers | Share of Revenue |
|---|---|---|---|
| Best Customers | 1,984 | 10.7% | 27.6% |
| Regular Customers | 7,394 | 40.0% | 25.8% |
| New Customers | 4,723 | 25.6% | 23.6% |
| Fading Customers | 3,344 | 18.1% | 14.2% |
| Lost Customers | 1,039 | 5.6% | 8.7% |
| **Total** | **18,484** | **100%** | **100%** |

Total revenue across all customers is **$29.36M**.

## Dashboard

The Power BI report has four pages, with a navigation bar on the left side of each page.

### Page 1: RFM Overview
*Who are our customers, and where does our revenue come from?*

Shows total customers (18,484), total revenue ($29.36M), and the share of Best Customers (10.73%). Includes a monthly revenue and customer trend, a pie chart of customers by RFM segment, and a donut chart of revenue by RFM segment. Filter by Country, CalendarYear, and Gender.

### Page 2: RFM Breakdown
*How do recency, frequency, and monetary value differ across customers?*

Shows top customer spend ($13,295), average recency (190.33 days), average frequency (1.50), and average monetary value ($1,588). Includes score distributions for recency, frequency, and monetary value, plus a matrix comparing frequency score against monetary score. Filter by RFM Segment and CalendarYear.

### Page 3: Purchase Behavior
*Exploring the relationship between customer characteristics and purchasing behavior.*

Compares average monetary value and average frequency by income group and by number of children. Filter by RFM Segment, CalendarYear, and Occupation.

### Page 4: Customer Details
*Search for or select a customer name to view detailed information.*

A customer lookup page. Selecting a customer shows their full name, RFM segment, RFM score, recency, frequency, monetary value, gender, yearly income, marital status, total children, and country.

## Key Insights

**Revenue and segments**
- Best Customers are only 10.7% of customers but generate 27.6% of revenue, the highest share of any segment.
- Regular Customers are the largest group at 40.0% of customers but bring in 25.8% of revenue, so there is room to grow their spend.
- Fading and Lost Customers together make up 23.7% of customers and 22.9% of revenue, which is a significant win-back opportunity.
- Revenue peaks in May and June, drops sharply in July, and recovers strongly in December.

**RFM scores**
- Frequency is very low across the base: 98.7% of customers have a frequency score of 1 or 2, and only 245 customers score 3 or higher. Most customers buy once or twice.
- 41.5% of customers (7,676) have the lowest monetary score of 1, while 3,479 customers have the highest score of 5.
- Recency scores are concentrated at 3 to 5, which means most customers have bought fairly recently.

**Customer characteristics**
- Higher income means higher spend. Average monetary value rises from $1.33K in the $10K to $39K income group to $2.52K in the $130K to $170K group.
- Higher income groups also buy slightly more often, from 1.42 average orders in the lowest group to 1.70 in the highest.
- Spending is highest for customers with one child ($1.76K) and falls for households with three or more children, down to $1.20K at five children.
- Purchase frequency stays almost flat across household sizes (about 1.44 to 1.53), so number of children affects how much customers spend, not how often they buy.

## Recommendations

| Segment | Suggested Action |
|---|---|
| Best Customers | Loyalty rewards, early access, and referral programmes to protect the group that drives over a quarter of revenue |
| Regular Customers | Cross-sell and upsell to raise frequency and spend |
| New Customers | Onboarding offers to encourage a second purchase, since most customers only buy once or twice |
| Fading Customers | Re-engagement campaigns before they are lost |
| Lost Customers | Low-cost win-back offers |

Because spending rises with income, higher-income customers are also a good target for premium products and bundles.

## How to Use

1. Download or clone this repository.
2. Open the `.pbix` file in Power BI Desktop.
3. If prompted, update the data source path to the location of `AdventureWorksDW.xlsx`.
4. Use the slicers on each page to explore the data, and the left navigation bar to move between pages.

## Author

**Fridaous Eyitayo Aremu**
Data Analyst | Power BI, Excel, SQL, Python

GitHub: [Aremu-fridaous](https://github.com/Aremu-fridaous)
