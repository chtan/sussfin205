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

# Horizontal and Vertical Analyses

+++ {"editable": true, "slideshow": {"slide_type": ""}}

*Horizontal analysis* refers to comparing accounting data over a number of periods. If the data in the initial period is taken as baseline, and data in other periods are expressed as a percentage of that, then the analysis assumes the name of *base-year analysis*. Horizontal analysis or base-year analysis allows one to talk about *trends* in data. 

If the number of periods (or time steps) is 2, then the analysis is also called a *change analysis*.

*Vertical analysis*, on the other, focusses on data in one accounting period. It involves comparing data with each other within that period. If a particular field is taken to be the reference and all other fields are expressed as a percentage of that, then the analysis assumes the name of *common size analysis*. Vertical analysis or common size analysis allows one to talk about the relative sizes and distribution of values in a collection (e.g. the relative sizes of components making up the asset side of the balance sheet, thus allowing one to appreciate the kinds of assets that a firm holds).

## Trends

A trend is a tendency in how a sequence of quantities evolve.

The basic types are: increasing/growing, decreasing/diminishing, or stationary/fluctuating

Among trends of the same types, one may talk about the strength of a trend. For example, one increasing trend may be stronger than another.

+++ {"editable": true, "slideshow": {"slide_type": ""}}

### Example

Let's visualize the trends on the income statement of The Coca-Cola Company FY2019-2023:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
tags: [hide-input]
---
# https://www.markhneedham.com/blog/2017/09/23/python-3-create-sparklines-using-matplotlib/
import matplotlib
matplotlib.use("Agg")
import matplotlib.pyplot as plt
import base64
from IPython.display import Markdown
from io import BytesIO

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

# This doesn't work for markdown tables because myst parses CommonMark
# Ref: https://github.com/executablebooks/jupyter-book/issues/1105
#display(Markdown(df.to_markdown()))

# Copy and paste the output of the following into a markdown cell,
# then comment it out.
#print(df.to_markdown())
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

