# FreshCo Foods Inc. — Inventory Management Dashboard
### SQL + Power BI · Cold Storage Inventory Analytics

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=flat&logo=powerbi&logoColor=black)
![SQL](https://img.shields.io/badge/SQL-SQLite-003B57?style=flat&logo=sqlite&logoColor=white)

## Overview
A comprehensive inventory management dashboard for FreshCo Foods Inc., designed to monitor stock levels, analyze expiry risks, and optimize warehouse utilization.

## Business Question
How much inventory value is quietly sitting at expiry risk inside a cold
storage facility — and is the warehouse running out of space before
management even notices?

## Key Findings
| Metric | Value |
|---|---|
| Total Inventory Value | ₱102.15M |
| Confirmed Expired Stock | ₱1.00M (87 pallets still marked Available) |
| Critical At-Risk (0–30 days) | ₱10.45M · 1,004 pallets |
| % Inventory At Risk | 12.23% |
| Warehouses Near Capacity | 3 of 5 (Cold Storage A at 97.28%) |
| Total Pallets | 8,921 · 445,289 boxes |

## Dashboard Pages
| Page | Business Question Answered |
|---|---|
| Executive Overview | What is the current financial state of inventory? |
| Inventory | What do we have, where, and how much is it worth? |
| Expiry | Which pallets are at risk and which SKUs are the problem? |
| Utilization | How full is each warehouse and where is space wasted? |

## Dashboard Screenshots
**Executive Overview** <img width="1628" height="910" alt="image" src="https://github.com/user-attachments/assets/73edc2a9-b329-4ddc-96da-db9b3ffcbad2" />

**Inventory Page** <<img width="1625" height="912" alt="image" src="https://github.com/user-attachments/assets/72b933b6-48a8-43c3-8d98-902a72ed4922" />

**Expiry Page** <img width="1628" height="912" alt="image" src="https://github.com/user-attachments/assets/b82b1473-6385-441b-a596-30d694f6c897" />

**Utilization Page** <img width="1626" height="913" alt="image" src="https://github.com/user-attachments/assets/6fc92508-f034-4ce9-a067-114b9b3a4114" />

## Project Structure
```
freshco-inventory-dashboard/
├── data/
│   ├── inventory_data.csv       # Anonymized dataset (8,921 rows)
│   └── data_dictionary.md       # Column definitions
├── sql/
│   └── dashboard_queries.sql # 15+ analytical queries   
├── screenshots/                 # Dashboard page screenshots
├── dashboard/
│   └── FreshCo_Inventory_Dashboard.pbix
└── README.md
```

## Tools Used
- **SQL (SQLite)** — schema design, data cleaning, analytical queries
- **Power BI Desktop** — data modeling, DAX measures, dashboard design
- **DAX** — custom measures for value at risk, utilization %, expiry KPIs

## DAX Measures Used
```dax
-- % of inventory value at risk
% Inventory At Risk =
DIVIDE(
    CALCULATE(SUM(inventory[TOTAL_VALUE]),
              inventory[EXPIRY_RISK] IN {"1. Expired","2. Critical (0-30d)"}),
    SUM(inventory[TOTAL_VALUE]),
    0
)

-- Warehouses near capacity = COUNTROWS(
    FILTER(
        'Warehouse Utilization',
        'Warehouse Utilization'[Utilization] >= 0.9
)
```

## Data Notes
- Dataset is **fully anonymized** , generated to mirror real cold storage
  WMS operations. No real company data, client names, or identifiable
  information is present.
- 8,921 pallet-level records across 5 warehouses and 17 SKUs
- Snapshot date: June 17, 2026

## Author
**Xyrus Glenn T. Buenaventura**
Industrial Engineer · Inventory Analyst · Aspiring Supply Chain Data Analyst
[LinkedIn](https://www.linkedin.com/in/xyrus-glenn-buenaventura-clssyb-so2-b19a40276/) · [GitHub](https://github.com/Xyreglenn)
