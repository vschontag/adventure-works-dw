# 02 — Complete Data Dictionary (AdventureWorksDW)

This reference documents all **28 tables** (1,047,457 total rows) included in [data/](../data/), including row counts, Primary Keys (PK), Foreign Keys (FK), SQL Server data types, nullability, and sample values.

---

## 1. Master Table Summary

| # | Table Name | Role | Rows | Columns | Primary Key (PK) | Size (KB) | Business Description |
| :---: | :--- | :--- | ---: | ---: | :--- | ---: | :--- |
| 1 | [DimAccount](#dimaccount) | Dimension | 99 | 10 | AccountKey | 5.7 | Chart of accounts hierarchy (Balance Sheet and Income Statement accounts) used in financial reporting. |
| 2 | [DimCurrency](#dimcurrency) | Dimension | 105 | 3 | CurrencyKey | 2.2 | ISO currency codes and names used across transactions, organizations, and exchange rates. |
| 3 | [DimCustomer](#dimcustomer) | Dimension | 18,484 | 29 | CustomerKey | 4,379.3 | Individual online retail (B2C) customers, including demographics, income, education, and household profile. |
| 4 | [DimDate](#dimdate) | Dimension | 3,652 | 19 | DateKey | 349.1 | Master calendar and fiscal date dimension (2005–2014) with multi-language month/day labels. |
| 5 | [DimDepartmentGroup](#dimdepartmentgroup) | Dimension | 7 | 3 | DepartmentGroupKey | 0.2 | Corporate department hierarchy used to slice financial actuals and budgets. |
| 6 | [DimEmployee](#dimemployee) | Dimension | 296 | 31 | EmployeeKey | 9,066.0 | Internal employees and sales representatives, including organizational parent-child hierarchy (ParentEmployeeKey). |
| 7 | [DimGeography](#dimgeography) | Dimension | 655 | 11 | GeographyKey | 55.9 | Cities, states/provinces, postal codes, and countries mapped to sales territories. |
| 8 | [DimOrganization](#dimorganization) | Dimension | 14 | 5 | OrganizationKey | 0.5 | Corporate legal/regional subsidiaries, ownership percentages, and reporting currencies. |
| 9 | [DimProduct](#dimproduct) | Dimension | 606 | 36 | ProductKey | 9,090.0 | Master product catalog (SKUs) with SCD Type 2 history, standard costs, list prices, dealer prices, and physical attributes. |
| 10 | [DimProductCategory](#dimproductcategory) | Dimension | 4 | 5 | ProductCategoryKey | 0.3 | Top-level product classification (*Bikes*, *Components*, *Clothing*, *Accessories*). |
| 11 | [DimProductSubcategory](#dimproductsubcategory) | Dimension | 37 | 6 | ProductSubcategoryKey | 1.7 | Second-level product classification (37 subcategories linked to DimProductCategory). |
| 12 | [DimPromotion](#dimpromotion) | Dimension | 16 | 16 | PromotionKey | 4.2 | Marketing promotions and discount campaigns with start/end dates and quantity thresholds. |
| 13 | [DimReseller](#dimreseller) | Dimension | 701 | 20 | ResellerKey | 117.5 | B2B wholesale bike shops, specialty stores, and distributors purchasing through sales representatives. |
| 14 | [DimSalesReason](#dimsalesreason) | Dimension | 10 | 4 | SalesReasonKey | 0.3 | Customer purchase motivations (*Price*, *Quality*, *Review*, *Manufacturer*, *On Promotion*). |
| 15 | [DimSalesTerritory](#dimsalesterritory) | Dimension | 11 | 6 | SalesTerritoryKey | 858.0 | Global commercial sales regions, countries, and macro-groups (*North America*, *Europe*, *Pacific*). |
| 16 | [DimScenario](#dimscenario) | Dimension | 3 | 2 | ScenarioKey | 0.1 | Financial planning scenarios (*Actual*, *Budget*, *Forecast*). |
| 17 | [FactAdditionalInternationalProductDescription](#factadditionalinternationalproductdescription) | Fact / Analytical | 1,911 | 3 | ProductKey, CultureName | 275.1 | Localized product descriptions across international cultures/languages. |
| 18 | [FactCallCenter](#factcallcenter) | Fact / Analytical | 120 | 14 | FactCallCenterID | 10.9 | Daily shift-level call center telemetry (call volume, orders, issues raised, service grade). |
| 19 | [FactCurrencyRate](#factcurrencyrate) | Fact / Analytical | 14,264 | 5 | CurrencyKey, DateKey | 1,025.4 | Daily average and end-of-day currency exchange rates relative to USD. |
| 20 | [FactFinance](#factfinance) | Fact / Analytical | 39,409 | 8 | FinanceKey | 2,251.2 | General Ledger financial entries by date, organization, department, account, and scenario. |
| 21 | [FactInternetSales](#factinternetsales) | Fact / Analytical | 60,398 | 26 | SalesOrderNumber, SalesOrderLineNumber | 11,511.4 | Direct-to-consumer (B2C) online sales order lines (60,398 rows). |
| 22 | [FactInternetSalesReason](#factinternetsalesreason) | Bridge Fact | 64,515 | 3 | SalesOrderNumber, SalesOrderLineNumber, SalesReasonKey | 822.7 | Many-to-many bridge table linking online sales order lines (FactInternetSales) to purchase reasons (DimSalesReason). |
| 23 | [FactProductInventory](#factproductinventory) | Fact / Analytical | 776,286 | 7 | ProductKey, DateKey | 30,348.5 | Daily warehouse stock movements (UnitsIn, UnitsOut, UnitsBalance, UnitCost) by product and date (776,286 rows). |
| 24 | [FactResellerSales](#factresellersales) | Fact / Analytical | 60,855 | 27 | SalesOrderNumber, SalesOrderLineNumber | 13,685.8 | Business-to-business (B2B) wholesale order lines placed by resellers (60,855 rows). |
| 25 | [FactSalesQuota](#factsalesquota) | Fact / Analytical | 163 | 7 | SalesQuotaKey | 9.7 | Quarterly sales revenue quotas assigned to sales employees. |
| 26 | [FactSurveyResponse](#factsurveyresponse) | Fact / Analytical | 2,727 | 8 | SurveyResponseKey | 184.2 | Customer product survey responses linked to dates, customers, and product categories/subcategories. |
| 27 | [NewFactCurrencyRate](#newfactcurrencyrate) | Fact / Analytical | 50 | 6 | CurrencyKey, DateKey | 1.9 | Supplemental staging sample of currency exchange rates. |
| 28 | [ProspectiveBuyer](#prospectivebuyer) | Fact / Analytical | 2,059 | 24 | ProspectiveBuyerKey | 368.1 | Marketing prospect leads and demographic profiles for targeted mail campaigns. |

---

## 2. Table-by-Table Column Definitions

### DimAccount
- **Role**: Dimension Table | **Rows**: 99 | **Columns**: 10 | **CSV File**: [data/DimAccount.csv](../data/DimAccount.csv)
- **Purpose**: Chart of accounts hierarchy (Balance Sheet and Income Statement accounts) used in financial reporting.

| # | Column Name | Data Type | Nullable | Key / Relationship | Sample Value (Row 1) |
| :---: | :--- | :--- | :---: | :--- | :--- |
| 1 | AccountKey | int | No | **PK** | 1 |
| 2 | ParentAccountKey | int | Yes | FK -> DimAccount[AccountKey] | *(empty)* |
| 3 | AccountCodeAlternateKey | int | Yes | — | 1 |
| 4 | ParentAccountCodeAlternateKey | int | Yes | — | *(empty)* |
| 5 | AccountDescription | nvarchar(50) | Yes | — | Balance Sheet |
| 6 | AccountType | nvarchar(50) | Yes | — | *(empty)* |
| 7 | Operator | nvarchar(50) | Yes | — | ~ |
| 8 | CustomMembers | nvarchar(300) | Yes | — | *(empty)* |
| 9 | ValueType | nvarchar(50) | Yes | — | Currency |
| 10 | CustomMemberOptions | nvarchar(200) | Yes | — | *(empty)* |

### DimCurrency
- **Role**: Dimension Table | **Rows**: 105 | **Columns**: 3 | **CSV File**: [data/DimCurrency.csv](../data/DimCurrency.csv)
- **Purpose**: ISO currency codes and names used across transactions, organizations, and exchange rates.

| # | Column Name | Data Type | Nullable | Key / Relationship | Sample Value (Row 1) |
| :---: | :--- | :--- | :---: | :--- | :--- |
| 1 | CurrencyKey | int | No | **PK** | 1 |
| 2 | CurrencyAlternateKey | nchar(3) | No | — | AFA |
| 3 | CurrencyName | nvarchar(50) | No | — | Afghani |

### DimCustomer
- **Role**: Dimension Table | **Rows**: 18,484 | **Columns**: 29 | **CSV File**: [data/DimCustomer.csv](../data/DimCustomer.csv)
- **Purpose**: Individual online retail (B2C) customers, including demographics, income, education, and household profile.

| # | Column Name | Data Type | Nullable | Key / Relationship | Sample Value (Row 1) |
| :---: | :--- | :--- | :---: | :--- | :--- |
| 1 | CustomerKey | int | No | **PK** | 11000 |
| 2 | GeographyKey | int | Yes | FK -> DimGeography[GeographyKey] | 26 |
| 3 | CustomerAlternateKey | nvarchar(15) | No | — | AW00011000 |
| 4 | Title | nvarchar(8) | Yes | — | *(empty)* |
| 5 | FirstName | nvarchar(50) | Yes | — | Jon |
| 6 | MiddleName | nvarchar(50) | Yes | — | V |
| 7 | LastName | nvarchar(50) | Yes | — | Yang |
| 8 | NameStyle | bit | Yes | — | 0 |
| 9 | BirthDate | date | Yes | — | 1971-10-06 |
| 10 | MaritalStatus | nchar(1) | Yes | — | M |
| 11 | Suffix | nvarchar(10) | Yes | — | *(empty)* |
| 12 | Gender | nvarchar(1) | Yes | — | M |
| 13 | EmailAddress | nvarchar(50) | Yes | — | jon24@adventure-works.com |
| 14 | YearlyIncome | money | Yes | — | 90000.0000 |
| 15 | TotalChildren | tinyint | Yes | — | 2 |
| 16 | NumberChildrenAtHome | tinyint | Yes | — | 0 |
| 17 | EnglishEducation | nvarchar(40) | Yes | — | Bachelors |
| 18 | SpanishEducation | nvarchar(40) | Yes | — | Licenciatura |
| 19 | FrenchEducation | nvarchar(40) | Yes | — | Bac + 4 |
| 20 | EnglishOccupation | nvarchar(100) | Yes | — | Professional |
| 21 | SpanishOccupation | nvarchar(100) | Yes | — | Profesional |
| 22 | FrenchOccupation | nvarchar(100) | Yes | — | Cadre |
| 23 | HouseOwnerFlag | nchar(1) | Yes | — | 1 |
| 24 | NumberCarsOwned | tinyint | Yes | — | 0 |
| 25 | AddressLine1 | nvarchar(120) | Yes | — | 3761 N. 14th St |
| 26 | AddressLine2 | nvarchar(120) | Yes | — | *(empty)* |
| 27 | Phone | nvarchar(20) | Yes | — | 1 (11) 500 555-0162 |
| 28 | DateFirstPurchase | date | Yes | — | 2011-01-19 |
| 29 | CommuteDistance | nvarchar(15) | Yes | — | 1-2 Miles |

### DimDate
- **Role**: Dimension Table | **Rows**: 3,652 | **Columns**: 19 | **CSV File**: [data/DimDate.csv](../data/DimDate.csv)
- **Purpose**: Master calendar and fiscal date dimension (2005–2014) with multi-language month/day labels.

| # | Column Name | Data Type | Nullable | Key / Relationship | Sample Value (Row 1) |
| :---: | :--- | :--- | :---: | :--- | :--- |
| 1 | DateKey | int | No | **PK** | 20050101 |
| 2 | FullDateAlternateKey | date | No | — | 2005-01-01 |
| 3 | DayNumberOfWeek | tinyint | No | — | 7 |
| 4 | EnglishDayNameOfWeek | nvarchar(10) | No | — | Saturday |
| 5 | SpanishDayNameOfWeek | nvarchar(10) | No | — | Sábado |
| 6 | FrenchDayNameOfWeek | nvarchar(10) | No | — | Samedi |
| 7 | DayNumberOfMonth | tinyint | No | — | 1 |
| 8 | DayNumberOfYear | smallint | No | — | 1 |
| 9 | WeekNumberOfYear | tinyint | No | — | 1 |
| 10 | EnglishMonthName | nvarchar(10) | No | — | January |
| 11 | SpanishMonthName | nvarchar(10) | No | — | Enero |
| 12 | FrenchMonthName | nvarchar(10) | No | — | Janvier |
| 13 | MonthNumberOfYear | tinyint | No | — | 1 |
| 14 | CalendarQuarter | tinyint | No | — | 1 |
| 15 | CalendarYear | smallint | No | — | 2005 |
| 16 | CalendarSemester | tinyint | No | — | 1 |
| 17 | FiscalQuarter | tinyint | No | — | 3 |
| 18 | FiscalYear | smallint | No | — | 2005 |
| 19 | FiscalSemester | tinyint | No | — | 2 |

### DimDepartmentGroup
- **Role**: Dimension Table | **Rows**: 7 | **Columns**: 3 | **CSV File**: [data/DimDepartmentGroup.csv](../data/DimDepartmentGroup.csv)
- **Purpose**: Corporate department hierarchy used to slice financial actuals and budgets.

| # | Column Name | Data Type | Nullable | Key / Relationship | Sample Value (Row 1) |
| :---: | :--- | :--- | :---: | :--- | :--- |
| 1 | DepartmentGroupKey | int | No | **PK** | 1 |
| 2 | ParentDepartmentGroupKey | int | Yes | — | *(empty)* |
| 3 | DepartmentGroupName | nvarchar(50) | Yes | — | Corporate |

### DimEmployee
- **Role**: Dimension Table | **Rows**: 296 | **Columns**: 31 | **CSV File**: [data/DimEmployee.csv](../data/DimEmployee.csv)
- **Purpose**: Internal employees and sales representatives, including organizational parent-child hierarchy (ParentEmployeeKey).

| # | Column Name | Data Type | Nullable | Key / Relationship | Sample Value (Row 1) |
| :---: | :--- | :--- | :---: | :--- | :--- |
| 1 | EmployeeKey | int | No | **PK** | 1 |
| 2 | ParentEmployeeKey | int | Yes | FK -> DimEmployee[EmployeeKey] | 18 |
| 3 | EmployeeNationalIDAlternateKey | nvarchar(15) | Yes | — | 14417807 |
| 4 | ParentEmployeeNationalIDAlternateKey | nvarchar(15) | Yes | — | *(empty)* |
| 5 | SalesTerritoryKey | int | Yes | FK -> DimSalesTerritory[SalesTerritoryKey] | 11 |
| 6 | FirstName | nvarchar(50) | No | — | Guy |
| 7 | LastName | nvarchar(50) | No | — | Gilbert |
| 8 | MiddleName | nvarchar(50) | Yes | — | R |
| 9 | NameStyle | bit | No | — | 0 |
| 10 | Title | nvarchar(50) | Yes | — | Production Technician - WC60 |
| 11 | HireDate | date | Yes | — | 2006-01-28 |
| 12 | BirthDate | date | Yes | — | 1981-11-12 |
| 13 | LoginID | nvarchar(256) | Yes | — | adventure-works\guy1 |
| 14 | EmailAddress | nvarchar(50) | Yes | — | guy1@adventure-works.com |
| 15 | Phone | nvarchar(25) | Yes | — | 320-555-0195 |
| 16 | MaritalStatus | nchar(1) | Yes | — | M |
| 17 | EmergencyContactName | nvarchar(50) | Yes | — | Guy Gilbert |
| 18 | EmergencyContactPhone | nvarchar(25) | Yes | — | 320-555-0195 |
| 19 | SalariedFlag | bit | Yes | — | 0 |
| 20 | Gender | nchar(1) | Yes | — | M |
| 21 | PayFrequency | tinyint | Yes | — | 1 |
| 22 | BaseRate | money | Yes | — | 12.4500 |
| 23 | VacationHours | smallint | Yes | — | 21 |
| 24 | SickLeaveHours | smallint | Yes | — | 30 |
| 25 | CurrentFlag | bit | No | — | 1 |
| 26 | SalesPersonFlag | bit | No | — | 0 |
| 27 | DepartmentName | nvarchar(50) | Yes | — | Production |
| 28 | StartDate | date | Yes | — | 2006-01-28 |
| 29 | EndDate | date | Yes | — | *(empty)* |
| 30 | Status | nvarchar(50) | Yes | — | Current |
| 31 | EmployeePhoto | varbinary(max) | Yes | — | FFD8FFE000104A46494600010101004800480000FF... |

### DimGeography
- **Role**: Dimension Table | **Rows**: 655 | **Columns**: 11 | **CSV File**: [data/DimGeography.csv](../data/DimGeography.csv)
- **Purpose**: Cities, states/provinces, postal codes, and countries mapped to sales territories.

| # | Column Name | Data Type | Nullable | Key / Relationship | Sample Value (Row 1) |
| :---: | :--- | :--- | :---: | :--- | :--- |
| 1 | GeographyKey | int | No | **PK** | 1 |
| 2 | City | nvarchar(30) | Yes | — | Alexandria |
| 3 | StateProvinceCode | nvarchar(3) | Yes | — | NSW |
| 4 | StateProvinceName | nvarchar(50) | Yes | — | New South Wales |
| 5 | CountryRegionCode | nvarchar(3) | Yes | — | AU |
| 6 | EnglishCountryRegionName | nvarchar(50) | Yes | — | Australia |
| 7 | SpanishCountryRegionName | nvarchar(50) | Yes | — | Australia |
| 8 | FrenchCountryRegionName | nvarchar(50) | Yes | — | Australie |
| 9 | PostalCode | nvarchar(15) | Yes | — | 2015 |
| 10 | SalesTerritoryKey | int | Yes | FK -> DimSalesTerritory[SalesTerritoryKey] | 9 |
| 11 | IpAddressLocator | nvarchar(15) | Yes | — | 198.51.100.2 |

### DimOrganization
- **Role**: Dimension Table | **Rows**: 14 | **Columns**: 5 | **CSV File**: [data/DimOrganization.csv](../data/DimOrganization.csv)
- **Purpose**: Corporate legal/regional subsidiaries, ownership percentages, and reporting currencies.

| # | Column Name | Data Type | Nullable | Key / Relationship | Sample Value (Row 1) |
| :---: | :--- | :--- | :---: | :--- | :--- |
| 1 | OrganizationKey | int | No | **PK** | 1 |
| 2 | ParentOrganizationKey | int | Yes | FK -> DimOrganization[OrganizationKey] | *(empty)* |
| 3 | PercentageOfOwnership | nvarchar(16) | Yes | — | 1 |
| 4 | OrganizationName | nvarchar(50) | Yes | — | AdventureWorks Cycle |
| 5 | CurrencyKey | int | Yes | FK -> DimCurrency[CurrencyKey] | 100 |

### DimProduct
- **Role**: Dimension Table | **Rows**: 606 | **Columns**: 36 | **CSV File**: [data/DimProduct.csv](../data/DimProduct.csv)
- **Purpose**: Master product catalog (SKUs) with SCD Type 2 history, standard costs, list prices, dealer prices, and physical attributes.

| # | Column Name | Data Type | Nullable | Key / Relationship | Sample Value (Row 1) |
| :---: | :--- | :--- | :---: | :--- | :--- |
| 1 | ProductKey | int | No | **PK** | 1 |
| 2 | ProductAlternateKey | nvarchar(25) | Yes | — | AR-5381 |
| 3 | ProductSubcategoryKey | int | Yes | — | *(empty)* |
| 4 | WeightUnitMeasureCode | nchar(3) | Yes | — | *(empty)* |
| 5 | SizeUnitMeasureCode | nchar(3) | Yes | — | *(empty)* |
| 6 | EnglishProductName | nvarchar(50) | No | — | Adjustable Race |
| 7 | SpanishProductName | nvarchar(50) | No | — | *(empty)* |
| 8 | FrenchProductName | nvarchar(50) | No | — | *(empty)* |
| 9 | StandardCost | money | Yes | — | *(empty)* |
| 10 | FinishedGoodsFlag | bit | No | — | 0 |
| 11 | Color | nvarchar(15) | No | — | NA |
| 12 | SafetyStockLevel | smallint | Yes | — | 1000 |
| 13 | ReorderPoint | smallint | Yes | — | 750 |
| 14 | ListPrice | money | Yes | — | *(empty)* |
| 15 | Size | nvarchar(50) | Yes | — | *(empty)* |
| 16 | SizeRange | nvarchar(50) | Yes | — | NA |
| 17 | Weight | float | Yes | — | *(empty)* |
| 18 | DaysToManufacture | int | Yes | — | 0 |
| 19 | ProductLine | nchar(2) | Yes | — | *(empty)* |
| 20 | DealerPrice | money | Yes | — | *(empty)* |
| 21 | Class | nchar(2) | Yes | — | *(empty)* |
| 22 | Style | nchar(2) | Yes | — | *(empty)* |
| 23 | ModelName | nvarchar(50) | Yes | — | *(empty)* |
| 24 | LargePhoto | varbinary(max) | Yes | — | 474946383961F0009500F700000000008000000080... |
| 25 | EnglishDescription | nvarchar(400) | Yes | — | *(empty)* |
| 26 | FrenchDescription | nvarchar(400) | Yes | — | *(empty)* |
| 27 | ChineseDescription | nvarchar(400) | Yes | — | *(empty)* |
| 28 | ArabicDescription | nvarchar(400) | Yes | — | *(empty)* |
| 29 | HebrewDescription | nvarchar(400) | Yes | — | *(empty)* |
| 30 | ThaiDescription | nvarchar(400) | Yes | — | *(empty)* |
| 31 | GermanDescription | nvarchar(400) | Yes | — | *(empty)* |
| 32 | JapaneseDescription | nvarchar(400) | Yes | — | *(empty)* |
| 33 | TurkishDescription | nvarchar(400) | Yes | — | *(empty)* |
| 34 | StartDate | datetime | Yes | — | 2003-07-01 00:00:00.000 |
| 35 | EndDate | datetime | Yes | — | *(empty)* |
| 36 | Status | nvarchar(7) | Yes | — | Current |

### DimProductCategory
- **Role**: Dimension Table | **Rows**: 4 | **Columns**: 5 | **CSV File**: [data/DimProductCategory.csv](../data/DimProductCategory.csv)
- **Purpose**: Top-level product classification (*Bikes*, *Components*, *Clothing*, *Accessories*).

| # | Column Name | Data Type | Nullable | Key / Relationship | Sample Value (Row 1) |
| :---: | :--- | :--- | :---: | :--- | :--- |
| 1 | ProductCategoryKey | int | No | **PK** | 1 |
| 2 | ProductCategoryAlternateKey | int | Yes | — | 1 |
| 3 | EnglishProductCategoryName | nvarchar(50) | No | — | Bikes |
| 4 | SpanishProductCategoryName | nvarchar(50) | No | — | Bicicleta |
| 5 | FrenchProductCategoryName | nvarchar(50) | No | — | Vélo |

### DimProductSubcategory
- **Role**: Dimension Table | **Rows**: 37 | **Columns**: 6 | **CSV File**: [data/DimProductSubcategory.csv](../data/DimProductSubcategory.csv)
- **Purpose**: Second-level product classification (37 subcategories linked to DimProductCategory).

| # | Column Name | Data Type | Nullable | Key / Relationship | Sample Value (Row 1) |
| :---: | :--- | :--- | :---: | :--- | :--- |
| 1 | ProductSubcategoryKey | int | No | **PK** | 1 |
| 2 | ProductSubcategoryAlternateKey | int | Yes | — | 1 |
| 3 | EnglishProductSubcategoryName | nvarchar(50) | No | — | Mountain Bikes |
| 4 | SpanishProductSubcategoryName | nvarchar(50) | No | — | Bicicleta de montaña |
| 5 | FrenchProductSubcategoryName | nvarchar(50) | No | — | VTT |
| 6 | ProductCategoryKey | int | Yes | — | 1 |

### DimPromotion
- **Role**: Dimension Table | **Rows**: 16 | **Columns**: 16 | **CSV File**: [data/DimPromotion.csv](../data/DimPromotion.csv)
- **Purpose**: Marketing promotions and discount campaigns with start/end dates and quantity thresholds.

| # | Column Name | Data Type | Nullable | Key / Relationship | Sample Value (Row 1) |
| :---: | :--- | :--- | :---: | :--- | :--- |
| 1 | PromotionKey | int | No | **PK** | 1 |
| 2 | PromotionAlternateKey | int | Yes | — | 1 |
| 3 | EnglishPromotionName | nvarchar(255) | Yes | — | No Discount |
| 4 | SpanishPromotionName | nvarchar(255) | Yes | — | Sin descuento |
| 5 | FrenchPromotionName | nvarchar(255) | Yes | — | Aucune remise |
| 6 | DiscountPct | float | Yes | — | 0.0 |
| 7 | EnglishPromotionType | nvarchar(50) | Yes | — | No Discount |
| 8 | SpanishPromotionType | nvarchar(50) | Yes | — | Sin descuento |
| 9 | FrenchPromotionType | nvarchar(50) | Yes | — | Aucune remise |
| 10 | EnglishPromotionCategory | nvarchar(50) | Yes | — | No Discount |
| 11 | SpanishPromotionCategory | nvarchar(50) | Yes | — | Sin descuento |
| 12 | FrenchPromotionCategory | nvarchar(50) | Yes | — | Aucune remise |
| 13 | StartDate | datetime | No | — | 2010-11-29 00:00:00.000 |
| 14 | EndDate | datetime | Yes | — | 2014-06-30 00:00:00.000 |
| 15 | MinQty | int | Yes | — | 0 |
| 16 | MaxQty | int | Yes | — | *(empty)* |

### DimReseller
- **Role**: Dimension Table | **Rows**: 701 | **Columns**: 20 | **CSV File**: [data/DimReseller.csv](../data/DimReseller.csv)
- **Purpose**: B2B wholesale bike shops, specialty stores, and distributors purchasing through sales representatives.

| # | Column Name | Data Type | Nullable | Key / Relationship | Sample Value (Row 1) |
| :---: | :--- | :--- | :---: | :--- | :--- |
| 1 | ResellerKey | int | No | **PK** | 1 |
| 2 | GeographyKey | int | Yes | FK -> DimGeography[GeographyKey] | 637 |
| 3 | ResellerAlternateKey | nvarchar(15) | Yes | — | AW00000001 |
| 4 | Phone | nvarchar(25) | Yes | — | 245-555-0173 |
| 5 | BusinessType | varchar(20) | No | — | Value Added Reseller |
| 6 | ResellerName | nvarchar(50) | No | — | A Bike Store |
| 7 | NumberEmployees | int | Yes | — | 2 |
| 8 | OrderFrequency | char(1) | Yes | — | S |
| 9 | OrderMonth | tinyint | Yes | — | 5 |
| 10 | FirstOrderYear | int | Yes | — | 2011 |
| 11 | LastOrderYear | int | Yes | — | 2011 |
| 12 | ProductLine | nvarchar(50) | Yes | — | Road |
| 13 | AddressLine1 | nvarchar(60) | Yes | — | 2251 Elliot Avenue |
| 14 | AddressLine2 | nvarchar(60) | Yes | — | *(empty)* |
| 15 | AnnualSales | money | Yes | — | 300000.0000 |
| 16 | BankName | nvarchar(50) | Yes | — | International Bank |
| 17 | MinPaymentType | tinyint | Yes | — | *(empty)* |
| 18 | MinPaymentAmount | money | Yes | — | *(empty)* |
| 19 | AnnualRevenue | money | Yes | — | 30000.0000 |
| 20 | YearOpened | int | Yes | — | 1974 |

### DimSalesReason
- **Role**: Dimension Table | **Rows**: 10 | **Columns**: 4 | **CSV File**: [data/DimSalesReason.csv](../data/DimSalesReason.csv)
- **Purpose**: Customer purchase motivations (*Price*, *Quality*, *Review*, *Manufacturer*, *On Promotion*).

| # | Column Name | Data Type | Nullable | Key / Relationship | Sample Value (Row 1) |
| :---: | :--- | :--- | :---: | :--- | :--- |
| 1 | SalesReasonKey | int | No | **PK** | 1 |
| 2 | SalesReasonAlternateKey | int | No | — | 1 |
| 3 | SalesReasonName | nvarchar(50) | No | — | Price |
| 4 | SalesReasonReasonType | nvarchar(50) | No | — | Other |

### DimSalesTerritory
- **Role**: Dimension Table | **Rows**: 11 | **Columns**: 6 | **CSV File**: [data/DimSalesTerritory.csv](../data/DimSalesTerritory.csv)
- **Purpose**: Global commercial sales regions, countries, and macro-groups (*North America*, *Europe*, *Pacific*).

| # | Column Name | Data Type | Nullable | Key / Relationship | Sample Value (Row 1) |
| :---: | :--- | :--- | :---: | :--- | :--- |
| 1 | SalesTerritoryKey | int | No | **PK** | 1 |
| 2 | SalesTerritoryAlternateKey | int | Yes | — | 1 |
| 3 | SalesTerritoryRegion | nvarchar(50) | No | — | Northwest |
| 4 | SalesTerritoryCountry | nvarchar(50) | No | — | United States |
| 5 | SalesTerritoryGroup | nvarchar(50) | Yes | — | North America |
| 6 | SalesTerritoryImage | varbinary(max) | Yes | — | FFD8FFE000104A46494600010101004800480000FF... |

### DimScenario
- **Role**: Dimension Table | **Rows**: 3 | **Columns**: 2 | **CSV File**: [data/DimScenario.csv](../data/DimScenario.csv)
- **Purpose**: Financial planning scenarios (*Actual*, *Budget*, *Forecast*).

| # | Column Name | Data Type | Nullable | Key / Relationship | Sample Value (Row 1) |
| :---: | :--- | :--- | :---: | :--- | :--- |
| 1 | ScenarioKey | int | No | **PK** | 1 |
| 2 | ScenarioName | nvarchar(50) | Yes | — | Actual |

### FactAdditionalInternationalProductDescription
- **Role**: Fact / Analytical Table | **Rows**: 1,911 | **Columns**: 3 | **CSV File**: [data/FactAdditionalInternationalProductDescription.csv](../data/FactAdditionalInternationalProductDescription.csv)
- **Purpose**: Localized product descriptions across international cultures/languages.

| # | Column Name | Data Type | Nullable | Key / Relationship | Sample Value (Row 1) |
| :---: | :--- | :--- | :---: | :--- | :--- |
| 1 | ProductKey | int | No | **PK** | 210 |
| 2 | CultureName | nvarchar(50) | No | **PK** | Afrikaans |
| 3 | ProductDescription | nvarchar(max) | No | — | Ons ligste en beste-gehalte aluminiumraam ... |

### FactCallCenter
- **Role**: Fact / Analytical Table | **Rows**: 120 | **Columns**: 14 | **CSV File**: [data/FactCallCenter.csv](../data/FactCallCenter.csv)
- **Purpose**: Daily shift-level call center telemetry (call volume, orders, issues raised, service grade).

| # | Column Name | Data Type | Nullable | Key / Relationship | Sample Value (Row 1) |
| :---: | :--- | :--- | :---: | :--- | :--- |
| 1 | FactCallCenterID | int | No | **PK** | 1 |
| 2 | DateKey | int | No | FK -> DimDate[DateKey] | 20140501 |
| 3 | WageType | nvarchar(15) | No | — | weekday |
| 4 | Shift | nvarchar(20) | No | — | AM |
| 5 | LevelOneOperators | smallint | No | — | 2 |
| 6 | LevelTwoOperators | smallint | No | — | 7 |
| 7 | TotalOperators | smallint | No | — | 9 |
| 8 | Calls | int | No | — | 405 |
| 9 | AutomaticResponses | int | No | — | 283 |
| 10 | Orders | int | No | — | 341 |
| 11 | IssuesRaised | smallint | No | — | 2 |
| 12 | AverageTimePerIssue | smallint | No | — | 79 |
| 13 | ServiceGrade | float | No | — | 2.9999999999999999E-2 |
| 14 | Date | datetime | Yes | — | 2014-05-01 00:00:00.000 |

### FactCurrencyRate
- **Role**: Fact / Analytical Table | **Rows**: 14,264 | **Columns**: 5 | **CSV File**: [data/FactCurrencyRate.csv](../data/FactCurrencyRate.csv)
- **Purpose**: Daily average and end-of-day currency exchange rates relative to USD.

| # | Column Name | Data Type | Nullable | Key / Relationship | Sample Value (Row 1) |
| :---: | :--- | :--- | :---: | :--- | :--- |
| 1 | CurrencyKey | int | No | **PK**, FK -> DimCurrency[CurrencyKey] | 3 |
| 2 | DateKey | int | No | **PK**, FK -> DimDate[DateKey] | 20101229 |
| 3 | AverageRate | float | No | — | 1.0 |
| 4 | EndOfDayRate | float | No | — | 0.99980003999200195 |
| 5 | Date | datetime | Yes | — | 2010-12-29 00:00:00.000 |

### FactFinance
- **Role**: Fact / Analytical Table | **Rows**: 39,409 | **Columns**: 8 | **CSV File**: [data/FactFinance.csv](../data/FactFinance.csv)
- **Purpose**: General Ledger financial entries by date, organization, department, account, and scenario.

| # | Column Name | Data Type | Nullable | Key / Relationship | Sample Value (Row 1) |
| :---: | :--- | :--- | :---: | :--- | :--- |
| 1 | FinanceKey | int | No | **PK** | 1 |
| 2 | DateKey | int | No | FK -> DimDate[DateKey] | 20101229 |
| 3 | OrganizationKey | int | No | FK -> DimOrganization[OrganizationKey] | 3 |
| 4 | DepartmentGroupKey | int | No | FK -> DimDepartmentGroup[DepartmentGroupKey] | 1 |
| 5 | ScenarioKey | int | No | FK -> DimScenario[ScenarioKey] | 1 |
| 6 | AccountKey | int | No | FK -> DimAccount[AccountKey] | 60 |
| 7 | Amount | float | No | — | 22080.0 |
| 8 | Date | datetime | Yes | — | 2010-12-29 00:00:00.000 |

### FactInternetSales
- **Role**: Fact / Analytical Table | **Rows**: 60,398 | **Columns**: 26 | **CSV File**: [data/FactInternetSales.csv](../data/FactInternetSales.csv)
- **Purpose**: Direct-to-consumer (B2C) online sales order lines (60,398 rows).

| # | Column Name | Data Type | Nullable | Key / Relationship | Sample Value (Row 1) |
| :---: | :--- | :--- | :---: | :--- | :--- |
| 1 | ProductKey | int | No | FK -> DimProduct[ProductKey] | 310 |
| 2 | OrderDateKey | int | No | FK -> DimDate[DateKey] | 20101229 |
| 3 | DueDateKey | int | No | FK -> DimDate[DateKey] | 20110110 |
| 4 | ShipDateKey | int | No | FK -> DimDate[DateKey] | 20110105 |
| 5 | CustomerKey | int | No | FK -> DimCustomer[CustomerKey] | 21768 |
| 6 | PromotionKey | int | No | FK -> DimPromotion[PromotionKey] | 1 |
| 7 | CurrencyKey | int | No | FK -> DimCurrency[CurrencyKey] | 19 |
| 8 | SalesTerritoryKey | int | No | FK -> DimSalesTerritory[SalesTerritoryKey] | 6 |
| 9 | SalesOrderNumber | nvarchar(20) | No | **PK** | SO43697 |
| 10 | SalesOrderLineNumber | tinyint | No | **PK** | 1 |
| 11 | RevisionNumber | tinyint | No | — | 1 |
| 12 | OrderQuantity | smallint | No | — | 1 |
| 13 | UnitPrice | money | No | — | 3578.2700 |
| 14 | ExtendedAmount | money | No | — | 3578.2700 |
| 15 | UnitPriceDiscountPct | float | No | — | 0.0 |
| 16 | DiscountAmount | float | No | — | 0.0 |
| 17 | ProductStandardCost | money | No | — | 2171.2942 |
| 18 | TotalProductCost | money | No | — | 2171.2942 |
| 19 | SalesAmount | money | No | — | 3578.2700 |
| 20 | TaxAmt | money | No | — | 286.2616 |
| 21 | Freight | money | No | — | 89.4568 |
| 22 | CarrierTrackingNumber | nvarchar(25) | Yes | — | *(empty)* |
| 23 | CustomerPONumber | nvarchar(25) | Yes | — | *(empty)* |
| 24 | OrderDate | datetime | Yes | — | 2010-12-29 00:00:00.000 |
| 25 | DueDate | datetime | Yes | — | 2011-01-10 00:00:00.000 |
| 26 | ShipDate | datetime | Yes | — | 2011-01-05 00:00:00.000 |

### FactInternetSalesReason
- **Role**: Fact / Analytical Table | **Rows**: 64,515 | **Columns**: 3 | **CSV File**: [data/FactInternetSalesReason.csv](../data/FactInternetSalesReason.csv)
- **Purpose**: Many-to-many bridge table linking online sales order lines (FactInternetSales) to purchase reasons (DimSalesReason).

| # | Column Name | Data Type | Nullable | Key / Relationship | Sample Value (Row 1) |
| :---: | :--- | :--- | :---: | :--- | :--- |
| 1 | SalesOrderNumber | nvarchar(20) | No | **PK**, FK -> FactInternetSales[SalesOrderNumber] | SO43697 |
| 2 | SalesOrderLineNumber | tinyint | No | **PK**, FK -> FactInternetSales[SalesOrderLineNumber] | 1 |
| 3 | SalesReasonKey | int | No | **PK**, FK -> DimSalesReason[SalesReasonKey] | 5 |

### FactProductInventory
- **Role**: Fact / Analytical Table | **Rows**: 776,286 | **Columns**: 7 | **CSV File**: [data/FactProductInventory.csv](../data/FactProductInventory.csv)
- **Purpose**: Daily warehouse stock movements (UnitsIn, UnitsOut, UnitsBalance, UnitCost) by product and date (776,286 rows).

| # | Column Name | Data Type | Nullable | Key / Relationship | Sample Value (Row 1) |
| :---: | :--- | :--- | :---: | :--- | :--- |
| 1 | ProductKey | int | No | **PK**, FK -> DimProduct[ProductKey] | 1 |
| 2 | DateKey | int | No | **PK**, FK -> DimDate[DateKey] | 20101228 |
| 3 | MovementDate | date | No | — | 2010-12-28 |
| 4 | UnitCost | money | No | — | .3200 |
| 5 | UnitsIn | int | No | — | 0 |
| 6 | UnitsOut | int | No | — | 0 |
| 7 | UnitsBalance | int | No | — | 875 |

### FactResellerSales
- **Role**: Fact / Analytical Table | **Rows**: 60,855 | **Columns**: 27 | **CSV File**: [data/FactResellerSales.csv](../data/FactResellerSales.csv)
- **Purpose**: Business-to-business (B2B) wholesale order lines placed by resellers (60,855 rows).

| # | Column Name | Data Type | Nullable | Key / Relationship | Sample Value (Row 1) |
| :---: | :--- | :--- | :---: | :--- | :--- |
| 1 | ProductKey | int | No | FK -> DimProduct[ProductKey] | 349 |
| 2 | OrderDateKey | int | No | FK -> DimDate[DateKey] | 20101229 |
| 3 | DueDateKey | int | No | FK -> DimDate[DateKey] | 20110110 |
| 4 | ShipDateKey | int | No | FK -> DimDate[DateKey] | 20110105 |
| 5 | ResellerKey | int | No | FK -> DimReseller[ResellerKey] | 676 |
| 6 | EmployeeKey | int | No | FK -> DimEmployee[EmployeeKey] | 285 |
| 7 | PromotionKey | int | No | FK -> DimPromotion[PromotionKey] | 1 |
| 8 | CurrencyKey | int | No | FK -> DimCurrency[CurrencyKey] | 100 |
| 9 | SalesTerritoryKey | int | No | FK -> DimSalesTerritory[SalesTerritoryKey] | 5 |
| 10 | SalesOrderNumber | nvarchar(20) | No | **PK** | SO43659 |
| 11 | SalesOrderLineNumber | tinyint | No | **PK** | 1 |
| 12 | RevisionNumber | tinyint | Yes | — | 1 |
| 13 | OrderQuantity | smallint | Yes | — | 1 |
| 14 | UnitPrice | money | Yes | — | 2024.9940 |
| 15 | ExtendedAmount | money | Yes | — | 2024.9940 |
| 16 | UnitPriceDiscountPct | float | Yes | — | 0.0 |
| 17 | DiscountAmount | float | Yes | — | 0.0 |
| 18 | ProductStandardCost | money | Yes | — | 1898.0944 |
| 19 | TotalProductCost | money | Yes | — | 1898.0944 |
| 20 | SalesAmount | money | Yes | — | 2024.9940 |
| 21 | TaxAmt | money | Yes | — | 161.9995 |
| 22 | Freight | money | Yes | — | 50.6249 |
| 23 | CarrierTrackingNumber | nvarchar(25) | Yes | — | 4911-403C-98 |
| 24 | CustomerPONumber | nvarchar(25) | Yes | — | PO522145787 |
| 25 | OrderDate | datetime | Yes | — | 2010-12-29 00:00:00.000 |
| 26 | DueDate | datetime | Yes | — | 2011-01-10 00:00:00.000 |
| 27 | ShipDate | datetime | Yes | — | 2011-01-05 00:00:00.000 |

### FactSalesQuota
- **Role**: Fact / Analytical Table | **Rows**: 163 | **Columns**: 7 | **CSV File**: [data/FactSalesQuota.csv](../data/FactSalesQuota.csv)
- **Purpose**: Quarterly sales revenue quotas assigned to sales employees.

| # | Column Name | Data Type | Nullable | Key / Relationship | Sample Value (Row 1) |
| :---: | :--- | :--- | :---: | :--- | :--- |
| 1 | SalesQuotaKey | int | No | **PK** | 1 |
| 2 | EmployeeKey | int | No | FK -> DimEmployee[EmployeeKey] | 272 |
| 3 | DateKey | int | No | FK -> DimDate[DateKey] | 20101229 |
| 4 | CalendarYear | smallint | No | — | 2010 |
| 5 | CalendarQuarter | tinyint | No | — | 4 |
| 6 | SalesAmountQuota | money | No | — | 28000.0000 |
| 7 | Date | datetime | Yes | — | 2010-12-29 00:00:00.000 |

### FactSurveyResponse
- **Role**: Fact / Analytical Table | **Rows**: 2,727 | **Columns**: 8 | **CSV File**: [data/FactSurveyResponse.csv](../data/FactSurveyResponse.csv)
- **Purpose**: Customer product survey responses linked to dates, customers, and product categories/subcategories.

| # | Column Name | Data Type | Nullable | Key / Relationship | Sample Value (Row 1) |
| :---: | :--- | :--- | :---: | :--- | :--- |
| 1 | SurveyResponseKey | int | No | **PK** | 1 |
| 2 | DateKey | int | No | FK -> DimDate[DateKey] | 20120309 |
| 3 | CustomerKey | int | No | FK -> DimCustomer[CustomerKey] | 11001 |
| 4 | ProductCategoryKey | int | No | — | 3 |
| 5 | EnglishProductCategoryName | nvarchar(50) | No | — | Clothing |
| 6 | ProductSubcategoryKey | int | No | — | 24 |
| 7 | EnglishProductSubcategoryName | nvarchar(50) | No | — | Tights |
| 8 | Date | datetime | Yes | — | 2012-03-09 00:00:00.000 |

### NewFactCurrencyRate
- **Role**: Fact / Analytical Table | **Rows**: 50 | **Columns**: 6 | **CSV File**: [data/NewFactCurrencyRate.csv](../data/NewFactCurrencyRate.csv)
- **Purpose**: Supplemental staging sample of currency exchange rates.

| # | Column Name | Data Type | Nullable | Key / Relationship | Sample Value (Row 1) |
| :---: | :--- | :--- | :---: | :--- | :--- |
| 1 | AverageRate | real | Yes | — | 1.0001 |
| 2 | CurrencyID | nvarchar(3) | Yes | — | ARS |
| 3 | CurrencyDate | date | Yes | — | 2011-03-03 |
| 4 | EndOfDayRate | real | Yes | — | 0.99960017 |
| 5 | CurrencyKey | int | Yes | **PK** | 3 |
| 6 | DateKey | int | Yes | **PK** | *(empty)* |

### ProspectiveBuyer
- **Role**: Fact / Analytical Table | **Rows**: 2,059 | **Columns**: 24 | **CSV File**: [data/ProspectiveBuyer.csv](../data/ProspectiveBuyer.csv)
- **Purpose**: Marketing prospect leads and demographic profiles for targeted mail campaigns.

| # | Column Name | Data Type | Nullable | Key / Relationship | Sample Value (Row 1) |
| :---: | :--- | :--- | :---: | :--- | :--- |
| 1 | ProspectiveBuyerKey | int | No | **PK** | 1 |
| 2 | ProspectAlternateKey | nvarchar(15) | Yes | — | 21596444800 |
| 3 | FirstName | nvarchar(50) | Yes | — | Adam |
| 4 | MiddleName | nvarchar(50) | Yes | — | *(empty)* |
| 5 | LastName | nvarchar(50) | Yes | — | Alexander |
| 6 | BirthDate | datetime | Yes | — | 1945-12-30 00:00:00.000 |
| 7 | MaritalStatus | nchar(1) | Yes | — | M |
| 8 | Gender | nvarchar(1) | Yes | — | M |
| 9 | EmailAddress | nvarchar(50) | Yes | — | aalexander@lucernepublishing.com |
| 10 | YearlyIncome | money | Yes | — | 40000.0000 |
| 11 | TotalChildren | tinyint | Yes | — | 3 |
| 12 | NumberChildrenAtHome | tinyint | Yes | — | 0 |
| 13 | Education | nvarchar(40) | Yes | — | Partial Co |
| 14 | Occupation | nvarchar(100) | Yes | — | Professional |
| 15 | HouseOwnerFlag | nchar(1) | Yes | — | 1 |
| 16 | NumberCarsOwned | tinyint | Yes | — | 2 |
| 17 | AddressLine1 | nvarchar(120) | Yes | — | 566 S. Main |
| 18 | AddressLine2 | nvarchar(120) | Yes | — | *(empty)* |
| 19 | City | nvarchar(30) | Yes | — | Cedar City |
| 20 | StateProvinceCode | nvarchar(3) | Yes | — | UT |
| 21 | PostalCode | nvarchar(15) | Yes | — | 84720 |
| 22 | Phone | nvarchar(20) | Yes | — | 516-555-0187 |
| 23 | Salutation | nvarchar(8) | Yes | — | Mr. |
| 24 | Unknown | int | Yes | — | 0 |

---

## Navigation
- Back to **[01 — Business Overview](./01-overview.md)**
- Next: **[03 — Star Schema & Relationships](./03-star-schema.md)**
- Connect: **[04 — Power BI Quickstart Guide](./04-quickstart-pbi.md)**
