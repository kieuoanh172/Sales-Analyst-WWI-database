# Sales-Analyst-WWI-database
## Overview
Database: WideWorldImporters (WWI)
## Schema Design: 
### Use a Star Schema:
1. Fact Table – FactSales
```sql
SELECT
SI.InvoiceID,
SI.InvoiceDate,
SI.CustomerID,
SIL.StockItemID,
SI.SalespersonPersonID,
SIL.Quantity,
SIL.UnitPrice,
SIL.TaxAmount,
SIL.LineProfit,
SIL.ExtendedPrice as Total_Incl_Tax
FROM Sales.Invoices SI
JOIN Sales.InvoiceLines SIL On SIL.InvoiceID = SI.InvoiceID
ORDER BY InvoiceID
```
2. Dim Tables
DimCustomer
```sql
SELECT
SC.CustomerID,
SC.CustomerName,
SCC.CustomerCategoryName,
AI.CityName,
AP.StateProvinceName,
ACO.CountryName
FROM sales.Customers SC
LEFT JOIN Sales.CustomerCategories SCC ON SCC.CustomerCategoryID = SC.CustomerCategoryID
LEFT JOIN Sales.BuyingGroups SB ON SB.BuyingGroupID = SC.BuyingGroupID
JOIN Application.Cities AI ON AI.CityID = SC.PostalCityID
JOIN Application.StateProvinces AP ON AP.StateProvinceID = AI.StateProvinceID
JOIN Application.Countries ACO ON ACO.CountryID = AP.CountryID
ORDER BY CustomerID
```
DimStockItem
```sql
SELECT *
From Warehouse.StockItems
```
DimEmployee
```sql
SELECT
PersonID,
FullName,
PreferredName,
IsSalesperson
FROM Application.People
Where Application.People.IsSalesperson =1
```
DimDate --Create in Power BI
```sql
Dim_Date = ADDCOLUMNS(CALENDAR(MIN(Fact_Sales[InvoiceDate]),MAX(Fact_Sales[InvoiceDate])),
"Year",FORMAT([Date],"YYYY"),
"Quater","Q"& FORMAT([Date],"Q"),
"Month Year",FORMAT([Date],"YYYY-MM"),
"Month Name",FORMAT([Date],"MMM"),
"Month No",FORMAT([Date],"MM"),
"Day",DAY([Date]),
"Day Name",FORMAT([Date],"ddd"),
"Weekday",WEEKDAY([Date],1) 
)
```
![image](https://github.com/user-attachments/assets/8c23330b-2890-47a8-84da-cc9ee4798032)

## Key Metrics

| **KPI**                          | **Mô tả**                                    |
| -------------------------------- | -------------------------------------------- |
| **Total Sales**                  | Tổng doanh thu (trước và sau thuế)           |
| **Sales Growth %**               | Tỷ lệ tăng trưởng doanh thu theo tháng / quý |
| **Top Selling Products**         | Sản phẩm bán chạy nhất                       |
| **Top Customers by Revenue**     | Khách hàng mang lại doanh thu cao nhất       |
| **Average Order Value**          | Tổng doanh thu / số đơn hàng                 |
| **Discount Rate**                | Trung bình mức chiết khấu %                  |
| **Sales by Region**              | Doanh thu theo khu vực địa lý                |
| **Sales per Employee**           | Hiệu suất bán hàng của từng nhân viên        |
| **Customer Retention Rate**      | (Nếu có dữ liệu về repeat orders)            |
| **Revenue per Product Category** | Doanh thu theo loại sản phẩm                 |


