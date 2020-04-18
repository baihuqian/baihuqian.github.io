---
layout: "post"
title: "Wharton on Coursera: Introduction to Financial Accounting"
date: "2020-04-14 20:41"
---

This is the course notes I took when studying [Introduction to Financial Accounting](https://www.coursera.org/learn/wharton-accounting/), offered by Wharton on Coursera.

* toc
{:toc}

Accounting is a system for *recording information about business transactions* to *provide summary statements of a company's financial position and performance* to *users who require such information*.

Most companies keep three sets of books:
* Financial accounting: standardized reports for external stakeholders (and what we'll focus in this course);
* Tax accounting: IRS rules for computing taxes payable;
* Managerial accounting: custom reports for internal decision making.

In US, financial reporting requirements are regulated by the SEC:
* 10 – K: Annual report
* 10 – Q: Quarterly report
* 8 – K: Current report (material events)

They must be prepared in accordance with Generally Accepted Accounting Principles (GAAP). Most countries with a stock market have similar requirements. Strictly speaking, these requirements are only applicable for public companies. But banks and venture capitals require such documents from private companies for loans and investments, so they become applicable to most companies.

GAAP rules are established by delegating from US Congress -> the SEC -> Financial Account Standards Board (FASB). International Financial Reporting Standards (IFRS) are established by the IASB and are required in over 100 countries. There is a high degree of overlap in the two standards.

Management of a company is responsible for preparing financial statements. The statements are audited by the Audit Committee of the Board of Directors (provides oversight of management’s process) and external auditors (hired by the Board to “express an opinion” about whether the statements are prepared in conformity with GAAP). The SEC and other regulators take action against the firm if any violations of GAAP or other rules are found, and information intermediaries (stock analysts, institutional investors, the media) may expose or flee firms with questionable accounting.

The required financial statements are
* Balance sheet: financial position (listing of resources and obligations) on a specific date;
* Income Statement: results of operations over a period of time using accrual accounting (i.e., recognition tied to business activities)
* Statement of Cash Flows: sources and uses of cash over a period of time;
* Statement of Stockholders’ Equity: changes in stockholders’ equity over a period of time.

Statement of cash flows reports cash transactions over *a period of time*, with the following categories:
* Operating Activities, transactions related to the provision of goods or services and other normal business activities, such as wages;
* Investing Activities, transactions related to the acquisition or disposal of long-lived productive assets, such as machinery;
* Financing Activities, transactions related to owners or creditors.

Income statement reports results of operations over a period of time using accrual accounting.
* Revenues are increases in "owners' equity" from providing goods or services;
* Expenses are decreases in "owners' equity" incurred in the process of generating revenues;
* Net Income (or Earnings or Net Profit) = Revenues – Expenses. Note that **Net Income does not equal to change in cash**.

Balance sheet reports financial position (resources and obligations) on a specific date.
* Assets are resources owned by a business that are expected to provide future economic benefits;
* Liabilities are claims on assets by "creditors" (non-owners such as debtors) that represent an obligation to make future payment of cash, goods, or services;
* Stockholders' Equity (Owners' Equity) are claims on assets by owners of business. It consists of Contributed Capital (arises from sale of shares) and Retained Earnings (arises from operations).

# Balance Sheet Equation
Balance sheet equation, or the accounting identity, is Asset = Liabilities + Stockholders' equity. Put it plainly, it means resources equal to claims on resources by outsiders (liabilities) and owners (stockholders' equity). Key features of balance sheet equations are
* Must always balance! (Double-entry bookkeeping)
* Changes over a period between two Balance Sheets are summarized in the Income Statement, Statement of Stockholders' Equity and Statement of Cash Flows. Specifically, assets are cash and noncash assets, and change in cash is the Statement of Cash Flows. Stockholders's equity is Contributed Capital and Retained Earnings, and the change in Contributed Capital is the Statement of Stockholders' Equity and change in Retained Earnings is the Income Statement.

If we put all components of balance sheet equation together,

$$ \begin{align}\begin{aligned}
\text{Assets}=&\text{Liabilities}+\text{Stockholders' Liability} \\
\text{Stockholders' Liability}=&\text{Contributed Capital} + \text{Retained Earnings} \\
\text{Retained Earnings} =& \text{Prior Retained Earnings} + \text{Net Income} – \text{Dividends} \\
\text{Net Income} =& \text{Revenues} – \text{Expenses}
\end{aligned}\end{align}$$

We get the complete balance sheet equation:

$$\begin{align}\begin{aligned}
\text{Assets} =& \text{Liabilities} + \text{Contributed Capital} + \text{Prior Retained Earnings} \\
+& \text{Revenues} – \text{Expenses} – \text{Dividends}
\end{aligned}\end{align}$$

## Asset
An asset is a resource that is expected to provide future economic benefits (i.e. generate future cash inflows or reduce future cash outflows).  An asset is recognized when:
* It is acquired in a past transaction or exchange;
* The value of its future benefits can be measured with a reasonable degree of precision.

Examples of an asset include Account Receivable (amount of money owed by customers for purchases made on credit), Inventory (the array of finished goods or raw materials), Prepaid Rent

## Liability
A liability is a claim on assets by “creditors” (non-owners) that represents an obligation to make future payment of cash, goods, or services. A liability is recognized when (the criteria is the same as asset):
* The obligation is based on benefits or services received currently or in the past (i.e., transactions);
* The amount and timing of payment is reasonably certain.

Examples of liability include Account Payable (amount of money due to vendors and suppliers), Income Tax Payable, Salary Payable, Debt Principal. Note that the interest becomes liability when the principal is outstanding over time.

## Stockholders' Equity
Stockholders' equity is the residual claim on assets after settling claims of creditors (= assets – liabilities). It is also called “net worth”, “net assets”, “net book value”.

Sources of Stockholders’ equity include
* Contributed capital (arises from sale of shares)
  * Common stock (par value, min value sold at initial offering)
  * Additional paid-in-capital (excess over par value)
  * Treasury Stock (stock repurchased by company)
* Retained earnings (arises from operations)
  * Accumulation of net income (revenues minus expenses), less dividends, since start of business.

Dividends are distributions of retained earnings to shareholders, and they are not an expense (not a cost of generating revenue). They're recorded as a reduction of retained earnings on the declaration date (creates a liability until payment date).
