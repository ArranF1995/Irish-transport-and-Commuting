# Irish Public Transport Trends & Analysis

### Excel | Power Query | SQL | Power BI | DAX

An end-to-end data analytics project examining **public transport passenger journeys across Ireland from 2019 to 2026**, covering Dublin Bus, regional bus services, Rail and Luas.

The project combines official Irish transport datasets to analyse passenger trends, transport mode performance, post-pandemic recovery and long-term growth.

Using **Excel, Power Query, SQL and Power BI**, I developed a structured analytical dataset and an interactive four-page dashboard to communicate key findings.

**Data Coverage:** 2019–2026 YTD  
**Dataset Size:** 2,831 records  
**Reporting Frequency:** Weekly  
**Latest Complete Year:** 2025

> **Note:** 2026 contains partial-year data and is excluded from complete-year comparisons unless reporting periods are aligned.

---

## Project Objectives

The main objectives of this project were to:

- Analyse changes in Irish public transport passenger journeys over time.
- Compare passenger volumes across major transport modes.
- Identify long-term growth and recovery patterns.
- Examine the impact of the 2020–2021 decline in passenger activity.
- Analyse weekly trends and seasonality.
- Develop reliable KPIs while preventing double-counting.
- Build an interactive Power BI dashboard for exploring transport performance.

The project also focuses on **data quality, validation and analytical accuracy**, particularly when working with overlapping categories and incomplete observations.

---

## Tools & Technologies

| Tool | Purpose |
|---|---|
| Excel | Data inspection, validation and pivot analysis |
| Power Query | Data cleaning, transformation and combining datasets |
| SQL | Data validation, aggregation and trend analysis |
| Power BI | Data modelling, visualisation and interactive dashboards |
| DAX | KPI calculations, YoY growth and dynamic measures |

---

## Data Sources

The analysis uses two official Irish public transport datasets.

### THA25 — Passenger Journeys by Public Transport

**Publisher:** National Transport Authority (NTA)

Contains weekly passenger journey data for:

- Dublin Metro Bus
- Bus, excluding Dublin Metro
- Rail
- All public transport, excluding Luas

### TII03 — Passenger Journeys by Luas

**Publisher:** Transport Infrastructure Ireland (TII)

Contains weekly passenger journey data for:

- Red Line
- Green Line
- All Luas Lines

Both datasets were standardised and combined into a single analytical dataset containing **2,831 records**.

---

## Data Cleaning & Preparation

The original datasets were preserved before transformation.

Using Excel and Power Query, I performed the following steps:

1. Imported and inspected both datasets.
2. Standardised column names and data types.
3. Extracted Year and Week Number from reporting periods.
4. Created a Data Status column to identify missing observations.
5. Added source identifiers to preserve data lineage.
6. Standardised transport mode categories.
7. Combined the datasets into one analytical table.
8. Preserved missing observations instead of replacing them with zero.
9. Classified aggregate, core and component-level transport categories.
10. Validated the cleaned dataset using Excel and SQL.

The resulting dataset supports analysis by:

**Year → Week → Transport Mode → Passenger Journeys**

### Preventing Double-Counting

An important challenge was identifying overlapping categories within the source data.

For example:

- All public transport, excluding Luas, is an aggregate category.
- Red Line and Green Line are components of the Luas network.
- All Luas Lines represents the overall Luas network.

Including both aggregate totals and their underlying components would overstate passenger journeys.

To prevent this, the main analysis uses four non-overlapping transport categories:

- Dublin Metro Bus
- Bus, excluding Dublin Metro
- Rail
- Luas

Aggregate and component-level categories are analysed separately.

This ensures the main dashboard KPIs are based on consistent transport classifications.

---

## Key Findings

### 1. Public Transport Recovery

Passenger activity declined sharply during 2020 and 2021 before recovering strongly from 2022 onwards.

Across the four core transport modes, annual passenger journeys reached approximately:

