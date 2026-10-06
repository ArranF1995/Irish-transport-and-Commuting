# Irish Public Transport Trends & Analysis

An end-to-end data analytics project exploring passenger journey trends across Ireland's major public transport modes, including **Bus, Rail and Luas**.

The project uses **Excel, Power Query, SQL, DAX and Power BI** to clean, combine, analyse and visualise weekly passenger journey data from official Irish transport datasets.

> **Data coverage:** 2019–2026 YTD  
> **Note:** 2026 is a partial year and is treated separately from complete-year comparisons.

---

## Project Overview

Public transport passenger data is available across multiple official datasets and contains both aggregate totals and component-level categories.

The aim of this project was to create a consistent analytical dataset and answer:

- How have passenger journeys changed over time?
- How do passenger volumes differ between transport modes?
- Which modes have experienced the strongest growth and recovery?
- How did passenger activity change following the decline in 2020–2021?
- How do missing values and partial-year data affect comparisons?

The final output is an interactive multi-page **Power BI dashboard** supported by SQL analysis, Excel validation and documented data-cleaning steps.

---

## Tools & Technologies

| Tool | Usage |
|---|---|
| **Excel** | Initial data inspection, validation and pivot analysis |
| **Power Query** | Data cleaning, transformation and dataset combination |
| **SQL** | Data-quality checks and analytical queries |
| **Power BI** | Data modelling and dashboard development |
| **DAX** | KPIs, YoY calculations, growth measures and dynamic reporting |

---

## Data Sources

The project uses two official Irish public transport datasets:

### THA25 — Passenger Journeys by Public Transport

Published by the **National Transport Authority (NTA)**.

Contains weekly passenger journey data for:

- Dublin Metro Bus
- Bus, excluding Dublin Metro
- Rail
- All public transport, excluding Luas

### TII03 — Passenger Journeys by Luas

Published by **Transport Infrastructure Ireland (TII)**.

Contains weekly passenger journey data for:

- Red Line
- Green Line
- All Luas Lines

The two datasets were standardised and combined into a single analytical dataset containing **2,831 records**.

---

## Data Preparation

The raw datasets were preserved unchanged before cleaning.

Using **Excel and Power Query**, I:

1. Standardised column names across both datasets.
2. Extracted reporting **Year** and **Week Number**.
3. Added a **Data Status** field to distinguish available and missing observations.
4. Added a **Source** field to retain dataset lineage.
5. Standardised transport-mode naming.
6. Appended THA25 and TII03 into one analytical table.
7. Preserved blank observations as missing rather than converting them to zero.
8. Separated core transport modes from aggregate and component-level categories.

The resulting structure supports analysis across:

`Year → Week → Transport Mode → Passenger Journeys`

---

## Preventing Double-Counting

One of the most important data-modelling challenges in this project was identifying overlapping categories.

The source datasets contain both **aggregate totals and their underlying components**.

For example:

```text
Public Transport
│
├── Dublin Metro Bus
├── Bus, excluding Dublin Metro
├── Rail
│
├── All Public Transport, excluding Luas  ← Aggregate
│
└── Luas                                  ← Network total
    ├── Red Line                          ← Component
    └── Green Line                        ← Component
```

Adding every category together would therefore significantly overstate passenger journeys.

For the main mode-level analysis, I use the following non-overlapping categories:

- Dublin Metro Bus
- Bus, excluding Dublin Metro
- Rail
- Luas

The following are analysed separately:

- `All public transport, excluding LUAS` — aggregate category
- `Red line` — component of Luas
- `Green line` — component of Luas

This classification prevents aggregate and component records from being counted more than once.

---

## Key Findings

### Bus Outside Dublin

Passenger journeys for **Bus, excluding Dublin Metro** increased by approximately **35.8% between 2019 and 2025**, representing the strongest growth among modes with comparable 2019 data.

### Dublin Metro Bus

Dublin Metro Bus passenger journeys increased from approximately **151.7 million in 2019** to **176.9 million in 2025**, an increase of approximately **16.6%**.

### Luas

Luas passenger journeys increased from approximately **48.1 million in 2019** to approximately **55.0 million in 2025**, an increase of approximately **14.2%**.

### Recovery

Passenger activity declined sharply during **2020 and 2021**, followed by a strong recovery beginning in **2022**.

For the four core transport modes, total passenger journeys reached approximately:

| Year | Core Passenger Journeys |
|---:|---:|
| 2019 | 236.7M* |
| 2020 | 135.7M |
| 2021 | 136.0M |
| 2022 | 241.6M |
| 2023 | 298.4M |
| 2024 | 327.6M |
| 2025 | 326.5M |

