# Online Store Orders – Excel Analysis \& Dashboard

An end-to-end Excel project that imports, cleans, analyses and visualises 1,200 online store orders (Jan 2023 – Jun 2025), and presents the results in an interactive, filter-driven dashboard.

**File:** `khushboo\_excel\_dashboard.xlsx`

\---

## Highlights

|Metric|Value|
|-|-|
|Orders analysed|1,200|
|Gross order value|1,264,761.96|
|Net revenue (excl. Cancelled \& Returned)|745,088.05|
|Average order value|1,053.97|
|Units sold|3,535|
|Cancellation rate|20.8%|
|Return rate|20.6%|
|Top product (net revenue)|Printer|
|Top payment method|Online|
|Top referral source|Instagram|

> Currency is not stated in the dataset, so values are shown as plain numbers. 2025 contains January–June only.

\---

## Workbook Structure

|Sheet|Role|Notes|
|-|-|-|
|`README`|Documentation|Start here – task map, data dictionary, cleaning log|
|`Dashboard`|Interactive report|6 KPI cards, 5 charts, 6 filters (yellow cells in row 6)|
|`Pivot\_Tables`|Summaries|6 formula-driven pivot-style tables with conditional formatting|
|`Advanced\_Formulas`|Lookups \& logic|VLOOKUP, INDEX-MATCH, nested IF, customer tiers|
|`Visualizations`|Charts|6 charts built on `Pivot\_Tables`|
|`Clean\_Data`|Analysis-ready table (`tblOrders`)|Blue headers = source fields, green headers = derived formulas|
|`Raw\_Data`|Original import|Never edited – audit trail|
|`Dash\_Calc`|Calculation engine|Feeds the Dashboard – do not edit|

\---

## Project Tasks

|#|Task|Where|What was done|Weight|
|-|-|-|-|-|
|1|Dataset selection \& import|`Raw\_Data`|Imported 1,200 rows × 14 columns untouched|5%|
|2|Data cleaning \& preparation|`Clean\_Data`|Duplicate check, blank coupons labelled, text trimmed, types verified, 9 helper columns added|10%|
|3|Pivot tables|`Pivot\_Tables`|6 summaries with grand totals and reconciliation checks|15%|
|4|Advanced formulas|`Advanced\_Formulas`|Order lookup, reverse lookup, customer tiers, product master, nested insights|15%|
|5|Data visualisation|`Visualizations` + `Pivot\_Tables`|6 charts; data bars, colour scales, icon sets, status colours|20%|
|6|Interactive dashboard|`Dashboard` (engine: `Dash\_Calc`)|KPI cards, 5 charts, drop-down \& date filters, dynamic named ranges|25%|
|7|Documentation|`README`|Organisation, naming, instructions|10%|

\---

## Dashboard

**Filters** (yellow cells, row 6): From date, To date, Product, Payment method, Referral source, Order size. Set any drop-down back to `All` to remove it.

**KPI cards:** Net Revenue · Total Orders · Avg Order Value · Units Sold · Cancellation Rate · Return Rate

**Charts:**

1. Monthly Net Revenue Trend (line)
2. Order Status Mix (doughnut)
3. Net Revenue by Product (bar)
4. Net Revenue by Payment Method (bar)
5. Net Revenue by Referral Source (bar)

Everything recalculates automatically when a filter changes. The monthly trend always shows all months; months outside the date range show 0.

\---

## Pivot Summaries (`Pivot\_Tables`)

|ID|Summary|
|-|-|
|PT1|Sales by Product (orders, units, gross, net, avg order value, % share)|
|PT2|Net Revenue: Product × Year|
|PT3|Order Status Summary|
|PT4|Payment Method × Order Status (order count)|
|PT5|Referral Source × Coupon Code (net revenue)|
|PT6|Quarterly Trend with QoQ growth|

The tables use `SUMIFS` / `COUNTIFS` on dynamic named ranges, so they update instantly with the data.

\---

## Advanced Formulas (`Advanced\_Formulas`)

1. **VLOOKUP** – find an order by `OrderID`
2. **INDEX-MATCH** – reverse lookup by `TrackingNumber` (lookup column is to the right of the result)
3. **Customer 360** – `COUNTIF` / `SUMIF` with a nested-IF tier (Gold / Silver / Bronze)
4. **Product master** – category mapping, `AVERAGEIFS` / `MINIFS` / `MAXIFS`, price bands
5. **Nested insights** – top product, top referral source, largest order, fulfilment health, pending high-priority orders

