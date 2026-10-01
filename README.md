# Identifying Key Drivers of Shipping Delays: A Data-Driven Supply Chain Analysis

This project analyzes supply chain and shipping data to identify the main factors associated with shipping delays and presents the findings through an interactive Power BI dashboard.

The analysis focuses on understanding how variables such as shipping mode, scheduled shipment duration, order quantity, product price, discount level, customer segment, region, and department are associated with delayed shipments.

The project follows a complete data analytics workflow, including data cleaning, preprocessing, exploratory data analysis, KPI development, factor comparison, and influencer analysis.

---

## Project Objectives

The main objectives of this project are to:

- Identify key factors associated with shipping delays.
- Analyze relationships between supply chain variables and delayed shipments.
- Determine the relative importance of potential delay factors.
- Develop an interactive Power BI dashboard for shipping-delay analysis.
- Present analytical findings in a clear format that can support supply chain decision-making.

---

## Dataset

This project uses the **DataCo SMART Supply Chain for Big Data Analysis** dataset.

The dataset contains information relating to:

- Orders
- Customers
- Products
- Shipping methods
- Delivery performance
- Geographical markets
- Sales
- Order quantities
- Discounts
- Delivery schedules

The raw dataset is not included in this repository because the main CSV file is too large.

### Download the Dataset

The original dataset can be downloaded from Kaggle:

