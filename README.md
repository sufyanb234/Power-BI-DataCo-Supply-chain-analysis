# Identifying Key Drivers of Shipping Delays: A Data-Driven Supply Chain Analysis

This project analyzes supply chain and shipping data to identify the main factors associated with shipping delays and present the findings through an interactive Power BI dashboard.

The study focuses on understanding how variables such as shipping mode, scheduled shipment duration, order quantity, product price, discount level, customer segment, region, and department relate to delayed shipments.

The project follows a complete data analytics workflow, including data cleaning, preprocessing, exploratory data analysis, KPI development, factor comparison, and influencer analysis.

## Project Objectives

- Identify key factors associated with shipping delays.
- Analyze relationships between supply chain variables and delayed shipments.
- Determine the relative importance of potential delay factors.
- Develop an interactive Power BI dashboard for shipping-delay analysis.
- Present findings in a clear format that can support supply chain decision-making.

## Dataset

The project uses the **DataCo Smart Supply Chain Dataset**, which contains order, customer, product, geographical, shipping, and delivery-related information.

The dataset was cleaned and prepared before analysis by:

- Removing irrelevant or highly incomplete columns.
- Checking missing values and data types.
- Excluding cancelled shipments from the delay analysis.
- Creating a custom `Delay Outcome` variable to classify shipments as:
  - Delayed
  - On Time
- Creating additional grouped variables for quantity, price, and discount analysis.

## Power BI Dashboard

The Power BI report is divided into three main sections:

### 1. Shipping Overview
Provides an overall view of delivery performance using:

- Total Shipments
- Delayed Shipments
- On-Time Shipments
- Delay Rate
- Delay Rate Over Time
- Delay Rate by Shipping Mode
- Delay Rate by Region
- Delay Rate by Department
- Delay Rate by Customer Segment

### 2. Key Delay Drivers
Examines the relationship between shipping delays and factors such as:

- Shipping Mode
- Scheduled Shipment Duration
- Order Quantity
- Product Price
- Discount Level
- Customer Segment

Power BI's **Key Influencers** visual is also used to identify factors that are most strongly associated with delayed shipments.

### 3. Supply Chain Insights
Summarizes the main patterns identified during the analysis and presents the findings in a form that can support interpretation and supply chain decision-making.

## Key Findings

The analysis showed that **shipping mode** and **scheduled shipment duration** were among the strongest factors associated with shipping delays.

In the Key Influencers analysis:

- First Class shipping showed a strong association with increased delay likelihood.
- Very short scheduled shipment durations were also strongly associated with delayed shipments.
- Second Class shipping and shorter scheduled delivery periods also showed increased delay likelihood.

These results represent **associations within the dataset and should not be interpreted as direct causal relationships**.

## Tools & Technologies

- **Power BI**
- **Power Query**
- **DAX**
- **Data Cleaning**
- **Exploratory Data Analysis**
- **Data Visualization**
- **Supply Chain Analytics**

## Research Topic

**Identifying Key Drivers of Shipping Delays: A Data-Driven Supply Chain Analysis**

This project was developed as part of a university research methodology assignment and demonstrates how supply chain data can be transformed into interpretable insights using data analytics and interactive visualization.