| Year | Passenger Journeys |
|---|---:|
| 2019 | 236.7M* |
| 2020 | 135.7M |
| 2021 | 136.0M |
| 2022 | 241.6M |
| 2023 | 298.4M |
| 2024 | 327.6M |
| 2025 | 326.5M |

*2019 excludes Rail because passenger journey observations were unavailable. It is therefore not directly comparable with later four-mode totals.*

### 2. Dublin Metro Bus Remains the Largest Mode

Dublin Metro Bus recorded approximately **176.9 million passenger journeys in 2025**, compared with 151.7 million in 2019.

This represents growth of approximately **16.6%**.

### 3. Strong Growth in Bus Services Outside Dublin

Bus services outside Dublin recorded approximately **35.8% growth between 2019 and 2025**.

This was the strongest growth among transport modes with comparable 2019 baseline data.

### 4. Luas Passenger Growth

Luas passenger journeys increased from approximately **48.1 million in 2019 to 55.0 million in 2025**.

This represents growth of approximately **14.2%**.

### 5. Passenger Activity Stabilised in 2025

Total passenger journeys across the four core modes reached approximately **326.5 million in 2025**, compared with 327.6 million in 2024.

This represents a marginal decrease of approximately **0.3%**, based on the reported annual totals.

---

## 2025 Transport Mode Comparison

| Transport Mode | Passenger Journeys |
|---|---:|
| Dublin Metro Bus | 176.9M |
| Luas | 55.0M |
| Bus, excluding Dublin Metro | 50.1M |
| Rail | 44.5M |
| **Total** | **326.5M** |

Dublin Metro Bus accounted for the largest proportion of journeys across the four core modes in 2025.

---

## Power BI Dashboard

The interactive Power BI dashboard contains four analytical pages designed to explore passenger trends, mode performance and data quality.

### 1. Public Transport Overview

![Public Transport Overview](06_Screenshots/Power%20Bi%20-%20Public%20Transport%20Overview.png)

Provides an overview of public transport activity across Ireland.

**Key Features:**

- Total passenger journey KPIs
- Annual passenger trends
- Year-on-year performance
- Transport mode comparisons
- Historical recovery analysis
- Interactive filters

### 2. Luas Analysis

![Luas Analysis](06_Screenshots/Power%20Bi%20-%20Luas%20Analysis.png)

Explores Luas passenger performance across the Red and Green Lines.

**Key Features:**

- Annual Luas passenger journeys
- Weekly passenger trends
- Red Line vs Green Line comparison
- Year-on-year growth
- Weekly seasonality
- Historical performance

### 3. Transport Mode Analysis

![Transport Mode Analysis](06_Screenshots/Power%20Bi%20-%20Transport%20Mode%20Analysis.png)

Compares passenger activity across the four core transport modes.

**Key Features:**

- Annual passenger journeys by mode
- Transport mode share
- Historical growth comparisons
- Recovery analysis
- Dynamic year selection
- Interactive comparisons

### 4. Data & Methodology

![Data and Methodology](06_Screenshots/Power%20Bi%20-%20Data%20Quality.png)

Documents the assumptions and data-quality considerations supporting the analysis.

**Key Features:**

- Dataset coverage
- Data source information
- Missing observations
- Transport mode classifications
- Aggregation methodology
- Partial-year reporting limitations

---

## SQL Analysis

SQL was used to validate the combined dataset and analyse passenger journey trends.

The analysis includes:

- Data-quality checks
- Annual passenger journey totals
- Transport mode comparisons
- Year-on-year performance
- Weekly passenger trends
- Seasonality analysis

### Example: Annual Passenger Journeys by Transport Mode

```sql
SELECT 

YEAR,
TRANSPORT_MODE,
SUM(PASSENGER_JOURNEYS) AS TOTAL_JOURNEYS

FROM FACT_PASSENGER_JOURNEYS
WHERE DATASTATUS = 'Available'

GROUP BY YEAR,TRANSPORT_MODE
ORDER BY YEAR,TRANSPORT_MODE;
```

This query calculates annual passenger journeys by transport mode using observations marked as available, supporting comparisons of passenger activity across different years and transport categories

---

