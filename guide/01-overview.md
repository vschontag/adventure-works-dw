# 01 — Business Overview: Adventure Works Cycles (`AdventureWorksDW`)

Welcome to the **AdventureWorks Data Warehouse (`AdventureWorksDW`)** guide! If you are learning Power BI, SQL, dimensional modeling, or business analytics, this document explains **what business you are analyzing** before you write your first formula or build your first chart.

---

## 1. Who is Adventure Works Cycles?

**Adventure Works Cycles** is a fictional multinational manufacturing company created by Microsoft as the standard benchmark dataset for Business Intelligence and Data Warehousing.

The company designs, manufactures, and sells **metal and composite bicycles** (Mountain, Road, and Touring bikes), along with **bicycle components** (frames, wheels, pedals, brakes), **cycling apparel** (jerseys, shorts, gloves), and **accessories** (helmets, water bottles, locks, pumps) across **North America, Europe, and the Pacific (Australia)**.

---

## 2. Core Business Channels & Processes

The data warehouse captures five interconnected areas of the company's global operations:

### A. Two Sales Channels: Online (B2C) vs. Wholesale Resellers (B2B)
Adventure Works sells its products through two distinct revenue streams:

| Channel | Fact Table | Customer Entity | How It Works |
| :--- | :--- | :--- | :--- |
| **Online Retail (B2C)** | [`FactInternetSales`](../data/FactInternetSales.csv) | [`DimCustomer`](../data/DimCustomer.csv) (`18,484` individual consumers) | Individual consumers order bikes, clothing, and accessories directly through the Adventure Works e-commerce website. Also links to [`FactInternetSalesReason`](../data/FactInternetSalesReason.csv) to track why customers bought (Price, Quality, Review, Promotion). |
| **Wholesale Distribution (B2B)** | [`FactResellerSales`](../data/FactResellerSales.csv) | [`DimReseller`](../data/DimReseller.csv) (`701` retail bike shops & distributors) | Physical bike shops and wholesale distributors place bulk orders handled by Adventure Works' internal sales representatives ([`DimEmployee`](../data/DimEmployee.csv)). |

> **Key Analytical Insight**: Because both `FactInternetSales` and `FactResellerSales` share the exact same **conformed dimensions** (`DimProduct`, `DimDate`, `DimSalesTerritory`, `DimPromotion`, and `DimCurrency`), you can analyze B2C and B2B revenue side-by-side or combine them into a unified **Total Global Revenue** model.

---

### B. Product Catalog Hierarchy
Products are organized into a 3-level hierarchy so you can drill down from high-level business lines to individual SKUs:
1. **[`DimProductCategory`](../data/DimProductCategory.csv)** (4 categories): *Bikes*, *Components*, *Clothing*, *Accessories*.
2. **[`DimProductSubcategory`](../data/DimProductSubcategory.csv)** (37 subcategories): e.g., *Mountain Bikes*, *Road Bikes*, *Touring Bikes*, *Helmets*, *Jerseys*.
3. **[`DimProduct`](../data/DimProduct.csv)** (606 individual products/SKUs): Tracks unit costs (`StandardCost`), catalog prices (`ListPrice`), dealer prices (`DealerPrice`), colors, sizes, weights, and product lifecycle dates (`StartDate`, `EndDate`, `Status`).

---

### C. Supply Chain & Warehouse Inventory
- **[`FactProductInventory`](../data/FactProductInventory.csv)** (`776,286` rows): Daily snapshot of warehouse stock movements (`UnitsIn`, `UnitsOut`, `UnitsBalance`, and `UnitCost`) per product and date. Ideal for practicing **semi-additive DAX measures** (such as end-of-period inventory balance using `LASTDATE` or `LASTNONBLANK`).

---

### D. Corporate Finance, Sales Quotas & Currency Exchange
- **General Ledger & Budgeting ([`FactFinance`](../data/FactFinance.csv))**: Tracks `39,409` financial entries across corporate subsidiaries ([`DimOrganization`](../data/DimOrganization.csv)), departments ([`DimDepartmentGroup`](../data/DimDepartmentGroup.csv)), chart of accounts ([`DimAccount`](../data/DimAccount.csv)), and scenarios ([`DimScenario`](../data/DimScenario.csv): *Actual*, *Budget*, *Forecast*).
- **Sales Force Quotas ([`FactSalesQuota`](../data/FactSalesQuota.csv))**: Quarterly revenue targets (`SalesAmountQuota`) assigned to sales representatives ([`DimEmployee`](../data/DimEmployee.csv)).
- **Multi-Currency Rates ([`FactCurrencyRate`](../data/FactCurrencyRate.csv))**: Daily `AverageRate` and `EndOfDayRate` conversions across currencies ([`DimCurrency`](../data/DimCurrency.csv)).

---

### E. Operations & Marketing Telemetry
- **Call Center Operations ([`FactCallCenter`](../data/FactCallCenter.csv))**: Tracks daily shift metrics (`Calls`, `AutomaticResponses`, `Orders`, `IssuesRaised`, `AverageTimePerIssue`, `ServiceGrade`).
- **Customer Surveys & Lead Generation**: [`FactSurveyResponse`](../data/FactSurveyResponse.csv) and [`ProspectiveBuyer`](../data/ProspectiveBuyer.csv) provide customer product survey responses and prospective marketing leads.

---

## 3. Why This Repository Exists

Although `AdventureWorksDW` is the most widely taught dataset in the Microsoft BI ecosystem, Microsoft's official repository only provides:
1. Binary SQL Server backup files (`.bak`) that require installing a local SQL Server instance and SQL Server Management Studio (SSMS) to restore, **or**
2. Raw installation scripts (`instawdbdw.sql`) paired with headerless, pipe-delimited (`|`) bulk dump files containing SQL Server `bcp` NUL bytes (`\x00`).

This repository removes all setup friction by providing **all 28 analytical tables**:
- Converted to **standard comma-delimited (`,`) CSV format**
- Encoded in clean **`UTF-8`**
- Enriched with **official column headers** extracted directly from Microsoft's DDL schema
- Ready to connect directly into **Power BI Desktop, Excel, Python, R, or DuckDB** via raw GitHub URLs in seconds.

---

## Next Steps

- **[02 — Data Dictionary](./02-data-dictionary.md)**: Browse all 28 tables, row counts, primary keys, and column definitions.
- **[03 — Star Schema & Dimensional Modeling Guide](./03-star-schema.md)**: Understand how Facts and Dimensions connect (including active vs. inactive role-playing relationships and snowflake chains).
- **[04 — Power BI Quickstart Guide](./04-quickstart-pbi.md)**: Step-by-step tutorial to load these tables directly from GitHub into Power BI via the Web connector.