| Field                                        |     2019 |     2020 |      2021 |     2022 |      2023 | Sparkline                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|:---------------------------------------------|---------:|---------:|----------:|---------:|----------:|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Revenue                                      | 3.73e+10 | 3.3e+10  |  3.87e+10 | 4.3e+10  |  4.58e+10 | <img style="display:inline-block;margin:0px;" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAUoAAAAnCAYAAABpNAE1AAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjguMCwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy81sbWrAAAACXBIWXMAAA9hAAAPYQGoP6dpAAAEFUlEQVR4nO3dT2/bZACA8SftBpv4M9qV/oFNWAKG+AjcufULcJ2ExFfwjWvFJ2AHPgXyjQMS4oLEhSuTqAZbq7ZrM9bB1rU1B9vCeZvEb1pnbZrnJ1m2nMxx1OrR67dO1snzHEnSYDPnfQKSdNEZSklqYCglqYGhlKQGhlKSGhhKSWpgKCWpgaGUpAaGUpIaXDnvE5B0OSVp1gGuA28PWd5qeLxaroXHX/57h5+/vcts76cLj4CEPP+rzfdiKCX1SNJslpMBiw1a+NzZlk/vOfAUePrp1h/5bJ5/GDw+C3wEGEpJJyVp9jrxMRsWvzdaPrWcIm77tfV+sC98vN/z9oHD6qCfPfhtJYdfOr1TiEfA/ZbPn45fiiFdHEmaXQUWgEXg3XKpthcYHr6rLZ/OAc0hqx571mdftf0vRSxb9+O9L7/4oLv5TacYSR4BX5Hn37X9OoZSGqNa+OrBG7Y918LLhtEK47XfZ1+/8B20cC5j9/UP95bu/vr9DeB+23OTFUMpjSBJsysMHvH123ea8B0Du8BjYKdcPwb2gCf0j9yz2vr4VG9uch2vr61ujvMFnKPUVKuFb1Down1nDV89frv0hrDa7jKmS1WdjqHUpRKEL+Zyd/4UL3NMMboLIxeOAqv1E6ZvlHepGEpdaGX4bhI3v7fI2cIXXuqGI71q2/BNGUOpc1EGcBm4VS63a9sr/B+/eaAz4uFz+l/qDopfF8OnIQylWldGcIWTAQxjGHsz8qDwDZvjM3xqzVhDmaTZTxQ3g24B2+XSb3tnfW11Im5FmHbl7S4rDA5gFcGY7xE4BDaBDeBRuWxQ/F7U5/m6GD6do7HdHlR+zvMF8TfBdhkc0nB7e31t9WXLpzz1kjR7DXiP3uiFQVwm7lL4JUUEq/iF6w2Kn6V/3dVZTfTtQR3gc+AOxV8hbwZLtW+eYvTxTrl8HHPwJM26xEV1i2LEOtVhLSP4PoMDeAtYIj6CYfTqI8JHFKNBI6hLYew3nCdptszwy7AORSDr8ay25/vsr8I6qj3iolpNBUxMWMvP+FYRHHRJvBR5uAOaI7iLEdTFMdEjylg5RcT2iPsw+wxwg96A1kenYWznyn8zVy53Yk4qSbMqrDGj1rGFNUmzaxQRHDYnuBh5uBf0BjC8JK4iKKnmIoRyVNU9b6OGNQxoGNZqfZawjjIVcJik2XWGR/B2eV4xnnNyFBiOCPcijyWpZhJDOap6WH+PeH41XxpGddAcaxjWT2JOKkmzfeDNyPfwHHjI4AhuYASlsXkVodx+Ba/Rto0RnjtDMTINvw6r/omR+rJAMS9bRfIf4E+KLxp9OGDbCErnyG8PkqQG/udiktTAUEpSA0MpSQ0MpSQ1MJSS1MBQSlIDQylJDQylJDUwlJLU4D8eUbadAdoLkQAAAABJRU5ErkJggg=="/>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| Cost of Goods Sold                           | 1.46e+10 | 1.34e+10 |  1.54e+10 | 1.8e+10  |  1.85e+10 | <img style="display:inline-block;margin:0px;" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAUoAAAAnCAYAAABpNAE1AAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjguMCwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy81sbWrAAAACXBIWXMAAA9hAAAPYQGoP6dpAAAD+UlEQVR4nO3dz24bRQDH8a+TNP1Dk/AnKCnQskhwRJx4Cr8A10pIvMLeuFo8AT3wFGhvHLhwQeLKhSJFJLROQ0v/APnf5TBreeOsM3aym9jJ9yONxl47s2vL/mlmdtZp5XmOJGm4mYs+AEmadAalJEUYlJIUYVBKUoRBKUkRBqUkRRiUkhRhUEpShEEpSRFzF30AkpqRpFmL8B2fB64X9XxD98/SxqmsvvyLn769z+zRqwsPgYQ83zhtu1UMSmnCFAG3DHwEJKX6feAG44XWNNoF9oD9gfrI7U+7v70xm+efDfztLPAxYFBK06wIwrfoB2DC8VC81cCuc0IIDQZP+X55227F4yeF1yjbhrXbe/xw1Bfz+cavd3L4uXV0CvEQeDjyOzKilj+KIdUvSbMlqgOwd3sh0kQOPCb0jNaBP4BHwDanC6w94HUdr22S/Pjgyy8+fN79phV6kofAV+T5d3Xvx6CUTiFJs9sM7w0mhB5jzBNCAG4U9XqpPCKEmyK+/uHByv1fvl8CHtY9N9ljUEoVkjS7yfEgLN9eHqGZp/R7g4Nh+CewU+cxX2Gv1zrtbpM7cI5SV1KSZteBewwfGq+M0MzfHB0aD4bhdr1HrYtiUOpSStLsGnCX4SdM7gCtSDOv6AdgLwx7IbhRPK4rwKDUVErSbI6wXCahOgw/IH5BxTYh/IaF4Yu6j1vTyaDURErSbAZ4j+EnTO4S//zucHxoXA7DZ3Ufty4ng1LnqlhDuEgY+g6W1aK+V5TYgul9+uE32BtcB7bqfwW6igxK1aLoAb5DdQAOlpsjNntIODtcDr9yGG4S1htKjWo0KJM0exP4Z63TPmhyP2pOcVJkhXj4rTDe5+kVYR3hZkXdJQRilzGu1JCa0nSP8nfg7STN/iNMjJ9UXp70+Fqn7eLbGiVpdovjQ96qskz87HDZU/qh1ytVYegaQk2NxhacF3NRe9QXxjvEw/bEstZp79Z0LBOpeM+XGG34uzhG0weE+b5y2FUF4FbxXOk8Nb7gvNErc4ph2yeEL+9CqSyOUdf54wC7nKFXW5SdtU77XOfFivm/dxl+8qNcbozR9A7Vvb1evVXUz3AuUJNruoMSIEmzVc72A8GznBywtyu2Vz2nLvucsWcLbK912nmSZvOMPv83O8YxviA+9H2CC6Z1OXgJI2Ey/3lRTmuGEJbj9mbLIbxAmKu7Rpi3G+Va32EOkjT7l9DTHlXO0fm/YUPgLZz/k2p1HkF5WdaytQjBuTRQFiu2Ddu+SAjtOfohuU84u/u4KN1SXd6+iWeApQvhrwdJUoT/XEySIgxKSYowKCUpwqCUpAiDUpIiDEpJijAoJSnCoJSkCINSkiL+Bzj9qzHSsHYdAAAAAElFTkSuQmCC"/>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Gross Profit                                 | 2.26e+10 | 1.96e+10 |  2.33e+10 | 2.5e+10  |  2.72e+10 | <img style="display:inline-block;margin:0px;" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAUoAAAAnCAYAAABpNAE1AAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjguMCwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy81sbWrAAAACXBIWXMAAA9hAAAPYQGoP6dpAAAELElEQVR4nO3dT2/bZADH8W+asnUU1vJna9aJ9TlsQpy4j5eQN8B1EhJvwTeuEa+AHXgVyDcOXLhM4sCBC5qQBy3JWEfSNR2tWGsOzxPVcW0/Do6TJv19pEdxU8v2Ju2rx3+SNeI4RkRE8q3M+wBERC47hVJExEOhFBHxUChFRDwUShERD4VSRMRDoRQR8VAoRUQ8FEoREY/VeR+AiFw9JgivA+8DH7jX5HLRezdG22i92ufHbx7RHP904SlgiOPdaR6vQiki/5sJwmvAe5SLXPJ36xV2ewoMPt6PTppxvJ36XRO4DyiUIjJdJghXuRi4MrO8dyrs9gw4AP4GBkDfvSaX+6nlAXAI8Fn0850YnjTGLyGeAk8rHFOmhr4UQ2R5mCBsYmd4ZU9lR8s3K+w25mLgkstZsetjg1cpQD88/uLznUHv64adSZ4CXxLH31bZZhaFUuSScaezN93YSC37wrdZcfcHTBa7AfAKOzuci6++f7z16KfvNoCn0742OaJQikxJQeCyfi5a5/oUDueQ7LCl30u+f8Acg1fBWdRp9+rcga5RypWXCNwkMasrcEmvscEbjSH+2PWxM7w3Uz6WK02hlIVVELgywZt14A6xARum3stb5wh7zU0uAYVS5s4E4RrwEbDjxi3KzehmFbii99I/K3BLSKGU2pkg3OA8gulxD2hV3MUocEPGZ21ZQctbR4GTXLWG0gThE6ABdHPGn8DzqNPW9ZQFZYKwAdwmP4Q72Fmgzz/Yh4R3gRdkn6qmAzd0Q4GTWtV219sE4Qpwgj/GMfYfRlFMu0Av6rSPazlYyeUeRL5L8YxwrcSm+sAe5zFMj/60j12ujIW/6/0Q+AR7arWFnXncTi03E8ufFm3MBGEff1C7Uac9rOHPspRMEN4gP4A72Eg2PZuJgR42eHtkB/F1DYcvMhO1P0dpgrBF/rcUrWAfkh2FMyumo9dJLtwP8cTUjUHUaS/tg6TutHiT4tPiWyU29S/j8UuHsOvWEZmHhZ9R+pwB+2784ll3k+yAJpdbwNvYz58+cKPIiQnCMkHdjzrtS/cgrru8sUVxCN8tsakjLs4AkzH8i4ofNRNZZPMO5SQGbvzqWW+dcjPUTews1bhR5I0JwucUx7TLlG9MmSB8i/HHZu5x8fT4WolNvST7uuDoNHkwrWMWWUaLFMqyjoDf3Ciyhj3t9EX1Q+zf0103isQmCNM3prKC2os67WMThOsUzwa3sU8NFDnj/PpgXgx1E0ykgmUMZVnHwB9uFFnFBtV32j/pjakjyn0n3wn5N0h2sZHU41UiNZpFKF/MYB912yuxzgp29nnHjVbO8jb2dHkUyQPgmRu/p16fYa8Pisgc6duDREQ89J+LiYh4KJQiIh4KpYiIh0IpIuKhUIqIeCiUIiIeCqWIiIdCKSLioVCKiHj8B7bYupmq+b1lAAAAAElFTkSuQmCC"/>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| Gross Profit Ratio                           | 0.6077   | 0.5931   |  0.6027   | 0.5814   |  0.5952   | <img style="display:inline-block;margin:0px;" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAUoAAAAnCAYAAABpNAE1AAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjguMCwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy81sbWrAAAACXBIWXMAAA9hAAAPYQGoP6dpAAAFS0lEQVR4nO3czU8cZQDH8e9AsVpLSw8W5KUMVXuwQhPjQY/G4/4DXo2m/gvEi9eN8eKxTTyrV83eTOQPKFZL1aYCDvQNS1FaBFrosh6eme4zs7P7sLCzb/19kskuW3Z2SsJ3n+eZWbxSqYSIiFTX0+oDEBFpdwqliIiDQiki4qBQiog4KJQiIg4KpYiIg0IpIuKgUIqIOCiUIiIOR7LasT9d8ICHQL/18BawYG3z1v3lIJ97mtXxiIgclJfVRxj96cJR4CvgTcAHRqg9gn0KBKRHdDHI57YzOVAREYfMQhnxpwtDmED2AWPAOCacvnX/DHDUsas7VAZ0AZgP8rn1hh+4NF04CzkO7Ab53ONWH49IpJmhrHkcwKuYcI4DE8SD2l/tiaF/qIxodH8lyOf0lz9aJIxfPzAIDIW39pZ87CWgCFwHZoEr4TaneEqrtEsoXU5RHoEmI/qK47nJdVE7oloXPYAwfieoHTz7sRcb8LJPgTlMPKOAzgX53JMG7Fukpk4JZS3HqD4S1broPlnx28+o7yDx2wRWw+0BcD+8TT62inljnAIuWLenUva5i4nnFcrxvB7kczt1HptITd0Qylr6gFHia6JRUOtZF61YGw3yuX8zOeIGCuN3kv2N+gZx/zySNqkMnr3Zjx/2TWeEeDgnSY/nDnCN+MjzN8Wzi3neKPAG8Cel0u1MXqLLQ1mLh4mET3kUao9KTzien7YuGn2d2bpoGL8B3KO+IeA09cfvP6oHLxnFVq8ZjhEfeU5ifjZJO8CvxEeevwf53G5zDlMy43kfl+CyZxqzB1ykVPq64S/zHIfSxV4XjW6jiJ52PHcLWCQ9ohXroon4pQUvGcUX6vy/bFA7ePZUuNXxO6wzxOM5Rfqb3hPgF8rhnMXEU2vWbSa81DD6/ZsAzgITY+v3zs1cvjjVG29YEfAbPbJUKA/GXhf1iUd0FPe66BKwjDkbHI38DhK/WsGzH+v0+B2WTzma0ZZ2JcVjTDztkecNxTNb/nShB3PVSyyE1v0RzAww5r2la3zz7Wdpu3yfUmmmkceY2SdzutwW8Ee4JUXromnXi45jpsKvhVvSI/Z3smMNxa8eQbh9H37tkR7P48C74RbZ9qcLV4mPPG8E+VyxCcfdNfzpwgCVIYxufdxLRNuYwcUScAtY3vO8jRJ86cUHJkXMzK2hNKJsrmhddBwYpjwlvo+Jny51aR0P80sbRTM6YfRyyvduAVeJjzxvPs/xTEyPkyGcIP3Em60I3KUcwmjWFd1fS3vSzKVPPhxfX/nCg95wH59qjVKkuTzML3vyhNGxlO/dBH4mPvK8GeRze8051GwlpsdpIUydHiesUT2E9zDLUnX7/MdLgx/N/nASmNdZb5H20IMJhH2p0luYTxQlbVA58pxv13ha0+O0EPq4p8dbmPjZWxTCW+G/Z2EvyOdWMto3oFCKNEIP8DrxKfsk6RflP6Jy5LnQjHha0+O0EJ4l/dIqWxFzbXEyhtGWOj1uAoVSpEP1YuJpX6Z0nvR4PsTE0x55LtZ7LW44PR6m+tnjYdzT4wdUD+FdTCzbjUIp0kV6gXPEz7SfJ31Ku07800WzwF+YT1pVGxGOV9mXLTk9fnYWmWynx1nKPJS6PEikeYqULyv7LnzsCPF4XsD8DdcB4INwi+zgvt42mh7bJ02iEC5hPlEmdWpGKFeb8BoinewO8JP1dR9mpPk28E54O0U5kn9jRpcB5hNg9v3btOf0uKNlPvUWEel0WjsUEXFQKEVEHBRKEREHhVJExEGhFBFxUChFRBwUShERB4VSRMRBoRQRcfgfnX4ge14JPdcAAAAASUVORK5CYII="/>                                                                                                                                                                                                             |
| Research and Development Expenses            | 0        | 0        |  0        | 0        |  0        | <img style="display:inline-block;margin:0px;" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAUoAAAAnCAYAAABpNAE1AAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjguMCwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy81sbWrAAAACXBIWXMAAA9hAAAPYQGoP6dpAAABW0lEQVR4nO3dMUoeURSG4e+oVZDUQSwMuBFXYWcTSBoX4DIs0ljYZwMWbsDezsJSrAIBEYuAXotfBAl6mhnij8/TTXPPVC935ha3xhgB4HUr//sFAN47oQRoCCVAQygBGkIJ0BBKgIZQAjSEEqAhlAANoQRoCCVAQygBGkIJ0BBKgIZQAjSEElhuVZup2knV5lwj1uZaeOvgpJJ8mmt9gPPD3b3Pyc9KVkbyUFXfM8bx1HNmC2UWkbydcX3gA/ty8zvrf+9ST8+1+EI+StVpxriacpZPb2Apff1zndV/r7JZTbI99aw5d5R3SdZnXB/4wPbPfm2M5KJebvjuk1xOPatcLgYsrapvSY6y2EneJ/kxxz9KoQSW2+K0ezvJ5dT/Jp9HCCXA2xzmADSEEqAhlAANoQRoCCVAQygBGkIJ0BBKgIZQAjSEEqAhlAANoQRoCCVAQygBGkIJ0HgEkPAyybTX9h0AAAAASUVORK5CYII="/>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| General and Administrative Expenses          | 2.01e+08 | 1.26e+08 |  3.37e+08 | 3.56e+08 |  3.21e+08 | <img style="display:inline-block;margin:0px;" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAUoAAAAnCAYAAABpNAE1AAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjguMCwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy81sbWrAAAACXBIWXMAAA9hAAAPYQGoP6dpAAAEm0lEQVR4nO3cXW/bVBzH8W/SNmTskTDYBkU1G4wnAYJpMLKXkFeBQPAC4MJ33Ea8A4p4Fch33MAFD9oGgiE0sU3LBEKL2rK1sC4kTczFsRc7sWcni52k/X2kI/ssTnySRb/+7WOn4LouIiISrzjtAYiIzDoFpYhIAgWliEgCBaWISAIFpYhIAgWliEgCBaWISAIFpYhIAgWliEiCxWkPQCSKZTtFYMlrpcD6OP1JvMY4/cLEP5jRtIBt4J63zGS9Ua91cntHUQqFZeB54Cqu+2cmu9AtjDJNlu0sAa8DVeA88A7wNLv3aKcN7AAdb5nU7wTWu8AjwD6g7C0H10v5vZX7dsg4jL31VqNe64X2XCi8B6xivi894ANc94tJv0EFpeTKsp0KJgyrXnsLeDTl04PhMWrIdMbsR73euK/RTfs5PYQi/eCMC9Pgety/pXnuNCrme17bXr7TbH+z+v7JYjjDuoA16cpSh96SGct2CsBp+tViFXgpYtNN4KLXLgA3MKEXDJqdHIa8G/ToV2FZKzFasI67HqyS/ccqz2w2KQ4XegvAc4CCUmaTZTv7gLP0q8Uq8HjEptcxgeiH4zVAhzbzp+21zYz3E6yS7wdoqdtZduHzQriy7WK+TxOloJSxWbbzFOFq8U2Gv1Mt4Gf61eJF4HaOw5T5F1klf33yzOWbR45/vHLn1qcFU0l2gQ+zmNDROUpJxbKdReBV+pXieWAlYtMm4WrxV8zhs0gmPvnqs2PvXvryMHBNs96SK8t2DgPn6FeLbwMHBjbrAb8RrhYz+aKKPECvUa/dynIHOvQWf9LlFOFq8RWGZzX/AS7RrxZ/BO7mN1KR6VBQ7kGW7ZQx5xP9arEKPBmxaYNwtfg7pooU2VMUlHuAZTvHCFeLZxi+MLlNf9LFb+s5DlNkZmUalJbtrGIu+1gLtPVgv1Gv/ZflGPYay3YWgJcJV4unIjZdJ1wtXgb0fyESIbPJHO+81zbmmqcH+Zf4IF2PeGyrUa9pBspj2c5BzESLXy2eAw4NbOYCVwhf1H0zx2GKZGmuJ3MKwEfAs5iLjiveMri+iJlJPeBtl0bbsh0/QJNCdQ3YaNRredw6ljnvj88K4WrxNYbvi97GTLr41eJPwFZ+IxXZXTK/PMiynePE/8DBIcLBObgcDNa09wQHucDfxAfpUL9Rr7XG2M/EWbZTAt4gfFH3iYhN/yBcLV4hn/uKRWbBXFeUaWx57UbK7cuY0IyqTgf7FeAxTGXrP/ZCmp1YtnOXlKHqtYmcDrBs5yjh2//OMnzqYgdzPjF4UXfzYfctIvGmHZSjagF/eS2NBeAI0UHqrw/2l4D9XrNS7qcTczogrr+BuczmRcLV4umI175NPxQvAL9gPgcRycm8BeWouphQ2hjhOQeJrk7jqtf9mHA9QfRhcRQXM8McNdF1lXC1eH2EsYtIBvIIyrUc9jFJTUb79ZEycBR4wlv663H9CuZ0QBnzu3o/AN8B3wLfox+MEJk5utdbRCTBbv25fRGRiVFQiogkUFCKiCRQUIqIJFBQiogkUFCKiCRQUIqIJFBQiogkUFCKiCT4H7wiwu/AIgJfAAAAAElFTkSuQmCC"/>                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| Selling and Marketing Expenses               | 7.12e+09 | 5.42e+09 |  6.67e+09 | 7.09e+09 |  7.61e+09 | <img style="display:inline-block;margin:0px;" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAUoAAAAnCAYAAABpNAE1AAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjguMCwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy81sbWrAAAACXBIWXMAAA9hAAAPYQGoP6dpAAAE4ElEQVR4nO3cTW8bRRzH8W+atmlC2hSk0hZaskDCUwoCLoC4cONgIXHkWgmJF8Blb71avAJ64D0goRUSQoJLL6AmUagqQdtko1Y0JYSaPBM1MYeZbWadXU9S7zp28vtIf633wfbkQT/Pzqy3p16vIyIi+Y7sdwNERDqdglJExENBKSLioaAUEfFQUIqIeCgoRUQ8FJQiIh4KShERDwWliIjH0f1ugIgcbEEY9QADwFCTOu3ZPwT0uq97bvFvrn19md70tws3gYB6/V6RP4OCUkRy2ZDrZ2+B1njsKYrLmi1gEVh6fX5ms7deDxr29wIjgIJSRPwyQu5Je3NFhtySrX/tcrFhmbXNPXY1ebEPZqfO1+GXnvQQ4iZwu6D2PtZT5k0xgjC6gvkBx4HJuFqplfZmIgeEDbg+YNCpk7Z225tLArCMkGsMMnfZbN9KQW157Oern382XJv7qsf0JDeBL6jXvyn6fUoLyiCMejG/nAFn8wwmNCdsjcfVylwpDRBpAxtqxzEh1hhsgy1sS43HtahOOuR8gZZ1bOEhV5QrP149e/n6d0PA7aLHJhNlBuUA8CXwPvAWcCHn0DkawhOI42pF93+TQjmhVkSQuetlDmGtY0IqqWY9ubzT1xVMWB5UW2V3uEo99QYIwugcZgzhNDAGvAlcsjUC9GQ8rQZMkg7Q3+Nq5VGpjZWO0RBqrQaZu97OUFvBjKktN6yvONtWnX3LDeurmNNJaa70oGznZE4NuGYr0Q+8gQnNJEBfw4TqR7YSa0EYTbHd65wAbsTVynq5zZYyBGF0FBgGRjEfmG49iwk3hZp0hP2e9V4DrttKHANeIR2eY5ixzvdsJR4FYXSTdHhOxtXKUvlNF58gjI4BATvDcNRu3+3/3zqthVhWICrUZNfaeerdiiPAi2yfsicB+nTO8bdIh+dEXK3Mt9gGyRCE0XHM3yYJQDcMh2k+KbEOxLZmnOUcCjXZvQM1RlmG50gH5yW7Lcs90hNGE8BdTRr5BWF0gvwwfIHmf981tkMwto+T9TkO9iSDtMeBGqMsw5+2fnC2PcN2aCYh+hJm1v0C8Ilz7EIQRo3heSuuVrbKb3pnCcKoH/N7ygrDi2RPuiVWgWmyw/BBSU0WaZtu71Hu1lOYSSO35/kq2R8UK5gZdzdAb8bVykZbWloie8nWy+wMwxFMGDazTDoAY7bDUcMasp/UoyzICvCrrUQfZtLIDc8xTKh+aCuxEYTRDdLhORVXKx13EW4QRoPkh+Hznqcvkh4rTJYzwEIpDRbpAoclKLP8B/xmK9GLOf1sHPccAt61ldgKwugP0td6TsTVyj9lNzwIo5PsnEVOHp/3PL1Gfhg+LKO9It3usJx6t+oiO2fcz+YcO0t6zHMcuL/XSaMgjIbID8O89048JH2a7D6u7aUdIl1Ap94d4q6t751tZ9gZnsNOfeoc+5edNHJ7n9OYnmpeGJ7xtGmB7DCcxdxtRUQKoqB8cvPAT7YSpzDjnG6AjmK+afKxrcQG5it6vveISc8oT2PCUBfVi7RJO4LyMM2IPsBc7P6ts+0EJjDfsfU25iYhfXb/fcz985K6Y1/jDmamWUT2WeljlCIi3a7bJ1lEREqnoBQR8VBQioh4KChFRDwUlCIiHgpKEREPBaWIiIeCUkTEQ0EpIuLxPyKl8QGHUVL0AAAAAElFTkSuQmCC"/>                                                                                                                                                                                                                                                                                                                                                             |
| Selling, General and Administrative Expenses | 1.21e+10 | 9.73e+09 |  1.21e+10 | 1.29e+10 |  1.4e+10  | <img style="display:inline-block;margin:0px;" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAUoAAAAnCAYAAABpNAE1AAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjguMCwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy81sbWrAAAACXBIWXMAAA9hAAAPYQGoP6dpAAAEjklEQVR4nO3dT28bRRzG8a/rQEMpxAhBUmjlScqhggMHbgiOnPwGuFZCQryDvXG1eAVUiFeB9sYBkDiAxIHeKiWpXf4kUUqVpG2S0ibLYWbr9XjX45jd2E6ej/ST155kPVKjJzO/3bi1JEkQEZFiFyY9ARGRaaegFBEJUFCKiAQoKEVEAhSUIiIBCkoRkQAFpYhIgIJSRCRAQSkiEjA36QmIyPlgorgGXAZeAxqu8o6LXrucPd/S3n1+/vom9f6/LjwCDEnyZ5lzV1CKyMhMFF8kHGpF4w2gXsI0HgO7N7bvPq0nSdMbqwPvAApKERmPieI68Cqjr+T88fkSpvEU2M3UXuB59rU97KqRD7u3ryTwa62/hXgErJYwxz41fSiGyOxw29dLjLd9bQALJUwjoT+4smG24x6LjneBwxLmAMAPtz77tLmz+VXNriSPgM9Jkm/LOn9KQSlSERdqL2J7a6PUy4HxV7BB90IJ0zugOMjyAjC7unuEDcup8OX3txZv/vbdArBadm8ypaAU4fmW9BLlBFq2yujJ5XnGaFvXvBXfQ+z296w47rRbm1W+gXqUMlNOuEo7SaC9VPHUD7EXIdLad/U453X/tYPMcRp6BxXPVzIqDUoTxd9gf3OtYRusa8B6p93ar/J9ZXaYKJ4HlrFXKq+7eoPh4Vflz+0xdmtZFGZF4Tbs6/bdeWVGVbb1NlF8AfsDcjFneINecGZDdK3Tbj2oZEIyMSaKF7ABmA3D9PnbQG3MUx8yepDlhVfe1z0Zcy4yOTO99a4DXwDvAyZTC8AVVx/732SieIeCEAU2Ou2WfjNPGbcdXqQ/ALPHrwdO8QjouOoCWwyGmFZpMjGVX8wxUbxE/31ODaBJLzizx4uB0x0A6+SHaLfTbp2lBvVUMVE8B1wjPwyvY7fEw9wH7mKDsEsvGDuAdhHyf8z0irLIjqvfc8bmGQzRZfd4Fdtwf8+V78hEcZfBAE239OqLBrh+4Qr5YWgYflvKMfA3+WHYxa4CRWbStF31PgTuuPLNYftZhsEQbWJDdsXVJ/43myjeID9EV89TX7SgX5geh/qF/wL36F8NpoH4B2frlhOR56YtKId5Rm+l8qM3VgPeJH8738Ru99O+6Ef+iV1f1A/R9Him+qJevzAvDEP9wocMbo3T5xtM0Y3GIqdlloJymAR7AWAL+CVnvEEvPP0QXXLjH7jyHZooXic/RCfSF/X6hX4YrhDuF27TH4DZ43OzuhYZ1VkJypAdwn3RbIAa9/yaG3/Xle/IRPE98kN0vdNujd2X8/qF/oWTZYb/2x0Df9ELwC79vUP1C0VO4LwE5TCj9EWz/VCTeUxvll4mvy+6SfGtTg+wt0r59xWmx1cD835Cr1/orwzVLxQpkYJyuGxf9Kec8UUGQ9TQ64suuRroi2IDOvSRVWm/MF0NduiFofqFIqfkNIJy+xTeY1K2gNsFYw36V4vZeoteSG7h3cqUqX8qmreInIA+PUhEJED/uZiISICCUkQkQEEpIhKgoBQRCVBQiogEKChFRAIUlCIiAQpKEZEABaWISMB/myrPSQJy1KsAAAAASUVORK5CYII="/>                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| Other Expenses                               | 4.58e+08 | 8.53e+08 |  8.46e+08 | 1.22e+09 | -2e+09    | <img style="display:inline-block;margin:0px;" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAUoAAAAnCAYAAABpNAE1AAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjguMCwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy81sbWrAAAACXBIWXMAAA9hAAAPYQGoP6dpAAADo0lEQVR4nO3cy1IjVQCA4T/AjFheR8sSKdTo6HhduRlgYZU7q9oHcOvGjS/AE1j9BuPGZ3DFfqZmAspcHF3qpi3dWLpwbkySGWgXDXb6CDmBJB0u/1eVAvp0wgGq/5xuII08z5Ek7W9q0hOQpKPOUEpShKGUpAhDKUkRhlKSIgylJEUYSkmKMJSSFGEoJSliZtITkOrQXFmdAp4AZnfe9r4/6LbD3GevbcMuUP4BrgKXd24/ZWmyNeRjqo+G/8I4nObKagM4S3EQ9B4cs5FtZ4FGn4fu94M56WMzjD5mZ/p8vknaAjo7t27wNnx/FvgIeDp4jDsU4bxCEc7bWZo8rmHup8axDuVOpGYYLEyDbjvM/XW85FRD1AXa7B+qYcf3C2GXIpQHMQ18CCwDi8BF4Jlgn3uUK84rwC3DOZyxhrK5svoJ8CSji9he2/qtyiahzf8PkDZ7Hyzb+zxGv6/pJIzF5FS/j70h6v2+Hma8A5ykaEwBH1CGcxF4NtjnPnCNMpw3szR5VOMcj71xh3KTIpR1CQ+SvW7tPfYbZFu/APbGT5qkKeB9YIkinheB54J9HlCEc/dU/Ybh7G/coWwBT1E+o4fP9INsG/TWpf91L+k0mgLeowjnEsWK8/lgn02gRbnivJ6liU/6PcZ+jbK5sjqHf4YkHRUN4F3KFecicC7Y5yGwRhnOjSxNOjXO8cgxlNLp1gDeoVxxLgEvBPu0KcK5e6q+kaVJu8Y5TpyhlBS6QBnNZeDFYLwDrFOG8/uTHk5DKSnmLcrT9GXgpWC8A/xAeaq+nqXJwzonOG6GUtJBnad6jfPlYLxLEc7dFed6liabdU5w1AylpGG9SfUa51ww/gjYoAznWpYmD+qc4LAMpaRRa1K9xvlKMP4YuE55qt7K0uR+jfM7MEMpadxepxrO+WB8C7hB+SIfrSxN7tU4vyhDKalur1L95dBCML4F3KQ8Vb+WpcndOicYMpSSJm2B6jXO14LxbeAW5an61SxN7vw32mgsAG8Dv5Lnf4xjgoZS0lEzT/W36s1gfBu4DVy+9N3XZz79Ze2rRtGYbeBL8vzbUU/IUEo66uYpgrm74nwDYO7u37S++YLpasO2gOaoV5Z1vML5XzV8Dkkn15/Aj8ClnY/ngY8Xf//58+k8/yzYd5riD+RHGspj/cK9kk6x4trkb1TPWMeyojSUkhThtUNJijCUkhRhKCUpwlBKUoShlKQIQylJEYZSkiIMpSRF/AvI3JBBke+a5wAAAABJRU5ErkJggg=="/>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Operating Expenses                           | 1.26e+10 | 1.06e+10 |  1.3e+10  | 1.41e+10 |  1.59e+10 | <img style="display:inline-block;margin:0px;" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAUoAAAAnCAYAAABpNAE1AAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjguMCwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy81sbWrAAAACXBIWXMAAA9hAAAPYQGoP6dpAAAEF0lEQVR4nO3dwW4TRwDG8b8JUZoQCBASDE3ULUI99Bl4g7wAV6RKvfe0NxCniCeAQ5+i2lsPvfRSqVLPLQe3JZAUB0LtxArEWQ4zK9brrGd2u2u78feTRt5drT1jWfk8szPrNOI4RkRE8l2YdANERKadglJExEFBKSLioKAUEXFQUIqIOCgoRUQcFJQiIg4KShERBwWliIjDxUk3QERmWxBGS8A6sJZ5zDu2AND8t83PTx8wN3h3YR8IiOMXVbZRQSkilQrC6DNMsPkE3xpwqUQ1va/af3bm4ng9c3wOuAsoKEVkfIIwmufs4MsLwCslqjkG9m1pZx6T7fR+717rt1sx/NIYvITYB56XqH+khn4UQ2S2BGE0B6wyenibPnatRDUnnB12ece6Zd7LT8++uf/Fwe6ThulJ9oFviePvy7zWKApKkf+5IIwuYMLMN/hWgUbBak6BN4wOu/Sxd//lPRXx8MdnNx/8+sMK8Lzqa5MJBaXIFLDD28uZciWzv8LgEDgJvhuYHlVRb/ELvjZwAExrWJy2trd266xA1yhFSgjCqAEskh9qRY8tVNCsd/gNc9uYkOxXUOdMUFDKzLDX5papJtiWKdeLczkGDjHX7Lqp7Y59TIa/6QBMyoca2iMoKGXKBWG0QDXBdhlYqqmZ2WDrZI51GQ6+vOMKuymkoJSxs9fjbgObqbKR2r7Bp5Cbr6EJJwyGmk+AZcMvee4R03vtTipSa1AGYfQY8w25myl7re2t93XWLZNhh7e3ODsAk9Kk+KxrD/9eWV6oJdvHZd+fzKa6e5TfkbPqPgijNwwH6Fllv7W9dVpzO8WDXYZyk/wA3MD0FH2u3X0AXgEvUyXZbzMYdodo4kEmqLblQbZn8Qj4ksGlDGsUG071gT38QrXb2t7SMKgEO4u7Rn4AbgKf4/fZ9TGfRxKAOwyH4j4asko1al8eVPs6yiCMmgzeYtQAruJ3O9T1gtUd4ReoMzX0tyF4jfwATLZ9lqicYr64sr3AdHltzxMZh3O5jjLGrOF6C/zuOPci5i4C103165jlGkvAHVtGOk9D/yCMVsgPwGTfd8b3H/ID8CUmJDUMlpky7bPeJ5g/zD2PcxfJD9Fs0M5jeqvXga8dr9sPwmhiQ/8gjJYZPTGygZkd9rHPpwDcYbhXuIuWp4gMmfagLKIH/GWLy1X8hv6rmImJ27a4HAVh5D30D8JokdEBuGnb6uOA4d7fq8y2ZntFSjhPQVnEgS1/OM6rc+jftc/x0SE/AJPS83wtESloHEH5egx11GnH87wlzNKZpi15203M0D8JyUPgb1t2UtsvUo+dCt6HiJSkXw8SEXHQPxcTEXFQUIqIOCgoRUQcFJQiIg4KShERBwWliIiDglJExEFBKSLioKAUEXH4CCw5uc5HgAjxAAAAAElFTkSuQmCC"/>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| Cost and Expenses                            | 2.72e+10 | 2.4e+10  |  2.83e+10 | 3.21e+10 |  3.44e+10 | <img style="display:inline-block;margin:0px;" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAUoAAAAnCAYAAABpNAE1AAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjguMCwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy81sbWrAAAACXBIWXMAAA9hAAAPYQGoP6dpAAAEFklEQVR4nO3dP2/bRhzG8Udx7Lhx3bR26j/NUKLo0pfQ1+Cha9cABfIWuHU18gqaoUNfQ8GpHbp0KdClc4C6aNO4kQzbsp0oiSVmuCN8OpE8UqJsSf1+gAOPlH0kYfvBT+RRbqVpKgBAsVs3fQAAMOsISgAIICgBIICgBIAAghIAAghKAAggKAEggKAEgACCEgACbt/0AQBYXFGcrEj6wGv3craF2l1/7J1uR79+91BLw08X9iVFStN/mjwPghLAkChOWpJWVRxadYLuTsOH15N0Lunsi/af6VKafua9viTpc0kEJYBRNuDWVC3AQmHXdDa8lHQmG3Je3192C147l/Q2G/DLv/7YTaXfWsOXEPuSnjZ87GrxoRjAbIniZFnSlqRtr22pPOzW1ex9h1TFYVYUeN2c1y5kAqxxvzz55utPTw4ft0wl2Zf0SGn6fdP7ISiBa+CF345GQ9Ddtjnh7gbKr8pCged/zUuZsJxp3/78ZPvh7z/ek/S06WuTGYISGJO9UeFXfkUhuFFz+L6kjqS2t+yqOPCy13oTnNY8Ghzs7x1OcwdcowQcXviFKr9Jw++Fs+63Y81BNfd/QVBi4dnwKws8d9tHNYfva7jqc0PQ3f5C0okIv7lEUGIuRXFyR8WVn79eN/wuZQLODbmiyu9EhN/CIygxM+wNj21JuwpXfh/WHD4LPzfk8io/wg8jCEpMXRQnt2Tu5D6Q9InTHnj9LUmtGkNfavTtbV7l1xHhhwlMNSijOPlK5g5cR9KRXV4c7O/xC7sA7ATndY0Gnh+Gu5KWKw6bhV9e5eeH4EkzZwKUm3ZF+YPMpFjXmyhO3OB0+/4y658RrtcripNVlVd/WX+txrAdSYe2/Webu34o8/PmZ42ZMrV5lPbt1k+6upO4ofGf+3yr4hAtWp4SrqOiOLktc52vqPrL+nWmvnSVH3puvy3n8TOgQVOfRzn1CedRnOzo6rGq92T+ALPgdPt52zZkHs4fx6VMYIaqVXd5erC/NxhzfzfKvg3eVDgAt1X9MbeeygMwW3/V1HkAY1i4CeevJD2zrapV5QeoH7Du8q7MuWV3SasaRHFSVqXmBezxtMM1ipN1lb/9zdpKxSH7Mtf8iqq/rH/a2EkAc2we7nr3JP1rW1WrMoFZFKZ529ZkKq2PbatqEMXJsapfFujIhGvfzgXcVfhmyPs1judIwxXf85z+kczzwAAqmIegHEdPJhSe1/ieFZVXqXkBm31ay6bqfZBBGsXJuf3+qs50VfEVVYFtSW9qjAmggusIyvY17KMpf9f8+mWZ0Lxv26bT8tbvy8wCyKbVSNJrXVXMZe1izHMCMCE+PQgAAvjnYgAQQFACQABBCQABBCUABBCUABBAUAJAAEEJAAEEJQAEEJQAEPAOuuK30MOE8ZwAAAAASUVORK5CYII="/>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| Interest Income                              | 5.63e+08 | 3.7e+08  |  2.76e+08 | 4.49e+08 |  9.07e+08 | <img style="display:inline-block;margin:0px;" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAUoAAAAnCAYAAABpNAE1AAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjguMCwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy81sbWrAAAACXBIWXMAAA9hAAAPYQGoP6dpAAAEa0lEQVR4nO3dy3LbVADG8b/jpHViaEsLNExhEPdb2zyIX4AtDDO8gnZsPTwBXfAUHe1YsGHJEqfQAOYypaRtfIE0obmIxZHGkqVjSbGU2PH3mzkjJdKxjjPKl6OjI6fm+z4iImK3dNYNEBGZdQpKEZEMCkoRkQwKShGRDApKEZEMCkoRkQwKShGRDApKEZEMCkoRkQzLZ90AEZFJHNerAa8At6Nlffjog++++nS5Hn+68Ahw8P0/y2yDglJEZobjeqvAh4yFIvDi+L5v9P6innwEuw68DSgoRWS+Bb3E14iH4QbwLulDgsfAz0AH2AQ2bwy3H/twtxbf/wjYKru9NX0ohohUyXG9JnCTURiGwXjZUqUH/EAQiMH6FrA/vuO3dz77+PX+wy9rpid5BHyO739d9ntQUIpIKRzXWwIckoH4FlBLqXII3MeEYYdRb3G7yHG/+ObO9U++v3sZ2Cp7bDKkoBSRwhzXuwTcIh6Kt4DnLFW2GfUOw57iFnBQQnOOu+3WwxJex0pjlCJi5bheHdMjjPYQb2N6jmn+A34iMpYYlCdVt7VKCkoRAcBxvask7zbfBFYtVR4QD8QO8AtmrPBcUVCKLBjH9ZaB90iG4quWKvvEw3ATuAf0q27rrKg0KB3X6wTHeAI8tixj6912q4wxCxEBHNd7mWQgfgRcsFT5nXggdoDfMNNzFlZlN3OCeVLPKB7GQyaHaWJbt91KTBsQWSSO610E3ic5L/G6pcouycvme8C/lTe2fHN/M2cDeAe4BrwAXJ2wvIKZQnApKG/mPYjjervkC9XoPk+77ZZu+ctcsTzOt4EJybTfZx/4lficxE3Mkys6/3OqfHqQ43rr5PvwjSXMBNRJYRpdhqV+wqbtk39IIFz+o3CVMgXBt8bofJ50vq9jpuBcs7zcgGQg/gjsVfcOZsLc9yiLOMbMyO8VqFMDnmfyyZW2bQVoADeCkteB43p5hwTC5aDbbi30+M4icFzvAvE/4EXOSdt4oU30cb7o5fODad+HpJuloDwJHzOmOQS6Beo1yR+q4bKBCdj1oOR15LjeDskQ3cGMB+0BT3Mu9zBDBudu+sUsCJ4siV7VFAm85pSHP8T0CPuRMhhb38HMUbyPma8op2Teg/KkdoPyR4E6DcwvRJGhgSZmaOCloJTCcb0D8odrrvC1beu2W4dltfs0BJeyTfL/EYyuXyH9UbsiBowCboC5Qop+3U9Z72N+5jKjFjUoT2Ifc2lT5PImvByz3bxaDUojx7IRed0VTM/H9qECpZkylAvtGw3l4C7ueJDlDbyVKd/2HtkBlxaCQxZ8Gs15paCs1jPg76BMqwZcpFi45glf27bQWYTyCuYGxzQOsF/KTurZDSjn+WM5R04jKB+dwjGkXDVGoblmWUZL1j627Wukh3LIx4RXDzM+14uU8a+j39tBl7JSIn16kIhIBv1zMRGRDApKEZEMCkoRkQwKShGRDApKEZEMCkoRkQwKShGRDApKEZEMCkoRkQz/A8lrv3FC40elAAAAAElFTkSuQmCC"/>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| Interest Expense                             | 9.46e+08 | 1.44e+09 |  1.6e+09  | 8.82e+08 |  1.53e+09 | <img style="display:inline-block;margin:0px;" src="data:image/png;base64,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"/> |
| Depreciation and Amortization                | 3.01e+09 | 3.73e+09 |  1.45e+09 | 2.92e+09 |  1.13e+09 | <img style="display:inline-block;margin:0px;" src="data:image/png;base64,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"/>                                                                                                                                                                                 |
| EBITDA                                       | 1.31e+10 | 1.27e+10 |  1.55e+10 | 1.38e+10 |  1.24e+10 | <img style="display:inline-block;margin:0px;" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAUoAAAAnCAYAAABpNAE1AAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjguMCwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy81sbWrAAAACXBIWXMAAA9hAAAPYQGoP6dpAAAE/UlEQVR4nO3d23LbRBzH8a9ybIYUKIUmJkmjAoWZzsAAhZYywxP4JbjiGfQIvuKSGS54DF3wAtyQtBymnEqKkzZpk6YF0pC0TRNxsVItyWutm9iOD7/PzI7sVFbWmfiX/66krRdFESIi0tjQcXdARKTbKShFRBwUlCIiDgpKEREHBaWIiIOCUkTEQUEpIuKgoBQRcVBQiog4jBx3B2Qw+UE4BrwPfApcibezll23gZvAUqolz5erlfLTTvRXBpunWxilE/wgnMYEYhKKF4ETud32gV+BLcAHSoBXcNh9YBl7iC5VK+Xt1r0DGWQKSmk5PwhHgHfJVovnLLv+DSwCC/H2B2An9e9jwBwwHzc/tT0LjDu6skE2RNNBul6tlPXLL01RUMqR+UF4mmy1+DHwQm63CPidWiguYgLrsDxgivoATUL1lOP1/1E8pN87Qt+kzygo5bn4QTgMXCAbjG9bdn0IXMUE4wJwLf5ap7yIqTp96sP0ddxD+hUaVKPVSrmT70O6gIJSCvlB+DJwmVooXsaEUN4S8D21avEPTBXZjcYwJ47yVaiPCdf83GnePepDNAnSuxrS9x8FpTzjB6EHvEO2WrxAffW1g6kQk2H0Vcx8Yz/wgDPYh/Q+7iH9DvVD+qRpSN+jFJQDzA/CSeAStVD8BHjFsusytVBcAH7DDE8H0UlqFahtSF90bfIB9iH9EnCzWilvtavTcjQKygERV4tvUAvFK8B71H+wHwE/UZtbXAQ2O9fTnjZK/ZA+2c7jHtJvYg/RJTSkP1YKyj7lB+EE8BHZYDxj2XWN2tziAvALoOFhezQa0s8Dpx2v3cUM6VeA28CtuD17XK2Udxq/XI5CQdkH4mpxjmwofkD9nVd7mGpxMdXudK6nUmCS+uF80mZp7nbjB2RD9Fbu+Wq1Ut5tdccHgYKyB/lBOI4JwnQwzlh23SA7t/gz8LhD3ZTWGcGE5VnMPGi6leLtZJPH2sQeosnj1WqlrN+RHAVlD/CDsEQ2FC9Sf1fKPnAdE4rJUPp2B7spx+sk2eC0tYkmj7VOcWW6Nmhn79salH4QXsd8oB/FbTf12Pb8KF/bBfZ6fcLbD8JRzEmWdDD6ll0fkD3h8iPmZyDSyEvYq9H0c9cJJzDXx96luDK9008LlrQ7KHdp7gffKhGHC9hWhPXjw4S0H4Svkg3FS9T/5Y8wi0Wk74v+63m/l0gTTmGmcWwhmmzHmjjOAeZEYVFlul6tlHviMrN2B+WH1Er+8bidyG1tj9PPkzZh+Vqyb7fIB2lRwA5hhtDnLcf5l9rtf4uYi7u1Eo50Aw9zhr5oiD9Nc0s4PsWEqS1Ek+cb1Ur5oLhH3izmc3SDKGrLdFPb5yjj5bXavUDwGI2DOB+6hwniokAvume4WTfIzi3+Sffe/ifi4gGv0Xh4P4NZ0GS4iWM9AVZpUJl+99Xnn5Uebn7pmYw5AL4gir5p7dvpn4V7n8TtOBYrGOVwQTyCWU3nGvBPpzst0kYR5oqLDczSeTbDmOtK00GaD9UpTBF0DssyfdNbm0xt309XKkPA13jet62uLDsRlPc68D1EpPes0ThIweRTCXON8Ey8ncNcKjV7/v7Km8NRlL/ldhh4ixZf8aHLg0SkN5m5yWWyU3v7gN/qilJBKSLioP+FUUTEQUEpIuKgoBQRcVBQiog4KChFRBwUlCIiDgpKEREHBaWIiMP/OKzkkrv7P4MAAAAASUVORK5CYII="/>                                                                                                                                                                                                                                                                                                                     |
| EBITDA Ratio                                 | 0.3514   | 0.3854   |  0.4003   | 0.3216   |  0.2719   | <img style="display:inline-block;margin:0px;" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAUoAAAAnCAYAAABpNAE1AAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjguMCwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy81sbWrAAAACXBIWXMAAA9hAAAPYQGoP6dpAAAD7UlEQVR4nO3cwVLbRgCH8c8QCklaGkjaQJu2TmhOeY3edO6p9z6DH0HXTk899DF86Dv0Bdo0rdpJhjIkDDgZCBBwD5IbWchaGyQw8P1mdiRLSFrD8Pdqd+VWv99HkjTazEVXQJKmnUEpSQEGpSQFGJSSFGBQSlKAQSlJAQalJAUYlJIUYFBKUsCNi66A6tPudFvAHDAPLGTL/HrZtjr359c/qOltrQO/lZQkiaOjmq4hVWr5CGOz2p3uDLAILAF3suVgfYH6w2ta7WflICtvc+v7hf1LwBrwScX5DoA/SEPzd3IhmsTRq2begq4rg3IM7U53luGQK4ZeseT3fczFdXEcMhxEB7llWVCF9o8bdMX9B6es/yLwiDQ08+Uh1R8KryhvhT5L4ui0ddE1dm2Cst3pzjFesJVtX6yhCm+BnVzpAXvUG2QHheVV/eO2gM85GaBrwGcVxx0Bf1EeohtJHF3V35fO6FIFZbvTvcn4LbliuVVDFXaBbYYDb/C6N2JfL1vfr+H6CrtJeYA+Am5XHNejPECfJnG012SFNf3ONSizwYbbTH77OijzNVRjEFzFsj1iez7w3tVwfV2cFcpD9AvSVmqZPvAP5SH6Iomj44brrCnQWFBmodgFlhkOu7OOtB8zOtBCgdfLjpfy5oE25f2hdyqO26UwkDR4ncTR6+aqq/PWaIuy3enuUN6/947yUNvmfYtv1L43XN2+N02fZYaD8+ts+RXVH/pOa7pCmg7Kb0n7BvO3u9ukgxjSZXYD+JLyW/l7FcflpzUNlSSOtpqssE6v8T7Kdqe7gk8A6XpZpDxAH1Ldz/6S0dOaDpussKoZlNL5mWH0tKbViuOOgD9J+z9fAP+S3tqv59Y3kjhyZkVDDEppOtzi/WDSoB90MK1p3KltWwyHZ349v63nnNHJGJTS9FslDc02cJ/00c7icm6C8+1xMkDLwnUziSOnxGFQSldBi3Qa06e5kg/S/PaPJjjvMbBJOFDXkzjareF9TC2DUrpeFjgZpGWBeo/J/m9fM95t/9ZlnKRvUEoqMwPcZXSQ5luuk3xr1SGwQbiVOv7gVKv1AHgMPKXffz5BXcZmUEo6qw8ZL1CXJjxvcXDqRKD++uN339zd3fmhlWbMMfA9/f7PZ35HBX5xr6SzepOVZ4GfmyMN01FBWhycWs7Kk7KTrfResrTXyz+kPwP8RKv1S90ty/MIys1zuIaky2HcAFsmHe1fyZXVrNwHVte2nj+Y7feL3wg1Szq9qtagvFRfsyZJ/0v7Jv9muGvvCGjX3aI0KCUpwEEWSQowKCUpwKCUpACDUpICDEpJCjAoJSnAoJSkAINSkgL+AwHuxQalvbYBAAAAAElFTkSuQmCC"/>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Operating Income                             | 1.01e+10 | 9e+09    |  1.4e+10  | 1.09e+10 |  1.13e+10 | <img style="display:inline-block;margin:0px;" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAUoAAAAnCAYAAABpNAE1AAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjguMCwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy81sbWrAAAACXBIWXMAAA9hAAAPYQGoP6dpAAAEwElEQVR4nO3cT28bRRzG8e82gVBE25R/qULSLE1LoSDuXJpwQ/Ib4IqK4C2suHC1eAUUiTO9oz3RQ0ullNLQkoo/TVohowZU4FBwmzhpHC+H2dVmx47HSXbd2H4+0mqd9dg7cZQnv52ZjRdFESIisr0DT7oDIiL7nYJSRMRBQSki4qCgFBFxUFCKiDgoKEVEHBSUIiIOCkoREQcFpYiIw/CT7oAMLj8Ix4CzwGy8nWnR7E/gNrBo7e9VyqVGVzoqA8/TLYzSLX4QHgNmMKE4A7zRotmvQBWYBl5s83Y14A7NIbpYKZce5ddrEQWlFMgPwnFMICbheLpFs1+Aq8AccA14sOW5I5jAnAZObtn7wFNtTv0HzRXoIqpCZZcUlJIbPwhfIVsxvmY1iYCfMcF4FfiebDB2agiYJBueyf6FNq+rAUs0h+iSqlBpR0Epu+YH4STZYDxpNYmAn0iD8RrwX8HdSqpQO0R92lehy1iX8PHjZVWhoqCUjvlBeJw0FGeBE1aTBiYY50grxmr3etjWEHCc1iHqqkK3BmfyWFXoAFFQyrb8IPTJVoyvWk0awC2yl9IPu9fD3IzSeix0CncV2jSZhKrQvqOgFAD8IPQwl6ezpOE4ZTXbBBaA7zBV43Wgn6uqpAq1K1BXFbqKGQu1Q3SpUi6tFNnhgeR5E8Ap4A5RtFzIKRSUgykOxhNkg3HSalbHBGNSMV4H9ItujNIcnsmMfLv1yfdoPSO/XCmX9Mu4U553DvgCc/NMA/iIKPoy99MoKAdDHIzTpIu7Z4AJq1kduElaMc5jqiPp3DDNY6HJ4+fbvG6V5rHQZEa+0J+BH4QHMEMM9vb0NseLbtdR2/Hq3yNXPj93dCibYZuAn3dlqaDsU3EwniJbMY5bzTYwwZhUjPOYyQspxlHS4LRn5F1V6G3gN8xKgrxDqCdvZX7n91t8deGTVk+9SxRdyvNcuoWxT8TBeJo0FGeBY1azx5hgnMNUjfPAWtc6KQ8wn/m8dTypQu0KdBpThU7SPCxStHq8bcT7x9bXnew3dti+vpPXjNaqoxFc8LJBvwnczfvDUEXZo+JgfJ3spfSY1WwduEFaMd5AwdhrtlahE5iKcq9h5Aq9ele+sxxcOv/h+1P/3v/MMxNvm8DHPTdG6QfhN5i/ltVdbGsa3E7FwXiGNBRngJetZuuYaiUZY7wZHxPpW59ePD/2wQ9fHwHu9uSstx+Ea8DILl9eZ3cBa28rvbimLR5gf5P0Uvos8JLVbA0TjEnF+CMKRhk8jUq5dL/IExQWlHEF9B5mbOUwcAh4Lt5vfdxq7+XYlQizCHovYfsQqFbKpcIuSeJgfItsxWiv1VvDLNFJ7nxZwFxGiQyy3g3KRPyvtXYyq+YBz2ICs5Ng3e7YIfKfrKqRT5W7Hn+fb5OtGO3lIzXM3S5JxbiAGUcSkVThQbkfZ70jzKLmFeCvPb7XCJ0Fqyt0n4nf72C82ZMmO5UMmh+0jq9igjGZlV6ghwbWRfpVN4Lyny6co2jDmMA8bG2tjrmOQ7p+7RFwBbgMfIuZlVYwiuwzWh4kIuLQkyvyRUS6SUEpIuKgoBQRcVBQiog4KChFRBwUlCIiDgpKEREHBaWIiIOCUkTE4X/wg/LxhmIqIwAAAABJRU5ErkJggg=="/>                                                                                                                                                                                                                                                                                                                                                                                                     |
| Operating Income Ratio                       | 0.2706   | 0.2725   |  0.3627   | 0.2537   |  0.2472   | <img style="display:inline-block;margin:0px;" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAUoAAAAnCAYAAABpNAE1AAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjguMCwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy81sbWrAAAACXBIWXMAAA9hAAAPYQGoP6dpAAAEhUlEQVR4nO3c7W4UVQDG8f90t7SKiEQlQKoZBUVEwa1QL2LuQhO/+20uYS6BmHgBXsBcgYD4RohtQUobp0bEhigiiA12u344O86eeel0zc7s2/NLJtN2zk5PN+3T8zpOp9NBRESKzQy7AiIio05BKSJSQkEpIlJCQSkiUkJBKSJSQkEpIlJCQSkiUkJBKSJSQkEpIlKiOewKyPRx/XAWOAO0gMXu8R7wXKroBnC557gdBZ62kkntHG1hlCq5fjgPvEsSiC3gHDCXU3wbuAUcAN4GnNT137CD83oUeE+rqblIQkEpA+P64SFMyzAOxEVM4DVyij8CVoHl7rECrAPt7vVDwPvAUvdoAfOpe2wDX5EE55dR4D0c2A8k0qWglP/F9cOXSMIwPr9RUPx37EBcBjaBfn75ZjEt06We40iqTAf4np5WZxR4P/fxPURyKShlT64fOsAJ7EBcBF4peMkvJGEYn+9VVL2T2MHp5pTZxO6u34wCb7ei+siEUlDKf7qh+Dp2ILaAowUv+RE7EFcwrcdhOQpcJAnOd8iu7PgDuEISnN9GgbddYx1lDCkop5Trhw3gNHYgtoDDOcV3gTWSQFwGbmLGGUfZQczPtoQJ0AvAM6kyT4FvSILzahR4wwx7GUEKying+uEccBa7+3yebGiACY4fsMcUb2EmTsZdEzO51Ntdfzmn3Cp2d31Ty5Kmm4Jywrh+eBATgr3d57OYyZC0J2RnnteAnVoqOxpc7OA8mVPmLnZwLkeB184pJxNKQTnGXD88QrIcJ24tvkV2/SHAA+yxxBXMGKMmNmwvYrrocXCeI7sx4xFwlSQ4v44C70mdlZR6KSjHhOuHx8hOsrxWUHyL7HKcuzVUcxLNY97rODgvkN1BtAN8RxKcV6LAu19nJaVaCsoR0515fhU7EBeB4wUv+YnszLP+SKszg9l+GU8QfQAcyyl3G7u7vqFxzvGloBwi1w9nMIu002sU0wupwSymXseeeV4FtBNl+BawxzlP55TZwg7OG1HgTdNY8FirNChdP/wEs6fXGeAxMyH3m8dMsqS7cQD/YFok6eU4f5e/6zICXsAe5zyP2b/e6y/gGklwXosC73GNdZQ+VB2UjzFr2aTYNvkzz3rYw+SYw0wKxcF5kex61TZwAxOaX2DGOX+tsY6yh6qD8hLJL0RnH8d+yw3qPmBmfQd1r/jjonv2ltnBdKU30MzztHGAN7G76ws55daxu+trGufM4TgLmCGsO3Q6leztr3yMsjtbqwcEi+ztBKalGW/BzHvM3EPM0qQ25h9t+pz3tYm6ltmn7zgfAZ9iMmYX+JhO57PSd7tPCkqR0fQ8yfbLosfMTasdoH38z/vty5c+fLZhZ1gbcAfdsqzjCedaqiLSvy3gDvB59/NZTHf9AObvtol5zmfeuYlpnBRd2+t1+73WqOCe8TlvF1mvJtB0H9yjkW3oNYBTwECDUsuDRGQ8mbHJTeweayUtSgWliEgJjR2KiJRQUIqIlFBQioiUUFCKiJRQUIqIlFBQioiUUFCKiJRQUIqIlPgXpyDScM8HI10AAAAASUVORK5CYII="/>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| Total Other Income                           | 7e+08    | 7.52e+08 | -1.6e+09  | 7.77e+08 |  1.64e+09 | <img style="display:inline-block;margin:0px;" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAUoAAAAnCAYAAABpNAE1AAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjguMCwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy81sbWrAAAACXBIWXMAAA9hAAAPYQGoP6dpAAAE/ElEQVR4nO3cz2/bZBzH8Xfa/Si0W8sQbOPH5o0NlrE2Bw5IiAMHDkjhgMSF6yQk/gXfuEb8BezA34AEMicOgLQDSJMoa9OOlS1DUNp1lCxboV3XmsPzuLYTp842u2nSz0v6KovjxE+r7pNvHj9Owfd9RESkvYFuD0BEZLdTUIqIpFBQioikUFCKiKRQUIqIpFBQioikUFCKiKRQUIqIpFBQioik2NftAYjI3uG43iAwBjwTue20RoFC8FrHGne4/PlFBuNXF24ADr7/R5bjVlCKyCNxXG8fYXiN8WhhdziDIfwH1M8t1R4O+v7LTY8NAmcABaWIPBnH9Q7w+GE3ksEQVoC7TVXf5n7w7wbwAOCtW5PHffipEJ9C3ADmMhhfTEFfiiHSmxzXG+Lxw+7pDIZwn84DLnq/ATzM4Ph8d+njj07WFz4rmE5yA/gE3/8ii9eOyjUoHdf7ENiPmVcYsLe9WHmMPXjn+8XWn7VKWe9assVxvVFgwlYJOAc8Sxh2QxkcpkHnARe938D8DXfdp99eOnrxytejwFzWc5OBvIPyPjCc2wH6yzJhaAY1XauU/+3qqCR3jusNAK9gwnAicut08HSf7T/C1ts81rC1mdGP0U2btUp5Ic8D5D1H+T3mTJVvC3u7GdnWXGzzWK/s18k+B4CzQBHzn+QI8I6tgO+43nVgkniA3lL32Zsc1ztMvEssAeO0/yg8D0wDVeAacId48N0j/JuTnOQ+R+m43jG0XjPNQUxonscEZxF4HROeSRq0dp9TtUr5Xv5DlU7YLvE08UCcAE61ecoqJgirkZrBhKFsr+c7SunMGjBlK+o5wtAMAvQsZonF27a2OK53AxOa0Q70Rq1S7oePV7uW43qHMF1hNBDHaX92eJ54GE4DN+mPj8F9SUG5uy3Z+iGybT/mo3oR04EGXehRTAdzGvggsv+K43pTxMPzaq1Sruc89r5ju8RTxOcRS5jfeZI1krvEet5jlWwpKHvPOjBr68vI9iOEXWfQhb6KOZn2pq0tjuv9ThicQYjO1SrlTJZt9DrbJV4g3iVO0L5L/IswCKuEXeKuODMsT0ZB2T+Wgcu2AoOYDig693keeBE4Yev9yP6rjutN0xSgtUr579xH3yWO6xUwZ5ejgVjCdO1J1oBfae0S/8l7rNI9Csr+FqzVnAO+imwfxazJiwZoEXgKeMPWFsf15omfOJoErtUq5fWcx58px/VGSO4SD7V5ygLxDnEG+A11iXuOgnJvugv8aCsQdFbRzrMInAResPVeZP91x/WqNAVorVJezHvwaWyXeJLkLrGQ8JQHxLvE4ASLukQBFJQS8jFzajeBbyLbh4l3n8HtCGEQbXFc7zatS5eqtUp5LY9BO643TNglRk+ytPvyhUWSu0TNzUpbCkpJswJcsRX1EvGz7kXM2d/ngXdtBTYc15ulNUA7vmzTdokniAdiCfNNMUld4jrJXeJyJ8cTidKCc8nSEPAarUuXxtrsn3jZpn3sAq2LtUfbvM4S8Q6xipmXVZe4N2jBufSUVcyJnsmm7cdpnfs8Q/Jlm8Gi66Q313XgOvEOcQZzWZ9IbnYiKJd24Biyuy0CPzdtO4gJzHHCK1lKmKuRAG5jAvcq4TrPWUxYiuwofR+liEgKzR2KiKRQUIqIpFBQioikUFCKiKRQUIqIpFBQioikUFCKiKRQUIqIpFBQioik+B+Pct7Ua++uzQAAAABJRU5ErkJggg=="/>                                                                                                                                                                                                                                                                                                                     |
| Income Before Tax                            | 1.08e+10 | 9.75e+09 |  1.24e+10 | 1.17e+10 |  1.3e+10  | <img style="display:inline-block;margin:0px;" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAUoAAAAnCAYAAABpNAE1AAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjguMCwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy81sbWrAAAACXBIWXMAAA9hAAAPYQGoP6dpAAAFF0lEQVR4nO3dy24bVRzH8a/jtLk1TWhzayvBIEUV11KarnkBvwDbSkjwBGh2bC0WrOmCp0CzY8GGDaIVCZcNoVjcGmLngkmbpIkzLM6MPDMee058jZPfRxqdcTIdHzfNT+ec+c805/s+IiLS3MigOyAictYpKEVEMigoRUQyKChFRDIoKEVEMigoRUQyKChFRDIoKEVEMigoRUQyjA66AyIiAI7r5YF54AZwM2ij+2G7BIwuVSt88/kD8vG7C2uAg+//2c2+KShFpKeCAFygMfCSYbgI5C1P6y9v/VHN+/5M4ut5YBlQUMr55LheDvMLMwNUg22vVCycDLRjkspxvVFMuKWFXzQEF7Ff5vOBMrAJbATtP4ltEyi/99vjeR++zcXPXQPWO/tkjXJ6KIYMiuN6S8D9xLaYcuge9eD8r8l+q++F+/ulYkH/4DM4rncJM71tNvUN2wUgZ3naE0wAJgMvGYYVTNhZ+frhB++/srvxac6MJGvAh/j+F7Z/3paCUvrCcb0FYIV6IK4At1IOrWFCbZruz3jCc2cFamYIl4qFF13uW885rncZE4Ct1v9uAnPYB2CNetCljf7CbQsTll33yVcPFx88+nIGWO/22mRIQSld57jedUwQRoPx5ZRDT4BfgFVgLWh/Bg6C748BV4JtOrKffD0NTAVts+Nsf/FtHdLeqDb5eq9ULFiPoNI4rjdGY/ClheDcKU57TPPgi359mx4F4CmclIqFjV6+gYJSOuK43ixwj/j0+dWUQ33gV0wYhsH4E/C8D93MARM0hms7ITzRg/49w24ke4iZ7iZD8Nop3uuI1lPfcNvB/MyGQc+DUhdzxJrjeleBd4mH4nKTw59gwjAcKf6IWWscBB8TyM8xIdCJPCY4W4WtzQh3GrgUnHMq2G500K9D4sEX3Y+G4U4H73FhKSglleN6V4C7xEPxNulT2N+JT59/wIyAzqNwnbMbn+8y8UAN95uF8DhmrS85CtwEdrvQH2lCQSk4rjcJvEP8QsvrpJd0/EV9+hyOFDVKac+LYNPf3xmnoLxgHNcbB+4Qv9DyJumFvk+pjxLDEeN2f3oqcnb0NCgd1/uMev1UJdGWgX9V19Y7QTnIW8Snz2+T/nMvA99jps1hKG72paMiZ1yvR5Qf0foq4bHjehUaQzQtVCtAZRjr1/ohKBJ+g3go3sGsgyVtE58+r2HWvUQkRc/KgxzXGwE+xtTPzWFKGK5H2qk2T12ldZgm2+p5G7UGt469RnxN8S5msT9pl/j0eRX4ux/9FOmT4a+jDG5TS7soMEY9NF8iHqLJNjymncfCHWEfqmVgq1QsHLXxPj0RPFDgNvVAvI8p0ZlMObxKvCRnDXNFWuQ8O9d1lIeYiwVPLY/PAbOY8Ay3ZqEa7k9gatXCOxWsOK63i12ohu1eN0atwSh8mfiFlnuY0pCkZzSGYonhKRIWGRrDdNXbx5RR7GDu8LAxjn2oXsOMWsNAnqV5MXXSYbDWahOqFUwtXA1zB0t0TXEFuJpy/n1MGU50+vwEhaJIXwxTULbjALMeZ7smN4J5xJdNqIbtOGYZ4RbpD3loZp/0C10HmFv7ouuK6wz+flqRC6sfQVnuw3t0k+1SQGgSc7FqPmjnIq/nMYE6H/laeF/uBKbYeBV4BHwHPMY8FOK4o08gIl2lh2KIiGTQfy4mIpJBQSkikkFBKSKSQUEpIpJBQSkikkFBKSKSQUEpIpJBQSkikkFBKSKS4X/JuNrJnosWCgAAAABJRU5ErkJggg=="/>                                                                                                                                                                                                                                                                                 |
| Income Before Tax Ratio                      | 0.2894   | 0.2953   |  0.3214   | 0.2717   |  0.2831   | <img style="display:inline-block;margin:0px;" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAUoAAAAnCAYAAABpNAE1AAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjguMCwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy81sbWrAAAACXBIWXMAAA9hAAAPYQGoP6dpAAAEhElEQVR4nO3cz2/bZBzH8beXbu1ga9WBYBJF8yo48EPdD9axTQL+gBzhwBWBxr+QG9eIExw3ib8C5cYhhQscuIDGNFaNdC2lDIY2NlhTyMzhseMfcfokxE6c5POSrCSdl7jS+u7zjZ05nuchIiLdHRj1AYiIFJ1CKSJioVCKiFgolCIiFgqliIiFQikiYqFQiohYKJQiIhYKpYiIxcyoD0Cmg1upLQDLwEl/i94/Ccym/LVrwBpQB9Ya1fKdoRysSIKjjzBKFtxK7RBwgjCAySguWp6iBWwDt4FjwEsp+1zHjyYmnDtZHLuIjUIpPXErtQPAcdJXhcvAc4BjeZq7mBAG2waw6d/fBv6N7LsIvA5cAi4CL6c83w3i4dzu/zsTsVMopc0fj5MBtI3HUY/oDGB0+3uAw1sEzmOieRF4hc4w/0h8VP95gNcTaVMop0jKeJyMYq/jcTSEwf0NzIpxWBYwK84gnK/SGc514ivOzSEen0wQhXKCRMbjbqvCJbIdj4tknnDFeQkTzuRVHbcIw1lvVMu3h3mAMr4UyjGTMh4n7/c7HifH5EHG4yI5SnxUX6EznD8RH9UbQzw+GSMKZcFExuNuq8Jjlqco0nhcJEeAVcKTQytAKbHPBpEVJ9BoVMv6ARGFMi9upeZgrlOdw6zykrdHCIMYjeEkj8dF8iQmnMGK8xSd1xVvEg/nLYVzOk1cKCOBSovTXJev5bWvLXjdTMt4XCRPAOcwK84LwBk6w7lFZFQH1hXO6ZBrKN1KbZnRRKqIH83c87emv+1iVn8aj4vpMCacwcmh08DBxD7bhOGsAzcVzsmUdygfYeI1Sv8QD1Qz8ng3cr/XfdIeNy37NAH9AI23OUw4L2DCeQY4lNhnh/iofkPhHALHWQJeBG7ieVu5vETOodzC/Ga2hSQZm2Ssetmn22P9Q5U8zAFnCd/jPEvnFQe/Eh/VryucGXOc94GrmCnyMXAZz/ss85fJ+z1Kt1I7TjFHYZEszWJWmcFZ9dfoDOcd4EvCUf0HhbN9/e9RzLWwC/423+W2ff/5e788Vb96eaUUb1gLcLNeWep/DxLJRhP42t/ARPI04YrzHPAM8I6/AfzuVmprhKvOa41q+fHwDnkw/onTWbrHrafgYSLZt6X7v1HqXOiVgBcwJ94yo1CK5KMJfONvn2DezzxFuOJcBZ4G3vY3gLtupRasONeA7/MKp1uplTCh+r9xC26TJ7gGsQc82Gf7E3jo3z4AZjz41IlfXdLCfHQ1Uxq9RUbjICacwVn1Vcz7+VF/AF8RjurfYd5zP8xgcZvHXMebFQ8TsLSgRcO2X/geYn659KV+5YN3T9zb+dgxK8kW8KHeoxSZXDOYcAZn1c9jru2M2vX3y3ISbLJ/vLpFLrr/X4zwpOlHX1x59r1vP18A1sfyrDe0l/gi0p8ZzJn0t4A3gTeIrwI9TKTuJ27Tvrbfn+3l/63kr1Ett/J8/on7ZI6ISNY0EouIWCiUIiIWCqWIiIVCKSJioVCKiFgolCIiFgqliIiFQikiYqFQiohY/Ac2P+RpQWkuHgAAAABJRU5ErkJggg=="/>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| Income Tax Expense                           | 1.8e+09  | 1.98e+09 |  2.62e+09 | 2.12e+09 |  2.25e+09 | <img style="display:inline-block;margin:0px;" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAUoAAAAnCAYAAABpNAE1AAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjguMCwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy81sbWrAAAACXBIWXMAAA9hAAAPYQGoP6dpAAAEtklEQVR4nO3dz2/bZBzH8XfasrXrj8EYjGqDmRUmJhisvSIhcSVHLpyQJiH4F3yDo8UfgLYDf8Q0fILDDnBBgks7aNdBPTSVIirY2rJ2P1JzeJzWTh7bTWenbvJ5SY8yNXbzNIo++T7P18lqYRgiIiLpBg56AiIiVaegFBHJoaAUEcmhoBQRyaGgFBHJoaAUEcmhoBQRyaGgFBHJoaAUEckxdNATkP7juP4zwAVgGpiJxiVgzHL4CrAQG7ei26XAqz/pxnxFavoIo5TJcf0R4CLJULwIHLUcvgX8CjwCzgEvZPzqx8Bv7AZnfKwGXl0vbCmMglIK47j+BKYyjIfiBWDQcvg6cBOYjcYccBtoxI6ZwATmVOy2+e/hjKncw16FLgZefWs/f5v0NwWl7Ivj+idJBuI08HrK4f+QDMRZ4A6w3xdfDZjEhOZrJIP0dHS/TRg9bmsVegu4G3j17X3OR3qcglIyOa5fw4RPayi+nHLKMrth2Lz9s/yZ7hgGXiVZgTbHRMZ5D4BF2qvQhcCrr5U5Yak+BaXscFx/ABMwraGYtlf4O8lQnMNUj1X1PMnle7MaPUt2Y3MFexW6FHj1x2VOWKpBQdmnHNcfAt5gNxSno2GruhqYaiu+fL4JbHRlsuUbAl6hvQo9B7yYcd4TTEOprQoF/lZDqUtqtTOYbZ9FwvBuKQ+hoOx9jusfBd4iWSW+g70h8hCYJxmK85iOdD8ax95QmiK/oWSrQhcDr75Z4nwPtWirZxg4tpfxxbdX3vv4528+HDD70tvAp4Th10XPS0HZYxzXH8OEYDMQZ4A3sS8t/6O987yIqZQkW7yh1FqFniG7ofQH9iq00g0lx/UH2WOARWO0w+ObI+25S3hpbZUfrlxmMJlhDcApurJUUB5ijuufILl0ngHOY3+h/cvufmIzFJfYf+dZ0g0DDvYq9HjGeZskG0o7QRp49ftpJ0VV2BHKCa34uUc6eA6K8BDznFjHB/Pfj351zXvXct77hOGNIieiT+YcEo7rT5JcOs9gmhA2K7RfjrPchWmKsYXZrpi33HeC9iq02VAaAd6ORoLj+n9h3tgGsYdetz+OvIm5UiA1yCyj0+Mzq+tLywuTIfxYS/7tDcz1uIVSRVkxUXXgkAzEGeBUyil3SIbiHLBa+kSlaIPYG0pTZDeUWjXYDaSnDbKtlOMqs1994+onH529t/JlzTx/DeAz7VH2mGjP5zzJUJwGnrUcvo15p2ztPOsav943hgnQ05iPd2aFXd/tL3/+3dVTl3+6fhy4ra53RUXXHo5iLqvpZJzEdKKPWX7tI8z+VHzp/AsVeicXqZDtwKuvlPkAfbtHGX2DzTidhZvt+HH22KVL8QBTGTaXzbOYDXxdyCxSEYcqKGPXWHUSZGljpODpNTDL4I3YWI/GhuV2jejrwsjZtBaRg9WVoHyK5altFD3nLexBlhZyacGnZbFIjyotKKPqLwCew1R6Resk3NLu26APN79FpDOlBWXg1cPo+wnjIdkA7mOWnfGxbvlZ8+e2+zbQhdIi0iXqeouI5NB/LiYikkNBKSKSQ0EpIpJDQSkikkNBKSKSQ0EpIpJDQSkikkNBKSKSQ0EpIpLjf8MT1lcahgWSAAAAAElFTkSuQmCC"/>                                                                                                                                                                                                                                                                                                                                                                                                                     |
| Net Income                                   | 8.92e+09 | 7.75e+09 |  9.77e+09 | 9.54e+09 |  1.07e+10 | <img style="display:inline-block;margin:0px;" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAUoAAAAnCAYAAABpNAE1AAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjguMCwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy81sbWrAAAACXBIWXMAAA9hAAAPYQGoP6dpAAAEmElEQVR4nO3dzW4bVRjG8b/jpEkKbSo1jV1ExQEVFUiEuAJuwDfAthIStzA7thZXQBdcBZodCzZskEBCAhSHfhil1E6TQEIpTsjHsDgz9djOzJlpPP5onp80Op7IMz6W4sdn3nMyKQVBgIiIJJsZdwdERCadglJExEFBKSLioKAUEXFQUIqIOCgoRUQcFJQiIg4KShERBwWliIjD7Lg7ICICYDz/ElAFbjq2FaBc/XuH7768S7n3rwtPAEMQPB5m3xSUIlIo4/mv4Q6/m8D1HKcNbu9u7peD4Frfz8vAbUBBKSLjZTy/BCxxduC90bd/Jcepj4BtYAt4mtLufPzox5UAvi/1lhBPgPsv/87OVtJNMUQkYjx/Blgm2whwIcepO7jDbwvYAzKH0rf3Pv3krb32FyU7kjwBPiMIvsrRr0wUlCIXgPH8OaCCO/wq5LvS3Cc9/KLH/wzjfZzl82/uVe7+8PUScH/YtcmIglLGxnj+LLaetAqshe0qcA17CTbs7big8744f7NeG+kHynj+ItlGf8tAKeNpA2CX5NCL2m3gYEhv5TxOm/Vau8gXUI1SChdezhm6YRi17wOXxtez4TOef0JxIQ92xjcegEs5undMN/BS63+x1xMUlDJEYYH/TQYD8QPgcsJhHWADWAca4baD/d2cC9v4Y1f7ssflbWexdbF+5XDLU787rwOy1f/+Ikf9T7oUlJJbGIgVesMwaq8mHHaInY1s9G2bTO+Ht0SxgVzu25/Bfon0h+Czot/oRaeglFTG869zdiAmrXk7Bh5iR4gbsbaJnZV8lQR0L407Y+6LFKjQoDSe/xP2W7AVbk9ij1/sN+s1/ZKNmfH8q3QnU+KhWE04JAAe0RuG69iQPCq6vyKjVNisd1jA/4+z6zj99kkO03ig6hLjnIznX8ZOoqzRG4i3Ug57TG8NsYG9jJ6EGU+RqZ/1/ghbyK9iZ+uithLbX8DO3C0B76WdzHj+cxxhGm57o16mMWmM588Ddxi8ZH6H5GUibQZriBvA86L7KzLJCl9HaTy/Svpdiq4wGJ7x/Uq4vZ7jZQ+wH3pXoO4267XTHOedOOFC4vhaxCgQ3yV5NP8n3RFi1G5gR/Yi02bqR5RZPAu3B47nLeIO0xXsYuUF7Lo94zjnkfH8Nulh2gKeNuu1sU5EGM8vA28zWEO8Q/JaxH0Gl940sIuJRSSjSQjKrDrYmdOm43nzDIZof5hWsLO2c9jaXFp9DuDUeP4W6WHaAtrNeu1cExnh0ptb9IbhGrauuJhw2L/0BmE0uVLot6zIRTFNQZnVIXZt3qbjeXPADWxwxoO0P1iXsaWD6C8h0gTG83dwB2or7GeVwRriKsl3WzkEfqM3DNeBP5jetYgiE+9VDMqsjrAh9sTxvOhuKmlhGoXtLDZ8bwAfOs7bIXmEeIwtRcTDsAH8Dkx1TVVkGo0iKLdH8BpFa2V4Tgl7OR/djy/tTs3z2JA8xS6z+SXcfgZ+xY4atRZRZELo7kEiIg7652IiIg4KShERBwWliIiDglJExEFBKSLioKAUEXFQUIqIOCgoRUQcFJQiIg7/A+KWxIiAPhraAAAAAElFTkSuQmCC"/>                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| Net Income Ratio                             | 0.2394   | 0.2347   |  0.2528   | 0.2219   |  0.2342   | <img style="display:inline-block;margin:0px;" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAUoAAAAnCAYAAABpNAE1AAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjguMCwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy81sbWrAAAACXBIWXMAAA9hAAAPYQGoP6dpAAAFP0lEQVR4nO3czW8UdRzH8fcutKXlWRS6PoQJWhLjQ4UUBIz6B/SgR+LNYDTx5HFuXhuvnjD6V5jxooei3tSLERWBdqH0mVIFhWrTrofvDLOzM91fu+xsd9vPK/ml0+5s97fQ+eT7e5gtVCoVRERkbcXN7oCISLtTUIqIOCgoRUQcFJQiIg4KShERBwWliIiDglJExEFBKSLioKAUEXHYudkdkO3D84O9wEBNOx5+PZTxlMvAJWAUuFQeGZ5rTU9Fkgq6hVGayfODXuA50kE4APQ7nj4DjAMHgOczHv+NMDSx4JxpSqdFHBSUsmGeH3QDx0iGYNSecTx9ARgL23jYouMHVecdBF4FzgFngBcyftcVksE51dAbEnFQUEomzw92Ah7Zw+Sj1J/f/otkEF6vOr7XYJcOAKex4DyLBWeh5pw/SA7VJxt8LZEEBeU25vlBEasAs4bJx6g/h/0PcTVYWx0u5tfrh/ZjFecZLDxfJB2c10hWnBMt6JdsQQrKLc7zgwJQIh2EA9hcYk+dpy8BZeIgrP46n1unG7MPqzjPhu0l0lXvGHFwjpZHhm+2soPSuRSUW0AYho+TDsLjWBjurvP0ZeAG2XOG00Cn/oHsBU5hoXkOeJl0cI6THKqXW9g/6SC5BqXnBz8A3cB9bKL+foPHWY8tl0eGO/UibojnBwfJnjMcwIaia1kBJkgGYRSGk+HjW90eLDijxaFBYEfNOTeoqjiB8nb7G5NseQflA2BXTr9+hUcP4PUE81J5ZHg1p/eQ4vnBHrKDcACrGtdSAaZIzxmOAzexylFiu4Eh4sWhQdJzshMkg3NMwdmGCoWnsevjKpXKrVxeIuegfA14CugDeqvarjrfu443426iJZofxitkryq79hrOkh4mj2FhuPTob3Xb6sOCM5rjPEE6OG8Rh+YocF3BuckKhQvAZ1gurALvU6l80fSXyXuO0vODfpobbt1sLFgbDebuJvZ5o6K9hrVzhuNYyEr+ekkHZ1fNOVMkK86rCs7mCuff9wFPYouSpej42YWJY19//uFbxeQ0uhUgTa4sO/EWxv/Cdjfn1ykSB+d6A3g9P4uOu7ALLWuLTd7vTdweAN+FDez/bYh4O9IJ7IJ9J2wAM54fjBIH5xUFZ7YwAA+SDMASGYGIXS8ph/9epJhea9yBLWBu+6BslVXiYbLIEvB92MCC8yRxxXkSmzY5HzaAWc8Pqofqv2/14Az35h7CHYD91N+aVusuMIdNPc0Cs/d6+pZW4aNicv/sCrZ/tqk6cegt0o56sCozWlUfIh0Ec4Sb37Hg/LVTgtPzgx3AE6xd9VUH4EYKsEXs32UOu9c/EYZV32fOv49efO/80T9nPilYJbkCfKA5SpHO0QO8QlxxDpHeAXKbqn2cwOVW7rCAh7eqHqF+AJbCc2q3U9WzQBx0UQDWhuE88O+jvoePv7l45N2fvtwPXOvIVW9QUIqEurEtSNF2pFOkg3MB+JZ4qP5Lo8EZfnBJP/Xn/krAYdK3fq6lgoXbWlVfdDxPa7ejreb9SVIKSpHN0YUFZ3Tn0CnSixZ3sOCMqs6fscB1LX6UqL/nttYKccWXFXxRFXib9rw5Ifeg1GKOyOZYBn4M26fYtThIvKp+GngMeDtsYMPUjSyALGPVXb25v1kskFs65O80rQjKdvvwBJF2NQl8FR7vxFbS3wTeAF7HbsMEC8xpbHvZNBaE1cfRY3fo3Hv124o+FENExEFzhyIiDgpKEREHBaWIiIOCUkTEQUEpIuKgoBQRcVBQiog4KChFRBwUlCIiDv8DEJX+GPgv3MEAAAAASUVORK5CYII="/>                                                                                                                                                                                                                             |
| EPS                                          | 2.09     | 1.8      |  2.26     | 2.2      |  2.48     | <img style="display:inline-block;margin:0px;" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAUoAAAAnCAYAAABpNAE1AAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjguMCwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy81sbWrAAAACXBIWXMAAA9hAAAPYQGoP6dpAAAEzklEQVR4nO3cy3LbVADG8b+bxE2bkoRbcytUQEpLYcOCLS/gF2BbhhleQTu2HlYs6YINr9DRjgUbNszAguEyKSU1bd04SZNJ007ipknE4hxjRZZ05MTX5PvNnLEsS8eyY305RzpSIQxDREQk3bl+b4CIyKBTUIqIOCgoRUQcFJQiIg4KShERBwWliIiDglJExEFBKSLioKAUEXEY7fcGiIg0eH4wCcwnlIXY8+Ls9hN++vYWI0evLjwAPMLwUSe3S0EpIl3n+cEFYI7s8JsHLuWtc3Hz4fZIGE7GZo8Ai4CCUkQGg+cHY8AM2eG3ALzaRrXbwCpQs4+rwEpkugasf7r86+sh/Fw4egjxALh3ks+UpKCbYohInOcH54A3SA+/xvzLQCFntXWa4VeLTa9Gyk7e7fzx9hefXd2qfV0wLckD4EvC8Lu86+eloBQ5Qzw/KABTZLf+5jHd5Lw9zn1gjeTwiz5ud+pzRH31w+2ZW7/cmQLudfrYZIOCUvrK84Nx4AbwoS03MTvyHvAy9uia1+l1XlbKpaHZQTw/mCDfiZALOasMgSckd32jrcJNu2y/HFbKpVo330DHKKUnPD84D1ynGYiN8h4DPEzN84N9uhfS7dYD2ccDp9r4aFskd32jgbiOaS2eeQpK6SjPD4rA+7QG4iLmOFKSp8BdYMmWDcxvc8yWUaAYee4qxdj60XnR1+LzignbNmrLxba/jP7YIbv1t4rpJtf7tYHDSEEpx2LPdi4CH3E0EK+R/rt6hgnCaCjexey8g6IRjNHwzBvQWaHtCugxx/uOYv7RrHM0BOOtwued/0qkq0Hp+cHnwC5QxYxrelwpl/SfbIh4fjCK6R7HW4jXMTtwkueYAIwH4kq3t7cD9m3R71T+1+0W5TfAK9EZnh9sYIIzXh5FpjeH6SD6aeD5wQjwLq2BeIPkLimYbl4jDKOh+Ljb2yvSS107621bIt8DHmaowSwwnnP1OmZnSwvSKrBSKZf2OrvVp58dH/cOyYGY9vfZBf6mNRCr9Pdspwj04Kx314cHeX4wS/Os5jTN0JyNTTeev9ZG9Wtkh2kVeHoWW6c2EK/SGogfkD48pI65qiHaXV4CHqJAlMF16oYHbdnyV8Yy5zFDINKCdNa+XsRcFXAZ+Dijvh3PD7KCtArUKuXSUA6DsAOI3yY5ECdSVnuBCcT4McQHwGGXN1lk6AziWe8XmB32QcYyBUzLMy1IG2UaM6zjmi1pDj0/WCU9SKtAtVIuPTvuhzopG4hXaA3Em6TfSGAP+IfWQPwXc7mXiOQwiEGZR4gZa7cB/J6x3Djurv4MZtjFnC2fpFXm+cE2GUFqy1qlXDp2CNlAnCc5EON3SmnYxwRi/BhiBQWiyIkNa1DmVQfu25KmcfF/UpjORR4vYYJqEtOtTbPv+cEK6UHaaLXWbd1JgTidUvcBsExrIN5HV1CIdM1pD8o8DjEnhdaA3zKWmyD7uOkc8CbmO33Lliy7pJ9UOcC0BuMnVZZpXsomIj3Si6Bc78F79Mqy4/URTGgu0LwONzp9xU5fxITkIabL/AfwZ+RxCXN8UUQGgO4eJCLiMLB3bRERGRQKShERBwWliIiDglJExEFBKSLioKAUEXFQUIqIOCgoRUQcFJQiIg7/AQH8y8QWNXiCAAAAAElFTkSuQmCC"/>                                                                                                                                                                                                                                                                                                                                                                                     |
| EPS Diluted                                  | 2.07     | 1.79     |  2.25     | 2.19     |  2.47     | <img style="display:inline-block;margin:0px;" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAUoAAAAnCAYAAABpNAE1AAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjguMCwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy81sbWrAAAACXBIWXMAAA9hAAAPYQGoP6dpAAAEwUlEQVR4nO3c327bVADH8W/WNFspapk21q4BZqBs3UA8Ai+QF+B2EhKv4DtuI56AXfAUyHdcICRuJsEN2kTHWgKlbfpno3Tr+mdJzcWxqeM4Ps4aJ273+0hHjlPbsaP6l3OOj13yfR8REentwqh3QESk6BSUIiIWCkoREQsFpYiIhYJSRMRCQSkiYqGgFBGxUFCKiFgoKEVELMqj3gERkZDjelPAXEKpxuYrs7vb/PTNXcY67y5sAw6+//cg90tBKSK5c1xvArhOevjNAW9m3eb805XdMd+fir09BswDCkoRKQbH9caBGdLDrwpc7mOzu8AG0AymG8B65HUT2Pps+ZcrPtwvdXYhtoHHpzmmJCU9FENE4hzXuwBcpXf4he9fA0oZN3vASfg1Y683IuVF1v384d4Xn9/YaX5dMjXJNvAlvv9t1vWzUlCKvEYc1ysB06TX/uYwzeSsLc4WsEly+EWnu4M6jqivvr83c/fn76aBx4PumwwpKGWkHNe7BCwAHwflDuZEPgJexqa29wa9zstGvXZmThDH9SbJdiFkIuMmfWCb5KZvtFb4NFh2VI4b9Vozzw9QH6UMheN6F4FbnARiWD6kwMPUHNdrkV9I97sdSO8PnO7j0HZIbvpGA3ELU1t87SkoZaAc16sAN+kOxHlMP1KSf4FHwGJQnmD+N8eDUgYqkXlbqcTWj74X/Vv8vUrCvpWD8kbfX8ZovCC99reBaSYfjGoHzyIFpbyS4GrnPPAJnYH4Eb3/r55hgjAaio8wJ29RhMEYDc+sAZ0W2raAHrd8bhnzQ7NFZwjGa4XPB/+VSK5B6bjeAqb/YrtRrx3n+VmSD8f1ypjmcbyGeAtzAid5jgnAeCCu572/A9AKimpc8r+8a5Q/Am8DLcf11oG1SFmNza8BO2ep8/w8cVxvDPiA7kBcILlJCqaZF4ZhNBTX8t5fkWHKLSiDmkgLczWsDLwblDT7juvFw7MrVBv12l5e+33eBePj3ic5EC/1WG0f+J3uQFxltFc7RYYi9+FBjutVMVfqZmPTGcxYrfB1vyP3k2qk0VBtNuq1w8EcxdkTBOINugPxNr2Hhxxg7mqINpcXgRUUiFJc52J4UJuTTuc0FzGj/GfpDNNowF7HXH2cCsrttA06rrdNelN/Fdhs1GvtVziuQggGEL9HciBO9ljtEBOI8T7EvwD1JYvEFOmq9yGm5rJiWW6SzvCMh2o4X8HcgnUV+DRle8eO6zWx958+GWX/aRCI79AdiHfo/SCBI2CJ7kD8E/MDJiIZFCkos9rDnPxLluUuk9zEjwbrNcyQi3DAbpqjLP2nwLPTBGoQiHMkB2L8SSmhFub7iPchNlAgipzaWQzKrP4Jym8py4Q3/seb+PFa6hVMDdUJSpo9x/Vs/afrmP7AWZID8a0e224Dy3QH4h/oDgqR3JznoMziGHOXwibwa8pyFcwwp17N/HA6hekauBmUNPv0vqjSxtQG4xdVljm5lU1EhmQYQbk1hM8YBlvfKZgLTWEzPvqQ0mpkvooZhjOBCeol4AHwMDJdxPQvikgB6OlBIiIWhX1qi4hIUSgoRUQsFJQiIhYKShERCwWliIiFglJExEJBKSJioaAUEbFQUIqIWPwHlITH6V5WozkAAAAASUVORK5CYII="/>                                                                                                                                                                                                                                                                                                                                                                                                     |
| Weighted Average Shares                      | 4.31e+09 | 4.32e+09 |  4.34e+09 | 4.35e+09 |  4.34e+09 | <img style="display:inline-block;margin:0px;" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAUoAAAAnCAYAAABpNAE1AAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjguMCwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy81sbWrAAAACXBIWXMAAA9hAAAPYQGoP6dpAAAD2ElEQVR4nO3dT2/bZADH8W/WreugSwsMjWqF+cB4Fxw59Q1wnYTgwBvwjRuqeAXssFeBfEDiwIULEuIFMGndWqTBOtE02/pnpOZghzgmzuNkbpqW70d6ZCe27CdS9NPzJ37SStMUSVK1S2ddAUmadwalJAUYlJIUYFBKUoBBKUkBBqUkBRiUkhRgUEpSgEEpSQGXz7oC0kUSxckl4DqwWigrpdejSvGcaRowPWBvTOkEjj/f2tzwMb0KLR9hlAaiOFkA2kwXcP3XrQaq8oos3DpAF9gHXgJv5vVr5/dq00yD54RwmO6NOae7tblx0kA95pJBqQslipPLDIJuXKkKwXZDVTkmC5X9Edti6VTsH05wr2sMQrNYVkbs97fX8/0V4MrUn3IgZRCidQK3fN7+1uZGb6o7t1rrwB3gN9J0Z6prhG5hUGqe5C26t5gs3IpluaGqHDI+2KoCrl+OGqrHLCwxPlxHhW9xu9hQPfaZcMjg+/tffvLR7qOvW9lwxQnwOWl6v6H6/Mug1EzlLb5bQFQotwv779NMV/KA+qFWPt4laxGqnqtM1potn7c0zU3f29/lp2/vsjCcYT0garpl6WSOGhXFyRVgndEhGOXHFmpc6gWTdVWL+12yMT7NxhHwNC/TWGQwFFCnNdsGVu48e3xjIU3fLl1rAfgQMCh1dqI4WSRr9ZUDMMrfWyc8a/uK7IvcL9uF8jvwJ/B3w1XX/DoGnuWlto8f/rqWws+t4e9bD3jQZOXArrdKoji5ShaEEaNbhbcIz+oeUx2C28AfZIP/0mv58d5nn97ee/JNK2tJ9oAvHKPUa4viZAn4gNHd4ghYIxyERwyH4E5p+xSDUDPy1Q/3bt795bsV4IGz3qolipNrZAFY1TVeq3GZQ4aDrxyGuxiEmh8nW5sbT07zBo5RnjNRnLzBf0Ow+PpmjcscMNwlLneTJxorki46g3LORHGyzCD4RrUK361xmRdUh+A28FeTdZYuOoNyBqI4aZE9erbK4IfT7zAYK4wYhOKNGpd8DjymesLEIJQaZFDWkAfdMuGnQlapfnqkzm8H+zqMnzXuTP4pJE3rfxGUFSu69EvdhQ+aWJKux/CPo/vBV5wo2cmPSZoT5yIoSyu6rFIv3IrnNbmiS9UTIcVVXqqOHzRQB0kzNpOgnPMVXeoseNDNt5Os6CLpgji1oMzH9R6STVqc1You3dJ752lFF0lz4tSCcmtzI43ipM1wSL4kvIxS1Zp2HVzRRdIZ8MkcSQrwz8UkKcCglKQAg1KSAgxKSQowKCUpwKCUpACDUpICDEpJCjAoJSngH/t8oKa5OWRhAAAAAElFTkSuQmCC"/>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| Weighted Average Shares Diluted              | 4.31e+09 | 4.32e+09 |  4.34e+09 | 4.35e+09 |  4.34e+09 | <img style="display:inline-block;margin:0px;" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAUoAAAAnCAYAAABpNAE1AAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjguMCwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy81sbWrAAAACXBIWXMAAA9hAAAPYQGoP6dpAAAD2ElEQVR4nO3dT2/bZADH8W/WreugSwsMjWqF+cB4Fxw59Q1wnYTgwBvwjRuqeAXssFeBfEDiwIULEuIFMGndWqTBOtE02/pnpOZghzgmzuNkbpqW70d6ZCe27CdS9NPzJ37SStMUSVK1S2ddAUmadwalJAUYlJIUYFBKUoBBKUkBBqUkBRiUkhRgUEpSgEEpSQGXz7oC0kUSxckl4DqwWigrpdejSvGcaRowPWBvTOkEjj/f2tzwMb0KLR9hlAaiOFkA2kwXcP3XrQaq8oos3DpAF9gHXgJv5vVr5/dq00yD54RwmO6NOae7tblx0kA95pJBqQslipPLDIJuXKkKwXZDVTkmC5X9Edti6VTsH05wr2sMQrNYVkbs97fX8/0V4MrUn3IgZRCidQK3fN7+1uZGb6o7t1rrwB3gN9J0Z6prhG5hUGqe5C26t5gs3IpluaGqHDI+2KoCrl+OGqrHLCwxPlxHhW9xu9hQPfaZcMjg+/tffvLR7qOvW9lwxQnwOWl6v6H6/Mug1EzlLb5bQFQotwv779NMV/KA+qFWPt4laxGqnqtM1potn7c0zU3f29/lp2/vsjCcYT0garpl6WSOGhXFyRVgndEhGOXHFmpc6gWTdVWL+12yMT7NxhHwNC/TWGQwFFCnNdsGVu48e3xjIU3fLl1rAfgQMCh1dqI4WSRr9ZUDMMrfWyc8a/uK7IvcL9uF8jvwJ/B3w1XX/DoGnuWlto8f/rqWws+t4e9bD3jQZOXArrdKoji5ShaEEaNbhbcIz+oeUx2C28AfZIP/0mv58d5nn97ee/JNK2tJ9oAvHKPUa4viZAn4gNHd4ghYIxyERwyH4E5p+xSDUDPy1Q/3bt795bsV4IGz3qolipNrZAFY1TVeq3GZQ4aDrxyGuxiEmh8nW5sbT07zBo5RnjNRnLzBf0Ow+PpmjcscMNwlLneTJxorki46g3LORHGyzCD4RrUK361xmRdUh+A28FeTdZYuOoNyBqI4aZE9erbK4IfT7zAYK4wYhOKNGpd8DjymesLEIJQaZFDWkAfdMuGnQlapfnqkzm8H+zqMnzXuTP4pJE3rfxGUFSu69EvdhQ+aWJKux/CPo/vBV5wo2cmPSZoT5yIoSyu6rFIv3IrnNbmiS9UTIcVVXqqOHzRQB0kzNpOgnPMVXeoseNDNt5Os6CLpgji1oMzH9R6STVqc1You3dJ752lFF0lz4tSCcmtzI43ipM1wSL4kvIxS1Zp2HVzRRdIZ8MkcSQrwz8UkKcCglKQAg1KSAgxKSQowKCUpwKCUpACDUpICDEpJCjAoJSngH/t8oKa5OWRhAAAAAElFTkSuQmCC"/>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |

