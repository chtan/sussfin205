---
jupytext:
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.16.1
kernelspec:
  display_name: Python 3 (ipykernel)
  language: python
  name: python3
---

+++ {"editable": true, "slideshow": {"slide_type": ""}}

# Financial Ratios

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
tags: [remove-input]
---
from IPython.display import HTML
html = HTML("""
<style>
img {
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 100%;
}
</style>
""")
display(html)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

## Categories of Ratios

Financial ratios can be broadly categorized into several classes based on the aspect of a company's performance or financial position that they measure. Here are some common classes of financial ratios:

1. **Liquidity Ratios**: These ratios assess a company's ability to meet its short-term obligations. Examples include the current ratio and the quick ratio.

2. **Profitability Ratios**: These ratios evaluate a company's ability to generate profits relative to its revenue, assets, equity, or other measures. Examples include gross profit margin, net profit margin, return on assets (ROA), and return on equity (ROE).

3. **Activity or Efficiency Ratios**: These ratios measure how effectively a company utilizes its assets to generate revenue. Examples include asset turnover ratio, inventory turnover ratio, and accounts receivable turnover ratio.

4. **Leverage or Solvency Ratios**: These ratios assess a company's ability to meet its long-term financial obligations. They measure the extent to which a company relies on debt financing. Examples include debt-to-equity ratio, debt ratio, and interest coverage ratio.

5. **Market Value Ratios**: These ratios evaluate the market's perception of a company's performance and potential. Examples include price-to-earnings (P/E) ratio, price-to-book (P/B) ratio, and earnings per share (EPS).

6. **Coverage Ratios**: These ratios measure the company's ability to cover its fixed charges such as interest and lease payments. Examples include the times interest earned ratio and debt service coverage ratio.

7. **Growth Ratios**: These ratios assess the rate at which various aspects of a company's operations are growing over time. Examples include revenue growth rate and earnings growth rate.

Each class of ratios provides insights into different aspects of a company's financial health and performance, and they are often used together to form a comprehensive analysis.rowth rate.

+++ {"editable": true, "slideshow": {"slide_type": ""}}

(common_financial_ratios)=
## Common Financial Ratios

The common ratios in these categories are:

1. **Liquidity Ratios**:
   - **Current Ratio**: Current Assets / Current Liabilities
   - **Quick Ratio (Acid-Test Ratio)**: (Current Assets - Inventory) / Current Liabilities

2. **Profitability Ratios**:
   - **Gross Profit Margin**: (Gross Profit / Revenue) * 100
   - **Net Profit Margin**: (Net Profit / Revenue) * 100
   - **Return on Assets (ROA)**: Net Income / Average Total Assets
   - **Return on Equity (ROE)**: Net Income / Average Shareholders' Equity

3. **Activity or Efficiency Ratios**:
   - **Asset Turnover Ratio**: Revenue / Average Total Assets
   - **Inventory Turnover Ratio**: Cost of Goods Sold / Average Inventory
   - **Accounts Receivable Turnover Ratio**: Net Credit Sales / Average Accounts Receivable

4. **Leverage or Solvency Ratios**:
   - **Debt-to-Equity Ratio**: Total Debt / Total Equity
   - **Debt Ratio**: Total Debt / Total Assets
   - **Interest Coverage Ratio**: Earnings Before Interest and Taxes (EBIT) / Interest Expense

5. **Market Value Ratios**:
   - **Price-to-Earnings (P/E) Ratio**: Market Price per Share / Earnings per Share (EPS)
   - **Price-to-Book (P/B) Ratio**: Market Price per Share / Book Value per Share
   - **Earnings Per Share (EPS)**: Net Income / Weighted Average Number of Common Shares Outstanding

6. **Coverage Ratios**:
   - **Times Interest Earned Ratio**: Earnings Before Interest and Taxes (EBIT) / Interest Expense
   - **Debt Service Coverage Ratio**: Net Operating Income / Total Debt Service

7. **Growth Ratios**:
   - **Revenue Growth Rate**: ((Current Year Revenue - Prior Year Revenue) / Prior Year Revenue) * 100
   - **Earnings Growth Rate**: ((Current Year Earnings - Prior Year Earnings) / Prior Year Earnings) * 100

+++ {"editable": true, "slideshow": {"slide_type": ""}}

## Why Ratios?

A ratio is an expression of the form 

$$\frac{A}{B}$$

For 2 arbitrary companies with their respective field values $(A,B)$ and $(A',B')$, it would generally be hard to compare them pitting $(A,B)$ against $(A',B')$ because the scale of these values may differ significantly, which shouldn't be a surprise because all companies are rather different from each other.

Ratios remove the scale difference and allow different companies to be compared via their ratios (viz. $\frac{A}{B}$ and $\frac{A'}{B'}$ in the current example).

Thus, it is absurd to be comparing the net income of MSFT and a small company. It may be hard to understand how a comparison between the revenue of KO and MSFT can make sense since they are from different industries. However, one can equally talk about and appreciate the profit margin of all companies on an equal footing as it intuitively translates to how efficient a company is in turning its resources and constraints into profits.

A ratio is sometimes called a *multiple*, due to the relationship:

$$\frac{A}{B} \times B = A$$

In other words, we may say that the quantity $A$ is $\frac{A}{B}$ times of $B$.

A common example of this usage occurs with the PE ratio. It is common to say that the stock of a company is selling at $n$ times its earnings. Here, $n$ is the PE ratio, and 

$$n \times \text{Earnings} = \frac{\text{Price}}{\text{Earnings}} \times \text{Earnings} = \text{Price}$$

Used in this way, the PE ratio conveys the idea of whether the stock of a company is cheap or expensive. A high PE ratio signifies "expensive", while a low PE ratio signifies "cheap". 

Thus, the PE ratio enables companies to be compared along the dimension of expensive/cheap.

+++ {"editable": true, "slideshow": {"slide_type": ""}}

## Calculating Ratios from the Financial Statements

+++ {"editable": true, "slideshow": {"slide_type": ""}}

We're going to compute all the FY2023 ratios listed in the [Common Financial Ratios](#common_financial_ratios) section from the income statement:

```{image} img/ko_ics.png
:width: 70%
:align: center
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

the balance sheet:

```{image} img/ko_bs.png
:width: 67%
:align: center
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

and the cash flow statement

```{image} img/ko_cfs.png
:width: 65%
:align: center
```

of The Coca-Cola Company.

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
tags: [hide-input]
---
# We'll download the data from the Google Sheet and compute the ratios using Python.
# We will also explain the worksheet formulas required for the ratio computations.

import openpyxl
import pandas as pd
import requests
from io import BytesIO
from IPython.display import display, Markdown

spreadsheetId = "1OCqC9k0lz1kRZ-RXYRMgI8ebjHum1xHEfAYKl4t3Nos"
url = "https://docs.google.com/spreadsheets/export?exportFormat=xlsx&id=" + spreadsheetId
res = requests.get(url)
data = BytesIO(res.content)
xlsx = openpyxl.load_workbook(filename=data)
for name in xlsx.sheetnames:
    if name == 'Income Statement':
        df_ics = pd.read_excel(data, sheet_name=name)
        break
for name in xlsx.sheetnames:
    if name == 'Balance Sheet':
        df_bs = pd.read_excel(data, sheet_name=name)
        break
for name in xlsx.sheetnames:
    if name == 'Cash Flow Statement':
        df_cfs = pd.read_excel(data, sheet_name=name)
        break
        
#rownum = 2
#print(df_bs.iloc[rownum-2][2023])
ratios = {}
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

### Current Ratio

This is the ratio: Current Assets / Current Liabilities

This ratio can be computed from the balance sheet.

The Current Assets is

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
rownum = 8
currentAssets = df_bs.iloc[rownum-2][2023]
print(currentAssets)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The Current Liabilities is

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
rownum = 23
currentLiabilities = df_bs.iloc[rownum-2][2023]
print(currentLiabilities)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The Current Ratio is

$$\frac{26700000000}{23600000000} \approx 1.13$$

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
currentRatio = currentAssets / currentLiabilities
print(currentRatio)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The worksheet formula for computing this ratio is

<p style="text-align:center;"><code>='Balance Sheet'!F8/'Balance Sheet'!F23</code></p>

+++ {"editable": true, "slideshow": {"slide_type": ""}}

Let's collate the value for an overall listing later:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
ratios['Current Ratio'] = currentRatio
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

### Quick Ratio

This is the ratio: (Current Assets - Inventory) / Current Liabilities

This ratio can be computed from the balance sheet.

We have earlier found the Current Assets and the Current Liabilities.

Inventory is

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
rownum = 6
inventory = df_bs.iloc[rownum-2][2023]
print(inventory)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The Quick Ratio is

$$\frac{26700000000 - 4420000000}{23600000000} \approx 0.94$$

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
quickRatio = (currentAssets - inventory) / currentLiabilities
print(quickRatio)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The worksheet formula for computing this ratio is

<p style="text-align:center;"><code>=('Balance Sheet'!F8-'Balance Sheet'!F6)/'Balance Sheet'!F23</code></p>

+++ {"editable": true, "slideshow": {"slide_type": ""}}

Let's collate the value for an overall listing later:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
ratios['Quick Ratio'] = quickRatio
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

