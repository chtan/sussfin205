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

# Horizontal and Vertical Analyses

```{code-cell} ipython3
# https://www.markhneedham.com/blog/2017/09/23/python-3-create-sparklines-using-matplotlib/
import matplotlib
matplotlib.use("Agg")
import matplotlib.pyplot as plt
import base64
from IPython.display import Markdown
from io import BytesIO

def sparkline_inline(data, figsize=(4, 0.25), **kwags):
    """
    Returns a HTML image tag containing a base64 encoded sparkline style plot
    """
    data = list(data)

    fig, ax = plt.subplots(1, 1, figsize=figsize, **kwags)
    ax.plot(data)
    for k,v in ax.spines.items():
        v.set_visible(False)
    ax.set_xticks([])
    ax.set_yticks([])

    plt.plot(len(data) - 1, data[len(data) - 1], 'r.')

    ax.fill_between(range(len(data)), data, len(data)*[min(data)], alpha=0.1)

    img = BytesIO()
    plt.savefig(img, transparent=True, bbox_inches='tight')
    img.seek(0)
    plt.close()

    x = base64.b64encode(img.read()).decode("UTF-8")

    return Markdown('<img style="display:inline-block;margin:0px;" src="data:image/png;base64,{}"/>'
                    .format(x))

def sparkline(data, figsize=(4, 0.25), **kwags):
    """
    Returns a HTML image tag containing a base64 encoded sparkline style plot
    """
    data = list(data)

    fig, ax = plt.subplots(1, 1, figsize=figsize, **kwags)
    ax.plot(data)
    for k,v in ax.spines.items():
        v.set_visible(False)
    ax.set_xticks([])
    ax.set_yticks([])

    plt.plot(len(data) - 1, data[len(data) - 1], 'r.')

    ax.fill_between(range(len(data)), data, len(data)*[min(data)], alpha=0.1)

    img = BytesIO()
    plt.savefig(img, transparent=True, bbox_inches='tight')
    img.seek(0)
    plt.close()

    x = base64.b64encode(img.read()).decode("UTF-8")

    return '<img style="display:inline-block;margin:0px;" src="data:image/png;base64,{}"/>'.format(x)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

*Horizontal analysis* refers to comparing accounting data over a number of periods. If the data in the initial period is taken as baseline, and data in other periods are expressed as a percentage of that, then the analysis assumes the name of *base-year analysis*. Horizontal analysis or base-year analysis allows one to talk about *trends* in data. 

If the number of periods (or time steps) is 2, then the analysis is also called a *change analysis*.

*Vertical analysis*, on the other, focusses on data in one accounting period. It involves comparing data with each other within that period. If a particular field is taken to be the reference and all other fields are expressed as a percentage of that, then the analysis assumes the name of *common size analysis*. Vertical analysis or common size analysis allows one to talk about the relative sizes and distribution of values in a collection (e.g. the relative sizes of components making up the asset side of the balance sheet, thus allowing one to appreciate the kinds of assets that a firm holds).

+++ {"user_expressions": [{"expression": "[1,2,4,4,7,8]", "result": {"status": "ok", "data": {"text/plain": "[1, 2, 4, 4, 7, 8]"}, "metadata": {}}}, {"expression": "sparkline_inline([1,2,4,4,7,8])", "result": {"status": "ok", "data": {"text/plain": "<IPython.core.display.Markdown object>", "text/markdown": "<img style=\"display:inline-block;margin:0px;\" src=\"data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAUoAAAAnCAYAAABpNAE1AAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjguMCwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy81sbWrAAAACXBIWXMAAA9hAAAPYQGoP6dpAAADX0lEQVR4nO3dy27UZhiH8WcYCIUUEhAiIFGwKGLTC+hFcANskZB6C951W/UKYMFVIO+66IZNJVZsUQUBqZCQllMmITR1F7bFeMaeL0PsOeX5SZa/OfpzFn+9tsdvOmmaIkmqd2zaE5CkWWdQSlKAQSlJAQalJAUYlJIUYFBKUoBBKUkBBqUkBRiUkhRwfNoTkDTfojjpAEvAt8ByvnztePC5U3XbvfT+DY/u3aFbvrtwH4hI05fN7aFBKR0ZUZx0gdMcLKDGHXcnsAu9fNkGej9s/NntpunNgfd0gRuAQSmpLIqT48AV4HrFcg04C3wzgans8SXQSsHWt97pG/cGxlWf6wG7QKl0/HH9yeUU/uiUTyHuA0+b3qmOTTGk+RDFyQrlAPyechgetPBJGQ6ibbIAO0yw9YB/D7eX4/n9/t3b196++rWTVZL7wE+k6YOmt2NQSjMirwq/o7oqvA6cD3zFHvACeJ4v6/n6JfCOcnW2MH7+7f7anccPV4CnTZ+bLBiU0gRFcbJKdUVYVIWhc31vKIdg//ovBg5Pj4j/nv1y61WbG/AcpdSgKE5OMLoqPBf4ik98qQqLECzG62QVoSbMoJTGFMXJeeqD8CrhqnCT4SAsHr/maFaFM82glAbkVeFV6sNwNfAVn6g+NC7WO23MW+0xKFUSxckysAZc7FsvTXVS7TvLcFUYumttg/qqcAOrwoViUC64/K6JVcrht1bxuBgvT2Wis2eX+qrwBVaFR4pBOYfyOywuMDrw+h+fGHMTu2RXV4tloX5OUmGH4TDcmOqMNFMMyhkRxclJ6gNvMPwuAJ0xN/GB7CJCEX6jxh8PtzfSYjEoW5If8hbn+0ZVe8V4ZcxNpMA/ZME2KvQ2gS2yCwySvoJBOUIUJ0uEmwGcob76q+18UuMzWcBtUR2Am32v/U12y5akls19UEZxcoz2OqI08ffZYTjk6gLwbQPbk9SwiQRlfhh6ksP3qhurX12DPlPdIGA7X7aoP+z16qg051oNyihOnpP9NGWZ9vvVFR1RBjucHKTzSaiLykQ7okiaLW1XlCtkP+btt0d2VbWoxvrHVc9VvV71His3Sa2we5AkBfjPxSQpwKCUpACDUpICDEpJCjAoJSnAoJSkAINSkgIMSkkKMCglKeB/UE5Cu4MrlxMAAAAASUVORK5CYII=\"/>"}, "metadata": {}}}, {"expression": "[1,2,4,4,7,8][::-1]", "result": {"status": "ok", "data": {"text/plain": "[8, 7, 4, 4, 2, 1]"}, "metadata": {}}}, {"expression": "sparkline_inline([1,2,4,4,7,8][::-1])", "result": {"status": "ok", "data": {"text/plain": "<IPython.core.display.Markdown object>", "text/markdown": "<img style=\"display:inline-block;margin:0px;\" src=\"data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAUoAAAAnCAYAAABpNAE1AAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjguMCwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy81sbWrAAAACXBIWXMAAA9hAAAPYQGoP6dpAAADXElEQVR4nO3cXW7TShyG8cctB5oWxFcDveAjSLCaiB1wzRq6hJwlgMQNe+jFWcsRVSjoSC0VLYXyUZr6XLhW3MRm0saThPD8JGsmTuyMb179PYknSdMUSVK1hWkPQJJmnUEpSQEGpSQFGJSSFGBQSlKAQSlJAQalJAUYlJIUYFBKUsClmCdvrW98Bq6O+PFvwBfgsNCO08/bo26n7eNHki4sifkIY2t94zXQBFaA5cJWfB1bj/HDtqz/tdtp9yYwfklTFjUoAVrrG2tU3+InwBLlAVoVrCtAo2Tf4DGXo1zQWd+BA+AtsFmyve922scTGIekiKYdlDFdojx0zxO8VfuTEcdwTBaibygJ0m6n/WnMa5Q0AfMclDEVq+DrwD3gIfDgtH0I3Cdc1X6kvBLdBN5ZjUqzwaCMZwFYox+eg+1q4Pge/Vv6oYq022nvRxm1pCEG5fQsk4VmsQrN+/eBK4Hj9/h1NfozzrClP49BOZsS4C7DAZq3zcDxPWCL4QB9Q1aN7sUZtjSfDMrfU4PqW/oHhKvRfaqr0S2rUeksg3L+JMAdssBsMRykdwLHnzBcjR5EGuusOAJ2gO287Xbah9MdkmaJQfnnaZDNgVZVo0vTG9pMOaQQnIVtp6S/79Nf882gVFFCNv85GJ6NaQ5qApbI/oXQPG1DUxeD8oq0LEQHX+/6RNfvx6CUhl2lH5qrJf0mcPu0vXbOc6fALuEqdRvY6XbaP8a8FtXAoJTGswTcoh+gVYHaBG4y+lNduU+MFqrbwKFTAHEYlNLkLNIP1WKAloXrKvDXOc//jepA/UxgEZhup3108UubbwalNLtuEA7UvF/HPPIx9a2uNbjS1kkN4yuXJPeAJ8C/pOn7KF9hUEpzYZny+dRVskBdob+oS74QTN4/b+V6EcX1Zmtbc3bz76fPFkhfkmXMCfCcNH1V9+CjLtwraWK+kv3/desCx+YrbYVWzQqtwFX22XxOtnG6hZ4qG9nawS5pQvbzWGYBeEGS/FN3ZTmJoPwwge+QNJsaZP8iyCvaYn+lYn/ZZwbfv/xo7z8Wh++IF4HHQK1BGf3WW5KiyOYm33J2aq8HtOquKA1KSQrwRxZJCjAoJSnAoJSkAINSkgIMSkkKMCglKcCglKQAg1KSAv4H3I989L5lQvsAAAAASUVORK5CYII=\"/>"}, "metadata": {}}}, {"expression": "[4,8,1,4,6,2]", "result": {"status": "ok", "data": {"text/plain": "[4, 8, 1, 4, 6, 2]"}, "metadata": {}}}, {"expression": "sparkline_inline([4,8,1,4,6,2])", "result": {"status": "ok", "data": {"text/plain": "<IPython.core.display.Markdown object>", "text/markdown": "<img style=\"display:inline-block;margin:0px;\" src=\"data:image/png;base64,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\"/>"}, "metadata": {}}}]}

# Trends

A trend is a tendency in how a sequence of quantities evolve.

The basic types are: increasing/growing, decreasing/diminishing, or stationary/fluctuating

The trend in a sequence of quantities may be visualized for quick and easy appreciation. Thus, an increasing sequence {eval}`[1,2,4,4,7,8]` may be visualized as {eval}`sparkline_inline([1,2,4,4,7,8])`, a decreasing sequence {eval}`[1,2,4,4,7,8][::-1]` may be visualized as {eval}`sparkline_inline([1,2,4,4,7,8][::-1])`, and a fluctuating sequence {eval}`[4,8,1,4,6,2]` may be visualized as {eval}`sparkline_inline([4,8,1,4,6,2])`.

Among trends of the same types, one may talk about the strength of a trend. For example, one increasing trend may be stronger than another.

+++

## Example

Let's visualize the trends on the income statement of The Coca-Cola Company FY2019-2023:

```{code-cell} ipython3
# This code reads just one sheet from a Google Sheet which may contain multiple sheets
#import pandas as pd
#sheet_id = "1OCqC9k0lz1kRZ-RXYRMgI8ebjHum1xHEfAYKl4t3Nos"
#df = pd.read_csv(f"https://docs.google.com/spreadsheets/d/{sheet_id}/export?format=csv")

