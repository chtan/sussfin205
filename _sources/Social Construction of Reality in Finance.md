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

# Social Construction of Reality in Finance

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

## The View from the Sociology of Knowledge

### What are you learning from this course?

Let's try to form a perspective of a road map here.

First, we learn about financial statements and about analyzing them. Then, we learn about the fundamental notion in finance called the Time Value of Money which calls out the fact that money does not have a fixed value over time and provides a way to value money at different time points as a basis for comparing cash flows. Finally, we make use of this to model financial statements, projecting cash flows into the future, and present valuing them to obtain a valuation for the company on hand.

Financial Analysis and Financial Modeling are two major activities associated to the financial statements. Producing the statements themselves is a major part of Financial Reporting. Thus, there are the following relationships that connect the various parts of this course to reality:

- Accountants
  - $\xrightarrow[\text{reporting}]{\text{financial}}$ Financial Statements

- Analysts
  - $\xrightarrow[\text{analysis}]{\text{financial}}$ Analyst Reports and Presentation
  - $\xrightarrow[\text{modeling}]{\text{financial}}$ Spreadsheets

Note that spreadsheets are commonly thought of as *Excel spreadsheets* due to the traditional prevalence of Excel use <i>on Wall Street</i>. But in this course, you learn that the output of modeling can also be on Google Sheets or in Python scripts. It does not necessarily be Excel. Nonetheless, "Spreadsheets" is used above to denote a definite and concrete product that is produced and communicated at the end of the activity of financial modeling.

These relationships further fit into the following diagram: 

```{mermaid}
%%{init: { 'logLevel': 'debug', 'theme': 'neutral' } }%%
flowchart LR
  Accountant -->|Reporting|id1(Financial Statements)
  id1 ==> id0((Finance Industry))

  Analyst -->|Analysis|id2(Report)
  id2 ==> id0

  Analyst -->|Modeling|id3(Model)
  id3 ==> id0

  Analyst -->|Presenting|id4(Presentation)
  id4 ==> id0

  style id0 fill:#f9f,stroke:#333,stroke-width:4px
```

Conceptualizing the diagram,

- accountants and analysts are actors

- reporting, analyzing, modeling, presenting are activities

- financial statements, analyst reports/presentations/models are discourse products

- finance industry is interactional arena

The general observation is that actors are engaged in activities that produce discourse products for the purpose of communicating in an interactional arena.

You learn how to produce financial statements, write an analyst report, present like an anlyst does or construct a financial model. You may do these for yourself, or for assignments in your course, or as a professional in the finance industry. In all cases, you are producing something meaningful for the purpose of communicating with someone. 

These thing that you produce is meaningful for these reasons:

- deontological: it must conform to the norms and rules governing the discourse (e.g. language, knowledge domain)

- teleological: it must serve the purpose for which is ia constructed (e.g. for your stock investment decision making as an individual, for passing the course as a student, or for informing and persuading the client as a sales-side analyst)

The analysis, model and/or presentation that you learn to produce in this course are of key importance to you as learning milestones and comprise the pedagogical objectives of this course as it aims to prepare you for a role in the finance industry. For their are meaningfulness, we call them semantical products.

For you, you learn the concepts, the examples and tools to construct these semantical products. For the finance industry, these products is part of the reality. 

### The Perspective of The Social Construction of Reality