\* Rail passenger journey values are unavailable for 2019, so the 2019 total is incomplete.

---

## 2025 Mode Comparison

For the latest complete year in the analysis:

| Transport Mode | Passenger Journeys |
|---|---:|
| Dublin Metro Bus | 176.9M |
| Luas | 55.0M |
| Bus, excluding Dublin Metro | 50.1M |
| Rail | 44.5M |
| **Total** | **326.5M** |

Dublin Metro Bus accounted for the largest share of passenger journeys among the four core modes.

---

## Power BI Dashboard

The Power BI report is organised into four analytical pages.

### 1. Public Transport Overview

Provides a high-level view of passenger activity, including:

- Passenger journey KPIs
- Annual trends
- Year-on-year change
- Transport mode comparison
- Key observations

### 2. Luas Analysis

Focuses specifically on Luas performance:

- Annual Luas journeys
- Weekly passenger trends
- Red vs Green Line comparison
- Year-on-year performance
- Weekly seasonality

### 3. Transport Mode Analysis

Compares the four core transport modes using:

- Passenger journeys by year
- Mode share
- Period growth
- Recovery analysis
- Dynamic year selection

### 4. Data & Methodology

Documents the analytical assumptions behind the dashboard:

- Dataset coverage
- Missing observations
- Data sources
- Aggregation rules
- Partial-year considerations
- Methodology

---

## Example DAX

A core passenger journey measure was created to prevent overlapping transport categories from being included in totals:

```DAX
Core Passenger Journeys =
CALCULATE(
    SUM('Public Transport Combined'[Passenger Journeys]),
    'Public Transport Combined'[Transport Mode] IN {
        "Dublin Metro Bus",
        "Bus, excluding Dublin Metro",
        "Rail",
        "Luas"
    }
)
```

---

## Example SQL

SQL was used to analyse annual passenger journeys by transport mode:

```sql
SELECT
    Year,
    [Transport Mode],
    SUM([Passenger Journeys]) AS Annual_Journeys
FROM fact_passenger_journeys
WHERE [Transport Level] = 'Core Mode'
GROUP BY
    Year,
    [Transport Mode]
ORDER BY
    Year,
    Annual_Journeys DESC;
```

Additional SQL analysis covers:

- Data-quality checks
- Annual passenger trends
- Year-on-year analysis
- Transport mode comparisons
- Weekly seasonality

---

## Data Quality & Limitations

### Rail 2019

Rail passenger journey values are unavailable for 2019.

Rail is therefore excluded from calculations requiring a 2019 baseline.

### 2026 Partial-Year Data

2026 is incomplete:

- THA25 extends to **Week 35**
- TII03 extends to **Week 38**

For this reason, 2026 totals are not directly compared with complete calendar years without aligning the reporting periods.

### Missing Values

Blank observations are treated as **missing**, not zero.

This prevents unavailable data from being incorrectly interpreted as zero passenger activity.

### Passenger Journeys

The dataset measures **passenger journeys**, not unique passengers.

One individual making multiple trips can therefore contribute multiple passenger journeys.

---

## Repository Structure

```text
irish-public-transport-analysis/
│
├── README.md
│
├── data/
│   ├── raw/
│   └── cleaned/
│
├── sql/
│   ├── 01_create_database.sql
│   ├── 02_create_tables.sql
│   ├── 03_load_data.sql
│   ├── 04_data_quality.sql
│   ├── 05_annual_analysis.sql
│   ├── 06_yoy_analysis.sql
│   └── 07_seasonality.sql
│
├── excel/
│   └── Irish_Transport_Analysis.xlsx
│
├── powerbi/
│   ├── Irish_Transport_Dashboard.pbix
│   └── screenshots/
│
└── documentation/
    ├── data_dictionary.md
    └── methodology.md
```

---

## Skills Demonstrated

This project demonstrates practical experience with:

- Data cleaning and transformation
- Power Query
- SQL querying
- Excel analysis
- Data validation
- Data modelling
- DAX
- Time-series analysis
- Year-on-year analysis
- Data-quality assessment
- Power BI dashboard design
- Communicating analytical findings

---

## Key Learning

A major takeaway from this project was the importance of understanding the **structure and meaning of a dataset before calculating KPIs**.

Identifying the relationship between aggregate totals and their underlying components was essential to preventing double-counting.

The project also provided practical experience working with missing observations, incomplete reporting periods, weekly time-series data and dynamic DAX measures.

---

## Author

**Arran Farrell**

Data Analyst



*This project was created as a portfolio project to demonstrate data analysis, data modelling and business intelligence skills.*