# This code reads all multiple sheets from a Google Sheet
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
        df = pd.read_excel(data, sheet_name=name)
        break
        
df['Sparkline'] = df.apply(lambda row: sparkline([row[2019], row[2020], row[2021], row[2022], row[2023]]), axis=1)
df = df.set_index(df.columns[0])
df.index.names = ['Field']
display(Markdown(df.to_markdown()))
```

We can't compare the magnitudes of trends here because the numerical data sets have their own individual scales. Though we can talk about the general shapes of these sparklines.

From the sparklines, we can quickly observe that the revenue and net income has been on a rising trend since 2020 when Covid-19 initially struck. On the other hand, Other Expenses, D&A and EBITDA have been decreasing. 

It is unusual for Other Expense to have a negative value.


+++

## Changes

The simplest trend is indicated by just 2 quantities $x_t, x_{t+1}$, in which we may observe the (absolute) change $x_{t+1} - x_t$ or the relative change $\frac{x_{t+1} - x_t}{x_t}$ commonly expressed in percentage format.

Let's take a look at the changes and relative changes on the FY22-23 income statement for The Coca-Cola Company: 

```{code-cell} ipython3
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
    if name == 'Income Statement Changes':
        df = pd.read_excel(data, sheet_name=name)
        break
        
df = df.set_index(df.columns[0])
df.index.names = ['Field']
def f(x):
    try: 
        return "{:.2%}".format(x)
    except:
        return x
