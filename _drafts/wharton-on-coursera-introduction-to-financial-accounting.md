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
* 10-K: Annual report
* 10-Q: Quarterly report
* 8-K: Current report (material events)

They must be prepared in accordance with Generally Accepted Accounting Principles (GAAP). Most countries with a stock market have similar requirements. Strictly speaking, these requirements are only applicable for public companies. But banks and venture capitals require such documents from private companies for loans and investments, so they become applicable to most companies.

GAAP rules are established by delegating from US Congress -> the SEC -> Financial Account Standards Board (FASB). International Financial Reporting Standards (IFRS) are established by the IASB and are required in over 100 countries. There is a high degree of overlap in the two standards.

Management of a company is responsible for preparing financial statements. The statements are audited by the Audit Committee of the Board of Directors (provides oversight of management’s process) and external auditors (hired by the Board to "express an opinion" about whether the statements are prepared in conformity with GAAP). The SEC and other regulators take action against the firm if any violations of GAAP or other rules are found, and information intermediaries (stock analysts, institutional investors, the media) may expose or flee firms with questionable accounting.

The required financial statements are
* Balance sheet: financial position (listing of resources and obligations) on a specific date;
* Income Statement: results of operations over a period of time using accrual accounting (i.e., recognition tied to business activities);
* Statement of Cash Flows: sources and uses of cash over a period of time;
* Statement of Stockholders’ Equity: changes in stockholders’ equity over a period of time.

Statement of Cash Flows reports cash transactions over *a period of time*, with the following categories:
* **Operating Activities**, transactions related to the provision of goods or services and other normal business activities, such as wages;
* **Investing Activities**, transactions related to the acquisition or disposal of long-lived productive assets, such as machinery;
* **Financing Activities**, transactions related to owners or creditors.

Income statement reports results of operations over a period of time using accrual accounting:
* Revenues are increases in "owners' equity" from providing goods or services;
* Expenses are decreases in "owners' equity" incurred in the process of generating revenues;
* Net Income (or Earnings or Net Profit) = Revenues * Expenses. Note that **Net Income does not equal to change in cash**.

Balance sheet reports financial position (resources and obligations) on a specific date.
* **Assets** are resources owned by a business that are expected to provide future economic benefits;
* **Liabilities** are claims on assets by "creditors" (non-owners such as debtors) that represent an obligation to make future payment of cash, goods, or services;
* **Stockholders' Equity** (Owners' Equity) are claims on assets by owners of business. It consists of Contributed Capital (arises from sale of shares) and Retained Earnings (arises from operations).

# Balance Sheet Equation
Balance sheet equation, or the accounting identity, is Asset = Liabilities + Stockholders' equity. Put it plainly, it means resources equal to claims on resources by outsiders (liabilities) and owners (stockholders' equity). Key features of balance sheet equations are
* Must always balance! (Double-entry bookkeeping)
* Changes over a period between two Balance Sheets are summarized in the Income Statement, Statement of Stockholders' Equity and Statement of Cash Flows. Specifically, assets are cash and noncash assets, and change in cash is the Statement of Cash Flows. Stockholders's equity is Contributed Capital and Retained Earnings, and the change in Contributed Capital is the Statement of Stockholders' Equity and change in Retained Earnings is the Income Statement.

If we put all components of balance sheet equation together,