### Gross Profit Margin

This is the ratio: (Gross Profit / Revenue) * 100

This ratio can be computed from the income statement.

The Gross Profit is

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
rownum = 4
grossProfit = df_ics.iloc[rownum-2][2023]
print(grossProfit)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The Revenue is

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
rownum = 2
revenue = df_ics.iloc[rownum-2][2023]
print(revenue)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The Gross Profit Margin is

$$\frac{27200000000}{45800000000} \times 100 \approx 59.4$$

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
grossProfitMargin = grossProfit / revenue
print(grossProfitMargin)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The worksheet formula for computing this ratio is

<p style="text-align:center;"><code>='Income Statement'!F4/'Income Statement'!F2</code></p>

+++ {"editable": true, "slideshow": {"slide_type": ""}}

Let's collate the value for an overall listing later:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
ratios['Gross Profit Margin'] = grossProfitMargin
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

### Net Profit Margin

This is the ratio: (Net Profit / Revenue) * 100

This ratio can be computed from the income statement.

The Net Profit is

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
rownum = 24
netIncome = netProfit = df_ics.iloc[rownum-2][2023]
print(netProfit)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The Net Profit Margin is

$$\frac{10700000000}{45800000000} \times 100 \approx 23.4$$

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
netProfitMargin = netProfit / revenue
print(netProfitMargin)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The worksheet formula for computing this ratio is

<p style="text-align:center;"><code>='Income Statement'!F24/'Income Statement'!F2</code></p>

+++ {"editable": true, "slideshow": {"slide_type": ""}}

Let's collate the value for an overall listing later:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
ratios['Net Profit Margin'] = netProfitMargin
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

### ROA

This is the ratio: Net Income / Average Total Assets

This ratio can be computed from the income statement and the balance sheet (2 years).

The Average Total Assets is

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The ROA is

$$\frac{10700000000}{95250000000} \approx 0.11$$

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
rownum = 17
totalAssets23 = df_bs.iloc[rownum-2][2023]
totalAssets22 = df_bs.iloc[rownum-2][2022]
averageTotalAssets = 0.5 * (totalAssets22 + totalAssets23)
print(averageTotalAssets)
```

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
roa = netProfit / averageTotalAssets
print(roa)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The worksheet formula for computing this ratio is

<p style="text-align:center;"><code>='Income Statement'!F24/(('Balance Sheet'!E17 + 'Balance Sheet'!F17)/2)</code></p>

+++ {"editable": true, "slideshow": {"slide_type": ""}}

Let's collate the value for an overall listing later:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
ratios['ROA'] = roa
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

### ROE

This is the ratio: Net Income / Average Shareholders' Equity

This ratio can be computed from the income statement and the balance sheet (2 years).

The Average Shareholders' Equity is

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
rownum = 37
shareholdersEquity23 = df_bs.iloc[rownum-2][2023]
shareholdersEquity22 = df_bs.iloc[rownum-2][2022]
averageShareholdersEquity = 0.5 * (shareholdersEquity22 + shareholdersEquity23)
print(averageShareholdersEquity)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The ROE is

$$\frac{10700000000}{25000000000} \approx 0.43$$

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
roe = netProfit / averageShareholdersEquity
print(roe)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The worksheet formula for computing this ratio is

<p style="text-align:center;"><code>='Income Statement'!F24/(('Balance Sheet'!E37 + 'Balance Sheet'!F37)/2)</code></p>

+++ {"editable": true, "slideshow": {"slide_type": ""}}

Let's collate the value for an overall listing later:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
ratios['ROE'] = roe
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

### Asset Turnover Ratio

This is the ratio: Revenue / Average Total Assets

This ratio can be computed from the income statement and the balance sheet (2 years).

The Asset Turnover Ratio is

$$\frac{45800000000}{95250000000} \approx 0.48$$

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
assetTurnoverRatio = revenue / averageTotalAssets
print(assetTurnoverRatio)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The worksheet formula for computing this ratio is

<p style="text-align:center;"><code>='Income Statement'!F2/(('Balance Sheet'!E17 + 'Balance Sheet'!F17)/2)</code></p>

+++ {"editable": true, "slideshow": {"slide_type": ""}}

Let's collate the value for an overall listing later:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
ratios['Asset Turnover Ratio'] = assetTurnoverRatio
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

### Inventory Turnover Ratio

This is the ratio: Cost of Goods Sold / Average Inventory

This ratio can be computed from the income statement and the balance sheet (2 years).

The COGS is

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
rownum = 3
costOfGoodsSold = df_ics.iloc[rownum-2][2023]
print(costOfGoodsSold)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The average inventory is

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
rownum = 6
inventory23 = df_bs.iloc[rownum-2][2023]
inventory22 = df_bs.iloc[rownum-2][2022]
averageInventory = 0.5 * (inventory22 + inventory23)
print(averageInventory)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The Inventory Turnover Ratio is

$$\frac{18500000000}{4325000000} \approx 4.28$$

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
inventoryTurnoverRatio = costOfGoodsSold / averageInventory
print(inventoryTurnoverRatio)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The worksheet formula for computing this ratio is

<p style="text-align:center;"><code>='Income Statement'!F3/(('Balance Sheet'!E6 + 'Balance Sheet'!F6)/2)</code></p>

+++ {"editable": true, "slideshow": {"slide_type": ""}}

Let's collate the value for an overall listing later:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
ratios['Inventory Turnover Ratio'] = inventoryTurnoverRatio
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

### Accounts Receivable Turnover Ratio

This is the ratio: Net Credit Sales / Average Accounts Receivable

This average accounts receivable may be found from the balance sheet (2 years). However, it is harder to find the net credit sales. It is not generally listed on the income statement. For an outsider, hence, one may only estimate based on information found in the notes, if there is any.

From the notes, we find the statement:

<i>
Our receivables will generally be collected in less than six months, in accordance with the underlying payment terms. All of our performance obligations under the terms of contracts with our customers have an original duration of one year or less.
</i>

(Ref: Note 3, p. 75)

A naive estimate would then be 2, based on the simple-minded model that credit sales are cleared every 6 months with no overlaps.

Another estimate is obtained by replacing the numerator by Net Sales, i.e. Net Sales / Average Accounts Receivable.

The average accounts receivable is

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
rownum = 5
accountsReceivable23 = df_bs.iloc[rownum-2][2023]
accountsReceivable22 = df_bs.iloc[rownum-2][2022]
averageAccountsReceivable = 0.5 * (accountsReceivable22 + accountsReceivable23)
print(averageAccountsReceivable)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The (estimated) Accounts Receivable Turnover Ratio is

$$\frac{18500000000}{3450000000} \approx 13.3$$

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
accountsReceivableTurnoverRatio = revenue / averageAccountsReceivable
print(accountsReceivableTurnoverRatio)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The accuracy of this estimate is subject to the proportion of sales being done on credit rather than spot. The estimate also shows that the naive estimate of 2 is likely to be incorrect.

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The worksheet formula for computing this ratio is

<p style="text-align:center;"><code>='Income Statement'!F2/(('Balance Sheet'!E5 + 'Balance Sheet'!F5)/2)</code></p>

+++ {"editable": true, "slideshow": {"slide_type": ""}}

Let's collate the value for an overall listing later:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
ratios['Accounts Receivable Turnover Ratio'] = accountsReceivableTurnoverRatio
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

### Debt-to-Equity Ratio

This is the ratio: Total Debt / Total Equity

This ratio can be computed from the balance sheet.

The Total Debt is

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
rownum = 43
totalDebt = df_bs.iloc[rownum-2][2023]
print(totalDebt)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The Total Equity is

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
rownum = 38
totalEquity = df_bs.iloc[rownum-2][2023]
print(totalEquity)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

Note that Net debt is calculated as Total Debt minus Cash and Cash Equivalents. It provides a more refined view of a company's debt position by considering its ability to cover its debt obligations with its available liquid resources. Thus, it is not the same as Total Debt.

The Debt-to-Equity Ratio is

$$\frac{42100000000}{27500000000} \approx 1.53$$

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
debtToEquityRatio = totalDebt / totalEquity
print(debtToEquityRatio)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The worksheet formula for computing this ratio is

<p style="text-align:center;"><code>='Balance Sheet'!F43/'Balance Sheet'!F38</code></p>

+++ {"editable": true, "slideshow": {"slide_type": ""}}

Let's collate the value for an overall listing later:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
ratios['Debt-to-Equity Ratio'] = debtToEquityRatio
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

### Debt Ratio

This is the ratio: Total Debt / Total Assets

This ratio can be computed from the balance sheet.

The Total Assets is

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
rownum = 17
totalAssets = df_bs.iloc[rownum-2][2023]
print(totalAssets)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The Debt Ratio is

$$\frac{42100000000}{97700000000} \approx 0.43$$

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
debtRatio = totalDebt / totalAssets
print(debtRatio)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The worksheet formula for computing this ratio is

<p style="text-align:center;"><code>='Balance Sheet'!F43/'Balance Sheet'!F17</code></p>

+++ {"editable": true, "slideshow": {"slide_type": ""}}

Let's collate the value for an overall listing later:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
ratios['Debt Ratio'] = debtRatio
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

### Interest Coverage Ratio

This is the ratio: EBIT / Interest Expense

This ratio can be computed from the income statement.

The EBIT is

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
rownum = 16
ebitda = df_ics.iloc[rownum-2][2023] 
rownum = 15
dna = df_ics.iloc[rownum-2][2023] 
ebit = ebitda - dna
print(ebit)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The interest expense is

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
rownum = 14
interestExpense = df_ics.iloc[rownum-2][2023]
print(interestExpense)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The Interest Coverage Ratio is

$$\frac{11270000000}{1530000000} \approx 7.37$$

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
interestCoverageRatio = ebit / interestExpense
print(interestCoverageRatio)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The worksheet formula for computing this ratio is

<p style="text-align:center;"><code>=('Income Statement'!F16 - 'Income Statement'!F15)/'Income Statement'!F14</code></p>

+++ {"editable": true, "slideshow": {"slide_type": ""}}

Let's collate the value for an overall listing later:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
ratios['Interest Coverage Ratio'] = interestCoverageRatio
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

### P/E Ratio

This is the ratio: Market Price per Share / Earnings per Share (EPS) 

EPS can be computed from the income statement:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
rownum = 26
eps = df_ics.iloc[rownum-2][2023] 
print(eps)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The 10-K is dated 31/12/2023. The market stock price of KO on that date, using data found on the internet, is:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
import yfinance as yf
ko = yf.Ticker("KO")
hist = ko.history(period="3mo")
print(hist.iloc[8]) # 2023-12-29, 58.46
price = 58.46
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The P/E Ratio is

$$\frac{58.46}{2.48} \approx 23.57$$

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
pERatio = price / eps
print(pERatio)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The worksheet formula for computing this ratio is

<p style="text-align:center;"><code>=58.46/'Income Statement'!F26</code></p>

+++ {"editable": true, "slideshow": {"slide_type": ""}}

Let's collate the value for an overall listing later:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
ratios['P/E Ratio'] = pERatio
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

### P/B Ratio

This is the ratio: Market Price per Share / Book Value per Share

Book Value per Share can be computed from the balance sheet and income statement as follows:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
rownum = 31
totalLiabilities = df_bs.iloc[rownum-2][2023]
bookValue = totalAssets - totalLiabilities
rownum = 28
numShares = df_ics.iloc[rownum-2][2023]
bookValuePerShare = bookValue / numShares
print(bookValuePerShare)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The P/B Ratio is

$$\frac{58.46}{6.34} \approx 9.23$$

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
pBRatio = price / bookValuePerShare
print(pBRatio)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The worksheet formula for computing this ratio is

<p style="text-align:center;"><code>=58.46/(('Balance Sheet'!F17 - 'Balance Sheet'!F31)/'Income Statement'!F28)</code></p>

+++ {"editable": true, "slideshow": {"slide_type": ""}}

Let's collate the value for an overall listing later:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
ratios['P/B Ratio'] = pBRatio
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

### EPS

This has been computed above. The worksheet formula for computing this ratio is

<p style="text-align:center;"><code>='Income Statement'!F26</code></p>

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
print(eps)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

Let's collate the value for an overall listing later:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
ratios['EPS'] = eps
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

### Times Interest Earned Ratio

This is the same as Interest Coverage Ratio. This ratio is both a solvency ratio as well as a coverage ratio.

+++ {"editable": true, "slideshow": {"slide_type": ""}}

### Debt Service Coverage Ratio

This is the ratio: Net Operating Income / Total Debt Service

Net Operating Income can be computed from the income statement as follows:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
rownum = 18
netOperatingIncome = df_bs.iloc[rownum-2][2023]
print(netOperatingIncome)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

Total Debt Service may be computed from the following formula:

$$\text{Interest Expense} + \text{Principal Payments} + \text{Other Debt-Related Expenses}$$

We will assume Other Debt-Related Expenses to be 0 as this is neither found on the income statement nor the cash flow statement. We may also verify against Note 11 on p. 87 on Debt and Borrowing Arrangements.

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
rownum = 19
principalPayments = -df_cfs.iloc[rownum-2][2023]
totalDebtService = interestExpense + principalPayments
print(totalDebtService)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The Debt Service Coverage Ratio is

$$\frac{15500000000}{3430000000} \approx 4.52$$

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
debtServiceCoverageRatio = netOperatingIncome / totalDebtService
print(debtServiceCoverageRatio)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The worksheet formula for computing this ratio is

<p style="text-align:center;"><code>='Income Statement'!F18/('Income Statement'!F14+'Cash Flow Statement'!F19)</code></p>

+++ {"editable": true, "slideshow": {"slide_type": ""}}

Let's collate the value for an overall listing later:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
ratios['Debt Service Coverage Ratio'] = debtServiceCoverageRatio
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

### Revenue Growth Rate

This is the ratio: ((Current Year Revenue - Prior Year Revenue) / Prior Year Revenue) * 100

It may be calculated from the income statement as folllows:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
rownum = 2
revenue2023 = df_ics.iloc[rownum-2][2023]
revenue2022 = df_ics.iloc[rownum-2][2022]
revenueGrowthRate = (revenue2023 - revenue2022) / revenue2022 * 100
print(revenueGrowthRate)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The revenue growth rate is 6.51%.

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The worksheet formula for computing this ratio is

<p style="text-align:center;"><code>=('Income Statement'!F2 - 'Income Statement'!E2)/'Income Statement'!E2 x 100</code></p>

+++ {"editable": true, "slideshow": {"slide_type": ""}}

Let's collate the value for an overall listing later:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
ratios['Revenue Growth Rate'] = revenueGrowthRate
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

### Earnings Growth Rate

This is the ratio: ((Current Year Earnings - Prior Year Earnings) / Prior Year Earnings) * 100

It may be calculated from the income statement as follows:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
rownum = 24
earnings2023 = df_ics.iloc[rownum-2][2023]
earnings2022 = df_ics.iloc[rownum-2][2022]
earningsGrowthRate = (earnings2023 - earnings2022) / earnings2022 * 100
print(earningsGrowthRate)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The earnings growth rate is 12.16%.

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The worksheet formula for computing this ratio is

<p style="text-align:center;"><code>=('Income Statement'!F24 - 'Income Statement'!E24)/'Income Statement'!E24 x 100</code></p>

+++ {"editable": true, "slideshow": {"slide_type": ""}}

Let's collate the value for an overall listing later:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
ratios['Earnings Growth Rate'] = earningsGrowthRate
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

## Obtaining Ratios from Code

+++ {"editable": true, "slideshow": {"slide_type": ""}}

We will require the API key to Financial Modeling Prep (FMP) which will be defined something like this (the real key hidden from view):

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
API_KEY = "*"
```

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
tags: [remove-input]
---
API_KEY = 'c3b801f14bc0119472d1f5269ea86a16'
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The Finance Toolkit package has ratios built into its API:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
from financetoolkit import Toolkit

