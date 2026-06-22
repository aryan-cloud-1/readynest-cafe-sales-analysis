# Cafe Sales Analysis — ReadyNest Corp Week 1

This is my Week 1 submission for the ReadyNest Corp Data Analytics Internship. The task was to take a messy, real-world-style dataset and turn it into something actually useful — clean data, proper analysis, and an interactive dashboard.

## What's in this repo

```
cafe_sales_analysis.ipynb   # main notebook — cleaning + full EDA
dirty_cafe_sales.csv        # the original raw dataset (messy)
cleaned_cafe_sales.csv      # output after cleaning, used for Tableau
```

## The Dataset

10,000 cafe transactions with columns for Item, Quantity, Price Per Unit, Total Spent, Payment Method, Location, and Transaction Date. The "dirty" version had missing values scattered everywhere, plus "ERROR" and "UNKNOWN" strings mixed in as placeholders — so the first job was figuring out what was actually salvageable before touching any analysis.

## What I did

**Data Cleaning**

The dataset had three types of missing data: real NaN, the string "ERROR", and the string "UNKNOWN" — all normalized to NaN first. From there, the interesting part was cross-imputation: since every item in the cafe has a fixed price (Coffee is always $2, Salad always $5, etc.), I built a price lookup table and used the relationship `Total Spent = Quantity × Price Per Unit` to recover a lot of rows that looked broken at first glance.

- Quantity missing: dropped from 479 → 23 unrecoverable
- Price Per Unit: 533 → 6 unrecoverable  
- Total Spent: 502 → 23 unrecoverable
- Item: 969 → 480 unrecoverable (Cake/Juice share the same price, Sandwich/Smoothie share the same price — so those couldn't be reverse-mapped safely)

Payment Method and Location had no way to be derived from other columns, so those stayed as "Not Specified" rather than guessing. About 480 fully unrecoverable rows (no item, no quantity, no price, no total) were dropped since they carried zero usable information.

**EDA**

Once the data was clean, I ran through:
- Descriptive stats (mean, median, std for Quantity, Price Per Unit, Total Spent)
- Univariate analysis — histograms, box plots, bar charts per category
- Bivariate analysis — scatter plot (Quantity vs Total Spent), correlation heatmap, revenue by item/location/payment method
- Time-based trends — monthly revenue, day of week breakdown

**Key findings**

- Salad and Sandwich bring in the most revenue despite not being the most frequently ordered items — higher price per unit makes up for lower volume
- Quantity per order drives most of the variation in basket size, not which item someone picks
- In-store and Takeaway channels are nearly identical in average order value ($9.03 vs $8.95) — no meaningful difference between the two
- About 26-33% of transactions were missing Payment Method or Location data in the raw file, which points to a data capture issue at the POS level rather than anything wrong with the analysis

## Dashboard

Built in Tableau Public. Includes:
- KPI cards (Total Revenue, Total Transactions, Average Order Value)
- Revenue by Item (bar chart, sorted)
- Monthly Revenue Trend (line chart + forecast)
- Revenue by Location and Payment Method (separate bar charts)
- Transaction Share by Item (pie chart)
- Quantity vs Total Spent (scatter plot with trend line)
- AOV by Location — A/B comparison
- Filters for Item, Month, Location, Payment Method

**Tableau Public link:** _[add your link here]_

## Tools used

- Python (pandas, numpy, matplotlib, seaborn)
- Jupyter Notebook
- Tableau Public

## Notes

The prices in this dataset are unitless in the raw CSV — no currency was specified in the original source. I used $ for display purposes in the dashboard since the pricing structure ($2 coffee, $4 sandwich, $5 salad) matches a typical US cafe menu more closely than any other currency.
