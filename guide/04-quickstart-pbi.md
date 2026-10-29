# 04 — Power BI Quickstart Guide (Connect Directly via Web)

You do **not** need to install SQL Server, restore `.bak` files, or even download these CSV files to your computer. You can connect **Power BI Desktop** (and refresh automatically in the **Power BI Service**) directly from this GitHub repository using the built-in **Web** connector!

---

## 1. Complete 28-Table Raw GitHub URL Directory

Below are the direct Raw URLs for all **28 tables** (`16 Dimensions` + `12 Facts`) so you can ingest the entire Data Warehouse at once:

### A. Dimension Tables (16 Tables)

| Table Name | Rows | Raw GitHub URL (Copy & Paste into Power BI Web Connector) |
| :--- | ---: | :--- |
| **`DimAccount`** | 99 | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/DimAccount.csv` |
| **`DimCurrency`** | 105 | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/DimCurrency.csv` |
| **`DimCustomer`** | 18,484 | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/DimCustomer.csv` |
| **`DimDate`** | 3,652 | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/DimDate.csv` |
| **`DimDepartmentGroup`** | 7 | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/DimDepartmentGroup.csv` |
| **`DimEmployee`** | 296 | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/DimEmployee.csv` |
| **`DimGeography`** | 655 | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/DimGeography.csv` |
| **`DimOrganization`** | 14 | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/DimOrganization.csv` |
| **`DimProduct`** | 606 | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/DimProduct.csv` |
| **`DimProductCategory`** | 4 | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/DimProductCategory.csv` |
| **`DimProductSubcategory`** | 37 | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/DimProductSubcategory.csv` |
| **`DimPromotion`** | 16 | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/DimPromotion.csv` |
| **`DimReseller`** | 701 | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/DimReseller.csv` |
| **`DimSalesReason`** | 10 | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/DimSalesReason.csv` |
| **`DimSalesTerritory`** | 11 | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/DimSalesTerritory.csv` |
| **`DimScenario`** | 3 | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/DimScenario.csv` |

### B. Fact & Analytical Tables (12 Tables)

| Table Name | Rows | Raw GitHub URL (Copy & Paste into Power BI Web Connector) |
| :--- | ---: | :--- |
| **`FactInternetSales`** | 60,398 | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/FactInternetSales.csv` |
| **`FactResellerSales`** | 60,855 | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/FactResellerSales.csv` |
| **`FactInternetSalesReason`** | 64,515 | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/FactInternetSalesReason.csv` |
| **`FactProductInventory`** | 776,286 | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/FactProductInventory.csv` |
| **`FactFinance`** | 39,409 | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/FactFinance.csv` |
| **`FactCurrencyRate`** | 14,264 | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/FactCurrencyRate.csv` |
| **`FactSurveyResponse`** | 2,727 | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/FactSurveyResponse.csv` |
| **`ProspectiveBuyer`** | 2,059 | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/ProspectiveBuyer.csv` |
| **`FactAdditionalInternationalProductDescription`** | 1,911 | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/FactAdditionalInternationalProductDescription.csv` |
| **`FactCallCenter`** | 120 | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/FactCallCenter.csv` |
| **`FactSalesQuota`** | 163 | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/FactSalesQuota.csv` |
| **`NewFactCurrencyRate`** | 50 | `https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/NewFactCurrencyRate.csv` |

---

## 2. Method A: Pro Workflow (Ingest All 28 Tables in Power Query)

Instead of repeating the Web connection wizard 28 times, you can create a reusable Power Query function and a master table catalog to finish the entire ingestion process at once.

### Step 1: Create the Reusable Loader Function (`fxLoadAWTable`)
1. Open **Power Query Editor** (**Home** -> **Transform Data**).
2. Right-click in the **Queries** pane (left sidebar) -> **New Query** -> **Blank Query**.
3. Rename the query to **`fxLoadAWTable`**.
4. Click **Advanced Editor** on the Home ribbon and paste the following M code:

```powerquery
(tableName as text) as table =>
let
    BaseUrl = "https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/",
    Source = Csv.Document(
        Web.Contents(BaseUrl, [RelativePath = tableName & ".csv"]),
        [Delimiter = ",", Encoding = 65001, QuoteStyle = QuoteStyle.None]
    ),
    PromotedHeaders = Table.PromoteHeaders(Source, [PromoteAllScalars = true]),
    TypedTable = Table.TransformColumnTypes(
        PromotedHeaders,
        List.Transform(
            Table.ColumnNames(PromotedHeaders),
            each {_, type any}
        )
    )
