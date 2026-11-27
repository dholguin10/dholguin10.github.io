# OM 620 – Safety Stock Analysis

[view the full repository on GitHub](https://github.com/dholguin10/OM620)

# OM 620 – Safety Stock Analysis

OM 620 (Tools and Technologies for Analytics), CSUSM, Fall 2026

Group: Daniel Ramirez, Diego Holguin, Jorge Hernandez Ramirez, Suleiman Sahawneh

## What This Project Is About

A company needs to keep extra items on the shelf so it does not run out when orders come in. This extra amount is called **safety stock**.

In this project we took a large file of company orders, cleaned it up, and used it to figure out how much safety stock each product needs.

We did this in two parts:

- **Assignment 1:** Clean the data so it is correct and easy to use.
- **Assignment 2:** Use the clean data to calculate safety stock.

This README walks through every step, from opening the files to getting the final answer.

## What Is in This Repository

- **data** folder: the order data and a file that explains each column
- **notebooks** folder: one notebook with all of our work for Assignments 1 and 2
- **images** folder: the screenshots used in this README

## Tools We Used

- **VS Code:** a free program for writing and running code
- **Python:** the programming language we used
- **Jupyter Notebook:** a file type (ending in .ipynb) that mixes code, results, and notes on one page
- **pandas, NumPy, and SciPy:** Python add-ons for working with tables of data and doing math

---

## Step 1: Get the Files Ready

1. Download this repository from GitHub (green **Code** button, then **Download ZIP**).
2. Unzip the download.
3. Open the **data** folder and unzip **transaction_data.csv.zip**. The data file was too big to upload to GitHub without zipping it.

![Screenshot: the project folders after unzipping](https://raw.githubusercontent.com/dholguin10/OM620/main/images/Step1-folders.png)

## Step 2: Open the Notebook in VS Code

1. Open VS Code.
2. Click **File**, then **Open Folder**, and choose the project folder.
3. On the left side, open the **notebooks** folder and click **assignment_1_and_2.ipynb**.
4. The notebook opens. Gray boxes are code. White boxes are notes.
5. To run a code box, click it and press **Shift + Enter**. The result shows up under the box.

![Screenshot: the notebook open in VS Code](https://raw.githubusercontent.com/dholguin10/OM620/main/images/step2-vscode.png)

## Step 3: Load the Data

We loaded two files:

- **transaction_data.csv:** the orders. It has 728,463 rows, and each row is one order.
- **meta_data.csv:** a short list that explains what each column means.

Each order tells us the product (called a SKU), what type of item it is, how long it takes to arrive (lead time), the price, where it was made, the date, and how many were ordered.

![Screenshot: the first few rows of the order data](https://raw.githubusercontent.com/dholguin10/OM620/main/images/step3-load-data.png)

## Step 4: Fix the Column Names

Three column names were typed wrong or did not match the others. We renamed them:

- **sku_numb3r** became **sku_number** (it had a 3 instead of an e)
- **Manufacturing Site** became **manufacturing_site**
- **Order-Quantity** became **order_quantity**

Now every column name is lowercase and uses underscores, just like the meta data file.

![Screenshot: the column names after fixing them](https://raw.githubusercontent.com/dholguin10/OM620/main/images/step4-columns.png)

## Step 5: Find and Fix Strange Numbers

We used a summary of the numbers to look for anything that did not make sense. We found three problems.

**Problem 1: Lead times that were way too long**

- 10 orders had lead times of 1,400, 2,100, or 2,800 days. Normal lead times are 14, 21, or 28 days.
- These were 100 times too big, so someone likely typed two extra zeros.
- **Fix:** we changed them back to 14, 21, and 28.

**Problem 2: Negative prices**

- 10 orders had a price below zero. A price cannot be negative.
- The same products had the normal, positive price in their other orders.
- **Fix:** we removed the minus sign.

**Problem 3: Negative order amounts**

- Some orders had a negative quantity.
- In Assignment 1 we first thought these were returns. Our professor's feedback said they were most likely typing mistakes, like the prices.
- **Fix:** we removed the minus sign from these too.

We also found 44 orders that were exact copies of other orders. A product can be ordered more than once on the same day, so we kept them.

![Screenshot: the number summary before the fixes](https://raw.githubusercontent.com/dholguin10/OM620/main/images/step5-before.png)

![Screenshot: the number summary after the fixes](https://raw.githubusercontent.com/dholguin10/OM620/main/images/step5-after.png)

## Step 6: Deal with Missing Information

Some rows had blank spots. Here is what we did:

- **Missing product number (4,109 rows):** We deleted these rows. Without a product number, we cannot tell which product the order is for.
- **Missing item type, stocking type, factory, or division:** We filled these in with the word "Unknown." This let us keep the rest of the row instead of throwing it away.

After this step we had 724,354 rows and no blanks left.

![Screenshot: count of blanks in each column after the fix](https://raw.githubusercontent.com/dholguin10/OM620/main/images/step6-missing.png)

## Step 7: Add a Country Column

Each factory code starts with a country code, for example US-1F or JP-C7. We used those first letters to make a new **country** column:

- US = USA
- CA = Canada
- PL = Poland
- JP = Japan
- Anything else = Unknown

![Screenshot: the new country column](https://raw.githubusercontent.com/dholguin10/OM620/main/images/step7-county.png)

## Step 8: Keep Only the Products We Need

Safety stock is for products that are finished and kept on the shelf before a customer orders them. So we kept only rows that were:

- **FG** (finished good), and
- **MTS** (make to stock)

This left us with:

- 267,212 orders
- 375 different products
- 18 factories
- 54 divisions

The largest single order was 208 units.

![Screenshot: the filtered data](https://raw.githubusercontent.com/dholguin10/OM620/main/images/step8-filter.png)

## Step 9: Summarize Each Product

Next we grouped the orders by product. For each of the 375 products we found:

- the smallest order
- the largest order
- the average order
- the middle order (median)
- how much the orders jump around (variance and standard deviation)
- the average lead time

The **standard deviation** is the key number. A high number means orders change a lot from one to the next, so the product needs more safety stock.

We noticed the products fell into two groups:

- **225 products** with small orders (about 9 units each) and short lead times (about 15 days)
- **150 products** with big orders (about 77 units each) and long lead times (about 26 days)

![Screenshot: the summary table for each product](https://raw.githubusercontent.com/dholguin10/OM620/main/images/step9-summary.png)

## Step 10: Calculate Safety Stock

We used this formula for each product:

**Safety stock = service level number × standard deviation × square root of lead time**

The **service level** is how often you want to have enough stock. We tried three levels:

- **75%** of the time (service level number 0.674)
- **90%** of the time (service level number 1.282)
- **95%** of the time (service level number 1.645)

The higher the service level, the more safety stock you need. Average safety stock per product:

- 75% service level: about 44 units
- 90% service level: about 83 units
- 95% service level: about 106 units

![Screenshot: safety stock for each product at the three service levels](https://raw.githubusercontent.com/dholguin10/OM620/main/images/step10-safety-stock.png)

## Step 11: Look at the Results (95% Service Level)

- **Largest safety stock:** product 084B19C4 needs about 353 units. Its orders change the most of any product.
- **Smallest safety stock:** product FE8A9BBE needs about 33 units. Its orders are steady and its lead time is short.
- **Average:** about 106 units per product.

The largest is about 11 times the smallest. This shows why one number for every product would not work. Some products would run out and others would have too much stock.

We also checked whether orders of 0 units changed our answers. Taking them out moved the average by less than 1 unit, so we kept them.

![Screenshot: largest, smallest, and average safety stock](https://raw.githubusercontent.com/dholguin10/OM620/main/images/step11-results.png)

## Step 12: Try a Second Method (Side Quest)

The formula in Step 10 assumes orders follow a bell-curve shape. Some products do not. So we tried a second method that uses each product's real order history instead of the bell curve.

- With this method, the average safety stock was about 103 units, a little lower than 106.
- Product 084B19C4 was still the largest, but it went down from 353 to about 280 units.
- The smallest product changed to A6AB6A6D, at about 34 units.

This shows the formula can overestimate safety stock for products whose orders do not follow a bell curve.

![Screenshot: comparing the two methods](https://raw.githubusercontent.com/dholguin10/OM620/main/images/step12-compare.png)

---

## Final Result

We started with 728,463 messy orders. After cleaning them and keeping only finished products that are made to stock, we calculated safety stock for 375 products.

At a 95% service level:

- Each product needs about **106 units** of safety stock on average.
- The amount ranges from about **33 units** to about **353 units**, depending on how much each product's orders change.
- A second method using real order history gave a similar average of about **103 units**.

## References

- OM 620 course lectures, notebooks, and assignment instructions (CSUSM, Fall 2026)
- Our professor's feedback on Assignment 1
- SciPy documentation: https://docs.scipy.org

## How to Run
1. Unzip `data/transaction_data.csv.zip` inside the `data` folder.
2. Open `notebooks/assignment_1_and_2.ipynb` in Jupyter or VS Code.
3. Run all cells from top to bottom.

## Tools Used
Python, pandas, NumPy, SciPy