+++ {"editable": true, "slideshow": {"slide_type": ""}}

We can't compare the magnitudes of the trends here because the scales of the numerical data sets are incompatible. Though we can talk about the general shapes of these sparklines.

We quickly observe from the sparklines that the revenue and net income has been on a rising trend since 2020 when Covid-19 initially struck. On the other hand, Other Expenses, D&A and EBITDA have been decreasing. 

Notice that Other Expense is negative.

:::{admonition} How may an expense on the income statement be negative?
:class: note
Expenses on an income statement are typically represented as positive numbers, reflecting the costs incurred by a company to generate revenue and operate its business. However, in certain situations, an expense may appear as negative. Here are some scenarios where this might occur:

1. **Reversal of Accruals or Provisions**: If a company reverses a previously recorded accrual or provision for an expense, the reversal could result in a negative expense. This might happen if the company overestimated an expense in a prior period and needs to adjust it downward in the current period.

2. **Recovery of Expenses**: In some cases, a company might recover expenses that were previously incurred. For example, if a company receives a refund or reimbursement for certain expenses, it could result in a negative expense on the income statement.

3. **Offsetting Revenues**: Occasionally, certain expenses might be offset against revenues or gains. For instance, if a company earns income from one source but incurs expenses related to another activity, the net effect could be a negative expense.

