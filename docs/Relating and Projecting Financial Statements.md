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

# Relating and Projecting Financial Statements

+++ {"editable": true, "slideshow": {"slide_type": ""}}

## Review of Debit and Credit

+++ {"editable": true, "slideshow": {"slide_type": ""}}

There are 5 *accounting elements*:
- assets
- liabilities
- equity
- income
- expenses

An accounting element is an *account type*. Each account belongs to a unique account.

More operationally, an account is a *ledger account* (or *T account*), which typically include the following information:ormation:

1. Account Name: The name of the account, indicating the type of asset, liability, equity, revenue, or expense being tracked.
2. Account Number: A unique identifier assigned to each account for organizational purposes.
3. Transaction Details: A chronological record of all transactions affecting the account, including the date, description of the transaction, and the amounts debited and credited.
4. Balance: The balance of the account, which is the net result of all debits and credits recorded in the account. The balance is used to determine the financial position and performance of the business.

Ledger accounts serve as a detailed record that supports the preparation of financial statements, such as the balance sheet, income statement, and cash flow statement. They provide essential information for management decision-making, financial analysis, and regulatory compliance.

In accounting, the terms *debit* and *changes refer to changes made on the left and right sides of a ledger account, respectively. They can also refer to the left and right sides of the ledger respectively.

The table below shows the typical effects of debit and credit entries on the five fundamental accounting elements: assets, liabilities, equity, revenue, and expenses:

| Accounting Element | Debit Effect | Credit Effect |
|--------------------|--------------|---------------|
| Assets             | Increase     | Decrease      |
| Liabilities        | Decrease     | Increase      |
| Equity             | Decrease     | Increase      |
| Revenue            | Decrease     | Increase      |
| Expenses           | Increase     | Decrease      |

Thus, a debit to the Assets and Expenses accounts increases them, while a credit to the Liabilities, Equity and Revenue accounts increases them[^math].

The (standard) Accounting Equation[^accounting_equation] states that $$A = L + E$$

Each *transaction*[^transaction] is a collection of at least 1 debt(s) and a collection of at least 1 credit(s). For example, purchasing a computer for USD 500 debits 500 in the Equipment account (under Assets) and credits 500 in the Accounts Payable account (under Liabilities and Equity). A transaction preserves the Accounting Equation.

The extended Accounting Equation states that

$$A + Ex = L + E + I$$

Here, Ex and I are considered to be temporary accounts, while A, L and E are considered to be permanent accounts. This is consistent with the view that the income statement spans over a year, recording changes occurring in the year.

Writing it this way:

$$A = L + (E + (I - Ex))$$

we recognize the equation to be saying that the (cumulative) retained earnings is an equity component and the (change to the) retained earnings derived from net income adds to the corresponding balance sheet component. Simultaneously, the retained earnings increase the assets of the firm.

Put another way, income accounts record all increases in Equity other than that contributed by the owner/s of the business/entity. Expense accounts record all decreases in Equity which occur as costs during the course of business.

In other words, the extended Accounting Equation expresses the change to the standard Accounting Equation after 1 year. To make it very clear, we may express the standard Accounting Equation as

$$A_0 = L_0 + E_0$$

and the extended Accounting Equation as

$$A_1 = L_1 + E_1,$$

where $E_1 = E_0 + (I - Ex)$.

This is a simplistic interpretation that says that the only change in Equity comes from the income statement. Real accounting may be more complex than this. Nonetheless, this formula clarifies the idea behind the extended Accounting Equation.

[^math]: Mathematically speaking, accounting comprises accounts (i.e. rows). Each account has 2 columns - it has a debit (left-hand) side and a credit (right-hand) side. These sides hold non-negative numbers. Each account belongs to 1 of 5 accounting element or type (A, L, E, I and Ex). A debit operation is $+(a,0)$ to an account that belongs to A or Ex. A credit operation is $+(0,b)$ to an account that belongs to L, E or I.

[^accounting_equation]: The Accounting Equation stipulates that the sum of numbers on the debit side is equal to the sum of numbers on the credit side.

[^transaction]: Each transaction is a sequence of debits and credits such that the sum to be debited is equal to the sum to be credited.

+++ {"editable": true, "slideshow": {"slide_type": ""}}

## Relationships among the Financial Statements

We consider 2 times: 0 and 1.

The <u>balance sheet</u> records the financial state of a firm at a given time. Hence, we have at time 0, the state

$$A_0, L_0, E_0$$

and at time 1, the state

$$A_1, L_1, E_1$$

The <u>income statement</u> records changes in the course of a year (i.e. from time 0 to time 1). It has 2 components:

$$I, Ex$$

The net income is $I - Ex$. Assuming that all of this becomes retained earnings, then we have (as mentioned in the earlier section)

$$E_1 = E_0 + (I - Ex)$$

Suppose that over the course of the year, a borrowing of an amount $\beta$ is made to invest in a fixed asset, then

$$L_1 = L_0 + \beta$$

Consequently, we may verify:

