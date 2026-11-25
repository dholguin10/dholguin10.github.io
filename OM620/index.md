# OM 620 – Safety Stock Analysis

[view the full repository on GitHub](https://github.com/dholguin10/OM620)

Assignments 1 and 2 for OM 620 (Tools and Technologies for Analytics), CSUSM, Fall 2026.
Group: Daniel Ramirez, Diego Holguin, Jorge Hernandez Ramirez, Suleiman Sahawneh.

The goal is to calculate safety stock for finished goods that are made to stock (MTS).

## Repository Structure
- `data/`: transaction data and its data dictionary
- `notebooks/`: one notebook with Assignments 1 and 2

## Assignment 1: Data Cleaning
Steps:
1. Renamed 3 columns to match the metadata (e.g., `sku_numb3r` → `sku_number`).
2. Fixed 10 lead times that were 100x too big (2800 → 28).
3. Made 10 negative unit prices positive.
4. Dropped 4,109 rows with no SKU and filled other blanks with "Unknown".
5. Added a `country` column from the manufacturing site code.
6. Filtered to finished goods made to stock (FG + MTS).

Findings:
- 267,212 FG/MTS transactions, 375 SKUs, 18 sites, 54 divisions.
- Largest order: 208 units.

## Assignment 2: Safety Stock
Steps:
1. Re-ran the Assignment 1 cleaning and made negative order quantities positive (per professor feedback).
2. Grouped by SKU to get min, max, mean, median, variance, and standard deviation of quantity, plus average lead time.
3. Calculated safety stock at 75%, 90%, and 95% service levels.
4. Side quest: compared the normal formula with an empirical (quantile) method.

Findings (95% service level):
- Largest safety stock: SKU `084B19C4` (353 units).
- Smallest: SKU `FE8A9BBE` (33 units).
- Average: about 106 units per SKU.
- The empirical method gives a slightly lower average (about 103 units).

## How to Run
1. Unzip `data/transaction_data.csv.zip` inside the `data` folder.
2. Open `notebooks/assignment_1_and_2.ipynb` in Jupyter or VS Code.
3. Run all cells from top to bottom.

## Tools Used
Python, pandas, NumPy, SciPy