4. **Contra Accounts**: Certain accounting practices involve the use of contra accounts, which are accounts that offset the balance of another related account. For example, contra-accounts like "Sales Returns and Allowances" or "Purchase Returns and Allowances" could result in negative amounts being reported as expenses.

5. **Adjustments for Errors**: If errors are discovered in the accounting records, adjustments may be made to correct them. These adjustments could result in negative expenses to rectify the overstatement of expenses in previous periods.

6. **Credit Balance in Expense Accounts**: Uncommonly, if a company overestimates its expenses and the actual expenses turn out to be less than anticipated, the expense account may show a credit balance, effectively representing a negative expense.

While these scenarios are less common and may vary depending on specific accounting practices and circumstances, they can lead to negative expenses appearing on an income statement. It's essential to carefully review the reasons behind any negative expenses to ensure accurate financial reporting and analysis.

:::

+++ {"editable": true, "slideshow": {"slide_type": ""}}

### Changes

The simplest trend is indicated by just 2 quantities $x_t, x_{t+1}$, in which we may observe the (absolute) change $x_{t+1} - x_t$ or the relative change $\frac{x_{t+1} - x_t}{x_t}$ commonly expressed in percentage format.

Let's take a look at the changes and relative changes on the FY22-23 income statement for The Coca-Cola Company:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
tags: [hide-input]
---
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