companies = Toolkit(
    tickers=['KO'],
    api_key=API_KEY,
)

# This shows the functionalities available
print(dir(companies.ratios))
```

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
import pandas as pd
pd.set_option('display.max_columns', 500)
pd.set_option('display.width', 1000)
#pd.set_option('display.max_rows', 500)

# In the following few code blocks,
# we will only uncomment when we need to download the data.
# We have copied and pasted the data as markdown tables
# in the following section.

# Let's take a look at the liquidity ratios
#liquidity_ratios = companies.ratios.collect_liquidity_ratios()
#print(liquidity_ratios)
```

```{code-cell} ipython3
# Let's take a look at the profitability ratios
#profitability_ratios = companies.ratios.collect_profitability_ratios()
#print(profitability_ratios)
```

```{code-cell} ipython3
# Let's take a look at the efficiency ratios
#efficiency_ratios = companies.ratios.collect_efficiency_ratios()
#print(efficiency_ratios)
```

```{code-cell} ipython3
# Let's take a look at the solvency ratios
#solvency_ratios = companies.ratios.collect_solvency_ratios()
#print(solvency_ratios)
```

```{code-cell} ipython3
# Let's take a look at the valuation ratios
#valuation_ratios = companies.ratios.collect_valuation_ratios()
#print(valuation_ratios)
```

This API facilitates the use of ratios in reasoning about companies as it alleviates us from having to calculate them from the financial statements.

+++ {"editable": true, "slideshow": {"slide_type": ""}}

## Reasoning with Ratios

+++ {"editable": true, "slideshow": {"slide_type": ""}}

### Interpreting the Ratios

+++

#### Intertemporal Comparison of Ratios

Let's take a look at the ratio tables FY19-FY23 for The Coca-Cola Company which are organized by:

1. Liquidity
2. Profitability
3. Efficiency
4. Solvency
5. Valuation

+++ {"editable": true, "slideshow": {"slide_type": ""}}

##### Liquidity

