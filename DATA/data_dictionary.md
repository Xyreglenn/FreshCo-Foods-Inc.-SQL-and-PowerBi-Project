# Data Dictionary — FreshCo Foods Inc. Inventory Dataset

**File:** `inventory_data.csv`
**Rows:** 8,921 (one row per pallet)
**Snapshot date:** June 17, 2026
**Note:** All data is fully anonymized. No real company, client, or product information is present.

---

## Column Definitions

| Column | Data Type | Description | Example |
|---|---|---|---|
| `PALLET_ID` | Text | Unique pallet identifier (synthetic) | PLT-00001 |
| `SKU` | Text | Stock Keeping Unit code (anonymized) | SKU-A001 |
| `DESCRIPTION` | Text | Product description (generalized — no brand names) | Frozen Chicken Breast 1kg |
| `CATEGORY` | Text | Product category | Poultry / Beef / Pork / Dairy / Produce |
| `SUBCATEGORY` | Text | Product subcategory / cut | Breast / Sirloin / Belly / Milk |
| `BATCH_NUMBER` | Text | Batch/lot production identifier (hashed) | BCH-A4F2C1 |
| `LOT_NUMBER` | Text | Lot number (sequential synthetic) | LOT-0001 |
| `SUPPLIER_CODE` | Text | Anonymized supplier reference | SUP-001 through SUP-005 |
| `MFG_DATE` | Date (YYYY-MM-DD) | Manufacturing / production date | 2026-01-15 |
| `EXP_DATE` | Date (YYYY-MM-DD) | Expiry / best-before date | 2027-01-15 |
| `DAYS_TO_EXPIRY` | Integer | Days remaining until expiry as of snapshot date (negative = already expired) | 212 / -7 |
| `AGING_DAYS` | Integer | Days elapsed since manufacture as of snapshot date | 153 |
| `SHELF_LIFE_DAYS` | Integer | Total shelf life of the product in days | 365 |
| `WAREHOUSE_CODE` | Text | Warehouse identifier | WH-A / WH-B / WH-C / WH-D / WH-E |
| `WAREHOUSE_NAME` | Text | Descriptive warehouse name | Cold Storage A - Poultry |
| `LOCATION` | Text | Specific bin/slot location (synthetic) | WH-A-LOC-1234 |
| `QTY_CASES` | Integer | Number of cases/boxes on this pallet | 45 |
| `TOTAL_KG` | Decimal | Total weight of the pallet in kilograms | 49.5 |
| `UNIT_COST` | Decimal (₱) | Cost per case/unit in Philippine Peso | 183.50 |
| `TOTAL_VALUE` | Decimal (₱) | Total inventory value of the pallet (QTY × UNIT_COST) | 8,257.50 |
| `UOM` | Text | Unit of measure | KG / PC |
| `STATUS` | Text | Current WMS status of the pallet | AVAILABLE / ALLOCATED / HOLD / PICKED |
| `EXPIRY_RISK` | Text | Pre-computed expiry risk bucket (sorted with number prefix) | 1. Expired / 2. Critical (0-30d) / 3. Warning (31-90d) / 4. Watch (91-180d) / 5. Healthy (180d+) |
| `RR_DATE` | Date (YYYY-MM-DD) | Receiving/inbound date (date pallet arrived at warehouse) | 2026-01-18 |
| `PUTAWAY_DATE` | Date (YYYY-MM-DD) | Date pallet was put into its storage location | 2026-01-19 |

---

## Status Definitions

| Status | Meaning |
|---|---|
| `AVAILABLE` | Pallet is in storage and available for dispatch or use |
| `ALLOCATED` | Pallet has been reserved for an order but not yet picked |
| `PICKED` | Pallet has been physically pulled from storage for an order |
| `HOLD` | Pallet is quarantined or blocked — cannot be dispatched |

---

## Expiry Risk Bucket Logic

Computed from `DAYS_TO_EXPIRY` as of snapshot date (2026-06-17):

| Bucket | Condition | Meaning |
|---|---|---|
| `1. Expired` | DAYS_TO_EXPIRY < 0 | Past expiry date — potential compliance risk if still AVAILABLE |
| `2. Critical (0-30d)` | 0 ≤ DAYS_TO_EXPIRY ≤ 30 | Expiring within 30 days — immediate action required |
| `3. Warning (31-90d)` | 31 ≤ DAYS_TO_EXPIRY ≤ 90 | Expiring within 90 days — monitor closely |
| `4. Watch (91-180d)` | 91 ≤ DAYS_TO_EXPIRY ≤ 180 | Expiring within 6 months — plan ahead |
| `5. Healthy (180d+)` | DAYS_TO_EXPIRY > 180 | More than 6 months remaining — no immediate concern |

---

## Warehouse Reference

| Warehouse Code | Name | Slot Capacity | Primary Products |
|---|---|---|---|
| WH-A | Cold Storage A - Poultry | 4,120 | Poultry SKUs |
| WH-B | Cold Storage B - Beef & Pork | 2,520 | Beef and Pork SKUs |
| WH-C | Cold Storage C - Dairy | 1,264 | Dairy and Produce SKUs |
| WH-D | Freezer D - Mixed | 1,992 | Mixed products |
| WH-E | Staging / Receiving | 600 | Inbound staging area |

---

## Dataset Summary

| Metric | Value |
|---|---|
| Total pallets | 8,921 |
| Total boxes | 445,289 |
| Total inventory value | ₱102,147,714.94 |
| Expired pallets | 87 |
| Critical pallets (0–30d) | 1,004 |
| Warehouses | 5 |
| Unique SKUs | 17 |
| Date range (MFG) | 2025 – 2026 |
