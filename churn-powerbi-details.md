## Dataset Summary

The dataset includes information on 7043 telecom customers, covering attributes such as:

- Demographics: Gender, SeniorCitizen, Partner, Dependents
- Services: Internet, Phone, Online Security, Streaming
- Account Info: Tenure, Monthly Charges, Total Charges
- Customer Behavior: Payment Method, Contract Type, Paperless Billing
- Churn Status: Whether the customer has churned or not

The goal was to explore how different features impact churn likelihood and revenue sustainability.

## Data Preparation & Transformation

### Basic Checks
- No null values or duplicates were found, so no imputation or filtering was needed.
- Data types were adjusted where necessary (e.g., numeric vs. categorical).

### Calculated Columns Created

- **IsSenior**  
  A binary column (Yes/No) based on the SeniorCitizen field  
  `IsSenior = IF([SeniorCitizen] = 1, "Yes", "No")`  
  Useful for quickly segmenting churn by age group.

- **TenureGroup_Years**  
  Groups customers by their tenure in yearly buckets  
  `TenureGroup_Years = SWITCH(TRUE(), [Tenure] <= 12, "0-1 Years", [Tenure] <= 24, "1-2 Years", ..., "5+ Years")`  
  Enables comparison of churn trends across customer lifetime.

- **Family Status**  
  Categorizes customers based on whether they have a partner and/or dependents.  
  Example values: Single, Couple with Dependents, etc.

- **Streaming Type**  
  Combines StreamingTV and StreamingMovies into one simplified column.  
  Used to identify how many customers use both, one, or neither streaming service.

- **Service Type**  
  Captures the type of internet service used (DSL, Fiber Optic, None).  
  Important for comparing churn across infrastructure types.

- **Service Status**  
  Groups customers into Low, Medium, or High Service Usage based on number of services subscribed.


## DAX Measures Used

A few core DAX measures were created for dynamic insights and KPIs:

- **Total Customers**  
  `Total Customers = COUNTROWS(CustomerTable)`

- **Churned Customers**  
  `Churned Customers = CALCULATE([Total Customers], CustomerTable[Churn] = "Yes")`

- **Churn Rate**  
  `Churn Rate = DIVIDE([Churned Customers], [Total Customers])`

- **Avg Monthly Charges**  
  `Average Monthly Charges = AVERAGE(CustomerTable[MonthlyCharges])`

- **Churn by Segment**  
  (used in visuals via slicers and filters)

These DAX measures powered various KPIs, slicers, and segment-specific analysis.
