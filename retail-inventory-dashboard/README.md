# Retail Inventory & Stock-Out Risk Dashboard

Excel (Power Pivot) and Power BI dashboards answering one question: **which SKU-store combinations are at stock-out risk right now, and what's the reorder priority?**

## Why synthetic data
Built on a generated dataset (3 stores, 50 SKUs, 365 days, 54,300 rows) instead of a public Kaggle file, so every number is explainable — the seasonal demand curve, the lead-time-driven reorder logic, and the ~8.9% baseline stockout rate are all known business rules, not unexplained quirks in someone else's data.

## Data model
Star schema — one fact table, four dimensions:

- **FactInventory** (54,300 rows) — daily snapshot per store x SKU: units demanded, units sold, units received, ending stock, stockout flag
- **DimProduct** — SKU, category, unit price, reorder point, safety stock, supplier link
- **DimStore** — 3 stores across Mumbai/Vasai/Virar
- **DimSupplier** — lead time per supplier
- **DimDate** — calendar attributes for time intelligence

Relationships: FactInventory connects many-to-one to DimProduct, DimStore, and DimDate. DimProduct connects many-to-one to DimSupplier (snowflake).

## Key DAX measures
```
Total Units Sold := SUM(FactInventory[UnitsSold])

Stockout Rate := DIVIDE(SUM(FactInventory[StockoutFlag]), COUNTROWS(FactInventory))

At Risk SKUs Today := CALCULATE(
    DISTINCTCOUNT(FactInventory[SKU]),
    FILTER(
        FactInventory,
        FactInventory[EndingStock] <= RELATED(DimProduct[ReorderPoint])
        && FactInventory[Date] = MAX(FactInventory[Date])
    )
)
```

**Design note:** the "At Risk SKUs" measure originally checked the *entire* 365-day history for any day a SKU dipped to its reorder point — since the reorder logic guarantees every SKU crosses that threshold at least once a year (that's literally what triggers a restock), this returned all 50 SKUs as "at risk," which is correct math but a meaningless business answer. Adding `Date = MAX(FactInventory[Date])` restricts the check to the latest day only, turning a historical tally into a current-state snapshot (28 of 50 SKUs).

## Dashboard features
- 3 KPI cards: overall stockout rate, total units sold, SKUs at risk today
- Category x Store stockout-rate matrix
- Monthly demand trend (shows Oct-Nov festive-season spike)
- SKU-level reorder-priority table, sorted by ending stock ascending
- Store and Category slicers cross-filtering every visual

## Files
- `Retail_Inventory_Model.xlsx` — Excel version (Power Pivot data model, PivotTables, slicers)
- `Retail_Inventory_Dashboard.pbix` — Power BI version (same star schema, native visuals)