[The_Social_Construction_of_Reality: A Treatise in the Sociology of Knowledge](https://en.wikipedia.org/wiki/The_Social_Construction_of_Reality) is the title of a book written by two sociologists Peter L. Berger and Thomas Luckmann first published in 1966. In 1998, the International Sociological Association listed The Social Construction of Reality as the fifth most-important book of 20th-century sociology. The intention of this latter information is more to suggest that the perspective being brought forth here is valid albeit unconventional, rather  than an appeal to authority.

:::{admonition} What is the Sociology of Knowledge?
:class: note
The discipline of the sociology of knowledge explores how social structures, institutions, and cultural contexts shape the creation, dissemination, and validation of knowledge within a society. It investigates how social factors influence what is considered valid or authoritative knowledge, and how these beliefs are maintained or challenged over time. This field examines the interplay between knowledge and society, including how knowledge reflects power dynamics, social hierarchies, and cultural norms.
:::

The perspective is that the reality of the finance industry, in itself as well as what one finds in it, is one that is socially constructed. In this view, what undergirds the reality is a network of actors meaningfully communicating with one another through the use of discourse product, generating social signals and facts along the way.

The financial statements, the analyst reports, models and presentations are examples of these products. They  are not dispensible objects merely there to provide information about what is truly going on. Without financial statements, there is no accountability and companies would not have existed. Analyst reports and presentations form the basis for many investors in their investment decisions, spurring trading activities in the markets. The model form the basis for the analysts when they pronounce forecasts and expected earnings for companies that they cover.

Bitcoin is an even more poignant illustration of this. It may now be recognized to have value now due to its high price and the stability of its peer-to-peer network and crytographic protocols. The fact that it is officially classified as a commodity in the US also contributes to its recognition. If we inspect Bitcoin under the hood, we will find a network of users communicating messages to one another. These messages are essentially transactions of a form that say "Alice sends to Bob BTC x". These transactions are stored on a blockchain which can be scrutinized to provide the evidence for a user like Alice to possess a wealth of say BTC 100, which translates to about USD 5,000,000 given today's price of bitcoin in USD (BTCUSD = 65,991.30 on 22/3/2024). The sense of this translation is that (1) the latest exchange between USD and BTC is at this exchange rate, (2) an exchange is grounded by the fact that there has been a flow of USD value (cash or otherwise) from one financial actor to another, and a matching transaction in the Bitcoin network that sends bitcoins from one user to another, and (3) the BTC 100 may be readily changed into (roughly) the amount of USD as given by the exchange rate as the bitcoin market is liquid (i.e. it is to be believed, based on what we know of how markets work, that many folks out there are ready to buy or sell bitcoins at the current exchange rate).

Thus, the fact that one may possess a wealth in bitcoins is in turn due to the fact that there actors sending transactions (semantical products) to each other, which are eventually recorded down in blockchains and observed by other market participants. The magic of being rich is a social reality consisting in actors communicating meaningfully with one another when that reality is carefully unveiled.

In the following sections, we will further elaborate on the flow of financial statements in the markets through time, the impacts from earnings announcements and the associated activities of reporting, analysis, modeling and presentation.

+++ {"editable": true, "slideshow": {"slide_type": ""}}

## Earnings Announcements

Earnings announcements are public statements made by companies about their profitability. These are made either quarterly or annually. They are typically made before the companies file 8-K or 10-K reports with the SEC.

:::{admonition} Do you know that companies can publish different versions of financial statements?
:class: note

Yes, it is true that companies can publish different versions of financial statements. 

There are several reasons why companies might publish different versions of their financial statements:

1. **Regulatory Requirements**: Companies may be required to prepare different versions of financial statements to comply with various regulatory requirements. For example, public companies in the United States must file annual financial statements with the Securities and Exchange Commission (SEC) in accordance with Generally Accepted Accounting Principles (GAAP). Additionally, companies operating in multiple countries may need to prepare financial statements according to different accounting standards, such as International Financial Reporting Standards (IFRS) or local Generally Accepted Accounting Principles (GAAP).

2. **Reporting to Stakeholders**: Companies often prepare different versions of financial statements for different stakeholders, such as shareholders, creditors, and regulators. For example, publicly traded companies typically issue audited financial statements to shareholders as part of their annual reports. At the same time, they may provide unaudited financial statements to creditors or other third parties for specific purposes, such as obtaining financing or fulfilling contractual obligations.

3. **Internal Management Reporting**: Companies may generate different versions of financial statements for internal management reporting purposes. These statements may be tailored to provide more detailed or summarized information to support decision-making processes within the organization. Internal management reports may include additional analyses, forecasts, or projections that are not included in external financial statements.

4. **Consolidated vs. Separate Financial Statements**: In cases where a company has subsidiaries or affiliates, it may prepare both consolidated financial statements, which combine the financial results of all subsidiaries into a single set of statements, and separate financial statements for each individual entity. Consolidated financial statements provide a comprehensive view of the entire group's financial position and performance, while separate financial statements focus on the financial results of each entity individually.

Overall, companies may prepare different versions of financial statements to meet the needs of various stakeholders, comply with regulatory requirements, and support internal decision-making processes. These versions may vary in terms of format, level of detail, and level of assurance provided  auditors.


:::

Companies usually make earnings announcements during the earnings seasons. There is one earning season associated to each quarter. Earning seasons generally begins in the month following the fiscal quarters, namely, January, April, July and October, as the companies need time after the end of each quarter's accounting period to put the information together. Each earnings season lasts for about 6 weeks (or 1.5 months). Then the cycle starts all over again.

Year-end performance, viz. the Q4 earnings, often provide a comprehensive view of the company's performance for the entire fiscal year.

The times and periods just mentioned are shown on the timeline here:

```{mermaid}
%%{init: { 'logLevel': 'debug', 'theme': 'neutral' } }%%
timeline
        title Earnings Announcements Timeline
        section Jan-Mar
          Q1 Fiscal Period : Q4/Annual Earnings Season ~ 6 wks
        section Apr-Jun
          Q2 Fiscal Period : Q1 Earnings Season ~ 6 wks
        section Jul-Sep
          Q3 Fiscal Period : Q2 Earnings Season ~ 6 wks
        section Oct-Dec
          Q4 Fiscal Period : Q3 Earnings Season ~ 6 wks
```

This webpage on Yahoo! Finance provides a visualization of the earnings season:

### Earnings Season on Yahoo! Finance:

```{figure} img/earnings_season.png
:width: 100%
```
<p style="text-align:center;"><a href="https://finance.yahoo.com/calendar/earnings/?from=2024-03-10&to=2024-03-16/">https://finance.yahoo.com/calendar/earnings/?from=2024-03-10&to=2024-03-16/</a></p>

This is Q4 earnings announcement period, which picks up (referring to the number of announcements per day) from the end of January, peaks and slows down again, lasting for about 6 weeks or 1.5 months, which is half the Q1 fiscal quarter.

Historical record of earnings announcement dates of The Coca-Cola Company is shon here:

### Earnings History of The Coca-Cola Company (KO)

```{figure} img/earnings_history.png
:width: 100%
```
<p style="text-align:center;"><a href="https://www.tipranks.com/">https://www.tipranks.com/</a></p>


Notice the data structure that summarizes an announcement as suggested from the header:

- date

- quarter

- forecast/actual/past eps


To this information, we may also add:

- day of week,

- time of announcement (before open, or after close)

- change in stock price between current open and last close on the announcement day

- expected/observed/past earnings

in order to capture the most salient aspects of an earnings announcement and its effects.

The following diagram show the cover page of the news release that contain the earnings annoucement:

### News Release

```{figure} img/news_release.png
:width: 80%
```

Earnings data for companies are readily found on the internet, e.g. [https://www.nasdaq.com/market-activity/stocks/ko/earnings](https://www.nasdaq.com/market-activity/stocks/ko/earnings).

Typically, after the earnings announcement, the company will submit filings at the SEC, a list of filings of KO is shwn here:

### SEC Filings

```{figure} img/ko_filings.png
:width: 100%
```
<p style="text-align:center;"><a href="https://www.sec.gov/edgar/browse/?CIK=21344&owner=exclude">https://www.sec.gov/edgar/browse/?CIK=21344&owner=exclude/a></p>

### Trades and Signals

Earnings announcements are be made pre-open (before open) or post-close (after close) outside of trading hours. They may occur on any day during the business week. When new information is released to investors, trading at new levels is precipitated, consequently significantly impacting stock prices, so that the opening price may register a jump from the closing price from the prior business day. The ensuing volume may also significantly increase for a while before slowing down again.

+++ {"editable": true, "slideshow": {"slide_type": ""}}

## Following the Analyst

### Types of Analysts

Analysts conduct research on companies to study their financial health or investment viability. Analysts who specialize in the latter are called investment analysts. They may fall into 2 categories: buy-side or sell-side, depending on the orientation of the firm that they are working for.

Buy-side analysts work for institutions that invest in securities, such as mutual funds, hedge funds, pension funds, and other asset management firms. Their primary role is to analyze investment opportunities and make recommendations on which securities to buy or sell for their firm's portfolio. Buy-side analysts focus on evaluating the potential returns and risks of investments, conducting in-depth research on companies, industries, and market trends to inform their investment decisions. They aim to generate profits for their firm by making successful investment choices.

Sell-side analysts work for brokerage firms, investment banks, and independent research firms. Their primary role is to provide research and analysis on publicly traded companies to institutional and retail clients. Sell-side analysts publish research reports, investment recommendations, and earnings estimates on various companies and industries. They often interact with institutional investors, such as asset managers and hedge funds, to provide insights and recommendations on investment opportunities. Sell-side analysts also play a role in underwriting securities offerings and providing advisory services to corporate clients.

### Sources of Information for Research

When researching a company, analysts typically gather information from a variety of sources to develop a comprehensive understanding of the company's operations, financial performance, industry dynamics, and competitive position. Some common sources of information include:

1. Financial statements: This includes the company's income statement, balance sheet, and cash flow statement, which provide insights into its revenue, expenses, assets, liabilities, and cash flows over a specific period.

2. Annual reports (Form 10-K): Companies are required to file annual reports with the Securities and Exchange Commission (SEC), providing detailed information about their financial performance, business operations, risks, and management discussions.

3. Quarterly reports (Form 10-Q): Companies also file quarterly reports with the SEC, offering updates on their financial results and key developments since the last annual report.

4. Company presentations and investor relations materials: Companies often publish presentations, investor decks, and other materials on their websites or through investor relations channels, which provide additional insights into their strategies, market positioning, and future prospects.

5. Industry reports and market research: Analysts may consult industry reports, market research studies, and industry publications to understand broader industry trends, competitive dynamics, and growth prospects.

6. News and press releases: Analysts monitor news articles, press releases, and media coverage related to the company, its competitors, and the broader industry to stay informed about recent developments, announcements, and market trends.

7. Analyst reports and research notes: Analysts may review research reports, investment recommendations, and research notes published by sell-side analysts, investment banks, and independent research firms, which offer insights and analysis on the company's performance and prospects.

8. Regulatory filings and disclosures: Analysts review regulatory filings, such as proxy statements, insider trading disclosures, and material event filings, to gain insights into corporate governance practices, executive compensation, and significant business events.

9. Industry conferences and events: Analysts may attend industry conferences, investor presentations, earnings calls, and other corporate events where company executives discuss their strategies, performance, and outlook with investors and analysts.

10. Interviews and discussions: Analysts may conduct interviews with company management, industry experts, suppliers, customers, and other stakeholders to gather additional insights and perspectives on the company and its operations.

By collating information from these diverse sources, analysts can conduct thorough research and analysis to form their investment recommendations and insights about a company.


### Key Sections in the Filings

Analysts commonly pay attention to several key sections within the 8-K and 10-K filings for companies. These sections provide important insights into the company's financial performance, operations, risks, and strategic direction. Some of the sections that analysts often focus on include:

For Form 8-K:

1. **Item 2.02 - Results of Operations and Financial Condition:** This section provides updates on the company's financial performance, including any significant events or developments that may impact its financial results.

2. **Item 7.01 - Regulation FD Disclosure:** Analysts pay attention to disclosures made under Regulation FD (Fair Disclosure), which require companies to disclose material information to the public.

3. **Item 8.01 - Other Events:** Analysts look for any other significant events or developments that may impact the company's operations, financial condition, or stock price.

For Form 10-K:

1. **Management's Discussion and Analysis (MD&A):** This section provides management's narrative analysis of the company's financial performance, results of operations, liquidity, and capital resources. Analysts closely examine this section to understand management's perspective on the company's performance and outlook.

2. **Financial Statements:** Analysts review the company's audited financial statements, including the income statement, balance sheet, and cash flow statement, to assess its financial health and performance.

3. **Risk Factors:** Analysts pay attention to the risk factors section, which outlines the key risks and uncertainties facing the company. Understanding these risks is crucial for assessing the company's potential vulnerabilities and challenges.

4. **Business Overview:** Analysts examine the business overview section to gain insights into the company's operations, industry dynamics, competitive position, and strategic direction.

5. **Corporate Governance:** Analysts may review information related to corporate governance practices, executive compensation, and board of directors' composition to assess the company's governance structure and practices.

6. **Legal Proceedings:** Analysts look for disclosures related to any material legal proceedings or litigation that may impact the company's financial condition or operations.

7. **Management and Executive Compensation:** Analysts may analyze information about executive compensation, including salaries, bonuses, stock options, and other benefits, to evaluate management's alignment with shareholder interests.

### The Outputs of an Investment Analyst

The outputs of the work of an investment analyst play a critical role in guiding investment decisions for clients or for the analyst's firm's portfolio. These outputs include:

1. **Earnings Forecast/Estimates:** Analysts provide earnings forecasts or estimates for publicly traded companies, predicting future earnings based on various factors such as historical performance, industry trends, and macroeconomic conditions.

2. **Financial Modelling:** Analysts build financial models to evaluate the financial performance and valuation of companies, projecting income statements, balance sheets, and cash flow statements based on various assumptions and scenarios.

3. **Revision of Estimates:** Analysts continuously monitor companies and industries, updating their earnings forecasts and financial models as new information becomes available, ensuring that investment decisions are based on the most up-to-date information.

4. **Analyst Reports:** Analysts produce research reports containing in-depth analysis and insights on companies, industries, and investment opportunities, providing investment recommendations, financial analysis, and qualitative assessments of business fundamentals and risks.

5. **Presenting to Clients:** Analysts present their research findings, investment recommendations, and insights to clients or internal stakeholders through meetings, presentations, conference calls, or webinars, ensuring that clients have access to actionable information.

6. **Attending Earnings Conference Calls:** Analysts participate in earnings conference calls hosted by publicly traded companies, where they gain insights into financial results, strategic initiatives, and future outlook, which informs their analysis and recommendations.

Overall, the outputs of an investment analyst are aimed at providing comprehensive research, analysis, and insights to support informed investment decision-making and help clients achieve their investment objectives.

The following diagram shows a listing of earnings call transcripts:

```{figure} img/ko_earnings_call_transcripts.png
:width: 80%
```
<p style="text-align:center;"><a href="https://seekingalpha.com/symbol/KO/earnings/transcripts">https://seekingalpha.com/symbol/KO/earnings/transcripts</a></p>

The tabs also neatly indicate the information that is regularly obtainable for a company: Analysis, Comments, News, Transcripts, SEC Filings, Press Releases, Related Analysis.

The figure below shows the beginning of the web page that contains the contents of a transcript:

```{figure} img/ko_earnings_call_transcript.png
:width: 75%
```
<p style="text-align:center;"><a href="https://seekingalpha.com/article/4669822-coca-cola-company-ko-q4-2023-earnings-call-transcript">https://seekingalpha.com/article/4669822-coca-cola-company-ko-q4-2023-earnings-call-transcript</a></p>

In the remaining sections, we will explore the overall structure of the 3 activities of financial analysts: financial analysis, modeling and presentation.

+++ {"editable": true, "slideshow": {"slide_type": ""}}

## Financial Analysis

Let us begin with some general considerations on the nature of analyses.

An analysis is an object of discourse and hence a semantical product. It may be simple or complex. To manage the complexity, analyses may be <u>classified</u> or may be pointed out to have certain <u>features</u>. The construction of an analysis may be <u>guided by questions</u>. Once an analysis is available, it <u>facilitates the understanding</u> of some aspects of the financial situation of the company, which in turn, provides a <u>basis for further queries</u> on the financial situation of the company.

Analyses may be classified as either *low level* or *high level*. Roughly speaking, low level analysis makes assertions that are substantiated by more or less directly from collected data, while high level analysis makes assertions that may, in addition, be substantiated by points made from low level analysis. Chaining analyses from low to high level enables the construction of evidence to support high level queries and reasoning and decision making.

### Low Level Analysis

Common examples of low level analyses are:

1. **Ratio Analysis**: Calculating and interpreting various financial ratios to evaluate different aspects of a company's performance, such as liquidity, profitability, efficiency, and solvency.

2. **Trend Analysis**: Examining financial data over multiple periods to identify trends and patterns, providing insights into the company's performance and potential future prospects.

3. **Comparative Analysis**: Comparing the financial performance of one company to another within the same industry or against industry benchmarks to assess relative strengths and weaknesses.

4. **Cash Flow Analysis**: Evaluating the cash inflows and outflows of a business over a specific period to assess its ability to generate cash and meet financial obligations.

5. **Risk Analysis**: Identifying and assessing various risks associated with an investment or business decision, such as market risk, credit risk, operational risk, and regulatory risk.

6. **Valuation Analysis**: Determining the intrinsic value of a company or investment asset using methods like discounted cash flow analysis or comparable company analysis.

7. **Sensitivity Analysis**: Assessing the impact of changes in key variables or assumptions on financial outcomes, helping to evaluate investment decisions under different scenarios.

8. **Capital Budgeting Analysis**: Evaluating investment projects or capital expenditures to determine their financial viability and potential return on investment using methods like net present value and internal rate of return.

9. **Financial Statements Examination**: Analyzing the components of financial statements, such as the income statement, balance sheet, and cash flow statement, to understand the financial health and performance of a company. Analysis may also be organized according to operating, investing or financing acitivities.

10. **Credit Risk Assessment**: Evaluating the creditworthiness of borrowers by analyzing factors such as credit history, financial stability, and debt levels to assess the risk of default.

11. **Collateral Evaluation**: Assessing the value and quality of collateral pledged by borrowers to secure loans, ensuring it provides adequate security in case of default.

#### Guiding Questions

Examples of questions guiding low level financial analyses are:

1. **Liquidity Analysis**:
   - How does the company's current ratio and quick ratio compare to industry benchmarks?
   - Is the company's cash position sufficient to meet short-term obligations?
   - What trends do we observe in the company's working capital management over time?

2. **Profitability Analysis**:
   - How does the company's return on assets (ROA) and return on equity (ROE) compare to competitors?
   - Are there any notable trends in the company's gross profit margin and net profit margin?
   - How has the company's profitability been impacted by changes in revenue, expenses, or pricing strategies?

3. **Efficiency Analysis**:
   - What is the company's inventory turnover ratio, and how does it compare to industry averages?
   - Are there opportunities to improve the company's accounts receivable turnover and accounts payable turnover?
   - How efficient is the company in utilizing its assets to generate sales and profits?

4. **Solvency Analysis**:
   - What is the company's debt-to-equity ratio, and how has it evolved over time?
   - Is the company able to cover its interest payments comfortably, as indicated by its interest coverage ratio?
   - What is the company's financial leverage, and how does it compare to industry peers?

5. **Cash Flow Analysis**:
   - How consistent is the company's operating cash flow, and is it sufficient to support ongoing operations and investments?
   - What are the main drivers of the company's cash flow from operations, investing activities, and financing activities?
   - How does the company manage its capital expenditures and working capital needs?

6. **Risk Analysis**:
   - What are the key risks facing the company, both internally and externally?
   - How sensitive is the company's financial performance to changes in interest rates, exchange rates, or commodity prices?
   - Are there any regulatory or compliance risks that could impact the company's operations or financial results?

7. **Valuation Analysis**:
   - What is the company's intrinsic value based on discounted cash flow (DCF) analysis or comparable company analysis (CCA)?
   - How does the company's current market valuation compare to its intrinsic value?
   - Are there any factors that could lead to a revaluation of the company's stock price in the future?


### High Level Analysis

On the other hand, high level analyses may be classified according to the various key roles of the actors that interact with the company in the course of its operations and their concerns:

- **Investors**: investment analysis, equity analysis, valuation

- **Creditors**: credit analysis

- **Management**: strategies for sustainable growth

- **Clients**: viability and going concern

- **Regulators and Auditors**: transparency and accuracy

#### Guiding Questions

Some questions that guide high level financial analysis are:

1. **Strategic Positioning**:
   - What are the company's long-term strategic goals, and how does its financial performance support these objectives?
   - How does the company differentiate itself from competitors, and what impact does this have on its financial performance?

2. **Market Trends and Opportunities**:
   - What are the key trends and developments in the company's industry, and how do these trends affect its financial outlook?
   - Are there emerging market opportunities or threats that could impact the company's financial performance in the future?

3. **Capital Allocation and Investment Strategy**:
   - How does the company allocate capital across different business units or investment opportunities, and what is the rationale behind these decisions?
   - What is the company's approach to evaluating and prioritizing investment projects, and how does this impact its long-term financial sustainability?

4. **Financial Risk Management**:
   - How does the company identify, assess, and mitigate financial risks such as market risk, credit risk, and operational risk?
   - What is the company's overall risk appetite, and how does it balance risk and return in its financial decision-making processes?

5. **Shareholder Value Creation**:
   - How does the company create value for its shareholders, and what metrics or key performance indicators (KPIs) are used to measure shareholder value?
   - What strategies does the company employ to enhance shareholder returns, such as dividend payments, share buybacks, or organic growth initiatives?

6. **Sustainability and ESG Factors**:
   - How does the company integrate environmental, social, and governance (ESG) factors into its financial strategy and decision-making processes?
   - What initiatives or policies does the company have in place to address sustainability challenges and stakeholder concerns?

7. **Regulatory and Compliance Considerations**:
   - How does the company navigate regulatory and compliance requirements in its financial operations, and what impact do these requirements have on its financial performance?
   - Are there any regulatory changes or upcoming legislation that could affect the company's financial outlook or operational efficiency?

8. **International Expansion and Global Markets**:
   - What is the company's strategy for international expansion, and how does it manage currency risk, geopolitical uncertainties, and cultural differences?
   - How does the company adapt its financial reporting and risk management practices to operate effectively in global markets?


### Environmental Context

Analyses often require an appreciation of the *environment* of the company. This may refer to the <u>economy</u>, or comparable companies or <u>competitors</u> from the industry. Sometimes, the environmental context is <u>event-based</u>. Common examples of this are mergers and acquisitions.

+++ {"editable": true, "slideshow": {"slide_type": ""}}

## Financial Modeling

Models in finance may be understood in various ways:

1. mathematical models employed in academic research papers for argumentation

2. mathematical models used in risk management (in particular, in pricing of derivatives-related financial products)

3. financial models on Excel

Customarily, financial modeling refers to the third sense above.

While Excel has been used for a long time for financial modeling, modeling on Excel is however not synonymous with financial modeling. First of all, the spreadsheet underlying Excel is also found in the Google Sheets product. Secondly, there is a distinction between the financial model and its implementation in software. To identify Excel modeling with financial modeling is to not recognize there is a difference here.

Financial modelling is directly applicable to reasoning with financial statements, in particular, for providing a basis to reason about the future (in the form of forecasts or expectation).

A financial model comprises the following parts:

- a history of financial statements data

- assumptions

  - projections
 
- applying the assumptions to extend the financial statements several years into the future

- making sure that the standard relationships between the following statements continue to hold in the extension

The extended financial statement is sometimes known as a pro forma financial statement. The SEC has a cautionary advice regarding the use of pro forma financial information in earnings releases: [https://www.sec.gov/rules/2001/12/cautionary-advice-regarding-use-pro-forma-financial-information-earnings-releases](https://www.sec.gov/rules/2001/12/cautionary-advice-regarding-use-pro-forma-financial-information-earnings-releases).

A financial model constructed by an analyst has a life cycle that roughly goes like this:
- constructed

- used to compute earnings estimate

- updated with the going-ons in the markets and industry (e.g. earnings conference call, changed economic outlook, etc.)

- re-adjusted earnings estimate

- used to substantiate presentation given to clients if analyst is sell-side

- withdrawn after it has served its purpose

+++ {"editable": true, "slideshow": {"slide_type": ""}}

## Financial Presentation

Presenting the result of financial analysis is a skill in itself.

How an analysis is to be, or may be presented, will depend on the kind of analysis that it is, and the type of audience that one has to communicate the analysis to.

In the context of investment analysis, particularly for the sell-side analyst, story telling is a particularly important technique. As the well-known guru in valuation and NTU professor in finance, Aswath Demodoran , puts it: 

> When I say the word 'valuation,' most of you think about models and numbers, and you're right. There are lots of models and lots of numbers. But there are three broad themes I hope to establish in these coming sessions. 
>
> The first is that valuation is simple; we choose to make it complex. 
>
> The second is that every valuation, even though it's about numbers, has a story, a narrative behind it. A good valuation is more about the story than about the numbers. 
> 
> And third, when valuations go bad, it's not because of the numbers.


Story-telling is a particularly important technique for sell-side investment analysts for these reasons:

1. **Engagement**: Storytelling captivates the audience's attention and keeps them engaged throughout the presentation. Instead of merely presenting dry facts and figures, storytelling adds a narrative element that resonates with the audience on a deeper level.

2. **Clarity**: Complex financial concepts and analysis can be difficult for non-experts to understand. By presenting information in the form of a story, investment analysts can simplify complex ideas and make them more accessible to a broader audience.

3. **Persuasion**: Effective storytelling can be persuasive, helping investment analysts convey their recommendations and persuade clients or investors to take action. By crafting a compelling narrative around their investment thesis, analysts can make a stronger case for their recommendations.

4. **Context**: Storytelling provides context and helps investors understand the broader market dynamics, industry trends, and competitive landscape shaping the investment opportunity. By placing their analysis within a narrative framework, analysts can help investors make more informed decisions.

5. **Emotion**: Stories have the power to evoke emotion, which can influence decision-making. By tapping into investors' emotions, investment analysts can create a stronger connection with their audience and motivate them to act on their recommendations.

6. **Memorability**: Stories are more memorable than dry facts and figures. By weaving their analysis into a compelling narrative, investment analysts can make a lasting impression on their audience and increase the likelihood that their recommendations will be remembered and acted upon.

Overall, storytelling enhances the effectiveness of presentations by making them more engaging, persuasive, and memorable. It allows investment analysts to communicate their analysis and recommendations in a way that resonates with their audience and drives action.

You may also witness the use of storytelling techniques in the CFA Investment Challenge, such as the presentations from the 2023 CFA Institute Research Challenge Global Finals illustrate:


### Blogs

In the context of this course, we will widen the scope of presentation to include writing.

It takes some skills over a good grasp of fundamental knowledge in companies and financial companies in order to blog well for an audience.

Here are some well known blogging personalities in investment analysis:

1. **Ben Carlson** - [A Wealth of Common Sense](https://awealthofcommonsense.com/)
2. **Morgan Housel** - [Collaborative Fund Blog](https://www.collaborativefund.com/blog/)
3. **Michael Batnick** - [The Irrelevant Investor](https://theirrelevantinvestor.com/)
4. **Barry Ritholtz** - [The Big Picture](https://ritholtz.com/)
5. **Joshua Brown** - [The Reformed Broker](https://thereformedbroker.com/)
6. **Tadas Viskanta** - [Abnormal Returns](https://abnormalreturns.com/)
7. **Jesse Livermore** - [Philosophical Economics](https://www.philosophicaleconomics.com/)
8. **Brian Lund** - [The Lund Loop](http://thelundloop.com/)
9. **Patrick O'Shaughnessy** - [Invest Like the Best](https://investorfieldguide.com/podcast/)
10. **Jason Zweig** - [The Intelligent Investor](https://www.wsj.com/news/types/investing-column)

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---

```