in
    TypedTable
```

> **Why use `[RelativePath = ...]`?** Using `RelativePath` inside `Web.Contents()` ensures that when you publish your report to the **Power BI Service (Cloud)**, scheduled refreshes work seamlessly without triggering dynamic data source errors!

### Step 2: Create the 28-Table Catalog (`_TableCatalog`)
1. Right-click in the **Queries** pane -> **New Query** -> **Blank Query**.
2. Rename it to **`_TableCatalog`** (and right-click -> uncheck **Enable load** so it stays strictly as a staging helper in Power Query).
3. Open **Advanced Editor** and paste:

```powerquery
let
    TableNames = {
        "DimAccount", "DimCurrency", "DimCustomer", "DimDate",
        "DimDepartmentGroup", "DimEmployee", "DimGeography", "DimOrganization",
        "DimProduct", "DimProductCategory", "DimProductSubcategory", "DimPromotion",
        "DimReseller", "DimSalesReason", "DimSalesTerritory", "DimScenario",
        "FactAdditionalInternationalProductDescription", "FactCallCenter",
        "FactCurrencyRate", "FactFinance", "FactInternetSales",
        "FactInternetSalesReason", "FactProductInventory", "FactResellerSales",
        "FactSalesQuota", "FactSurveyResponse", "NewFactCurrencyRate",
        "ProspectiveBuyer"
    },
    ToTable = Table.FromList(TableNames, Splitter.SplitByNothing(), {"TableName"}, null, ExtraValues.Error),
    InvokedData = Table.AddColumn(ToTable, "Data", each fxLoadAWTable([TableName]), type table)
in
    InvokedData
```

### Step 3: Materialize the Individual Tables
From `_TableCatalog`, right-click the `[Table]` cell inside the **`Data`** column for each row you want in your semantic model and select **Add as New Query**:
1. Rename the newly created query to its `TableName` (e.g., `DimDate`, `DimProduct`, `FactInternetSales`).
2. Select all columns (`Ctrl + A`) -> **Transform** tab -> **Detect Data Type**.

---

## 3. Method B: Point-and-Click Wizard (No Code Required)

If you prefer loading tables through the standard interface without writing M code:
1. Open **Power BI Desktop**.
2. On the **Home** ribbon, click **Get Data** -> **Web**.
3. Paste any Raw GitHub URL from Section 1 above:
   ```text
   https://raw.githubusercontent.com/vschontag/adventure-works-dw/main/data/FactInternetSales.csv
   ```
4. If prompted for authentication credentials, select **Anonymous** and click **Connect**.
5. In the preview window, verify that:
   - **File Origin**: `65001: Unicode (UTF-8)`
   - **Delimiter**: `Comma`
6. Click **Transform Data** to open Power Query Editor, or **Load** to import immediately.

---

## 4. Essential Post-Load Setup in Power BI

Once your tables are loaded into Power BI Desktop:

### 1. Mark `DimDate` as the Official Date Table
1. Switch to **Table View** or **Model View** and select `DimDate`.
2. Right-click `DimDate` -> **Mark as date table** -> select column **`FullDateAlternateKey`**.
3. Sort month names chronologically: select `EnglishMonthName` -> **Sort by column** -> **`MonthNumberOfYear`**.

### 2. Configure Star Schema Relationships (`Model View`)
Power BI's Auto-Detect may try to create unintended relationships. Verify your relationships match **[03 — Star Schema Guide](./03-star-schema.md)**:
- Connect `DimDate[DateKey]` -> `FactInternetSales[OrderDateKey]` (**Active**, `1:*`)
- Connect `DimDate[DateKey]` -> `FactResellerSales[OrderDateKey]` (**Active**, `1:*`)
- Connect `DimProduct[ProductKey]`, `DimCustomer[CustomerKey]`, `DimReseller[ResellerKey]`, and `DimSalesTerritory[SalesTerritoryKey]` to their respective Fact tables.

---

## Navigation
- Back to **[Repository Home (`README.md`)](../README.md)**
- Review **[01 — Business Overview](./01-overview.md)**
- Browse **[02 — Data Dictionary](./02-data-dictionary.md)**
- Inspect **[03 — Star Schema & Relationships](./03-star-schema.md)**
