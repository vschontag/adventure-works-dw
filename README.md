# AdventureWorks Data Warehouse (`AdventureWorksDW`) — Clean UTF-8 CSV Dataset & Kimball Modeling Guide

[![Dataset: AdventureWorksDW](https://img.shields.io/badge/Dataset-AdventureWorksDW-0078D4?style=for-the-badge&logo=data:image/svg%2Bxml;base64,PHN2ZyB4bWxucz0naHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnIHZpZXdCb3g9JzAgMCAyNCAyNCcgZmlsbD0nd2hpdGUnPjxwYXRoIGQ9J00xMiAzQzcuNTggMyA0IDQuNzkgNCA3czMuNTggNCA4IDQgOC0xLjc5IDgtNC0zLjU4LTQtOC00TTQgOXYzYzAgMi4yMSAzLjU4IDQgOCA0czgtMS43OSA4LTRWOWMwIDIuMjEtMy41OCA0LTggNHMtOC0xLjc5LTgtNG0wIDV2M2MwIDIuMjEgMy41OCA0IDggNHM4LTEuNzkgOC00di0zYzAgMi4yMS0zLjU4IDQtOCA0cy04LTEuNzktOC00eicvPjwvc3ZnPg%3D%3D)](./data/)
[![Format: UTF--8 CSV](https://img.shields.io/badge/Format-UTF--8%20CSV%20(With%20Headers)-2EA44F?style=for-the-badge&logo=files&logoColor=white)](./guide/02-data-dictionary.md)
[![Power BI: Web Ready](https://img.shields.io/badge/Power%20BI-Web%20Connector%20Ready-F2C811?style=for-the-badge&logo=data:image/svg%2Bxml;base64,PHN2ZyB4bWxucz0naHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnIHZpZXdCb3g9JzAgMCAyNCAyNCcgZmlsbD0nYmxhY2snPjxwYXRoIGQ9J00xNiAzYTIgMiAwIDAgMSAyIDJ2MTRhMiAyIDAgMCAxLTIgMmgtMlYzaDJ6bS00IDVhMiAyIDAgMCAxIDIgMnYxMWgtNFYxMGEyIDIgMCAwIDEgMi0yem0tNiA1YTIgMiAwIDAgMSAyIDJ2Nkg0di02YTIgMiAwIDAgMSAyLTJ6Jy8%2BPC9zdmc%2B)](./guide/04-quickstart-pbi.md)
[![LinkedIn: Vinicius Schöntag](https://img.shields.io/badge/LinkedIn-Vinicius%20Sch%C3%B6ntag-0A66C2?style=for-the-badge&logo=data:image/svg%2Bxml;base64,PHN2ZyB4bWxucz0naHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnIHZpZXdCb3g9JzAgMCAyNCAyNCcgZmlsbD0nd2hpdGUnPjxwYXRoIGQ9J00xOSAzYTIgMiAwIDAgMSAyIDJ2MTRhMiAyIDAgMCAxLTIgMkg1YTIgMiAwIDAgMS0yLTJWNWEyIDIgMCAwIDEgMi0yaDE0bS0uNSAxNS41di01LjNhMy4yNiAzLjI2IDAgMCAwLTMuMjYtMy4yNmMtLjg1IDAtMS44NC41Mi0yLjI4IDEuM3YtMS4xMWgtMi43OXY4LjM3aDIuNzl2LTQuOTNjMC0uNzcuNjItMS40IDEuMzktMS40YTEuNCAxLjQgMCAwIDEgMS40IDEuNHY0LjkzaDIuNzVNNi44OCA4LjU2YTEuNjggMS42OCAwIDAgMCAxLjY4LTEuNjhjMC0uOTMtLjc1LTEuNjktMS42OC0xLjY5YTEuNjkgMS42OSAwIDAgMC0xLjY5IDEuNjljMCAuOTMuNzYgMS42OCAxLjY5IDEuNjhtMS4zOSA5Ljk0di04LjM3SDUuNXY4LjM3aDIuNzd6Jy8%2BPC9zdmc%2B)](https://www.linkedin.com/in/vinicius-schontag/)
[![GitHub: @vschontag](https://img.shields.io/badge/GitHub-@vschontag-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/vschontag)

---

## Why This Repository Exists

**AdventureWorks** is the most famous benchmark dataset in the Microsoft Business Intelligence ecosystem. However, if you visit Microsoft's official `sql-server-samples` repository to practice **Power BI**, **Excel**, **SQL**, or **Python**, you immediately hit an unnecessary technical barrier:

1. **Binary `.bak` Backups**: Require installing a local SQL Server instance and SQL Server Management Studio (SSMS) just to restore the tables.
2. **Headerless Pipe-Delimited Dumps**: Microsoft's raw `.csv` files are SQL Server `BULK INSERT` dumps that **lack column headers** (loading as `Column1, Column2, ...`), use pipe delimiters (`|`), and contain raw `bcp` NUL bytes (`\x00`).

### What This Repository Gives You
This repository provides **all 28 analytical tables (`1,047,457` rows)** of `AdventureWorksDW` ready for immediate analysis:
- **Official Column Headers Injected**: Extracted directly from Microsoft's `instawdbdw.sql` DDL script.
- **Clean Standard CSVs**: Comma-delimited (`,`), pure `UTF-8` encoding, and stripped of `bcp` NUL byte artifacts.
- **Zero Local Setup**: Connect **Power BI Desktop**, **Power BI Service**, **DuckDB**, **Python**, **Excel** or other tools directly via GitHub Raw URLs in seconds.
- **Beginner-Friendly Documentation**: Human-readable guides explaining the business context, data dictionary, and Star Schema relationships without needing SQL Server or AI tools.

---

## Step-by-Step Learning & Modeling Guides (`guide/`)

| Guide | What You Will Learn |
| :--- | :--- |
| **[01 — Business Overview](./guide/01-overview.md)** | Who **Adventure Works Cycles** is, how its B2C (`FactInternetSales`) and B2B (`FactResellerSales`) channels operate, and how Inventory and Corporate Finance fit together. |
| **[02 — Complete Data Dictionary](./guide/02-data-dictionary.md)** | Full schema reference for all **28 tables**, including row counts, Primary Keys (`PK`), Foreign Keys (`FK`), SQL data types, and sample values. |
| **[03 — Star Schema & Dimensional Modeling](./guide/03-star-schema.md)** | Visual Mermaid diagrams, complete Foreign Key mapping, role-playing `DimDate` relationships (`OrderDateKey`, `DueDateKey`, `ShipDateKey`), and Power BI modeling best practices. |
| **[04 — Power BI Quickstart (Web Connector)](./guide/04-quickstart-pbi.md)** | Copy-and-paste tutorial to connect Power BI directly to GitHub via Web (Point-and-Click or reusable Power Query M function `fxLoadAWTable`) + starter DAX measures. |

---

## Star Schema Architecture at a Glance

![AdventureWorksDW Star Schema Architecture](./guide/assets/star-schema.svg)

*(See **[03 — Star Schema Guide](./guide/03-star-schema.md)** for the complete relationship matrix, including `FactFinance`, `FactProductInventory`, `FactSalesQuota`, and `FactCurrencyRate`.)*

---

## Complete Dataset Directory (`data/`)

Click **Browse CSV** to view the file on GitHub, or copy the **Raw URL** (`https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/<TableName>.csv`):

### 1. Dimension Tables (16 Tables)

| Table Name | Rows | Cols | Primary Key (`PK`) | Browse CSV | Direct Raw URL |
| :--- | ---: | ---: | :--- | :---: | :--- |
| **`DimAccount`** | 99 | 10 | `AccountKey` | [CSV](./data/DimAccount.csv) | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/DimAccount.csv` |
| **`DimCurrency`** | 105 | 3 | `CurrencyKey` | [CSV](./data/DimCurrency.csv) | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/DimCurrency.csv` |
| **`DimCustomer`** | 18,484 | 29 | `CustomerKey` | [CSV](./data/DimCustomer.csv) | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/DimCustomer.csv` |
| **`DimDate`** | 3,652 | 19 | `DateKey` | [CSV](./data/DimDate.csv) | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/DimDate.csv` |
| **`DimDepartmentGroup`** | 7 | 3 | `DepartmentGroupKey` | [CSV](./data/DimDepartmentGroup.csv) | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/DimDepartmentGroup.csv` |
| **`DimEmployee`** | 296 | 31 | `EmployeeKey` | [CSV](./data/DimEmployee.csv) | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/DimEmployee.csv` |
| **`DimGeography`** | 655 | 11 | `GeographyKey` | [CSV](./data/DimGeography.csv) | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/DimGeography.csv` |
| **`DimOrganization`** | 14 | 5 | `OrganizationKey` | [CSV](./data/DimOrganization.csv) | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/DimOrganization.csv` |
| **`DimProduct`** | 606 | 36 | `ProductKey` | [CSV](./data/DimProduct.csv) | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/DimProduct.csv` |
| **`DimProductCategory`** | 4 | 5 | `ProductCategoryKey` | [CSV](./data/DimProductCategory.csv) | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/DimProductCategory.csv` |
| **`DimProductSubcategory`** | 37 | 6 | `ProductSubcategoryKey` | [CSV](./data/DimProductSubcategory.csv) | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/DimProductSubcategory.csv` |
| **`DimPromotion`** | 16 | 16 | `PromotionKey` | [CSV](./data/DimPromotion.csv) | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/DimPromotion.csv` |
| **`DimReseller`** | 701 | 20 | `ResellerKey` | [CSV](./data/DimReseller.csv) | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/DimReseller.csv` |
| **`DimSalesReason`** | 10 | 4 | `SalesReasonKey` | [CSV](./data/DimSalesReason.csv) | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/DimSalesReason.csv` |
| **`DimSalesTerritory`** | 11 | 6 | `SalesTerritoryKey` | [CSV](./data/DimSalesTerritory.csv) | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/DimSalesTerritory.csv` |
| **`DimScenario`** | 3 | 2 | `ScenarioKey` | [CSV](./data/DimScenario.csv) | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/DimScenario.csv` |

### 2. Fact & Analytical Tables (12 Tables)

| Table Name | Rows | Cols | Primary Key (`PK`) | Browse CSV | Direct Raw URL |
| :--- | ---: | ---: | :--- | :---: | :--- |
| **`FactInternetSales`** | 60,398 | 26 | `SalesOrderNumber`, `SalesOrderLineNumber` | [CSV](./data/FactInternetSales.csv) | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/FactInternetSales.csv` |
| **`FactResellerSales`** | 60,855 | 27 | `SalesOrderNumber`, `SalesOrderLineNumber` | [CSV](./data/FactResellerSales.csv) | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/FactResellerSales.csv` |
| **`FactInternetSalesReason`** | 64,515 | 3 | `SalesOrderNumber`, `SalesOrderLineNumber`, `SalesReasonKey` | [CSV](./data/FactInternetSalesReason.csv) | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/FactInternetSalesReason.csv` |
| **`FactProductInventory`** | 776,286 | 7 | `ProductKey`, `DateKey` | [CSV](./data/FactProductInventory.csv) | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/FactProductInventory.csv` |
| **`FactFinance`** | 39,409 | 8 | `FinanceKey` | [CSV](./data/FactFinance.csv) | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/FactFinance.csv` |
| **`FactCurrencyRate`** | 14,264 | 5 | `CurrencyKey`, `DateKey` | [CSV](./data/FactCurrencyRate.csv) | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/FactCurrencyRate.csv` |
| **`FactSurveyResponse`** | 2,727 | 8 | `SurveyResponseKey` | [CSV](./data/FactSurveyResponse.csv) | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/FactSurveyResponse.csv` |
| **`ProspectiveBuyer`** | 2,059 | 24 | `ProspectiveBuyerKey` | [CSV](./data/ProspectiveBuyer.csv) | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/ProspectiveBuyer.csv` |
| **`FactAdditionalInternationalProductDescription`** | 1,911 | 3 | `ProductKey`, `CultureName` | [CSV](./data/FactAdditionalInternationalProductDescription.csv) | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/FactAdditionalInternationalProductDescription.csv` |
| **`FactCallCenter`** | 120 | 14 | `FactCallCenterID` | [CSV](./data/FactCallCenter.csv) | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/FactCallCenter.csv` |
| **`FactSalesQuota`** | 163 | 7 | `SalesQuotaKey` | [CSV](./data/FactSalesQuota.csv) | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/FactSalesQuota.csv` |
| **`NewFactCurrencyRate`** | 50 | 6 | `CurrencyKey`, `DateKey` | [CSV](./data/NewFactCurrencyRate.csv) | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/NewFactCurrencyRate.csv` |

---

## Quick Connect in Python / Pandas / DuckDB

```python
import pandas as pd
import duckdb

BASE_URL = "https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/"

TABLES = [
    # Dimensions (16)
    "DimAccount", "DimCurrency", "DimCustomer", "DimDate",
    "DimDepartmentGroup", "DimEmployee", "DimGeography", "DimOrganization",
    "DimProduct", "DimProductCategory", "DimProductSubcategory", "DimPromotion",
    "DimReseller", "DimSalesReason", "DimSalesTerritory", "DimScenario",
    # Facts & Analytical Tables (12)
    "FactAdditionalInternationalProductDescription", "FactCallCenter",
    "FactCurrencyRate", "FactFinance", "FactInternetSales",
    "FactInternetSalesReason", "FactProductInventory", "FactResellerSales",
    "FactSalesQuota", "FactSurveyResponse", "NewFactCurrencyRate",
    "ProspectiveBuyer",
]

# Option A: Load all 28 tables as individual Pandas DataFrames
dfs = {table: pd.read_csv(f"{BASE_URL}{table}.csv") for table in TABLES}

# Option B: Register all 28 tables as individual tables in DuckDB
con = duckdb.connect()
for table in TABLES:
    con.execute(
        f"CREATE OR REPLACE VIEW {table} AS "
        f"SELECT * FROM read_csv_auto('{BASE_URL}{table}.csv')"
    )
```

---

## About the Author

Curated and documented by **Vinicius Schöntag** — Senior BI Professional & Power BI Solutions Architect specializing in enterprise data modeling, DAX performance optimization, and end-to-end BI architecture.

- **LinkedIn**: [linkedin.com/in/vinicius-schontag](https://www.linkedin.com/in/vinicius-schontag/)
- **GitHub**: [github.com/vschontag](https://github.com/vschontag)

---

## License & Upstream Attribution

The underlying `AdventureWorksDW` sample data is copyright Microsoft Corporation and distributed under the [MIT License](https://github.com/microsoft/sql-server-samples/blob/master/license.txt) via [`microsoft/sql-server-samples`](https://github.com/microsoft/sql-server-samples/tree/master/samples/databases/adventure-works/data-warehouse-install-script). Data engineering cleanup, header injection, and modeling guides in this repository are released under the MIT License for the global BI & analytics community.

