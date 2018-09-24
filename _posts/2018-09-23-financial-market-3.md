---
layout: "post"
title: "Financial Market - 3"
date: "2018-09-23 13:29"
---

This is third of the note series I took when studying for [Financial Markets](https://www.coursera.org/learn/financial-markets-global/home/welcome) by Prof. Robert Shiller, offered by Coursera. This covers the content for Week 3.

This module explores concepts of stocks, bonds, dividends, shares, and market caps.

# Bonds and Interest
Term is the minimum amount of time that you have to leave your money in and cannot get it out at least without a penalty.

Federal funds rate: shortest-term interest rate in the U.S. (i.e. overnight rate).

EONIA (European Over Night Index Average, the European counter part to Fed funds) is negative at the moment. Why is interest rate negative, as cash pays zero interest? It is because of cost of storing cash of large volume, and banks do not hold onto large amount of cash.

## Causes of Interest Rates
Eugen von Bohn-Bawerk: Capital and Interest, 1884: interest rate is close to the rate of technological progress, people's time preference of having it now versus later, and thus the return in the future must be higher than present value, and advantages from delayed but more productive manufacture processes.

## Compound Interest
If annual rate is $$r$$, compounding once per yaer, balance is $$(1 + r)^t$$ after $$t$$ years.

If compounded $$n$$ times per year, balance is $$(i + \frac{r}{n})^{nt}$$ after $$t$$ years.

Continuous compounding, balance is $$e^{rt}$$.

## Discount Bonds
Bonds typically pay coupons. Coupon is an old word. The origin of the term "coupon" is that bonds were historically issued in the form of bearer certificates. Physical possession of the certificate was proof of ownership. Several coupons, one for each scheduled interest payment, were printed on the certificate. At the date the coupon was due, the owner would detach the coupon and present it for payment (an act called "clipping the coupon").

Discount bonds (or zero-coupon bond) provide no coupon payments, just principal at maturity date (conventionally, $100). They are initially sold at a discount (less than $100) and price rises through time, creating income.

If the term of a discount bond is $$T$$, and the yield to maturity (YTM) is $$r$$ annually, then the initial value of the bond is $$\frac{1}{(1+r)^t}$$ if compounded annually, or $$\frac{1}{(1+r/2)^{2t}}$$ as bond typically pay interest every six months.

## Present Discounted Value (PDV)
We can calculate the present discounted value of any kind of assets, not just discount bonds. For example, the PDV of a dollar in $$n$$ years is $$\frac{1}{(1+r)^n}$$. PDV of a stream of payments $$x_1, ..., x_n$$ is $$\sum_{i=1}^{n}\frac{x_i}{(1+r)^i}$$.

For conventional coupon-carrying bonds, which is issued at par ($100) and coupons every six months of annualized amount $$c$$, and time-to-maturity (i.e. term) is $$T$$, then the PDV is

$$P_T = \sum_{i = 1}^{2T} \big(\frac{c}{2} \cdot \frac{1}{(1+r/2)^i}\big) + \frac{100}{(1+r/2)^{2T}} = \frac{c}{2}\big(\frac{1}{r/2}-\frac{1}{(1+r/2)^{2T}}\cdot\frac{1}{r/2}\big)+\frac{100}{(1+r/2)^{2T}}$$

If $$T$$ is infinite, the PDV of the bond is $$\frac{c}{r}$$. In other words, the yield-to-maturity of a bond is $$r=\frac{c}{P}$$. One important thing about bond is that the coupon is fixed at the time the bond is issued, but the market price $$P$$ can fluctuate. When the market price goes up, the yield of the bond decreases. Thus, bond bears market risk besides default risk.

## Consol and Annuity
Consol pays constant quantity $$x$$ forever. It's equivalent to a conventional coupon-carrying bonds with infinite term, which has a PDV of $$\frac{x}{r}$$. Growing consol pays $$x(1+g)^{t-1}$$ in year $$t$$, and its PDV is $$\frac{x}{r-g}$$.

Annuity pays $$x$$ from time $$1$$ to $$T$$. Then its PDV is $$x\frac{1-\frac{1}{(1+r)^T}}{r}$$.

## Forward Rates
Forward rates are interest rates that can be taken in advance using term structure. For example, suppose I in 1925 expect to have $100 to invest in 1926, but want the money back by 1927. I'd like to guarantee the interest rate of the $100 investment today. In other words, I would like to lock in the interest rate of 1-year period from 1926 to 1927 in 1925.

* In 1925, buy in 2-year discount bonds maturing at $100 in 1927 of amount $$\frac{(1+r_2)^2}{(1+r_1)}$$, and the cost is $$\frac{1}{1+r_1}$$.
* In 1925, short one unit of 1-year discount bonds maturing at $100 in 1926 to receive $$\frac{1}{1+r_1}$$.

Then, I pay $0 in 1925 and $100 in 1926 and receive principal payback in 1927. Thus I have now locked in the interest rate $$1+f=\frac{(1+r_2)^2}{1+r_1}$$ between 1926 and 1927.

Thus, the forward rate $$f_k$$ between $$k-1$$ and $$k$$ year in the future is $$1+f_k=\frac{(1+r_k)^k}{(1+r_{k-1})^{k-1}}$$, where $$r_k$$ is YTM of the $$k$$-year discount bond.

## Inflation and Interest Rates
The real interest rate is the nominal rate adjusted by inflation, i.e. consumer price index.

$$ 1+ r_{nominal} = (1+r_{real})(1+i)$$.

The simplified term is $$r_{nominal} \approx r_{real} + i$$. It is missing the the cross-product term $$2r_{real}i$$ which is close to 0.

Indexed bonds are bonds with coupons tied to inflation rate.

## Leverage
Leverage increases risk.

Irving Fisher's Debt Deflation Theory: Deflation redistributes real wealth from debtors to creditors. Because creditors tend to be more cautious (i.e. less open to risk), then deflation awards people with lower risk.

# Stocks
Market Capitalization is the price per share multiplied by the number of shares. To define what the stock is, we need to first think about the corporation.

## Corporation
The word corporation comes from the Latin word corpus, meaning body. A corporation is an organization that is incorporated, meaning it is made as if it has a body, as if it is a person. In fact, the word person in law typically includes corporations, whereas individuals are referred as natural person. The corporation means a body corporate legally authorized to act as a single individual, an artificial person created by royal charter, prescription, or act of legislature, and having authority to preserve certain rights in perpetual succession.

The modern corporation in the U.S. (specifically those of for-profit companies) are governed by a board of directors elected by its shareholders. The board of directors votes for the president. The CEO is hired by the board of directors.

For-profit corporation is owned by its shareholders, who have equal claims to the profit of the corporation after debts paid, subject to corporate profits tax. Non-profit is not owned, has self-perpetuating directors, and is not subject to corporate profits tax. For-profit exists to benefit its shareholders, and non-profit does not. Thus, for-profit has a price per share but non-profit does not. Ideally, for-profit has value only because the company is dedicated to advancing the shareholder, either through dividends or through share repurchase.

Non-profit, though the name is misleading, does not mean that it don't make profits. It means that it does not distribute profits to shareholders. Non-profit can make profits but the profit has to be kept in the company and spent on some defined purposes. Non-profit must have defined purposes. Otherwise, it will just sit on the accumulated profits forever.

## Shares and Dividends
The ownership of a company equals number of shares owned divided by total number of shares. Thus, share splits are essentially meaningless. The reason behind share split is the ease for people to buy it as they cannot buy fractional shares.

In the U.S., a corporation must be incorporated in a certain state. The state law defines the rights and responsibilities of shareholders and board of directors.

A dividend is a distribution of money from the company's earnings to its shareholders. There are two types of returns in stock investment: capital gains and dividends. In the long-term, dividends are the reason to invest in stocks. Ideally, if the company pays a dividend, the value of share should go down by the amount of the dividend per share. Thus, the company must define when and to whom the dividend (usually quarterly) is paid. The date is called ex-dividend date, and company values drop on the ex-dividend date.

## Common vs. Preferred Stock
Preferred stock has a specified dividend which does not grow through time. And like common stock, it does not have to be paid. In other words, the company is supposed to pay out a fixed dividend to the preferred stockholders, but it does not have to. However, it cannot pay a common stock dividend until it has paid up its preferred stock dividends.

Although preferred stock has defined dividend pay-out like bonds, it is different from corporate bonds. Company is contractually obligated to pay coupons and principal on the maturity date for corporate bonds, but does not have to pay out preferred stock dividends on time.

## Corporate Charter
The basic corporate charter says all common shareholders equally. Here equality means that if the corporation pays out dividends, every share gets the same amount. And that's where the word equity comes in; it's equality of shareholders. thus The word equity refers to common stock.

The charter does not say that the firm ever has to raise debt, pay dividends, repurchase shares (another way to increase shareholder value), or issue warrants (a form of option), convertible debt or anything else; the board decides so. But the shareholders elect the board.

There are criticisms to shareholder democracy, noticeably by Berle and Means, as ownership is so widely scattered that working control can be maintained with but a minority interest. This leads to separation of ownership and control. This leads to regulatory effort to improve shareholder democracy. The Securities Exchange Act of 1934 established rules for proxy contests, which allows outside parties to solicit proxies from individual shareholders (i.e. ask for the permission to vote on their behalf) so that they can influences corporate governance.

## Classes of Shares
As permitted by state laws, companies can issue different classes of shares with different voting rights.

## Corporations Raise Money
There are mainly two ways of raising money for a corporation, apart from retaining earnings:
* Debt, either through loans or corporate debt;
* Issue new shares.

When new shares are issued, it *dilutes* existing shareholders, since they now own a smaller fraction of the company. But it creates new earning power for the company to offset that.

Firms really don't like to issue new shares, because public gives them a bad price, mistrusts management if doing so, and it is costly and difficult to issue shares. Stuart Mayers proposes "pecking order theory": firms like to raise money through retained earnings first, through borrowing second,  and equity is only at last resort.

Equity issues include issues of stock to employees via options and grants.

## Dilution
If the company sells new shares at market price, that generally does not lower the value of my shares because the company has the money and my shares worth the same amount as before.

Stock dividend, i.e. dividends paid in the form of newly-issued shares to current shareholders, does not provide any value to shareholders as the ownership of any shareholders does not change at all. The company that issues the stock dividends wishes to maintain the stock price so it creates "shareholder value", but it inflates the share price because now there are more shares against the same company.

## Share Repurchase
The opposite of dilution is share repurchase, when a firm buys its own shares in the market. The value of the firm should go down by the amount it spends, but the share does not lose value. Share repurchase reduces number of shares outstanding and each shareholder now owns a larger portion of the company.

Share repurchase is like paying dividends, but share repurchase provides tax break for investors. While tax rate on capital gains is equal to that on dividends, capital gain tax can be postponed.

Share repurchase instead of dividends has behavior finance consequences.

## Stock Price as PDV of Expected Dividends
In the long-term, investment is stocks is about dividends. The meaning of equity is the right to its dividends. By this logic, the price of a stock is the PDV of all future dividends. With Gordon Model, we have $$P/E=1/(r-g)$$. The efficient markets theory purports to explain why P/E varies across stocks in terms of $$r$$ and $$g$$. Lower p/E does not mean that the stock is a "bargain", it only means that earnings are rationally forecasted to decrease in the future (i.e. low $$g$$) or that risk is high (i.e. high $$r$$).

Value investing instructs to invest in only low P/E stocks.

## Why Do Firms Pay Dividends?
Even when there was a stronger tax advantage to capital gains, firms still paid dividends. Why?
* A lot of conservative investors live off dividends. Their rule of thumb is not to dip into the principal.
* Framing effect: dividends are framed as income.
* University endowments once required high-yield investments to provide income.
* Dividend signaling (doing something not for the intrinsic value, but to prove something about yourself): firms shows it can court bankruptcy by raising dividends.
* People don't believe in share repurchase compared to dividends.

The firm must balance between dividends and earnings, as earnings can vary widely but dividends should rise only steadily so that the company does not need to cut it in the future.