# This doesn't work for markdown tables because myst parses CommonMark
# Ref: https://github.com/executablebooks/jupyter-book/issues/1105
#display(Markdown(df.to_markdown()))

# Copy and paste the output of the following into a markdown cell,
# then comment it out.
#print(df.to_markdown())
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

| Field                                        |     2022 |      2023 |    Change | Relative Change   |
|:---------------------------------------------|---------:|----------:|----------:|:------------------|
| Revenue                                      | 4.3e+10  |  4.58e+10 |  2.8e+09  | 6.51%             |
| Cost of Goods Sold                           | 1.8e+10  |  1.85e+10 |  5e+08    | 2.78%             |
| Gross Profit                                 | 2.5e+10  |  2.72e+10 |  2.2e+09  | 8.80%             |
| Gross Profit Ratio                           | 0.5814   |  0.5952   |  0.0138   | 2.37%             |
| Research and Development Expenses            | 0        |  0        |  0        | #DIV/0!           |
| General and Administrative Expenses          | 3.56e+08 |  3.21e+08 | -3.5e+07  | -9.83%            |
| Selling and Marketing Expenses               | 7.09e+09 |  7.61e+09 |  5.2e+08  | 7.33%             |
| Selling, General and Administrative Expenses | 1.29e+10 |  1.4e+10  |  1.1e+09  | 8.53%             |
| Other Expenses                               | 1.22e+09 | -2e+09    | -3.22e+09 | -263.93%          |
| Operating Expenses                           | 1.41e+10 |  1.59e+10 |  1.8e+09  | 12.77%            |
| Cost and Expenses                            | 3.21e+10 |  3.44e+10 |  2.3e+09  | 7.17%             |
| Interest Income                              | 4.49e+08 |  9.07e+08 |  4.58e+08 | 102.00%           |
| Interest Expense                             | 8.82e+08 |  1.53e+09 |  6.48e+08 | 73.47%            |
| Depreciation and Amortization                | 2.92e+09 |  1.13e+09 | -1.79e+09 | -61.30%           |
| EBITDA                                       | 1.38e+10 |  1.24e+10 | -1.4e+09  | -10.14%           |
| EBITDA Ratio                                 | 0.3216   |  0.2719   | -0.0497   | -15.45%           |
| Operating Income                             | 1.09e+10 |  1.13e+10 |  4e+08    | 3.67%             |
| Operating Income Ratio                       | 0.2537   |  0.2472   | -0.0065   | -2.56%            |
| Total Other Income                           | 7.77e+08 |  1.64e+09 |  8.63e+08 | 111.07%           |
| Income Before Tax                            | 1.17e+10 |  1.3e+10  |  1.3e+09  | 11.11%            |
| Income Before Tax Ratio                      | 0.2717   |  0.2831   |  0.0114   | 4.20%             |
| Income Tax Expense                           | 2.12e+09 |  2.25e+09 |  1.3e+08  | 6.13%             |
| Net Income                                   | 9.54e+09 |  1.07e+10 |  1.16e+09 | 12.16%            |
| Net Income Ratio                             | 0.2219   |  0.2342   |  0.0123   | 5.54%             |
| EPS                                          | 2.2      |  2.48     |  0.28     | 12.73%            |
| EPS Diluted                                  | 2.19     |  2.47     |  0.28     | 12.79%            |
| Weighted Average Shares                      | 4.35e+09 |  4.34e+09 | -1e+07    | -0.23%            |
| Weighted Average Shares Diluted              | 4.35e+09 |  4.34e+09 | -1e+07    | -0.23%            |

