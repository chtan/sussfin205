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

# Quotes in the Markets

+++

## Currencies

+++

### Quotes

+++

Currency quotes look like this:

```{figure} img/fx.png
:width: 100%
:name: fx-table

https://www.oanda.com/currency-converter/live-exchange-rates/
```

+++

Currency quotes are also known as foreign exchange quotes or FX quotes.

While the quotes are prices of things, namely, currencies, they are a bit more complicated than the usual price quotes due to the fact that they express money in terms of money. More precisely, a currency quote expresses how much one currency costs in terms of another currency.

For example, the quote of 1.09145 for EUR/USD in the [table](#fx-table), says that, EUR 1 costs USD 1.09145.

+++

:::{tip} Mental Crutch
In an exchange rate X/Y, it is useful to regard X as the stuff that one wishes to buy using the currency Y. Thus, if EUR/USD is 1.1, it means that 1 unit of the stuff EUR (namely, EUR 1) costs USD 1.1..
:::

+++

:::{attention} What is the quote 1.09161 beside 1.09145 for EUR/USD?
:class: dropdown
This is the *ask* quote, while 1.09145 is the *bid* quote. Bid is the price at which the dealer buys or is willing to buy from the client to which the quote is reported or advertised. Ask is the sell price. As quotes of published by dealers, it is natural that the bid/buy and ask/sell are taken from their perspectives.
:::

+++

For the purpose of calculations, there is the notion of a *mid-quote*, *mid-price* or simply *mid*. The mid for EUR/USD is:

$$\frac{1.09145 + 1.09161}{2} \approx 1.09153$$

+++

### Turnover Distribution of Currencies

+++

Currencies are traded in the foreign exchange (FX) market. Unlike the stock market or the commodities futures market, the FX market is not primarily concentrated at exchanges. Instead, dealers of various sizes act as intermediaries and the dealers are themselves connected through trades. Some of the largest dealers are banks. For this reason, the FX market is sometimes described as being *over the counter*.

Due to this nature, it is not an easy task to collate data and produce statistics on the state of the FX market. The Bank for International Settlements (BIS) exercises its clout to do so and produces the Triennial Central Bank Survey of foreign exchange and Over-the-counter (OTC) derivatives markets. The recent survey is dated 2022:
- [https://www.bis.org/statistics/rpfx22_fx_annex.pdf](https://www.bis.org/statistics/rpfx22_fx_annex.pdf)

According to the report, the currencies with the highest turnovers are:
* USD
* EUR
* JPY
* GBP
* CNY
* AUD
* CAD
* CHF
* etc.

+++

### Calculating Cross Rates

Consider the exchange rates EUR/USD and USD/JPY.

For the purpose of the discussion here, we will only be considering mid quotes.

A client wishes to change EUR 1 into USD, and then from USD into JPY.

According to the [table](#fx-table), the listed rates are EUR/USD = 1.09153 and USD/JPY = 147.687, since

$$\frac{147.680 + 147.694}{2} \approx 147.687$$

+++

This means that EUR 1 gets changed to USD 1.09153, which in turn, gets changed to JPY 161.205, since

$$1.09153 \times 147.687 \approx 161.205$$

+++

In other words, EUR/JPY = 161.205.

As this exchange rate has been calculated from given rates EUR/USD and USD/JPY, it is said to be an *implied* rate.

On the other hand, the dealer also lists EUR/JPY as 161.207, since

$$\frac{161.197 + 161.218}{2} \approx 161.207$$

+++

The difference between the implied and the quoted rates is slight. When the difference is large, it either means that there is a mispricing or the market isn't liquid, making it difficulty for someone to actually realize the implied rate through actual trading. When the difference is large and someone is able to make money out of it, it is known as an *arbitrage opportunty* in finance.

+++

```{exercise}
:label: cross-rate
:nonumber:

In the exchange rates [table](#fx-table), suppose that the mid quote of USD/JPY is 140.000.

Explain how a trader can take advantage of an arbitrage opportunity.
```

+++

````{solution} cross-rate
:label: solution-cross-rate

Note that 3 currencies are involved here, namely, EUR, USD and JPY. This suggests that we consider the relationships between EUR/USD, USD/JPY and EUR/JPY.

As noted earlier, the mid of EUR/USD is 1.09153.

The mid of EUR/JPY is calculated from the [table](#fx-table) to be $\frac{161.197 + 161.218}{2} \approx 161.207$.

Thus, the implied USD/JPY is

$$\text{USD}/\text{EUR} \times \text{EUR}/\text{JPY} = \frac{1}{1.09153} \times 161.207 \approx 147.689$$

from the chain of currency exchanges USD → EUR → JPY. There is a significant discrepancy between the implied exchange rate and the listed exchange rate as given in the exercise.

An astute trader may act as follows, using the mantra *Buy Low, Sell High*:
- Buy USD 1 at USD/JPY 140 using JPY 140
- Buy EUR $\frac{1}{1.09153}$ using USD 1
- Buy JPY $\frac{1}{1.09153} \times 161.207$ using EUR $\frac{1}{1.09153}$

According to the calculations above, the trader ends up with JPY 147.689, i.e. JPY 7.689 richer!
````