**[DataCo SMART Supply Chain for Big Data Analysis](https://www.kaggle.com/datasets/shashwatwork/dataco-smart-supply-chain-for-big-data-analysis)**

The main file used in this project is:

```text
DataCoSupplyChainDataset.csv
```

After downloading the dataset, use this file when connecting or refreshing the Power BI report.

---

## Data Preparation

The dataset was cleaned and prepared in Power Query before analysis.

The preparation process included:

- Checking column data types
- Checking missing and empty values
- Removing irrelevant or highly incomplete columns
- Excluding cancelled shipments from the delay analysis
- Creating a custom delay classification
- Creating grouped variables for selected numerical features
- Preparing measures for dashboard analysis

The following columns were removed because they were not useful for the analysis:

```text
Order Zipcode
Product Description
Product Image
```

---

## Delay Outcome

A custom column named `Delay Outcome` was created to classify shipments into delayed, on-time, and excluded deliveries.

```powerquery
if [Delivery Status] = "Late delivery" then "Delayed"
else if [Delivery Status] = "Shipping canceled" then "Excluded"
else "On Time"
```

The classifications were:

```text
Late delivery      → Delayed
Shipping canceled  → Excluded
Advance shipping   → On Time
Shipping on time   → On Time
```

Cancelled shipments were excluded from the final shipping-delay analysis.

---

## Delay Flag

A numerical target variable was created to support factor analysis.

```DAX
Delay Flag =
IF(
    'DataCoSupplyChainDataset'[Delay Outcome] = "Delayed",
    1,
    0
)
```

Where:

```text
1 = Delayed
0 = On Time
```

---

## Power BI Measures

Several DAX measures were created to calculate the main shipping KPIs.

### Total Shipments

```DAX
Total Shipments =
COUNTROWS('DataCoSupplyChainDataset')
```

### Delayed Shipments

```DAX
Delayed Shipments =
CALCULATE(
    [Total Shipments],
    'DataCoSupplyChainDataset'[Delay Outcome] = "Delayed"
)
```

### On-Time Shipments

```DAX
On-Time Shipments =
CALCULATE(
    [Total Shipments],
    'DataCoSupplyChainDataset'[Delay Outcome] = "On Time"
)
```

### Delay Rate

```DAX
Delay Rate =
DIVIDE(
    [Delayed Shipments],
    [Total Shipments]
)
```

### On-Time Rate

```DAX
On-Time Rate =
DIVIDE(
    [On-Time Shipments],
    [Total Shipments]
)
```

---

## Feature Engineering

Additional calculated columns were created to make numerical variables easier to analyze and visualize.

### Quantity Group

```DAX
Quantity Group =
SWITCH(
    TRUE(),
    'DataCoSupplyChainDataset'[Order Item Quantity] <= 1, "1",
    'DataCoSupplyChainDataset'[Order Item Quantity] <= 2, "2",
    'DataCoSupplyChainDataset'[Order Item Quantity] <= 3, "3",
    'DataCoSupplyChainDataset'[Order Item Quantity] <= 4, "4",
    "5+"
)
```

### Discount Group

```DAX
Discount Group =
SWITCH(
    TRUE(),
    'DataCoSupplyChainDataset'[Order Item Discount] < 0.10, "0–9%",
    'DataCoSupplyChainDataset'[Order Item Discount] < 0.20, "10–19%",
    'DataCoSupplyChainDataset'[Order Item Discount] < 0.30, "20–29%",
    'DataCoSupplyChainDataset'[Order Item Discount] < 0.40, "30–39%",
    "40%+"
)
```

### Price Group

```DAX
Price Group =
SWITCH(
    TRUE(),
    'DataCoSupplyChainDataset'[Product Price] < 50, "Under $50",
    'DataCoSupplyChainDataset'[Product Price] < 100, "$50–$99",
    'DataCoSupplyChainDataset'[Product Price] < 200, "$100–$199",
    'DataCoSupplyChainDataset'[Product Price] < 300, "$200–$299",
    "$300+"
)
```

---

## Dashboard Structure

The Power BI report is organized into three main sections.

### 1. Shipping Overview

The Shipping Overview page provides a high-level view of overall delivery performance.

It includes:

- Total Shipments
- Delayed Shipments
- On-Time Shipments
- Delay Rate
- Delay Rate Over Time
- Delay Rate by Shipping Mode
- Delay Rate by Order Region
- Delay Rate by Department
- Delay Rate by Customer Segment

This page answers:

> What is happening in the supply chain?

---

### 2. Key Delay Drivers

The Key Delay Drivers page focuses on identifying factors associated with delayed shipments.

Factors analyzed include:

- Shipping Mode
- Scheduled Shipment Duration
- Order Quantity
- Product Price
- Discount Level
- Customer Segment
- Order Region
- Department

Power BI's **Key Influencers** visual was used to identify conditions that were most strongly associated with delayed shipments.

This page answers:

> Which factors are associated with shipping delays?

---

### 3. Supply Chain Insights

The Supply Chain Insights page summarizes the main findings from the analysis and presents them in an accessible form for supply chain users and decision-makers.

This section focuses on:

- Important delay patterns
- Strong delay influencers
- High-risk shipping conditions
- Delivery performance insights
- Decision-support information

This page answers:

> What do the analytical findings mean?

---

## Key Findings

The analysis identified **shipping mode** and **scheduled shipment duration** as important factors associated with shipping delays.

The Power BI Key Influencers analysis showed that:

- **First Class shipping** showed one of the strongest associations with an increased delay outcome.
- **Scheduled shipment durations of 0–1 days** were also strongly associated with a higher delay outcome.
- **Second Class shipping** showed an increased association with delayed shipments.
- **Scheduled shipment durations of 1–2 days** were also associated with increased delay likelihood.

These findings suggest that shorter scheduled delivery periods and certain shipping modes are associated with higher levels of delayed shipments within the dataset.

> **Important:** These findings represent associations within the dataset and should not be interpreted as proof of direct causal relationships.

---

## Data Leakage Consideration

Some fields in the original dataset directly describe or reveal the final delivery outcome.

These were not used as explanatory delay drivers:

```text
Delivery Status
Late_delivery_risk
Days for shipping (real)
```

Using these variables as predictors could lead to data leakage because they contain information that is directly related to the final shipment outcome.

The analysis therefore focused on variables that could reasonably be examined as potential factors associated with shipping delays.

---

## Proposed System

The overall analytical workflow used in this project is:

```text
Shipping & Supply Chain Data
            ↓
Data Collection & Integration
            ↓
Data Cleaning & Pre-processing
            ↓
Exploratory Data Analysis (EDA)
            ↓
Identification of Potential Delay Factors
            ↓
Data-Driven & Statistical Analysis
            ↓
Evaluation of Factor Importance
            ↓
Interactive Shipping Delay Analytics Dashboard
            ↓
Visualisations & Insights
            ↓
Supply Chain Decision Support
```

---

## Tools & Technologies

- Microsoft Power BI
- Power Query
- DAX
- Data Cleaning
- Data Preprocessing
- Exploratory Data Analysis
- Data Visualization
- KPI Analysis
- Supply Chain Analytics
- Key Influencers Analysis
- Business Intelligence

---

## Project Workflow

```text
Raw Dataset
    ↓
Data Cleaning
    ↓
Data Transformation
    ↓
Feature Creation
    ↓
KPI Development
    ↓
Exploratory Data Analysis
    ↓
Delay Driver Analysis
    ↓
Key Influencers Analysis
    ↓
Power BI Dashboard
    ↓
Supply Chain Insights
```

---

## How to Use This Project

1. Clone or download this repository.

2. Download the dataset from Kaggle:

   **[DataCo SMART Supply Chain for Big Data Analysis](https://www.kaggle.com/datasets/shashwatwork/dataco-smart-supply-chain-for-big-data-analysis)**

3. Extract the downloaded files.

4. Locate:

```text
DataCoSupplyChainDataset.csv
```

5. Open the Power BI `.pbix` file included in this repository.

6. If Power BI cannot locate the original CSV file, update the data source path to the location where you downloaded:

```text
DataCoSupplyChainDataset.csv
```

7. Refresh the Power BI report.

8. Explore the dashboard pages:

```text
Shipping Overview
Key Delay Drivers
Supply Chain Insights
```

---

## Research Topic

**Identifying Key Drivers of Shipping Delays: A Data-Driven Supply Chain Analysis**

This project was developed as part of a university Research Methods for Computing and Technology project.

The project demonstrates how supply chain data can be cleaned, analyzed, and transformed into an interactive decision-support dashboard using Power BI.

---

## Disclaimer

The results of this project are based on the available DataCo supply chain dataset and are intended for academic and analytical purposes.

The identified influencers represent associations within the dataset and should not automatically be interpreted as causal relationships.

---

## Author

**Sufyan Baig**

Bachelor of Computer Science (Hons)  
Specialization in Data Analytics  
Asia Pacific University of Technology & Innovation

[GitHub](https://github.com/sufyanb234) | [LinkedIn](https://www.linkedin.com/in/muhammad-sufyan-632968343/)