$$
\begin{align*}
A_1 &= A_0 + \beta + (I - Ex) \\
    &= L_0 + E_0 + \beta + (I - Ex)\\
    &= (L_0 + \beta) + (E_0 + (I - Ex))\\ 
    &= L_1 + E_1
\end{align*}
$$

Now, let's consider the <u>cash flow statement</u>. It comprises

$$CFO, CFI, CFF$$

with net cash flow 

$$CF = CFO + CFI + CFF$$

In the present situation,

$$CFO = I - Ex$$

$$CFI = -\beta$$

$$CFF = \beta$$

and $$CF = I - Ex$$

Hence, we may rewrite the verification as

$$
\begin{align*}
A_1 &= A_0 - CFI + CF\\
    &= L_0 + E_0 - CFI + CF\\ 
    &= (L_0 - CFI) + (E_0 + CF)\\
    &= (L_0 + CFF) + (E_0 + (I - Ex))\\
    &= L_1 + E_1
\end{align*}
$$

Hence, we observe the following relationships between the balance sheet, the income statement and the cash flow statement:

- Retained earnings (balance sheet and income statement): $E_1 = E_0 + (I - Ex)$

- Indirect method of cash flow statement construction (income statement and cash flow statement): $CFO = I - Ex$

- Components of the cash flow statement: $CF = CFO + CFI + CFF$

- Change in assets (balance sheet and cash flow statement): $A_1 = A_0 - CFI + CF$

- Change in liabilities (balance sheet and cash flow statement): $L_1 = L_0 + CFF$

+++ {"editable": true, "slideshow": {"slide_type": ""}}

## Three-Statement Modelling

Modelling here refers to projecting current and historical financial statements into the future for the purpose of forecasting.

Due to the fact the the 3 financial statements (income statement, balance sheet and cash flow statement) are inter-linked as was discussed in the earlier section, projecting them into the future requires ensuring that the relationships are consistently projected as well.

+++ {"editable": true, "slideshow": {"slide_type": ""}}

Let's continue to use the notation from the previous section.

We will add time indices to the income statement and cash flow statement components as follows:

$$I_1 = I, Ex_1 = Ex, CFO_1 = CFO, CFI_1 = CFI, CFO_1 = CFO, CF_1 = CF$$

This is consitent with how we read income statement and cash flow statements, i.e. they are timed at the end of the observation/financial period. For example, $I$ is the income read at time 1, for the period from time 0 to time 1, hence it is reported together with the balance sheet at time 1, and it should rightly be indexed as $I_1$.

+++ {"editable": true, "slideshow": {"slide_type": ""}}

We will now try to create pro forma financial statements by projecting sales into time 2. 

So, let's assume that

$$I_2 = (1 + g)I_1$$

where $g$ is a growth rate.

For simplicity, let's assume that the firm is able to hold costs down while increasing sales. Over the course of the year from time 1 to time 2, we will also encounter an amortization of the fixed asset by an amount $\alpha$ and interest expense of $r\beta$, where $r$ is the interest rate of borrowing. Thus

$$Ex_2 = Ex_1 + \alpha + r\beta$$

Retained earnings is $I_2 - Ex_2$ and thus, we have the equity relationship

$$E_2 = E_1 + (I_2 - Ex_2)$$

as we assume that all earnings are retained.

Liability remains unchanged:

$$L_2 = L_1$$

as there is no change to the borrowing situation.

By the indirect method for cash flow statement construction,

$$CFO_2 = I_2 - Ex_2 + \alpha + r\beta$$

as amortization and interest expense are both added back.

As no investment is made over the period, 

$$CFI_2 = 0$$

while the interest payment implies that

$$CFF_2 = -r\beta$$

Now we may compute the net cash flow to be

$$
\begin{align*}
CF_2 &= CFO_2 + CFI_2 + CFF_2\\
     &= I_2 - Ex_2 + \alpha + r\beta + 0 - r\beta\\
     &= (1 + g)I_1 - Ex_1 - \alpha - r\beta + \alpha + r\beta - r\beta\\
     &= (1 + g)I_1 - Ex_1 - r\beta
\end{align*}
$$

On the assets side of the balance sheet,

$$A_2 = A_1 - \alpha + CF_2$$

due to amortization of the fixed asset and the net cash flow into C&CE.

On the liabilities side, 

$$L_2 = L_1$$

since there is no change to the borrowing.

On the equities side,

$$E_2 = E_1 + (I_2 - Ex_2)$$

due to cumulating retained earnings.

Now, let's check for balance:

$$
\begin{align*}
L_2 + E_2 &= L_1 + E_1 + (I_2 - Ex_2)\\
          &= A_1 + (1 + g)I_1 - (Ex_1 + \alpha + r\beta)\\
          &= A_1 - \alpha + ((1 + g)I_1 - Ex_1 - r\beta)\\
          &= A_1 - \alpha + CF_2\\
          &= A_2
\end{align*}
$$

Indeed, the balance sheet remains balanced and we are confident that our pro forma balance sheet ($A_2, L_2, E_2$), pro forma income statement ($I_2, Ex_2$) and pro forma cash flow statement ($CF_2, CFO_2, CFI_2, CFF_2$) are alright.