## DAX Measures

DAX was used to create dynamic measures supporting the Power BI dashboard.

Examples include:

- Total Passenger Journeys
- Previous Year Journeys
- Year-on-Year Change
- Year-on-Year Growth %
- Transport Mode Share %

### Example: Previous Year Journeys

```dax
Previous Year Journeys = 
VAR CurrentYear =
    SELECTEDVALUE('Public_Transport_Combined'[Year])

RETURN
    CALCULATE(
        [Total Passenger Journeys],
        REMOVEFILTERS('Public_Transport_Combined'[Year]),
        'Public_Transport_Combined'[Year] = CurrentYear - 1
    )
```

This measure calculates passenger journeys for the previous year based on the currently selected year, supporting year-on-year comparisons within the Power BI dashboard.

---

## Data Quality & Limitations

Several data-quality considerations were addressed during the project.

**Missing Rail Data (2019)**

Rail passenger journey observations are unavailable for 2019. Rail is therefore excluded from comparisons requiring a 2019 baseline.

**Partial-Year Data (2026)**

The 2026 datasets contain different reporting coverage:

- THA25: Week 35
- TII03: Week 38

As a result, 2026 totals are not directly compared with complete years unless reporting periods are aligned.

**Missing Observations**

Missing values were preserved as blank rather than converted to zero, preventing unavailable observations from being interpreted as zero passenger activity.

**Overlapping Transport Categories**

Aggregate and component-level categories were excluded from core mode totals to prevent double-counting.

**Passenger Journey Definition**

The data represents passenger journeys rather than unique passengers. Individuals making multiple trips may therefore be counted more than once.

---

## Repository Structure

```text
irish-transport-and-commuting/
│
├── README.md
|
├──01_Raw Data
|      ├── THA25_Public_Transport_Raw.csv
|      ├── TII03.Luas Data Raw.csv
|
├──02_Clean Data
|      ├── IrishTransportAnlysis.csv
|      ├── IrishTransportAnlysis.xlsx
|      ├── THA25_Public_Transport_Clean.xlsx
|      ├── TII03.Luas Data Clean.xlsx
|
├──03_SQL
|      ├── Annual Mode Comparison.sql
|      ├── Average Weekly Journeys.sql
|      ├── Database Structure.png
|      ├── Total Journeys by Transport Mode.sql
|      ├── Total Journeys by Year.sql
|      ├── Year Over Year Analysis.sql
|
├──04_Excel
|      ├── IrishTransportAnlysis.xlsx
|
├──05_Power Bi
|      ├── Irish transport & Commuting.pbix
|      ├── Irish Transport.jpg
|
├──06_Screenshots
       ├── Power Bi - Data Quality.png
       ├── Power Bi - Luas Analysis.png
       ├── Power Bi - Public Transport Overview.png
       ├── Power Bi - Transport Mode Analysis.png
```

---

## Skills Demonstrated

This project demonstrates practical experience in:

- Data cleaning and transformation
- Excel and Power Query
- SQL data analysis
- Data validation and quality assurance
- Data modelling
- DAX measure development
- Power BI dashboard design
- Time-series and trend analysis
- Year-on-year performance analysis
- Handling incomplete datasets
- Communicating analytical findings

---

## Key Learnings

This project reinforced the importance of understanding the structure of a dataset before calculating performance metrics.

A key challenge was identifying overlapping transport categories and designing calculations that prevent double-counting.

The analysis also strengthened my experience working with weekly time-series data, missing observations, partial-year reporting periods and dynamic Power BI measures.

Combining Excel, SQL and Power BI provided practical experience across the full analytics workflow, from raw data preparation to communicating findings through interactive dashboards.

---
**Data source:** 
https://data.gov.ie/dataset/tha25-passenger-journeys-by-public-transport
https://data.gov.ie/dataset/tii03-passenger-journeys-by-luas

## Author

**Arran Farrell**  
Data Analyst

*This project was developed as a portfolio project to demonstrate practical skills in data analysis, SQL, Power BI, data modelling and business intelligence.*