|                                    |       2019 |       2020 |       2021 |       2022 |       2023 |
|:-----------------------------------|-----------:|-----------:|-----------:|-----------:|-----------:|
| Current Ratio                      |  0.7567    |  1.3177    |  1.1301    |  1.1454    |  1.1341    |
| Quick Ratio                        |  0.5615    |  0.9628    |  0.8089    |  0.7665    |  0.7243    |
| Cash Ratio                         |  0.4143    |  0.7475    |  0.6328    |  0.5897    |  0.5797    |
| Working Capital                    | -6.562e+09 |  4.639e+09 |  2.595e+09 |  2.867e+09 |  3.161e+09 |
| Operating Cash Flow Ratio          |  0.3882    |  0.6742    |  0.6328    |  0.5586    |  0.4921    |
| Operating Cash Flow to Sales Ratio |  0.281     |  0.2982    |  0.3266    |  0.2562    |  0.2535    |
| Short Term Coverage Ratio          | -2.6429    | -2.079     | -1.6411    | -1.3723    | -1.516     |

+++ {"editable": true, "slideshow": {"slide_type": ""}}

Ignoring Working Capital and Short Term Coverage Ratio, the ratios remain about the same over the years.

Perhaps we may note that Operating Cash Flow Ratio heightened to 0.67 and 0.62 in 2020 and 2021. Then it falls to about 0.5. This increase is also observed in the other ratios here. Thus, the data suggests that the liquidity of the company rose during the Covid-19 period.

+++

##### Profitability

|                                             |     2019 |   2020 |   2021 |    2022 |   2023 |
|:--------------------------------------------|---------:|-------:|-------:|--------:|-------:|
| Gross Margin                                |   0.6077 | 0.5931 | 0.6027 |  0.5814 | 0.5952 |
| Operating Margin                            |   0.2706 | 0.2725 | 0.3627 |  0.2537 | 0.2472 |
| Net Profit Margin                           |   0.2394 | 0.2347 | 0.2528 |  0.2219 | 0.2342 |
| Interest Coverage Ratio                     |  13.8446 | 8.8532 | 9.6894 | 15.678  | 8.146  |
| Income Before Tax Profit Margin             |   0.2894 | 0.2953 | 0.3214 |  0.2717 | 0.2831 |
| Effective Tax Rate                          |   0.167  | 0.2032 | 0.2109 |  0.181  | 0.1736 |
| Return on Assets                            | nan      | 0.0892 | 0.1076 |  0.102  | 0.1125 |
| Return on Equity                            | nan      | 0.3656 | 0.4235 |  0.3765 | 0.402  |
| Return on Invested Capital                  | nan      | 0.2313 | 0.2585 |  0.2588 | 0.2775 |
| Return on Capital Employed                  |   0.1964 | 0.1536 | 0.188  |  0.1717 | 0.1955 |
| Return on Tangible Assets                   | nan      | 0.0475 | 0.0569 |  0.054  | 0.06   |
| Income Quality Ratio                        |   1.1654 | 1.2673 | 1.2877 |  1.1512 | 1.0826 |
| Net Income per EBT                          |   0.832  | 0.7964 | 0.7885 |  0.8186 | 0.8265 |
| Free Cash Flow to Operating Cash Flow Ratio |   0.8038 | 0.8804 | 0.8917 |  0.8653 | 0.8403 |
| EBT to EBIT Ratio                           |   0.9189 | 0.8713 | 0.8858 |  0.9297 | 0.8946 |
| EBIT to Revenue                             |   0.3131 | 0.3382 | 0.3619 |  0.2916 | 0.3167 |

+++

These ratios do not fluctuate too much over the years, consistent with the fact that The Coca-Cola Company is a mature company.

The net profit margin is consistly about 23%, ROA is about 11% and ROE is about 40%.

+++

##### Efficiency

|                                      |     2019 |      2020 |      2021 |      2022 |      2023 |
|:-------------------------------------|---------:|----------:|----------:|----------:|----------:|
| Days of Inventory Outstanding        | nan      |   90.2786 |   79.384  |   77.5321 |   85.3079 |
| Days of Sales Outstanding            | nan      |   39.3314 |   31.4247 |   29.7023 |   27.5102 |
| Operating Cycle                      | nan      |  129.61   |  110.809  |  107.234  |  112.818  |
| Days of Accounts Payable Outstanding | nan      |  305.1    |  306.175  |  307.898  |  307.786  |
| Cash Conversion Cycle                | nan      | -175.489  | -195.366  | -200.663  | -194.968  |
| Cash Conversion Efficiency           |   0.281  |    0.2982 |    0.3266 |    0.2562 |    0.2535 |
| Receivables Turnover                 | nan      |    0.1078 |    0.0861 |    0.0814 |    0.0754 |
| Inventory Turnover Ratio             | nan      |    4.043  |    4.5979 |    4.7077 |    4.2786 |
| Accounts Payable Turnover Ratio      | nan      |    1.1963 |    1.1921 |    1.1855 |    1.1859 |
| SGA-to-Revenue Ratio                 |   0.3248 |    0.2948 |    0.3142 |    0.2995 |    0.3054 |
| Fixed Asset Turnover                 | nan      |    0.4927 |    0.5527 |    0.6058 |    0.6483 |
| Asset Turnover Ratio                 | nan      |    0.3802 |    0.4256 |    0.4596 |    0.4804 |
| Operating Ratio                      |   0.7294 |    0.7275 |    0.7333 |    0.7463 |    0.7528 |

+++

Days of Sales Oustanding has been falling steadily from 2020 to 2023.

Cash Conversion Cycle is negative. This means that inventory is sold before the company has to pay for it. Or, in other words, its vendors are financing its business operations.

Fixed Asset Turnover and Asset Turnover Ratio have been rising.

+++ {"editable": true, "slideshow": {"slide_type": ""}}

:::{admonition} Fixed Asset Turnover has been rising. What does this mean?
:class: note
When the Fixed Asset Turnover ratio is rising, it typically indicates that a company is generating more revenue relative to its investment in fixed assets. The Fixed Asset Turnover ratio measures how efficiently a company utilizes its fixed assets to generate sales. Here's what a rising Fixed Asset Turnover ratio may signify:

1. **Improved Asset Utilization**: A rising Fixed Asset Turnover ratio suggests that the company is using its fixed assets more effectively to generate revenue. This could result from improved production processes, better asset management practices, or increased capacity utilization.

2. **Increased Efficiency**: The company may be improving operational efficiency, leading to higher sales without a proportional increase in fixed assets. This could be achieved through better inventory management, streamlined operations, or enhanced distribution channels.

3. **Optimized Capital Investment**: Rising Fixed Asset Turnover could indicate that the company is making strategic investments in fixed assets that are generating higher returns. This might involve investing in technologies or equipment that improve productivity or enhance product quality.

4. **Effective Cost Management**: The company may be effectively controlling costs associated with maintaining and utilizing fixed assets, allowing it to generate more sales with the same level of investment in fixed assets.

Overall, a rising Fixed Asset Turnover ratio is generally considered a positive sign as it suggests that the company is becoming more efficient in utilizing its fixed assets to generate revenue. However, it's essential to analyze other financial metrics and consider the company's specific circumstances to get a comprehensive understanding of its financial performance and operational efficiency.
:::

+++ {"editable": true, "slideshow": {"slide_type": ""}}

:::{admonition} Asset Turnover Ratio has been rising. What does this mean?
:class: note
When the Asset Turnover Ratio is rising, it typically indicates that a company is generating more revenue per unit of assets. The Asset Turnover Ratio measures how efficiently a company utilizes its assets to generate sales. A higher Asset Turnover Ratio suggests that the company is generating more revenue with the same level of assets or using its assets more efficiently to generate sales. Here's what a rising Asset Turnover Ratio may indicate:

1. **Improved Operational Efficiency**: A rising Asset Turnover Ratio often suggests that the company is becoming more efficient in utilizing its assets to generate sales. This may be due to improvements in production processes, supply chain management, or distribution channels.

2. **Increased Sales Growth**: The company may be experiencing higher sales growth without a proportional increase in assets. This could result from market expansion, product innovation, or effective marketing strategies.

3. **Optimized Asset Utilization**: Rising Asset Turnover Ratio may indicate that the company is optimizing its asset utilization by focusing on high-performing assets and divesting underperforming ones. This could lead to better allocation of resources and improved overall profitability.

4. **Effective Inventory Management**: The company may be managing its inventory more efficiently, reducing excess inventory levels and improving inventory turnover. This results in a higher Asset Turnover Ratio as sales are generated with lower levels of inventory investment.

5. **Strategic Investments**: The company may be making strategic investments in assets that generate higher returns or support sales growth. These investments could include technology upgrades, capacity expansions, or acquisitions that enhance the company's ability to generate revenue.

Overall, a rising Asset Turnover Ratio is generally considered a positive sign as it indicates that the company is generating more sales per unit of assets. However, it's essential to analyze other financial metrics and consider the company's specific industry and competitive landscape to get a comprehensive understanding of its operational efficiency and financial performance.

:::

+++ {"editable": true, "slideshow": {"slide_type": ""}}

##### Solvency