df['Relative Change'] = df['Relative Change'].map(f)
display(Markdown(df.to_markdown()))
```

We note that over the years 2022-2023, the net income of KO increased by 12.16% even though the revenue growth is only 6.51%. This may be attributed to the decrease in General and Administrative Expenses (-9.83%), Other Expenses (-263.93%) and D&A (-61.30%).

You may also find the the sheet here: 
- [](https://docs.google.com/spreadsheets/d/1OCqC9k0lz1kRZ-RXYRMgI8ebjHum1xHEfAYKl4t3Nos/edit#gid=884666173)

+++

## Base-Year Analysis

+++

## Common-Size Analysis

+++ {"editable": true, "slideshow": {"slide_type": ""}}

## Reasoning about Trends

### Inconsistencies

Suppose that there is a 20% increase in sales. If it is also known that there is a 25% increase in freight-out cost, then the situation calls for further investigation and explanation.

Suppose that there is a 20% increase in accounts receivable. If the sales increase is just 5%, then one should try to find out the reasons behind the significant increase in accounts receivable which does not commensurate with the increase in sales. In instances where there appears to be a relaxation in maintaining credit quality, it prompts a deeper inquiry into the underlying reasons behind such a shift.

In the first case, the directions of trends supported by the evidence differ. In the second, the magnitudes of changes are incompatible.

```{code-cell} ipython3

```