+++ {"editable": true, "slideshow": {"slide_type": ""}}

We note that over the years 2022-2023, the net income of KO increased by 12.16% even though the revenue growth is only 6.51%.

There are some fields with significantly large magnitudes of relative change, namely, Other Expenses, Interest Income, Interest Expense, D&A and Total Other Income. To find out more, one may try to ascertain their magnitudes relative to the top or bottom line (refer to the {ref}`Common-Size Analysis <common_size_analysis>` section on how to do this), and/or to find out from the notes if any mention is made of these quantities.

You may find the sheet [here](https://docs.google.com/spreadsheets/d/1OCqC9k0lz1kRZ-RXYRMgI8ebjHum1xHEfAYKl4t3Nos/edit#gid=884666173).

+++ {"editable": true, "slideshow": {"slide_type": ""}}

## Base-Year Analysis

Let's assume that a data table is arranged into columns chronologically, each column being indexed by a year. In base-year analysis, the leftmost column is taken as the reference. Hence, the values in that column are taken to be 100%, while values in other columns are taken as a percentage relative to the corresponding values in the first column.

The base-year analysis for the income statement of The Coca-Cola Company FY2019-2023 is shown as follows:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
tags: [hide-input]
---
import openpyxl
import pandas as pd
import requests
from io import BytesIO
from IPython.display import display, Markdown
import numpy as np

spreadsheetId = "1OCqC9k0lz1kRZ-RXYRMgI8ebjHum1xHEfAYKl4t3Nos"
url = "https://docs.google.com/spreadsheets/export?exportFormat=xlsx&id=" + spreadsheetId
res = requests.get(url)
data = BytesIO(res.content)
xlsx = openpyxl.load_workbook(filename=data)
for name in xlsx.sheetnames:
    if name == 'BS Base-Year':
        df = pd.read_excel(data, sheet_name=name)
        break
        
df = df.set_index(df.columns[0])
df.index.names = ['Field']
for y in range(2019, 2024):
    df.drop(y, axis='columns', inplace=True)
df = df.rename(columns={str(k) + '.1':k for k in range(2019, 2024)})
def f(x):
    try: 
        return "{:.2%}".format(x)
    except:
        return x
df = df.apply(np.vectorize(f))

# This doesn't work for markdown tables because myst parses CommonMark
# Ref: https://github.com/executablebooks/jupyter-book/issues/1105
#display(Markdown(df.to_markdown()))

# Copy and paste the output of the following into a markdown cell,
# then comment it out.
#print(df.to_markdown())
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

| Field                                    | 2019    | 2020    | 2021    | 2022    | 2023    |
|:-----------------------------------------|:--------|:--------|:--------|:--------|:--------|
| Cash and Cash Equivalents                | 7.50%   | 7.79%   | 10.25%  | 10.26%  | 9.59%   |
| Short Term Investments                   | 5.44%   | 4.72%   | 3.11%   | 2.27%   | 4.40%   |
| Cash and Short Term Investments          | 12.96%  | 12.49%  | 13.35%  | 12.50%  | 14.02%  |
| Accounts Receivable                      | 4.59%   | 3.60%   | 3.72%   | 3.76%   | 3.49%   |
| Inventory                                | 3.91%   | 3.75%   | 3.61%   | 4.56%   | 4.52%   |
| Other Current Assets                     | 2.19%   | 2.20%   | 3.17%   | 3.49%   | 5.36%   |
| Total Current Assets                     | 23.61%  | 21.99%  | 23.83%  | 24.35%  | 27.33%  |
| Property, Plant and Equipment            | 12.50%  | 12.37%  | 10.51%  | 10.60%  | 9.46%   |
| Goodwill                                 | 19.44%  | 20.05%  | 20.55%  | 20.26%  | 18.83%  |
| Intangible Assets                        | 11.57%  | 12.60%  | 16.21%  | 15.95%  | 15.25%  |
| Long Term Investments                    | 23.03%  | 23.02%  | 19.49%  | 20.26%  | 20.27%  |
| Tax Assets                               | 2.79%   | 2.82%   | 2.26%   | 1.89%   | 1.60%   |
| Other Fixed Assets                       | 7.04%   | 7.08%   | 7.13%   | 6.67%   | 7.33%   |
| Fixed Assets                             | 76.39%  | 78.01%  | 76.06%  | 75.65%  | 72.67%  |
| Other Assets                             | 0.00%   | 0.00%   | 0.00%   | 0.00%   | 0.00%   |
| Total Assets                             | 100.00% | 100.00% | 100.00% | 100.00% | 100.00% |
| Accounts Payable                         | 13.08%  | 12.71%  | 15.47%  | 16.92%  | 15.86%  |
| Short Term Debt                          | 17.59%  | 3.06%   | 4.93%   | 2.98%   | 6.67%   |
| Tax Payables                             | 0.48%   | 0.90%   | 0.73%   | 1.29%   | 1.61%   |
| Deferred Revenue                         | 0.00%   | 0.00%   | 0.00%   | 0.00%   | -14.33% |
| Other Current Liabilities                | 0.48%   | 0.90%   | 0.73%   | 1.29%   | 15.86%  |
| Total Current Liabilities                | 31.25%  | 16.72%  | 21.19%  | 21.23%  | 24.16%  |
| Long Term Debt                           | 31.83%  | 45.93%  | 40.36%  | 39.22%  | 36.34%  |
| Deferred Revenue Non Current             | 0.00%   | 0.00%   | 0.00%   | 0.00%   | 0.00%   |
| Deferred Tax Liabilities                 | 2.64%   | 2.10%   | 2.99%   | 3.14%   | 2.70%   |
| Other Non Current Liabilities            | 9.85%   | 10.82%  | 9.12%   | 8.53%   | 8.67%   |
| Total Non Current Liabilities            | 44.33%  | 58.88%  | 52.44%  | 50.86%  | 47.80%  |
| Other Liabilities                        | 0.00%   | 0.00%   | 0.00%   | 0.00%   | 0.00%   |
| Capital Lease Obligations                | 1.28%   | 1.49%   | 1.23%   | 1.20%   | 1.02%   |
| Total Liabilities                        | 75.58%  | 75.60%  | 73.62%  | 72.09%  | 71.85%  |
| Preferred Stock                          | 0.00%   | 0.00%   | 0.00%   | 0.00%   | 0.00%   |
| Common Stock                             | 2.04%   | 2.02%   | 1.86%   | 1.90%   | 1.80%   |
| Retained Earnings                        | 76.16%  | 76.29%  | 73.20%  | 76.51%  | 75.54%  |
| Accumulated Other Comprehensive Income   | -15.05% | -17.18% | -14.83% | -16.16% | -14.33% |
| Other Total Shareholder Equity           | -40.51% | -38.95% | -36.02% | -36.64% | -35.82% |
| Total Shareholder Equity                 | 21.99%  | 22.11%  | 24.36%  | 25.97%  | 26.51%  |
| Total Equity                             | 24.42%  | 24.40%  | 26.38%  | 27.80%  | 28.15%  |
| Total Liabilities and Shareholder Equity | 100.00% | 100.00% | 100.00% | 100.00% | 100.00% |
| Minority Interest                        | 2.45%   | 2.28%   | 1.97%   | 1.85%   | 1.58%   |
| Total Liabilities and Equity             | 100.00% | 100.00% | 100.00% | 100.00% | 100.00% |
| Total Investments                        | 28.47%  | 27.72%  | 22.67%  | 22.52%  | 24.67%  |
| Total Debt                               | 49.54%  | 49.03%  | 45.34%  | 42.13%  | 43.09%  |
| Net Debt                                 | 42.01%  | 41.24%  | 35.06%  | 31.90%  | 33.47%  |

+++ {"editable": true, "slideshow": {"slide_type": ""}}

Notice the Total Assets and Total Liabilities and Equity rows are filled with 100%.

Fixed assets is about 75% of total assets, cash and cash equivalents is about 10%, while inventory is about 4%. Goodwill is about 20% over the years. This came about from acquisition (and divestitures when goodwill is reduced).

Liabilities-to-equity ratio is about 7:3. Long-term debt is about 35%.

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The sheet that contains the base-year analysis may be found [here](https://docs.google.com/spreadsheets/d/1OCqC9k0lz1kRZ-RXYRMgI8ebjHum1xHEfAYKl4t3Nos/edit#gid=890470595).

+++ {"editable": true, "slideshow": {"slide_type": ""}}

(common_size_analysis)=
## Common-Size Analysis

In common size analysis, a field is selected to be a reference value, and other fields are expressed as a percentage relative to it.

For example, on the income statement, one may choose the Sales or top line, to be the reference figure. It will have the value of 100%. Other field values will be then expressed as a percentage of sales. 

On the balance sheet, one may take the Total Assets or Total Liabilities and Equity as reference value (does not matter which from the Accounting Equation for the balance sheet), and all other field values are expressed as a percentage of the reference value. This makes good sense as each field on the balance sheet is either an asset, or a liability or an equity.

The common-size analysis for the balance sheet of The Coca-Cola Company FY2019-2023 is shown as follows:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
tags: [hide-input]
---
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
    if name == 'IS Common-Size':
        df = pd.read_excel(data, sheet_name=name)
        break
        
df = df.set_index(df.columns[0])
df.index.names = ['Field']
for y in range(2019, 2024):
    df.drop(y, axis='columns', inplace=True)
df = df.rename(columns={str(k) + '.1':k for k in range(2019, 2024)})
def f(x):
    try: 
        return "{:.2%}".format(x)
    except:
        return x
df = df.apply(np.vectorize(f))

# This doesn't work for markdown tables because myst parses CommonMark
# Ref: https://github.com/executablebooks/jupyter-book/issues/1105
#display(Markdown(df.to_markdown()))

# Copy and paste the output of the following into a markdown cell,
# then comment it out.
#print(df.to_markdown())
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

| Field                                        | 2019    | 2020    | 2021     | 2022    | 2023     |
|:---------------------------------------------|:--------|:--------|:---------|:--------|:---------|
| Revenue                                      | 100.00% | 88.47%  | 103.75%  | 115.28% | 122.79%  |
| Cost of Goods Sold                           | 100.00% | 91.78%  | 105.48%  | 123.29% | 126.71%  |
| Gross Profit                                 | 100.00% | 86.73%  | 103.10%  | 110.62% | 120.35%  |
| Gross Profit Ratio                           | 100.00% | 97.60%  | 99.18%   | 95.67%  | 97.94%   |
| Research and Development Expenses            | #DIV/0! | #DIV/0! | #DIV/0!  | #DIV/0! | #DIV/0!  |
| General and Administrative Expenses          | 100.00% | 62.69%  | 167.66%  | 177.11% | 159.70%  |
| Selling and Marketing Expenses               | 100.00% | 76.12%  | 93.68%   | 99.58%  | 106.88%  |
| Selling, General and Administrative Expenses | 100.00% | 80.41%  | 100.00%  | 106.61% | 115.70%  |
| Other Expenses                               | 100.00% | 186.24% | 184.72%  | 266.38% | -436.68% |
| Operating Expenses                           | 100.00% | 84.13%  | 103.17%  | 111.90% | 126.19%  |
| Cost and Expenses                            | 100.00% | 88.24%  | 104.04%  | 118.01% | 126.47%  |
| Interest Income                              | 100.00% | 65.72%  | 49.02%   | 79.75%  | 161.10%  |
| Interest Expense                             | 100.00% | 152.22% | 169.13%  | 93.23%  | 161.73%  |
| Depreciation and Amortization                | 100.00% | 123.92% | 48.17%   | 97.01%  | 37.54%   |
| EBITDA                                       | 100.00% | 96.95%  | 118.32%  | 105.34% | 94.66%   |
| EBITDA Ratio                                 | 100.00% | 109.68% | 113.92%  | 91.52%  | 77.38%   |
| Operating Income                             | 100.00% | 89.11%  | 138.61%  | 107.92% | 111.88%  |
| Operating Income Ratio                       | 100.00% | 100.70% | 134.04%  | 93.75%  | 91.35%   |
| Total Other Income                           | 100.00% | 107.43% | -228.57% | 111.00% | 234.29%  |
| Income Before Tax                            | 100.00% | 90.28%  | 114.81%  | 108.33% | 120.37%  |
| Income Before Tax Ratio                      | 100.00% | 102.04% | 111.06%  | 93.88%  | 97.82%   |
| Income Tax Expense                           | 100.00% | 110.00% | 145.56%  | 117.78% | 125.00%  |
| Net Income                                   | 100.00% | 86.88%  | 109.53%  | 106.95% | 119.96%  |
| Net Income Ratio                             | 100.00% | 98.04%  | 105.60%  | 92.69%  | 97.83%   |
| EPS                                          | 100.00% | 86.12%  | 108.13%  | 105.26% | 118.66%  |
| EPS Diluted                                  | 100.00% | 86.47%  | 108.70%  | 105.80% | 119.32%  |
| Weighted Average Shares                      | 100.00% | 100.23% | 100.70%  | 100.93% | 100.70%  |
| Weighted Average Shares Diluted              | 100.00% | 100.23% | 100.70%  | 100.93% | 100.70%  |

+++ {"editable": true, "slideshow": {"slide_type": ""}}

Notice that the leftmost column for FY2019 have values 100%.

The revenue growth over the years 2019 to 2023 is expressed by: 100.00%, 88.47%, 103.75%, 115.28%, 122.79%.

The net income growth over the same period is expressed by: 100.00%, 86.88%, 109.53%, 106.95%, 119.96%.

+++ {"editable": true, "slideshow": {"slide_type": ""}}

Note that the trends as depicted by the sparklines are similar in shape to those depicted by the sparklines on the original income statment that does not have base-year analysis performed on it. 

Nevertheless, there is one advantage to consider trends using field values in a base-year analysis, namely, the sparklines are mutually comparable in scale and they are start at 100%.

Hence, we may superpose all the resulting sparklines on a single graph to compare them:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
tags: [hide-input]
---
trends = []
names = []
for i in range(len(df)):
    try:
        trends.append([float(x.strip('%'))/100 for x in list(df.iloc[i])])
        names.append(df.index[i])
    except:
        pass

from IPython.core.display import HTML
HTML("""
<style>
.output_png {
    display: table-cell;
    text-align: center;
    vertical-align: middle;
}
</style>
""")

# importing package 
import matplotlib.pyplot as plt 
%matplotlib inline
  
# create data 
x = [10,20,30,40,50] 
y = [30,30,30,30,30] 
  
# plot lines 
for i, trend in enumerate(trends):
    plt.plot(range(5), trend, label = names[i])
#plt.legend()
plt.show()
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

Comparing trends in base-year analysis is sometimes called *Index Number Trend Analysis*.

+++ {"editable": true, "slideshow": {"slide_type": ""}}

The sheet that contains the common-size analysis may be found [here](https://docs.google.com/spreadsheets/d/1OCqC9k0lz1kRZ-RXYRMgI8ebjHum1xHEfAYKl4t3Nos/edit#gid=1727892542).

+++ {"editable": true, "slideshow": {"slide_type": ""}}

## Reasoning

### Computational Artefacts

Identifying and comprehending the underlying causes of significant quantities is worthwhile. Sometimes, the causes are unrelated the financial reality, but are rather due to how calculations turn out.

For example, if $x_t$ is a small positive number (say, close to 0), resulting in a large fraction $\frac{x_{t+1} - x_t}{x_t}$, then the cause of the large fraction may simply be due to this fact.

A change is signs may also result in a large relative fraction. For example, $x_t$ may be positive while $x_{t+1}$ may be negative. In this, it may be more reasonable to note that there has been a change causing the signs to flip, and not to interpret the relative change too much.

Missing data may also cause difficulties in reasoning for the obvious reason that there is no data to reason with!

### Inconsistencies

Suppose that there is a 20% increase in sales. If it is also known that there is a 25% increase in freight-out cost, then the situation calls for further investigation and explanation.

Suppose that there is a 20% increase in accounts receivable. If the sales increase is just 5%, then one should try to find out the reasons behind the significant increase in accounts receivable which does not commensurate with the increase in sales. In instances where there appears to be a relaxation in maintaining credit quality, it prompts a deeper inquiry into the underlying reasons behind such a shift.

In the first case, the directions of trends supported by the evidence differ. In the second, the magnitudes of changes are incompatible.

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---

```