|                               |     2019 |    2020 |    2021 |    2022 |    2023 |
|:------------------------------|---------:|--------:|--------:|--------:|--------:|
| Debt-to-Assets Ratio          |   0.4951 |  0.4902 |  0.4532 |  0.422  |  0.4305 |
| Debt-to-Equity Ratio          |   2.0269 |  2.0106 |  1.7201 |  1.5159 |  1.5307 |
| Debt Service Coverage Ratio   |   0.3739 |  0.6162 |  0.7029 |  0.5531 |  0.4799 |
| Equity Multiplier             | nan      |  4.0979 |  3.9366 |  3.6917 |  3.5731 |
| Free Cash Flow Yield          |   0.0403 |  0.0404 |  0.0469 |  0.0358 |  0.0384 |
| Net-Debt to EBITDA Ratio      |   2.7703 |  2.8296 |  2.1376 |  2.1428 |  2.6287 |
| Cash Flow Coverage Ratio      |   0.0403 |  0.0404 |  0.0469 |  0.0358 |  0.0384 |
| CAPEX Coverage Ratio          |  -5.0979 | -8.3636 | -9.2356 | -7.4245 | -6.263  |
| Dividend CAPEX Coverage Ratio |  -1.1766 | -1.197  | -1.4648 | -1.2108 | -1.1831 |

+++

Debt-to-Equity Ratio has been decreasing. 

Equity Multiplier has been decreasing. This typically indicates that a company is relying less on debt financing relative to equity financing.

Both ratios suggest that the company is relying less on debt financing relative to equity financing which may be a sign of good financial health or strengthened equity base.

+++

##### Valuation

|                           |          2019 |           2020 |         2021 |            2022 |          2023 |
|:--------------------------|--------------:|---------------:|-------------:|----------------:|--------------:|
| Earnings per Share        |   2.0677      |    1.792       |  2.2514      |     2.1936      |   2.4692      |
| Revenue per Share         |   8.6384      |    7.6368      |  8.9067      |     9.886       |  10.5448      |
| Price-to-Earnings         |  23.4367      |   27.7121      | 24.5669      |    27.8902      |  23.6757      |
| Price-to-Earnings-Growth  | nan           | -207.893       | 95.8521      | -1085.22        | 188.351       |
| Book Value per Share      |   4.3999      |    4.4643      |  5.2993      |     5.5414      |   5.9786      |
| Price-to-Book             |  11.0139      |   11.1238      | 10.4372      |    11.0405      |   9.7782      |
| Interest Debt per Share   |   9.5434e+07  |    1.45167e+08 |  1.62086e+08 |     9.80025e+07 |   1.57514e+08 |
| CAPEX per Share           |  -0.4761      |   -0.2723      | -0.315       |    -0.3411      |  -0.4268      |
| Earnings Yield            |   0.0427      |    0.0361      |  0.0407      |     0.0359      |   0.0422      |
| Dividend Payout Ratio     |   0.7674      |    0.9096      |  0.7422      |     0.7982      |   0.7422      |
| Dividend Yield            |   0.033       |    0.033       |  0.0304      |     0.0288      |   0.0315      |
| Weighted Dividend Yield   |   0.0327      |    0.0328      |  0.0302      |     0.0286      |   0.0313      |
| Price-to-Cash-Flow        |  19.9653      |   21.8082      | 19.0135      |    24.1544      |  21.8689      |
| Price-to-Free-Cash-Flow   |  24.8374      |   24.7698      | 21.3222      |    27.9141      |  26.0242      |
| Market Cap                |   2.09056e+11 |    2.1468e+11  |  2.40045e+11 |     2.66133e+11 |   2.53658e+11 |
| Enterprise Value          |   2.47456e+11 |    2.52663e+11 |  2.74983e+11 |     2.97484e+11 |   2.87895e+11 |
| EV-to-Sales               |   6.6403      |    7.6532      |  7.1138      |     6.9176      |   6.2922      |
| EV-to-EBIT                |  21.2099      |   22.6299      | 19.6571      |    23.7247      |  19.8685      |
| EV-to-EBITDA              |  18.8941      |   19.8603      | 17.7707      |    21.5132      |  23.1445      |
| EV-to-Operating-Cash-Flow |  23.6326      |   25.6667      | 21.7809      |    26.9998      |  24.8207      |
| Tangible Asset Value      |   4.334e+09   |    3.778e+09   |  5.497e+09   |     7.044e+09   |   9.122e+09   |
| Net Current Asset Value   |  -6.562e+09   |    4.639e+09   |  2.595e+09   |     2.867e+09   |   3.161e+09   |

+++

Revenue per Share and Tangible Asset Value have been rising from 2020. This suggests positive trends in the company's revenue generation and asset efficiency.

Book Value per Share has been steadily rising. This indicates positive trends in the company's financial health, shareholder value, and management effectiveness.

Dividend Yield is roughly constant at 3%.

The other ratios are about the same or fluctuate slightly over the years. This is again consistent with the fact the the company is a mature one.

+++

#### Industry Comparison of Ratios

+++

In intertemporal analysis, we could say if the ratios are increasing or decreasing over time. However, we would be hard pressed to say what a given ratio value actually signifies. For example, what does a P/E ratio of 2 actually signify, beyond the fact that the stock price is twice that of earnings, which follows from the definition?

In industry comparison, we derive meanings in the ratios by pitting the values of a ratio against each other for companies that belong together. If the value of a ratio is too low for a company as compared to its peers, then we may ask why that is so.

The <wiki:Global_Industry_Classification_Standard> (GICS®) developed by S&P Dow Jones Indices and MSCI is an industry-accepted grouping scheme for companies which we may use for this purpose.

In the code below, we try to figure out where The Coca-Cola Company is located within the GICS:

```{code-cell} ipython3
# Note that the value for Industry
# is not the same as that under GICS
companies.get_profile()
```

```{code-cell} ipython3
import financedatabase as fd

# Initialize the Equities database
equities = fd.Equities()
```