Thresholds (order size, priority, customer tier) live in blue-on-yellow input cells, so nothing is hard-coded.

\---

## Data Dictionary (`Clean\_Data`)

**Source fields**

|Column|Type|Meaning|
|-|-|-|
|OrderID|Text|Unique order identifier|
|Date|Date|Order date|
|CustomerID|Text|Customer identifier|
|Product|Text|Product ordered (7 products)|
|Quantity|Integer|Units in the order|
|UnitPrice|Number|Price per unit|
|ShippingAddress|Text|Delivery address|
|PaymentMethod|Text|Online, Cash, Credit Card, Debit Card, Gift Card|
|OrderStatus|Text|Delivered, Shipped, Pending, Cancelled, Returned|
|TrackingNumber|Text|Shipment tracking ID|
|ItemsInCart|Integer|Items in the cart at checkout|
|CouponCode|Text|Coupon applied (blank → `No Coupon`)|
|ReferralSource|Text|Marketing channel|
|TotalPrice|Number|Order value = Quantity × UnitPrice|

**Derived fields**

|Column|Formula idea|Purpose|
|-|-|-|
|Year / Quarter / MonthStart|`YEAR`, `ROUNDUP`, `DATE`|Time grouping|
|CalcTotal / TotalCheck|`ROUND(E\*F,2)`; `IF`|Validate `TotalPrice`|
|NetRevenue|`IF(OR(status="Cancelled","Returned"),0,Total)`|Revenue actually kept|
|OrderSize|Nested `IF` with `thr\_Large` / `thr\_Medium`|Small / Medium / Large|
|Category|`VLOOKUP` to product master|Electronics / Furniture|
|Priority|Nested `IF` with `thr\_Priority`|High-value pending orders|

\---

## Data Cleaning \& Integrity Checks

Cleaning actions: whitespace trimmed in text fields; exact-duplicate rows and duplicate OrderIDs removed (none found); blank `CouponCode` replaced with `No Coupon` (309 rows); dates and numbers verified as true date/number types. `Raw\_Data` is kept unchanged.

The `README` sheet contains **16 live integrity checks** (row counts, duplicates, blanks, date types, price validation, pivot-to-data reconciliation, dashboard reconciliation). **All 16 pass.**

\---

## How to Use

1. Open **`Dashboard`** and change the yellow cells in row 6 to filter the report.
2. KPI cards, the insight bar and all five charts update automatically.
3. On **`Advanced\_Formulas`**, edit the blue-on-yellow cells (OrderID, TrackingNumber, CustomerID, thresholds) to test the lookups.
4. To **add new orders**, append rows to the `Clean\_Data` table (`tblOrders`). Helper formulas fill down automatically and the dynamic named ranges grow with the data.
5. *Optional:* for a native PivotTable and Slicer, click any cell in `Clean\_Data` → **Insert → PivotTable**, then **PivotTable Analyze → Insert Slicer** (Product, PaymentMethod, ReferralSource) and **Insert Timeline** (Date).

\---

## Naming Conventions

* **Sheets:** `Title\_Case\_With\_Underscores`
* **Named ranges:** `n\_` data columns · `f\_` dashboard filters · `thr\_` thresholds · `rng\_` lookup tables
* **Colours:** blue text on yellow = input · black = formula · navy header = source field · green header = derived field

\---

## Assumptions \& Notes

* **Net Revenue** = `TotalPrice` of orders that are not Cancelled or Returned. **Gross Value** includes every order.
* Product category (Electronics / Furniture), price bands, order-size thresholds and customer tiers are analyst-defined assumptions held in input cells.
* Fulfilment health bands (30% / 50%) are illustrative.
* The dataset has no currency field.
* 2025 is a half-year (Jan–Jun), so year-over-year comparisons for 2025 are not like-for-like.
* `MINIFS` / `MAXIFS` require Excel 2019 / Microsoft 365 or later; in older versions the Min / Max Unit Price columns in the product master show `#NAME?`.

\---

## Requirements

Microsoft Excel 2019 or Microsoft 365 (recommended). No macros or external add-ins are used.

