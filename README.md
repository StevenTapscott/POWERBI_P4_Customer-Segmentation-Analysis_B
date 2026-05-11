#  Customer Segmentation & RFM Analysis

##  Overview  
This project focuses on customer behaviour analysis using the Global Superstore dataset in Power BI. The objective was to apply the RFM (Recency, Frequency, Monetary) framework to segment customers based on purchasing behaviour and identify high-value, low-engagement, and at-risk customer groups.

The project involved building a star schema data model, creating customer-level analytical metrics using DAX, and developing an interactive dashboard to support customer retention and revenue-focused decision-making.
---

##  Objectives  
- Analyse customer purchasing behaviour using RFM analysis  
- Identify high-value and at-risk customers  
- Build a customer segmentation dashboard in Power BI  
- Create customer scoring logic using DAX  
- Deliver actionable business insights through interactive visualisations  

---

##  Tools & Technologies  
- Power BI Desktop  
- Power Query  
- DAX (Data Analysis Expressions)  
- Star Schema Data Modelling  
- Data Visualisation  
- Customer Analytics  

---

##  Dataset  
**Global Superstore Dataset**

Dataset includes:
- Order ID  
- Order Date  
- Ship Date  
- Customer ID  
- Customer Name  
- Segment  
- Region  
- Product ID  
- Category  
- Sales  
- Quantity  
- Discount  
- Profit  

---

#  Data Model Structure

A star schema model was created to improve analytical performance and reporting structure.

##  Fact Table
### Orders
Contains transactional sales data:
- Sales
- Profit
- Quantity
- Discount
- Order Date
- Customer ID
- Product ID

---

##  Dimension Tables

### Customers
Contains customer attributes and RFM metrics:
- Customer Name
- Segment
- Region
- Recency
- Frequency
- Monetary
- Customer Segment Group

### Products
Contains product hierarchy information:
- Category
- Sub-Category
- Product Name

### Date Table
Contains:
- Date
- Year
- Quarter
- Month Name

---

#  Data Preparation

### Power Query Transformations
- Converted date columns using locale formatting (English - United States)
- Corrected numeric and currency data types
- Removed duplicate Customer IDs and Product IDs
- Structured dimension tables using query references
- Hid duplicate dimension fields from the Orders table

---

#  DAX Calculations

## Date Table Columns

```DAX
Year = YEAR('Date Table'[Date])
```

```DAX
Month Number = MONTH('Date Table'[Date])
```

```DAX
Month Name = FORMAT('Date Table'[Date], "MMMM")
```

```DAX
Quarter = "Q" & FORMAT('Date Table'[Date], "Q")
```

---

## KPI Measures

```DAX
Total Sales = SUM(Orders[Sales])
```

```DAX
Total Profit = SUM(Orders[Profit])
```

```DAX
Total Customers =
DISTINCTCOUNT(Customers[Customer ID])
```

```DAX
Total Orders =
DISTINCTCOUNT(Orders[Order ID])
```

```DAX
Average Order Value =
DIVIDE(
    [Total Sales],
    [Total Orders]
)
```

---

#  RFM Analysis Calculations

## Last Purchase Date

```DAX
Last Purchase Date =
CALCULATE(
    MAX(Orders[Order Date])
)
```

---

## Recency

```DAX
Recency =
VAR LatestDatasetDate =
    CALCULATE(
        MAX(Orders[Order Date]),
        ALL(Orders)
    )
RETURN
DATEDIFF(
    Customers[Last Purchase Date],
    LatestDatasetDate,
    DAY
)
```

---

## Frequency

```DAX
Frequency =
CALCULATE(
    DISTINCTCOUNT(Orders[Order ID])
)
```

---

## Monetary

```DAX
Monetary =
CALCULATE(
    SUM(Orders[Sales])
)
```

---

#  Customer Segmentation Logic

```DAX
Customer Segment =
SWITCH(
    TRUE(),

    Customers[Monetary] >= 5000 &&
    Customers[Frequency] >= 10 &&
    Customers[Recency] <= 90,
    "Champions",

    Customers[Monetary] >= 3000 &&
    Customers[Frequency] >= 5,
    "High Value",

    Customers[Recency] > 180 &&
    Customers[Monetary] >= 1000,
    "At Risk",

    Customers[Frequency] <= 2,
    "Low Engagement",

    "Standard"
)
```

---

#  Dashboard Features

## KPI Cards
- Total Sales
- Total Profit
- Total Customers
- Average Order Value

---

## Visualisations
- Customer Behaviour Analysis scatter plot
- Total Sales by Customer Segment
- Total Customers by Customer Segment
- Total Profit by Category
- Total Sales by Year

---

## Interactive Features
- Region slicers
- Ship Mode filters
- Category filters
- Customer Segment filtering

---

#  Key Insights

- High Value customers generated the highest proportion of total revenue.
- Standard customers represented the largest customer segment by volume.
- Champion customers demonstrated high purchase frequency and strong monetary value.
- At Risk customers contributed meaningful revenue while showing weaker engagement patterns.
- The Technology category generated the highest overall profit.
- Sales performance increased consistently between 2015 and 2017.

---

#  Business Recommendations

- Prioritise retention strategies for At Risk customers using targeted engagement campaigns.
- Continue investing in High Value and Champion customer relationships.
- Expand focus on Technology products due to strong profitability performance.
- Develop upselling strategies to convert Standard customers into High Value customers.
- Use RFM segmentation as an ongoing customer monitoring framework for marketing and retention initiatives.

---

#  Challenges Faced

- Designing a customer-focused star schema model
- Managing relationship cardinality and duplicate keys
- Creating effective RFM segmentation logic
- Configuring scatter plot aggregation correctly
- Balancing dashboard readability with analytical depth

---

#  Outcome

Successfully developed an interactive Power BI customer segmentation dashboard using RFM analysis techniques and star schema modelling. The project demonstrates practical skills in customer analytics, DAX calculations, dimensional modelling, and business intelligence storytelling.

---

#  Dashboard Preview
Should be in the screenshots folder.
---

#  Report Access
Published to Power BI Service via My Workspace.
Link - (https://app.powerbi.com/groups/me/reports/425527c5-dfd5-42fa-90cd-1526f0e66e71/290369dafabb78de805d?experience=power-bi)

---

#  Future Improvements
- Add advanced customer scoring models
- Implement drill-through customer profile pages
- Create dynamic customer retention KPIs
- Expand analysis with predictive churn modelling
- Implement Row-Level Security (RLS)