```{code-cell} ipython3
# Extract the mega caps
beverages_mega = equities.search(industry="Beverages", country="United States", market_cap="Mega Cap", exclude_exchanges=True)
beverages_mega
```

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
# Extract the large caps
beverages_large = equities.search(industry="Beverages", country="United States", market_cap="Large Cap", exclude_exchanges=True)
beverages_large
```

```{code-cell} ipython3
# Get the symbols into a list
symbols_mega = list(beverages_mega.index)
symbols_large = list(beverages_large.index)
symbols = symbols_mega + symbols_large
symbols
```

##### Liquidity

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
"""
import pandas as pd

# Now let's get all the ratios for these companies FY2023
companies = Toolkit(
    tickers=symbols,
    api_key=API_KEY,
    start_date="2023-12-31"
)

ratios = companies.ratios.collect_liquidity_ratios()
series = ratios['2023']
index_levels = series.index.levels[0].tolist()
column_levels = series.index.levels[1].tolist()
df = pd.DataFrame(index=index_levels, columns=column_levels)

# Populating the DataFrame with values from the Series
for idx in df.index:
    for col in df.columns:
        df.loc[idx, col] = series[(idx, col)]

df = df.T
df['average'] = df.mean(axis=1)
        
print(df.to_markdown())
"""
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

|                                    |      BF-A |      BF-B |        CELH |        KDP |         KO |        MNST |        PEP |       STZ |     STZ-B |         TAP |       TAP-A |      average |
|:-----------------------------------|----------:|----------:|------------:|-----------:|-----------:|------------:|-----------:|----------:|----------:|------------:|------------:|-------------:|
| Cash Ratio                         | 0.345     | 0.345     | 2.733       |  0.0299    |  0.5797    | 2.8005      |  0.3161    | 0.0136    | 0.0136    |  0.2123     |  0.2123     |  0.691       |
| Current Ratio                      | 3.5065    | 3.5065    | 4.3049      |  0.3785    |  1.1341    | 4.8111      |  0.8516    | 1.1778    | 1.1778    |  0.696      |  0.696      |  2.02189     |
| Operating Cash Flow Ratio          | 0.5904    | 0.5904    | 0.5105      |  0.1491    |  0.4921    | 1.4787      |  0.4247    | 0.9288    | 0.9288    |  0.508      |  0.508      |  0.646318    |
| Operating Cash Flow to Sales Ratio | 0.1514    | 0.1514    | 0.1071      |  0.0897    |  0.2535    | 0.1638      |  0.147     | 0.2917    | 0.2917    |  0.1777     |  0.1777     |  0.182064    |
| Quick Ratio                        | 1.1338    | 1.1338    | 3.4055      |  0.2003    |  0.7243    | 3.8283      |  0.6578    | 0.3173    | 0.3173    |  0.4272     |  0.4272     |  1.14298     |
| Short Term Coverage Ratio          | 0.2769    | 0.2769    | 0.3792      | -1.4199    | -1.516     | 1.0729      |  2.9778    | 1.4832    | 1.4832    | -4.4414     | -4.4414     | -0.351691    |
| Working Capital                    | 2.717e+09 | 2.717e+09 | 9.14167e+08 | -5.541e+09 |  3.161e+09 | 4.42731e+09 | -4.697e+09 | 5.278e+08 | 5.278e+08 | -1.2441e+09 | -1.2441e+09 |  2.05989e+08 |

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The liquidity position of the company, likely referring to Coca-Cola (KO), is below average compared to other large and cap beverage companies in the US. Despite having a relatively strong Operating Cash Flow to Sales Ratio, other liquidity indicators, such as the short-term coverage ratio, suggest a more moderate situation. The negative short-term coverage ratio implies that the company may not be generating enough earnings to cover its interest obligations, indicating potential financial strain and highlighting liquidity challenges.

+++ {"editable": true, "slideshow": {"slide_type": ""}}

##### Profitability

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
"""
ratios = companies.ratios.collect_profitability_ratios()
series = ratios['2023']
index_levels = series.index.levels[0].tolist()
column_levels = series.index.levels[1].tolist()
df = pd.DataFrame(index=index_levels, columns=column_levels)

# Populating the DataFrame with values from the Series
for idx in df.index:
    for col in df.columns:
        df.loc[idx, col] = series[(idx, col)]

df = df.T
df['average'] = df.mean(axis=1)
        
print(df.to_markdown())
"""
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

|                                             |     BF-A |     BF-B |     CELH |      KDP |       KO |     MNST |      PEP |      STZ |    STZ-B |      TAP |    TAP-A |    average |
|:--------------------------------------------|---------:|---------:|---------:|---------:|---------:|---------:|---------:|---------:|---------:|---------:|---------:|-----------:|
| EBIT to Revenue                             |   0.2597 |   0.2597 |   0.2415 |   0.2196 |   0.3167 |   0.2082 |   0.1332 |   0.0828 |   0.0828 |   0.1264 |   0.1264 |   0.187    |
| EBT to EBIT Ratio                           |   0.9262 |   0.9262 |   0.9167 |   0.8475 |   0.8946 |   0.9473 |   0.9302 |   0.4903 |   0.4903 |   0.8418 |   0.8418 |   0.822991 |
| Effective Tax Rate                          |   0.2301 |   0.2301 |   0.2226 |   0.2089 |   0.1736 |   0.2115 |   0.1981 |   1.1004 |   1.1004 |   0.2364 |   0.2364 |   0.377136 |
| Free Cash Flow to Operating Cash Flow Ratio |   0.7141 |   0.7141 |   0.8766 |   0.6381 |   0.8403 |   0.8594 |   0.5895 |   0.6244 |   0.6244 |   0.677  |   0.677  |   0.712264 |
| Gross Margin                                |   0.5899 |   0.5899 |   0.4778 |   0.5454 |   0.5952 |   0.3618 |   0.5448 |   0.5045 |   0.5045 |   0.3733 |   0.3733 |   0.4964   |
| Income Before Tax Profit Margin             |   0.2405 |   0.2405 |   0.2214 |   0.1861 |   0.2831 |   0.1973 |   0.1248 |   0.0406 |   0.0406 |   0.107  |   0.107  |   0.162627 |
| Income Quality Ratio                        |   0.8174 |   0.8174 |   0.6227 |   0.6094 |   1.0826 |   1.0532 |   1.4814 | -71.6078 | -71.6078 |   2.1738 |   2.1738 | -12.0349   |
| Interest Coverage Ratio                     |  13.5556 |  13.5556 |  10.1804 |   7.8871 |   8.146  |  17.5654 |  20.0412 |   8.0931 |   8.0931 |   9.0641 |   9.0641 |  11.386    |
| Net Income per EBT                          |   0.7699 |   0.7699 |   0.7774 |   0.7911 |   0.8265 |   0.7885 |   0.8005 |  -0.1004 |  -0.1004 |   0.7622 |   0.7622 |   0.622491 |
| Net Profit Margin                           |   0.1852 |   0.1852 |   0.1721 |   0.1472 |   0.2342 |   0.1555 |   0.0992 |  -0.0041 |  -0.0041 |   0.0811 |   0.0811 |   0.121145 |
| Operating Margin                            |   0.2666 |   0.2666 |   0.2021 |   0.2155 |   0.2472 |   0.1863 |   0.154  |   0.3008 |   0.3008 |   0.1229 |   0.1229 |   0.216882 |
| Return on Assets                            | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan        |
| Return on Capital Employed                  |   0.1641 |   0.1641 |   0.2526 |   0.0753 |   0.1955 |   0.2561 |   0.177  |   0.0361 |   0.0361 |   0.0664 |   0.0664 |   0.135427 |
| Return on Equity                            | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan        |
| Return on Invested Capital                  | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan        |
| Return on Tangible Assets                   | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan        |

+++ {"editable": true, "slideshow": {"slide_type": ""}}

KO is the leader when it comes to profitability. Other than the interest coverage ratio (KO: 8.1, mean: 11.4), the other (profitability) ratios shown on the table here for KO are all above the industry (large cap and mega cap) averages. It is the most profitable firm among others shown here in terms of the net income margin. Its level at 23.4 is more than twice that of the industry (large cap and mega cap) average.

+++ {"editable": true, "slideshow": {"slide_type": ""}}

|                                             |     BF-A |     BF-B |     CELH |      KDP |       KO |     MNST |      PEP |      STZ |    STZ-B |      TAP |    TAP-A |    average |
|:--------------------------------------------|---------:|---------:|---------:|---------:|---------:|---------:|---------:|---------:|---------:|---------:|---------:|-----------:|
| EBIT to Revenue                             |   0.2597 |   0.2597 |   0.2415 |   0.2196 |   0.3167 |   0.2082 |   0.1332 |   0.0828 |   0.0828 |   0.1264 |   0.1264 |   0.187    |
| EBT to EBIT Ratio                           |   0.9262 |   0.9262 |   0.9167 |   0.8475 |   0.8946 |   0.9473 |   0.9302 |   0.4903 |   0.4903 |   0.8418 |   0.8418 |   0.822991 |
| Effective Tax Rate                          |   0.2301 |   0.2301 |   0.2226 |   0.2089 |   0.1736 |   0.2115 |   0.1981 |   1.1004 |   1.1004 |   0.2364 |   0.2364 |   0.377136 |
| Free Cash Flow to Operating Cash Flow Ratio |   0.7141 |   0.7141 |   0.8766 |   0.6381 |   0.8403 |   0.8594 |   0.5895 |   0.6244 |   0.6244 |   0.677  |   0.677  |   0.712264 |
| Gross Margin                                |   0.5899 |   0.5899 |   0.4778 |   0.5454 |   0.5952 |   0.3618 |   0.5448 |   0.5045 |   0.5045 |   0.3733 |   0.3733 |   0.4964   |
| Income Before Tax Profit Margin             |   0.2405 |   0.2405 |   0.2214 |   0.1861 |   0.2831 |   0.1973 |   0.1248 |   0.0406 |   0.0406 |   0.107  |   0.107  |   0.162627 |
| Income Quality Ratio                        |   0.8174 |   0.8174 |   0.6227 |   0.6094 |   1.0826 |   1.0532 |   1.4814 | -71.6078 | -71.6078 |   2.1738 |   2.1738 | -12.0349   |
| Interest Coverage Ratio                     |  13.5556 |  13.5556 |  10.1804 |   7.8871 |   8.146  |  17.5654 |  20.0412 |   8.0931 |   8.0931 |   9.0641 |   9.0641 |  11.386    |
| Net Income per EBT                          |   0.7699 |   0.7699 |   0.7774 |   0.7911 |   0.8265 |   0.7885 |   0.8005 |  -0.1004 |  -0.1004 |   0.7622 |   0.7622 |   0.622491 |
| Net Profit Margin                           |   0.1852 |   0.1852 |   0.1721 |   0.1472 |   0.2342 |   0.1555 |   0.0992 |  -0.0041 |  -0.0041 |   0.0811 |   0.0811 |   0.121145 |
| Operating Margin                            |   0.2666 |   0.2666 |   0.2021 |   0.2155 |   0.2472 |   0.1863 |   0.154  |   0.3008 |   0.3008 |   0.1229 |   0.1229 |   0.216882 |
| Return on Assets                            | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan        |
| Return on Capital Employed                  |   0.1641 |   0.1641 |   0.2526 |   0.0753 |   0.1955 |   0.2561 |   0.177  |   0.0361 |   0.0361 |   0.0664 |   0.0664 |   0.135427 |
| Return on Equity                            | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan        |
| Return on Invested Capital                  | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan        |
| Return on Tangible Assets                   | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan        |

+++

##### Efficiency

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
"""
ratios = companies.ratios.collect_efficiency_ratios()
series = ratios['2023']
index_levels = series.index.levels[0].tolist()
column_levels = series.index.levels[1].tolist()
df = pd.DataFrame(index=index_levels, columns=column_levels)

# Populating the DataFrame with values from the Series
for idx in df.index:
    for col in df.columns:
        df.loc[idx, col] = series[(idx, col)]

df = df.T
df['average'] = df.mean(axis=1)
        
print(df.to_markdown())
"""
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

|                                      |     BF-A |     BF-B |     CELH |      KDP |       KO |     MNST |     PEP |      STZ |    STZ-B |      TAP |    TAP-A |    average |
|:-------------------------------------|---------:|---------:|---------:|---------:|---------:|---------:|--------:|---------:|---------:|---------:|---------:|-----------:|
| Accounts Payable Turnover Ratio      | nan      | nan      | nan      | nan      | nan      | nan      | nan     | nan      | nan      | nan      | nan      | nan        |
| Asset Turnover Ratio                 | nan      | nan      | nan      | nan      | nan      | nan      | nan     | nan      | nan      | nan      | nan      | nan        |
| Cash Conversion Cycle                | nan      | nan      | nan      | nan      | nan      | nan      | nan     | nan      | nan      | nan      | nan      | nan        |
| Cash Conversion Efficiency           |   0.1514 |   0.1514 |   0.1071 |   0.0897 |   0.2535 |   0.1638 |   0.147 |   0.2917 |   0.2917 |   0.1777 |   0.1777 |   0.182064 |
| Days of Accounts Payable Outstanding | nan      | nan      | nan      | nan      | nan      | nan      | nan     | nan      | nan      | nan      | nan      | nan        |
| Days of Inventory Outstanding        | nan      | nan      | nan      | nan      | nan      | nan      | nan     | nan      | nan      | nan      | nan      | nan        |
| Days of Sales Outstanding            | nan      | nan      | nan      | nan      | nan      | nan      | nan     | nan      | nan      | nan      | nan      | nan        |
| Fixed Asset Turnover                 | nan      | nan      | nan      | nan      | nan      | nan      | nan     | nan      | nan      | nan      | nan      | nan        |
| Inventory Turnover Ratio             | nan      | nan      | nan      | nan      | nan      | nan      | nan     | nan      | nan      | nan      | nan      | nan        |
| Operating Cycle                      | nan      | nan      | nan      | nan      | nan      | nan      | nan     | nan      | nan      | nan      | nan      | nan        |
| Operating Ratio                      |   0.7334 |   0.7334 |   0.7979 |   0.7844 |   0.7528 |   0.8137 |   0.846 |   0.6992 |   0.6992 |   0.8642 |   0.8642 |   0.780764 |
| Receivables Turnover                 | nan      | nan      | nan      | nan      | nan      | nan      | nan     | nan      | nan      | nan      | nan      | nan        |
| SGA-to-Revenue Ratio                 |   0.2952 |   0.2952 |   0.2744 |   0.3316 |   0.3054 |   0.174  |   0.382 |   0.2038 |   0.2038 |   0.2376 |   0.2376 |   0.267327 |

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The many "nan" entries here indicate that many of the activity ratios are not available for the US beverage industry (large cap and mega cap) from the finance toolkit API.

KO's cash coversion efficiency and sga-to-revenue ratios are above the average, while its operating ratio is below the average. 

Note that the operation ratio is a measure of a firm's operating expense relative to its revenue, hence below-average is a positive indication. 

KO's SGA expenses are significant as indicated by its above-average sga-to-revenue ratio.

The cash conversion efficiency (0.2535) of The Coca-Cola Company is only below that of Constellation Brands, Inc. (0.2917).

+++ {"editable": true, "slideshow": {"slide_type": ""}}

##### Solvency

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
"""
ratios = companies.ratios.collect_solvency_ratios()
series = ratios['2023']
index_levels = series.index.levels[0].tolist()
column_levels = series.index.levels[1].tolist()
df = pd.DataFrame(index=index_levels, columns=column_levels)

# Populating the DataFrame with values from the Series
for idx in df.index:
    for col in df.columns:
        df.loc[idx, col] = series[(idx, col)]

df = df.T
df['average'] = df.mean(axis=1)
        
print(df.to_markdown())
"""
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

|                               |     BF-A |     BF-B |     CELH |      KDP |       KO |     MNST |      PEP |      STZ |    STZ-B |      TAP |    TAP-A |    average |
|:------------------------------|---------:|---------:|---------:|---------:|---------:|---------:|---------:|---------:|---------:|---------:|---------:|-----------:|
| CAPEX Coverage Ratio          |  -3.4973 |  -3.4973 |  -8.1006 |  -2.763  |  -6.263  |  -7.1114 |  -2.436  |  -2.6626 |  -2.6626 |  -3.0961 |  -3.0961 |  -4.10782  |
| Cash Flow Coverage Ratio      |   0.0161 |   0.0167 |   0.0093 |   0.0183 |   0.0382 |   0.0241 |   0.0338 |   0.037  | inf      |   0.1061 |   0.1007 | inf        |
| Debt Service Coverage Ratio   |   1.0397 |   1.0397 |   0.963  |   0.358  |   0.4799 |   1.6815 |   0.4451 |   0.9578 |   0.9578 |   0.3514 |   0.3514 |   0.784118 |
| Debt-to-Assets Ratio          |   0.3746 |   0.3746 |   0.0014 |   0.2844 |   0.4305 |   0.0068 |   0.4444 |   0.5053 |   0.5053 |   0.2378 |   0.2378 |   0.309355 |
| Debt-to-Equity Ratio          |   0.8914 |   0.8914 |   0.002  |   0.5773 |   1.5307 |   0.008  |   2.3964 |   1.4268 |   1.4268 |   0.4667 |   0.4667 |   0.916745 |
| Dividend CAPEX Coverage Ratio |  -1.1408 |  -1.1408 |  -3.1455 |  -0.8189 |  -1.1831 |  -7.1114 |  -1.1018 |  -1.6985 |  -1.6985 |  -2.0259 |  -2.0259 |  -2.09919  |
| Equity Multiplier             | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan      | nan        |
| Free Cash Flow Yield          |   0.0161 |   0.0167 |   0.0093 |   0.0183 |   0.0382 |   0.0241 |   0.0338 |   0.037  | inf      |   0.1061 |   0.1007 | inf        |
| Net-Debt to EBITDA Ratio      |   2.3124 |   2.3124 |  -2.794  |   3.7211 |   2.6287 |  -1.1036 |   2.0517 |   3.8206 |   3.8206 |   2.5469 |   2.5469 |   1.98761  |

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The table has a number of "nan"'s and "inf"'s. One should be careful about interpreting these.

Some fields contain predominantly negative values. We should figure out why. Here, CAPEX has been taken to be a negative number, hence the negativity of the associated ratios.

Considering the debt-in-the-numerator ratios, KO is above average in the debt-to-assets, debt-to-equity and net-debt-to-EBITDA ratios. In fact, these ratios are the third highest in the companies under consideration. This indicates that KO takes on a significant level of debt. In particular, its capital strucure is about 3 parts debt to 2 parts equity.

Correspondingly, this is reflected in the below-average status of its debt service coverage ratio.

On the other hand, its cash flow coverage ratio and free cash flow yield are both ranked third (disregarding the "inf"s in the data) in the industry on hand. The ratios of TAP and TAP-A (Molson Coors Beverage Company) are significantly larger than the other firms'. KO's CAPEX Coverage Ratio is also the third highest in absolute terms (i.e. removing the minus sign). 

In summary, while the debt that KO carries is significant, it is fairly well covered.

+++ {"editable": true, "slideshow": {"slide_type": ""}}

##### Valuation

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
"""
ratios = companies.ratios.collect_valuation_ratios()
series = ratios['2023']
index_levels = series.index.levels[0].tolist()
column_levels = series.index.levels[1].tolist()
df = pd.DataFrame(index=index_levels, columns=column_levels)

# Populating the DataFrame with values from the Series
for idx in df.index:
    for col in df.columns:
        df.loc[idx, col] = series[(idx, col)]

df = df.T
df['average'] = df.mean(axis=1)
        
print(df.to_markdown())
"""
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

|                           |          BF-A |          BF-B |          CELH |           KDP |            KO |          MNST |           PEP |             STZ |          STZ-B |           TAP |         TAP-A |        average |
|:--------------------------|--------------:|--------------:|--------------:|--------------:|--------------:|--------------:|--------------:|----------------:|---------------:|--------------:|--------------:|---------------:|
| Book Value per Share      |   6.8003      |   6.8003      |   1.1143      |  18.2306      |   5.9786      |   7.7778      |  13.3789      |    43.6973      |   43.6973      |  60.7271      |  60.7271      |   24.4481      |
| CAPEX per Share           |  -0.3808      |  -0.3808      |  -0.0736      |  -0.3415      |  -0.4268      |  -0.2283      |  -3.9899      |    -5.3775      |   -5.3775      |  -3.0902      |  -3.0902      |   -2.06883     |
| Dividend Payout Ratio     |   0.4828      |   0.4828      |   0.1211      |   0.5236      |   0.7422      |   0           |   0.7364      |   -15.2649      |  -15.2649      |   0.3738      |   0.3738      |   -2.42666     |
| Dividend Yield            |   0.0141      |   0.0147      |   0           |   0.0248      |   0.0313      |   0           |   0.0292      |     0.0144      |  nan           |   0.0269      |   0.0255      |    0.01809     |
| EV-to-EBIT                |  28.2358      |  27.1708      |  41.9362      |  18.7134      |  19.9574      |  27.0489      |  22.1233      |    75.6298      |   16.1678      |  12.7862      |  13.2642      |   27.5485      |
| EV-to-EBITDA              |  28.2358      |  27.1708      |  49.4688      |  15.561       |  23.248       |  29.2072      |  15.8259      |    18.3361      |    3.9198      |   8.916       |   9.2493      |   20.8308      |
| EV-to-Operating-Cash-Flow |  48.442       |  46.6149      |  94.5078      |  45.8048      |  24.9317      |  34.3846      |  20.0561      |    21.4608      |    4.5878      |   9.0961      |   9.4361      |   32.6657      |
| EV-to-Sales               |   7.3327      |   7.0562      |  10.126       |   4.1093      |   6.3204      |   5.6328      |   2.9474      |     6.2591      |    1.3381      |   1.616       |   1.6764      |    4.94676     |
| Earnings Yield            |   0.0275      |   0.0287      |   0.0171      |   0.0471      |   0.042       |   0.0266      |   0.0387      |    -0.0008      | -inf           |   0.0715      |   0.0679      | -inf           |
| Earnings per Share        |   1.6293      |   1.6293      |   0.9571      |   1.5486      |   2.4692      |   1.5416      |   6.5611      |    -0.2         |   -0.2         |   4.3668      |   4.3668      |    2.24271     |
| Enterprise Value          |   3.10029e+10 |   2.98335e+10 |   1.33462e+10 |   6.08746e+10 |   2.89182e+11 |   5.90643e+10 |   2.69595e+11 |     5.91652e+10 |    1.26481e+10 |   1.89108e+10 |   1.96177e+10 |    7.84764e+10 |
| Interest Debt per Share   |   1.33628e+07 |   1.33628e+07 |   2.87141e+09 |   4.7124e+07  |   1.57514e+08 |   1.84507e+09 |   2.63216e+07 |     6.16042e+06 |    6.16042e+06 |   8.10873e+06 |   8.10873e+06 |    4.54791e+08 |
| Market Cap                |   2.84639e+10 |   2.72945e+10 |   1.32755e+10 |   4.63176e+10 |   2.54945e+11 |   6.12959e+10 |   2.34511e+11 |     4.65171e+10 |    0           |   1.32698e+10 |   1.39767e+10 |    6.72606e+10 |
| Net Current Asset Value   |   2.717e+09   |   2.717e+09   |   9.14167e+08 |  -5.541e+09   |   3.161e+09   |   4.42731e+09 |  -4.697e+09   |     5.278e+08   |    5.278e+08   |  -1.2441e+09  |  -1.2441e+09  |    2.05989e+08 |
| Price-to-Book             |   8.7099      |   8.3521      |  50.2767      |   1.8039      |   9.8278      |   7.449       |  12.6742      |     5.5288      |    0           |   1.0056      |   1.0592      |    9.69884     |
| Price-to-Cash-Flow        |  44.4748      |  42.6477      |  94.0072      |  34.8515      |  21.9799      |  35.6838      |  17.4461      |    16.873       |    0           |   6.3828      |   6.7228      |   29.1881      |
| Price-to-Earnings         |  36.353       |  34.8596      |  58.5344      |  21.2364      |  23.7958      |  37.5822      |  25.8442      | -1207.97        |   -0           |  13.9843      |  14.7293      |  -85.5498      |
| Price-to-Earnings-Growth  | nan           | nan           | nan           | nan           | nan           | nan           | nan           |   nan           |  nan           | nan           | nan           |  nan           |
| Price-to-Free-Cash-Flow   |  62.2842      |  59.7254      | 107.246       |  54.6198      |  26.1563      |  41.5227      |  29.595       |    27.0213      |    0           |   9.4279      |   9.9302      |   38.8663      |
| Revenue per Share         |   8.798       |   8.798       |   5.5621      |  10.5183      |  10.5448      |   9.9112      |  66.1374      |    49.0934      |   49.0934      |  53.8523      |  53.8523      |   29.651       |
| Tangible Asset Value      |   1.811e+09   |   1.811e+09   |   1.07436e+09 |   5.474e+09   |   9.122e+09   |   6.8108e+09  |   9.09e+08    |     8.085e+08   |    8.085e+08   |   8.1098e+09  |   8.1098e+09  |    4.07716e+09 |
| Weighted Dividend Yield   |   0.0133      |   0.0138      |   0.0021      |   0.0247      |   0.0312      |   0           |   0.0285      |     0.0126      |  inf           |   0.0267      |   0.0254      |  inf           |

+++ {"editable": true, "slideshow": {"slide_type": ""}}

KO has the highest dividend payout ratio (0.7422) and dividend yield (3.13%).

Its EPS at 2.47 is higher than average (2.24) while its book value per share (5.98) is below average (24.45).

Its price-to-book is slightly above average, while its PE ratio is below average if we compute an average without the negative value data.

In summary, compared to its peers among the large to mega caps in the beverage industry, KO pays good dividends. It is moderately priced from the perspective of the PE and PB ratios. 89e+08 |

+++ {"editable": true, "slideshow": {"slide_type": ""}}

### Valuation by Multiples

+++ {"editable": true, "slideshow": {"slide_type": ""}}

#### Basic Idea

The idea behind the methdology of Valuation by Multiples may be illustrated as follows.

Suppose a firm $X$ is required to be valued.

We will find a collection of companies $(Y_i)_{i \in I}$ that are deemed similar to $X$, and a ratio $\rho = \frac{v}{w}$, so that $v$ is the valuation required and $w$ is a measure that can be readily obtained from the financial statements or market data for the firm. Let's call I the *collection of similarity* and $\rho$ the *valuation multiple*.

Then we compute the average ratio 

$$\overline{\rho} = \frac{1}{n}\sum_{i \in I} \rho(Y_i)$$

for this collection of firms, and take this to be *the* ratio that is associated to this collection.

We will asssert that the correct ratio for $X$ is also $\overline{\rho}$ as it belongs to this collection.

Hence, we may conclude that

$$v(X) = \overline{\rho}w(X)$$

There can be many variations to this description. For instance, instead of averaing that ratio, we may consider the weighted average, with suitable weights.

Nonetheless, two choices that commonly stand out in discusions of Valuation of Multiples are:

- Collection of Similarity:
  - Companies from the same industry (commonly called: Comparable Company Analysis ("Comps"))
  - Companies in the same transactional situation (commonly called: Precedent Transaction Analysis ("Precedents"))
 
- Valuation Multiple:
  - Equity Multiple (e.g. P/E, P/B, Price/Sales, Dividend Yield (=DPS/Price))
  - Enterprise Multiple (e.g. EV/Revenue, EV/EBITDA, EV/Invested Capital)

Enterprise value takes cash and debt into account, unlike equity value.

+++

:::{admonition} What is Enterprise Value?
:class: note
Enterprise value (EV) is a financial metric that represents the total value of a company's operating assets. It is calculated as the market capitalization of the company plus its total debt minus its cash and cash equivalents. In essence, enterprise value reflects the theoretical takeover price of a company, considering both its equity and debt and excluding any cash that could potentially be used to pay off debt.

The formula for calculating enterprise value is:

$$ \text{Enterprise Value (EV)} = \text{Market Capitalization} + \text{Total Debt} - \text{Cash and Cash Equivalents} $$

This metric is often used by investors and analysts to assess the overall value of a company, especially when comparing it to other companies within the same industry. It provides a more comprehensive picture of a company's value than just looking at its market capitalization, as it takes into account the impact of debt and cash holdings.ings.
ings.

:::

+++

#### Example (Comps)

##### Premise

Suppose that a company X belongs to the Diversified Consumer Services industry according to the Global Industry Classification Standard (GICS). Use the P/E ratio to value X by the Valuation by Multiples method if it is given that its latest earnings is USD 50 million.

+++

##### Discussion

+++ {"editable": true, "slideshow": {"slide_type": ""}}

#### Example (Precedents)
##### Premise


A list of the largest M&A's between January 2023 and March 2024 is found [here](https://docs.google.com/spreadsheets/d/1rHLlOIsjG1DinZeJhFyArBlCL6vSOHyQv8YWUa_CJa8/edit#gid=0), a part of it is shown here:

| Announced Month and Year | Acquiring Company                                  | Acquired Company                                      | Deal Size     |   |
|--------------------------|----------------------------------------------------|-------------------------------------------------------|---------------|---|
| March 2024               | AstraZeneca                                        | Amolyt Pharma                                         | $1.05 billion |   |
| March 2024               | Reliance                                           | Paramount Media (Stake)                               | $570 million  |   |
| March 2024               | Alcoa                                              | Alumina                                               | $2.2 billion  |   |
| March 2024               | EQT Corp                                           | Equitrans Midstream                                   | $14 billion   |   |
| March 2024               | Nationwide Building Society                        | Virgin Money                                          | $3.7 billion  |   |
| March 2024               | Sentinel Capital Partners                          | Carrier Global Corporation’s Industrial Fire business | $1.4 billion  |   |
| March 2024               | VIAVI Solutions                                    | Spirent                                               | $1.3 billion  |   |
| February 2024            | Capital One Financial Corporation                  | Discover Financial Services                           | $35.3 billion |   |
| February 2024           | "Stone Point Capital and Clayton, Dubiler & Rice" | Truist Financial Corporation                          | $15.5 billion |   |

Data Source: [intellizence.com](https://intellizence.com/insights/merger-and-acquisition/largest-merger-acquisition-deals/)

Value a firm X that is currently (March 2024) in the process of being acquired with the Valuation by Multiples method using the ratio EV/EBITDA if it is known that the EBITDA of X is USD 50 million.

+++ {"editable": true, "slideshow": {"slide_type": ""}}

##### Discussion

+++

Many of these acquired companies are not listed, so it is not easy to find financial accounting data about them.

Let $c$ represent any one of these acquired companies, and $\rho(c)$ represent the ratio for the company.

The average ratio is given by

$$\overline{\rho} = \frac{1}{N}\sum_{c} \rho(c)$$

supposing there are $N$ of them on the list.

SSince the EBITDA of X is  USD 50 million, therefore, its EV is USD 50&rho;
 million.