$$ \begin{align}\begin{aligned}
\text{Assets}=&\text{Liabilities}+\text{Stockholders' Liability} \\
\text{Stockholders' Liability}=&\text{Contributed Capital} + \text{Retained Earnings} \\
\text{Retained Earnings} =& \text{Prior Retained Earnings} + \text{Net Income} - \text{Dividends} \\
\text{Net Income} =& \text{Revenues} - \text{Expenses}
\end{aligned}\end{align}$$

We get the complete balance sheet equation:

$$\begin{align}\begin{aligned}
\text{Assets} =& \text{Liabilities} + \text{Contributed Capital} + \text{Prior Retained Earnings} \\
+& \text{Revenues} - \text{Expenses} - \text{Dividends}
\end{aligned}\end{align}$$

## Asset
An asset is a resource that is expected to provide future economic benefits (i.e. generate future cash inflows or reduce future cash outflows).  An asset is recognized when:
* It is acquired in a past transaction or exchange;
* The value of its future benefits can be measured with a reasonable degree of precision.

Examples of an asset include Account Receivable (amount of money owed by customers for purchases made on credit), Inventory (the array of finished goods or raw materials with the *intention of selling as quickly as possible at a markup*), Prepaid Rent.

## Liability
A liability is a claim on assets by “creditors” (non-owners) that represents an obligation to make future payment of cash, goods, or services. A liability is recognized when (the criteria is the same as asset):
* The obligation is based on benefits or services received currently or in the past (i.e., transactions);
* The amount and timing of payment is reasonably certain.

Examples of liability include Account Payable (amount of money due to vendors and suppliers), Income Tax Payable, Salary Payable, Debt Principal. Note that the interest becomes liability when the principal is outstanding over time.

## Stockholders' Equity
Stockholders' equity is the residual claim on assets after settling claims of creditors (= assets - liabilities). It is also called “net worth”, “net assets”, “net book value”.

Sources of Stockholders’ equity include
* Contributed capital (arises from sale of shares)
  * Common stock (par value, min value sold at initial offering)
  * Additional paid-in-capital (excess over par value)
  * Treasury Stock (stock repurchased by company)
* Retained earnings (arises from operations)
  * Accumulation of net income (revenues minus expenses), less dividends, since start of business.

Dividends are distributions of retained earnings to shareholders, and they are not an expense (not a cost of generating revenue). They're recorded as a reduction of retained earnings on the declaration date (creates a liability until payment date).

# Debit and Credit Bookkeeping
There are three fundamental bookkeeping equations:

$$
\text{Assets} = \text{Liabilities}+\text{Stockholders' Liability} \\
\text{Sum of Debits} = \text{Sum of Credits} \\
\text{Beginning account balance} + \text{Prior Increases Earnings} - \text{Decreases} = \text{Ending account balance} \\
$$

These equations must be in balance at all times! The balance sheet equation can be preserved through the use of "debits" and "credits". The definitions of Debit and Credit are:
* Debit (Dr.) = Left-side Entry
* Credit (Cr.) = Right-side Entry

The reason they are called left and right-side entries is from the complete balance sheet equation by moving the negative Expenses to the left side:

$$\underbrace{\text{Assets} + \text{Expenses}}_{Debits} = \underbrace{\text{Liabilities} + \text{Contrib. Capital} + \text{Retained Earnings} + \text{Revenues}}_{Credits}$$

Note that dividends is not in the equation. It is treated as changes in retained earnings. Rules of Debits and Credits:
* Every transaction must have at least one debit and at least one credit;
* Debits must equal credits for all transactions;
* No negative numbers are allowed.

Bookkeeping is done via accounts. The **T Account** is a visual representation of individual accounts that looks like a "T", with debits listed on the left side of the T and credits listed on the right side of the T, making it so that all additions and subtractions (debits and credits) to the account can be easily tracked and represented visually.

The normal balance of an account is the *type of balance* (debit or credit) the account carries under normal circumstances. The Account Balance is the difference between sum of debits and sum of credits for the account. Thus, the change in account balance equation:

$$\text{Beginning Balance} + \text{Increases} - \text{Decreases} = \text{Ending Balance}$$

Assets and Expenses are Debit accounts (i.e., the *normal balance* is debit). The account balance *increases* through debit, and *decreases* through credit. Beginning (Debit) Balance + Debits - Credits = Ending (Debit) Balance.

Liabilities, Stockholders' Equity, Revenues are Credit accounts (i.e., the *normal balance* is credit). The account balance *decreases* through debit, and *increases* through credit. Beginning (Credit) Balance + Credits - Debits = Ending (Credit) Balance.

The super T account below shows how accounts balances change.

![Super T Account]({{ "/assets/posts/coursera-corporate-accounting/super-t-account.png" | absolute_url }})

When transactions are entered into journals, always ask the following three questions:
* Which specific asset, liability, stockholders' equity, revenue or expense accounts does the transaction affect?
* Does the transaction increase or decrease the affected accounts?
* Should the accounts be debited or credited?

When entering them into journals, **always list Debits first and always indent Credits**, like this:

```
Dr. <Name of Account Debited> $XXX
    Cr. <Name of Account Credited> $XXX
```

The accounting cycle is done for each period:
![Accounting Cycle]({{ "/assets/posts/coursera-corporate-accounting/accounting-cycle.png" | absolute_url }})


# Revenues and Expenses
The Income Statement reports increase in shareholders’ equity due to operations over a period of time. The income statement equation is Net Income = Revenue – Expenses. Net income is also called "earnings" or "net profit". All income statement items are based on **Accrual Accounting principles**, which is the accounting recognition of revenues and expenses are tied to
*business activities*, not to *cash flows*.

* **Revenue recognition criteria**: Revenues are recognized when goods or services are provided. $$\text{Revenues} \neq \text{Cash inflows}$$.
* Matching principle: Expenses are recognized in the same period as the revenues they helped to generate. $$\text{Expenses} \neq \text{Cash outflows}$$.

Thus, $$\text{Net income} \neq \text{Net cash flow}$$.

Revenue is an increase in shareholders’ equity (not necessarily cash) from providing goods or services. The revenue recognition criteria says that revenue is recognized when both:
* It is earned (i.e. goods or services are provided) and
* It is realized (i.e. payment for goods or services received in cash or something that can be converted to a known amount of cash, e.g., invoices).

Sometimes goods and services are provided over time (not fully earned) so the upfront payment cannot be fully booked at once.

Expenses are decreases in shareholders’ equity (not necessarily cash) that arise in the process of generating revenues. Expenses are recognized when **either**:
* Related revenues are recognized (*product costs*) or
* Incurred, if difficult to match with revenues (*period costs* and unusual events)

For manufacturing, product costs are the cost of raw materials, labor, factory, etc., and will stay in inventory until the goods are sold, at which time they become expenses. Thus, product costs follow the product. Period costs are those not directly related to the manufacturing but to running the business, such as R&D, operations, sales and marketing, human resource, and top management. They are SG&A (selling, general and administrative cost).

The underlying recognition concepts are the
* Matching principle (product vs. period costs);
* Conservatism principle (unusual events): recognize anticipated losses immediately, recognize anticipated gains only when realized (anti-human nature to avoid losses).

In most time, revenue of sales comes with expenses of cost of goods sold (COGS) and they book together.

# Adjusting Entries
Adjusting entries are internal transactions that update account balances in accordance with accrual accounting prior to the preparation of financial statements.

There are two types of adjusting entries:
* Deferred Revenues and Expenses
  * Update existing account balances to reflect current accounting values
  * Cash flow in **past**; record revenue/expense **now**
* Accrued Revenues and Expenses
  * Create new account balances to reflect unrecorded assets or liabilities
  * Record revenue/expense **now**; cash flow **in future**

Deferred Expenses answer the question that *"Are there any assets that have been “used up” this period and should be expensed?"*. Examples include Prepaid Rent (rent paid in the past but property is used in this period), Prepaid Insurance, Depreciation or amortization. The journal entry includes **Dr. Expense** and **Cr. Prepaid Asset**.

Deferred Revenues answers the question that *"Are there any liabilities that have been fulfilled by delivery of goods or services that should be recognized as revenue?"* This covers the scenario where cash was received before but goods and services are delivered over time. Examples include unearned rental income, deferred subscription revenue. The journal entry includes **Dr. Unearned Revenue Liability** and **Cr. Revenue**.

Accrued Expenses answers the question that *"Have any expenses accumulated during the period that have not yet been recorded (paid)?"* These accounts are all *payable* type. Examples include Income Taxes Payable, Interest Payable, Salaries and Wages Payable. The journal entry includes **Dr. Expense** and **Cr. Payable Liability**.

Accrued Revenue answers the questions that *"Have any revenues accumulated during the period that have not yet been recorded (received)?"* These accounts are all *receivable* type. Examples include Interest Receivable, Rent Receivable. The journal entry includes **Dr. Receivable Asset** and **Cr. Revenue**.

Note that, although in theory, revenue and expenses should be recorded when the transactions happen. However, for goods and services delivered over time, it is easier to do so at the end of the period versus over time. Adjusting entries are visualized below:

![Adjusting Entries]({{ "/assets/posts/coursera-corporate-accounting/adjusting-entries.png" | absolute_url }})

The purpose of **depreciation** and **amortization** is to allocate the original cost of a long-lived asset over its useful life, so that the total cost of asset matches to the revenues it generates over its period of use. Tangible assets (physical assets) require *depreciation* and intangible assets (abstract assets) require *amortization*.

Depreciation is not deducted from the tangible asset account. Rather, it is recorded in a Contra Asset Account (XA, opposite to asset but still part of asset in the balance sheet equation) called *Accumulated Depreciation*, which has a **credit** balance and is subtracted from PP&E (property, plant, and equipment) on the balance sheet to get the "Net Book Value". The journal entry includes **Dr. Depreciation Expense** and **Cr. Accumulated Depreciation**.

Amortization is often (but not always) deducted directly from the intangible asset account. There are companies with large amount of intangible asset starting to use accumulated amortization, which works just like depreciation.

Most companies use Straight-line Depreciation, where the depreciation expense = (Original Cost – Salvage Value) / Useful Life. Compared to accelerated depreciation, which depreciates faster at earlier years, it produces smoother financial statements.
