---
layout: "post"
title: "Financial Markets"
date: "2018-10-16 20:57"
tags:
 - Finance
full-width: true
---

This is the note series I took when studying for [Financial Markets](https://www.coursera.org/learn/financial-markets-global/home/welcome) by Prof. Robert Shiller, offered by Coursera.

* TOC
{:toc}

# Module 1

This module covers the basics of financial markets, insurance, and CAPM (Capital Asset Pricing Model).

A fundamental principle of human life is collaboration. There aren't that many things that you can do as an individual that are useful, you have to join an organization. When you join a enterprise or an organization, you have to do things for the organization and you have to take a different perspective. You have to help manage a productive venture that involves many people. So this is a course about understanding how the institutions work and how we can predict what will happen.

Behavior finance is more important than what people might think. The market involves people, and the human nature is interesting to study.

Finance is a technology. Technologies can be used and misused.

## Risk and its Measure
### VaR
VAR in finance means two things:
1. Variance;
2. Value at risk.

When "A" is not capitalized, it means only "value at risk". It appears after the stock market crash of 1987.

It's a measure to quantify risk of of an investment or of a portfolio and it's quoted in units of dollars for a given probability and time horizon. For example, if it says 1% one-year VaR of 10 million, it means that there is a 1% chance that the portfolio will lose 10 million in one year.

### Stress Test
The term "Stress Test" is originally referred to a medical procedure to test for cardiovascular fitness. It now refers to procedures to test the risks associated with financial firms or portfolios. The idea of a stress test is that, let's look at a portfolio not just by its historical returns and how variable they are, but let's look at the details of the portfolio and ask what vulnerabilities there are for various kinds of financial crisis. Because what actually stresses firms the most are crisis, which is not just normal variation but some extreme events.  

The stress test is usually ordered by government to see how some firm will stand up to a financial crisis. For example, the Dodd-Frank Act of 2010 requires Federal Reserve to do annual stress tests for nonbank financial institutions it supervises for at least three different scenarios, e.g. financial crisis, sudden change in exchange rates of the dollar, or short-term liquidity crisis. EU, UK, China, etc. all perform stress tests now.

### Market Risk vs Idiosyncratic Risk
The risk of an investment can be described as the variation in returns. If we plot returns for an asset class (e.g. a stock) and the entire market on a 2D plane, and fit them with a straight line, we can get

$$ y = \beta \cdot x + \alpha $$

where $$x$$ is the market return, $$y$$ is the return of the asset class.

The CAPM implies that the expected return of an asset class is determined from its beta. **Beta** is the regression slope coefficient when the return on the asset class is regressed on the return of the market.

So the variance of the return on an asset class is equal to it's beta squared times the variance of the market return and that's called systematic risk, plus the variance of the residual and the regression, or the idiosyncratic risk.

Slope beta tells how much a particular asset class co-moves with the market and thus as a measure of the systematic risk. The idiosyncratic risk is the risk that the point will lie above or below that line. And you can see, there's a lot of idiosyncratic risk for a particular asset class.

### Distribution and Outliers
In life, many things follow the normal distribution. Normal distribution implies that it is highly unlikely, or close to impossible for large deviation from the mean to happen. However, outliers exist in the stock market. For example, if we use Cauchy Distribution, which contains a fat tail, large deviations can be observed from random sampling.

Central Limit Theorem implies that average of large number of independent identically distributed stocks (whose variation is finite) are approximately normally distributed. However, it can fail if the underlying stocks are fat tailed, or the underlying stocks lose their independence.

### Covariance
Covariance quantifies correlation between two asset classes. A covariance of 0 means they are independent; positive number means they tend to move towards the same direction; and negative number means they tend to move towards opposite direction.

Covariance can be used to quantify beta:

$$ \beta_i = \frac{Cov(r_i, r_{market})}{VAR(r_{market})}$$

So the average $$\beta$$ of individual asset classes that makes up the market must be 1 because covariance to itself is the variance.

## Insurance
Risk pooling is the source of all values in insurance. If $$n$$ policies, each has *independent* probability $$p$$ of a claim, then the number of claims follows binomial distribution. The standard deviation, i.e. risk, of the fraction of policies that result in a claim is $$\sqrt{p(1-p)/n}$$. By the law of large numbers, pooling large number of risks together is no longer risky anymore (variance approaches to 0).

Sometimes this assumption does not work, due to two reasons:
1. Moral hazard, meaning people taking more risk knowing they're insured. It is partially dealt with deductions and co-insurance.
2. Selection bias, meaning the insurance company may not know all the factors related to risks compared to its customers, e.g. health insurance attracts more to patients. It is dealt with by group policies, by testing and referrals, and by mandatory government insurance.

Insurance is regulated at local level, at least in the US. Insurance companies set up state charter to do business in different states, and there're no national insurance companies.

Health insurance is designed to overcome moral hazard problem of doctors earning fees for procedures make more money if people are sick. HMO (Health Maintenance Organization) doctors are salaried, each patient has a "primary" who serves as gatekeeper.

Insurance industry is heavily regulated to enforce both insurance company and people to do the "right thing". For example, flood insurance became mandatory so that people can stop building houses on flood plains because the mandatory flood insurance costs a lot.

## Portfolio Management
An alternative to insurance in order to reduce risk is through diversification. You have to take on risks in order to get a good investment, so risk is inherent to investing.

All that should matter to an investor is the performance of the entire portfolio, and mean and variance of portfolio matter.

Capital Asset Pricing Model (CAPM) asserts that all investors hold their optimal portfolio. Due to the consequence of [the mutual fund theorem](https://en.wikipedia.org/wiki/Mutual_fund_separation_theorem), all investors hold the same portfolio of risky assets, the tangency portfolio. Therefore, the CAPM says that the tangency portfolio equals the market portfolio.

Individual investors are too small to buy many asset classes in the market, thus investment companies act as providers of diversification.

Due to diversification, investor cares about systematic risk (beta), not the idiosyncratic risk, because idiosyncratic risks from different asset classes cancel out.

Equity Premium Puzzle: in the past 200 years, the inflation-adjusted return of equity (stocks) was 6.6% a year, while short-term government bond was only 2.7% a year. So the premium of stocks over short term saving vehicles over 200 years was 3.9%. Then why does anyone invest in the short term government bonds?

Under the CAPM assumptions with the optimal portfolio owned by everyone, the return of an asset class can be computed as follows:

$$r_i = r_f + \beta_i(r_{market} - r_f)$$

where $$r_f$$ is the risk-free return. This means higher return is associated with higher beta, which means higher risk.

### Optimal Portfolio
Assume there is a risky asset with expected return $$r_1$$, and a risk-free asset of return $$r_f$$. By investing $$x$$ into risky asset:

* Expected return: $$r = xr_1 + (1 - x)r_f$$;
* Variance: $$x^2var(r_1)$$;
* Standard deviation: $$\sigma = \mid \frac{r-r_f}{r_1-r_f} \mid \sigma(r_1)$$.

The insight here is that the standard deviation of the portfolio is linear to the expected return of the portfolio. One can achieve any kind of expected return by exposing to risk. One way to do so is to short the risk-free asset and invest the proceeds in the risky asset. By doing so the expected return will increase at the cost of elevated standard deviation.

Assume there are two risky assets with expected return $$r_1$$ and $$r_2$$ respectively. By investing $$x$$ into the first asset:

* Expected return: $$r = xr_1 + (1 - x)r_2$$;
* Variance: $$x^2var(r_1) + (1 -x )^2var(r_2) + 2x(1 - x)cov(r_1, r_2)$$.

### Short Sale
In order to own a negative quantity of an asset class, one could you use short sale: borrow the asset from brokerage and sell it, escrow the proceeds. Then you receive the proceeds and owe the asset class.

However, in the tangency portfolio of CAPM there is not shorted asset class. The reason is that everyone holds the same tangency portfolio and there is no one to provide the shares to short.

### Efficient Portfolio Frontier
Efficient portfolio frontier expresses the relationship between the standard deviation of the portfolio and the return of the portfolio.

The section below the minimum variance point is called dominated, and you don't ever want to be there. The dominated section means that there exists a portfolio of the same asset classes with the same risk but higher returns.

### Gordon Growth Model
Gordon Growth Model, also known as Dividend Discount Model, is a method for calculating the intrinsic value of a stock, exclusive of current market conditions. Assuming there is an asset which yields a return (e.g. dividend) of $$x$$ amount in a year, and yearly growth rate of the return is $$g$$ (could be negative). The future yield of this asset will be $$x(1+g)$$ after year 2, $$x(1+g)^2$$ after year 3, etc.

Gordon growth model computes the present value of this asset by discounting all future yields into current value:

$$ PV = \frac{x}{1+r} + \frac{x(1+g)}{(1+r)^2} + \frac{x(1+g)^2}{(1+r)^3} + ... = \frac{x}{r-g}$$

Where $$r$$ is the discount rate, which can be riskless interest rate if the asset is riskless (i.e. $$g$$ is fixed). $$r$$ will be higher if there is risk in the asset.

# Module 2

This module dives into some details of behavioral finance, forecasting, pricing, debt, and inflation.

## Practical Financial Innovation
### Limited Liability
In order to encourage investing, investors must be protected against failures of invested companies. Investors' liability to their invested companies should not exceed the amount they put in.

Limited Liability New York State 1811: divide up an enterprise into shares, and not shareholder is liable for more than he or she puts in.

Limited Liability was not obviously a good idea: it creates [agency problems](https://www.investopedia.com/terms/a/agencyproblem.asp) (conflict of interest inherent in any relationship where one party is expected to act in another's best interests) for shareholders, who might pursue risky strategies at the expense of bondholders.

But why Limited Liability was so successful?
* Investors' overestimation of minuscule probability of loss beyond initial investment will discourage investment.
* Lottery effect: with limited liability, an investment in a corporation was a throwaway item, like a lottery ticket.
* Allowed for investors to hold a highly diversified portfolio.

### Inflation Indexed Debt
History shows many examples of nominal debt being wiped out in real terms by high inflation. But debt indexed against real terms or commodity is protected against fluctuations in currency valuation.

Indexed debt was first attempted in Massachusetts to fund the Revolutionary War, but it didn't appear in the US until 1997. Still there is no private indexed debt today.

### Unidad de Fomento
The Unidad de Fomento (UF, literally: unit of development) is a Unit of account that is used in Chile. The exchange rate between the UF and the Chilean peso is now (today) constantly adjusted for inflation so that the value of the Unidad de Fomento remains constant on a daily basis during low inflation.

In economics, money is often defined in terms of the three functions it provides: a store of value, a unit of account and a medium of transaction. You can separate out those functions. You can then have a separate unit of account that is not money. So Chile invented something called the UF, a Unidad de Fomento, and they allowed its value to be tied to the Consumer Price Index.

### Real Estate Risk Management Devices
Value of homes is a major source of risk in real estate. There are home insurance, casualty insurance against accidents in your home, or fire insurance, but there is no protection against fluctuations of home value.

One way to protect you against collapse is to short the housing market in your own city. Chicago Mercantile Exchange offers home price futures and options. Another way is through equity-protected mortgages, which associates the remaining balance of the housing mortgages with the value of the house. If the house value decreases, the balance is corrected downwards.

Another big risk everyone bears is human capital. How to protect yourselves from the risk of your own skills? One way is to short a company in your industry, assuming it correlates with your skills. Another way is through some insurance payout when people lose their job and switch to another permanent job with lower wages.

## Market Forecasting
There are many theories to forecast the market movement.
* The efficient market hypothesis states that asset prices fully reflect all available information.
* The random walk hypothesis states that the markets movement at a time is independent from the last, thus it is impossible to forecast the market movement. $$x_t=x_{t-1} + \epsilon_t$$ where $$x_t$$ is the forecast, $$x_{t-1}$$ is the prior knowledge, and $$\epsilon_t$$ is the noise at time $$t$$ that is totally unforecastable.
* First-order autoregressive (AR-1) model: $$x_t = \bar{x} + \rho(x_{t-1} - \bar{x}) + \epsilon_t$$. With $$ -1 \lt \rho \lt 1$$, $$x_t$$ is going to revert to the mean more than pure random walk. The advice would be to stay out of the market when it's higher than the mean, and get back in the market when it is lower than the mean.

There are three variants of the efficient market hypothesis: "weak", "semi-strong", and "strong" form.
* The weak form of the EMH claims that prices on traded assets (e.g., stocks, bonds, or property) already reflect all past publicly available information.
* The semi-strong form of the EMH claims both that prices reflect all publicly available information and that prices instantly change to reflect new public information.
* The strong form of the EMH additionally claims that prices instantly reflect even hidden "insider" information.

The efficient market hypothesis forms the basis of CAPM and mathematical finance.

### Intuition of Market Efficiency
The difference in the speed of information can be exploited to make money in the market. However, thanks to technology the speed of information is much faster at cheaper cost, thus it is harder to make money from information. Thus, market must be more "efficient" to adjust to information.

There are doubts about the efficient market hypothesis after the 2008-09 financial crisis.

### Price as Present Discounted Value of Expected Dividends
According to Gordon model, if earnings equal to dividends and if dividends grow at long-turn rate of $$g$$, then $$PV = \frac{E}{r - g}$$ or $$P/E = \frac{1}{r - g}$$.

So the efficient market hypothesis must explain why P/E varies across stocks. Low P/E does not mean that the stock is a "bargain", it only means that earnings are rationally forecasted to decrease in the future. A high P/E means market expects the growth of future earnings is fast.

However, there exist differences in P/E across industries or countries that are difficult to explain.

## Behavior Finance
The research of the role of psychology in finance. Aspects of Psychology play a role in many economic institutions:
* Insurance and loss aversion
* Corporate stocks and gambling
* Bonds and money illusion
* Banks and trust
* Central banks and bubbles
* Investment banks and framing
* Exchange and sensation seeking
* Options and salience

### Prospect Theory
It was proposed by Kahneman and Tverksy in Econometrica (a most prestigious mathematical economics journal) in 1979. They replace the core theory of economics, expected utility theory, with a constructive alternative. It consists with two changes:

1. The utility function is replaced with a value function.
2. The probabilities is replaced subjective probabilities determined by a weighting function in terms of the actual probabilities.

In economics, the utility function exhibits diminishing marginal utility everywhere, so it is concaved down. The value function (a function between gain/loss and value) is indeed concaved down in the gain part. The loss part, however, is concaved up, and it is not smooth at the origin. This means people are willing to take more risk to escape the loss part.

![Value Function](https://upload.wikimedia.org/wikipedia/commons/thumb/4/4e/Valuefun.jpg/350px-Valuefun.jpg)

The other thing is, that the utility function is not usually between gains as losses in economics. Rather, your utility is determined by how much you have. The value function is about the reaction to an opportunity on any given day. So the origin of the value function forms a reference point of your utility, and the function represents the "value" of different utilities relative to that reference point.

The weight function is the relationship between the stated probability and the decision weight. In a completely rational world, it is a straight line between 0 and 1 (inclusive) because the decision weight must equal to the stated probability. In reality, it is a curve of slope less than 1, and it does not cover very small and large probabilities (i.e. close to 0 or 1). This means people either ignore it or exaggerate it when the probability is low or high.

### Overconfidence
Most people think they are better than average. Wishful thinking bias is a term of psychology that people overestimate of the probability of the things that they identify with and want to see happen.

### Cognitive Dissonance
It is a term used by social psychologists that refers to the mental conflict that occurs when one's beliefs are discovered to be wrong. People show avoidance behavior to evidences that disprove their beliefs.

### Mental Compartments
CAPM assumes the whole portfolio to be taken into consideration. Compartments refer to investors having two or more portfolios, one "safer" and one risky that they can have fun with.

### Attention Anomalies
Psychologists have long understood that you can't pay attention to everything. You have to decide what you pay attention to. A lot of people pay attention to things that are irrelevant to them, such as day-to-day movement of the stock market.

Second, there's a social basis for attention and that is that you tend to pay attention to the same things that other people pay attention. This could result in mispricing of stocks, as many investors pay attention to a specific news that may drive the stock price overly high or low.

### Anchoring
Anchoring refers to a tendency in ambiguous situations to allow one's decisions to be affected by some anchor. For example, stock prices are anchored to past values, or to other stocks in the same country.

### Representativeness Heuristic
People judge by similarity to familiar types, without regard to base rate probabilities. It refers to the tendency to see patterns in what is really random walk.

### Disjunction Effect
The disjunction effect is the inability to make decisions in advance in anticipation of future information. People don't anticipate their emotions later. So, they can't work through a decision tree correctly. This can explain the reaction of stock market to news and how people make stock strategies to trade on news.

### Magical Thinking
Discovered by B. F. Skinner in 1948, it shows repeated events can develop superstitions (strong irrational beliefs) with fallacious attributions of causal relationships between actions and events. Stock market responses to events may have similar origins in similar way.

### Quasi-Magical Thinking
It describes the cases in which people act as if they erroneously believe that their action influences the outcome, even though they do not really hold that belief. People may realize that a superstitious intuition is logically false, but act as if it were true because they do not exert an effort to correct the intuition. For example, people vote because they think their vote may have an impact of the outcome (whose probability is minuscule), or people bet more on coin not yet tossed, or pay more for lottery ticket in which they choose the number.

Newcomb’s paradox describes the case that people sometimes change their behavior when they learn about a prediction which has been made about the future.

### Culture and Social Contagion
Culture and "collective memory" influences people's thinking. There are moral anchors for the markets in the form of human stories, and people's memory is formed by stories.

### Antisocial Personality Disorder
It is a sociopathy defined in Diagnostic and Statistical Manual 5:
* Identity: egocentric, self-esteem from personal gain
* Self-direction: absence of prosocial internal standards
* Lack of empathy, incapacity for intimacy
* Manipulative, deceitful, callous, hostile
* Irresponsible, impulsive, risk-taking

# Module 3

This module explores concepts of stocks, bonds, dividends, shares, and market caps.

## Bonds and Interest
Term is the minimum amount of time that you have to leave your money in and cannot get it out at least without a penalty.

Federal funds rate: shortest-term interest rate in the U.S. (i.e. overnight rate).

EONIA (European Over Night Index Average, the European counter part to Fed funds) is negative at the moment. Why is interest rate negative, as cash pays zero interest? It is because of cost of storing cash of large volume, and banks do not hold onto large amount of cash.

### Causes of Interest Rates
Eugen von Bohn-Bawerk: Capital and Interest, 1884: interest rate is close to the rate of technological progress, people's time preference of having it now versus later, and thus the return in the future must be higher than present value, and advantages from delayed but more productive manufacture processes.

### Compound Interest
If annual rate is $$r$$, compounding once per year, balance is $$(1 + r)^t$$ after $$t$$ years.

If compounded $$n$$ times per year, balance is $$(1 + \frac{r}{n})^{nt}$$ after $$t$$ years.

Continuous compounding, balance is $$e^{rt}$$.

### Discount Bonds
Bonds typically pay coupons. Coupon is an old word. The origin of the term "coupon" is that bonds were historically issued in the form of bearer certificates. Physical possession of the certificate was proof of ownership. Several coupons, one for each scheduled interest payment, were printed on the certificate. At the date the coupon was due, the owner would detach the coupon and present it for payment (an act called "clipping the coupon").

Discount bonds (or zero-coupon bond) provide no coupon payments, just principal at maturity date (conventionally, $100). They are initially sold at a discount (less than $100) and price rises through time, creating income.

If the term of a discount bond is $$T$$, and the yield to maturity (YTM) is $$r$$ annually, then the initial value of the bond is $$\frac{1}{(1+r)^t}$$ if compounded annually, or $$\frac{1}{(1+r/2)^{2t}}$$ as bond typically pay interest every six months.

### Present Discounted Value (PDV)
We can calculate the present discounted value of any kind of assets, not just discount bonds. For example, the PDV of a dollar in $$n$$ years is $$\frac{1}{(1+r)^n}$$. PDV of a stream of payments $$x_1, ..., x_n$$ is $$\sum_{i=1}^{n}\frac{x_i}{(1+r)^i}$$.

For conventional coupon-carrying bonds, which is issued at par ($100) and coupons every six months of annualized amount $$c$$, and time-to-maturity (i.e. term) is $$T$$, then the PDV is

$$P_T = \sum_{i = 1}^{2T} \big(\frac{c}{2} \cdot \frac{1}{(1+r/2)^i}\big) + \frac{100}{(1+r/2)^{2T}} = \frac{c}{2}\big(\frac{1}{r/2}-\frac{1}{(1+r/2)^{2T}}\cdot\frac{1}{r/2}\big)+\frac{100}{(1+r/2)^{2T}}$$

If $$T$$ is infinite, the PDV of the bond is $$\frac{c}{r}$$. In other words, the yield-to-maturity of a bond is $$r=\frac{c}{P}$$. One important thing about bond is that the coupon is fixed at the time the bond is issued, but the market price $$P$$ can fluctuate. When the market price goes up, the yield of the bond decreases. Thus, bond bears market risk besides default risk.

### Consol and Annuity
Consol pays constant quantity $$x$$ forever. It's equivalent to a conventional coupon-carrying bonds with infinite term, which has a PDV of $$\frac{x}{r}$$. Growing consol pays $$x(1+g)^{t-1}$$ in year $$t$$, and its PDV is $$\frac{x}{r-g}$$.

Annuity pays $$x$$ from time $$1$$ to $$T$$. Then its PDV is $$x\frac{1-\frac{1}{(1+r)^T}}{r}$$.

### Forward Rates
Forward rates are interest rates that can be taken in advance using term structure. For example, suppose I in 1925 expect to have $100 to invest in 1926, but want the money back by 1927. I'd like to guarantee the interest rate of the $100 investment today. In other words, I would like to lock in the interest rate of 1-year period from 1926 to 1927 in 1925.

* In 1925, buy in 2-year discount bonds maturing at $100 in 1927 of amount $$\frac{(1+r_2)^2}{(1+r_1)}$$, and the cost is $$\frac{1}{1+r_1}$$.
* In 1925, short one unit of 1-year discount bonds maturing at $100 in 1926 to receive $$\frac{1}{1+r_1}$$.

Then, I pay $0 in 1925 and $100 in 1926 and receive principal payback in 1927. Thus I have now locked in the interest rate $$1+f=\frac{(1+r_2)^2}{1+r_1}$$ between 1926 and 1927.

Thus, the forward rate $$f_k$$ between $$k-1$$ and $$k$$ year in the future is $$1+f_k=\frac{(1+r_k)^k}{(1+r_{k-1})^{k-1}}$$, where $$r_k$$ is YTM of the $$k$$-year discount bond.

### Inflation and Interest Rates
The real interest rate is the nominal rate adjusted by inflation, i.e. consumer price index.

$$ 1+ r_{nominal} = (1+r_{real})(1+i)$$.

The simplified term is $$r_{nominal} \approx r_{real} + i$$. It is missing the the cross-product term $$2r_{real}i$$ which is close to 0.

Indexed bonds are bonds with coupons tied to inflation rate.

### Leverage
Leverage increases risk.

Irving Fisher's Debt Deflation Theory: Deflation redistributes real wealth from debtors to creditors. Because creditors tend to be more cautious (i.e. less open to risk), then deflation awards people with lower risk.

## Stocks
Market Capitalization is the price per share multiplied by the number of shares. To define what the stock is, we need to first think about the corporation.

### Corporation
The word corporation comes from the Latin word corpus, meaning body. A corporation is an organization that is incorporated, meaning it is made as if it has a body, as if it is a person. In fact, the word person in law typically includes corporations, whereas individuals are referred as natural person. The corporation means a body corporate legally authorized to act as a single individual, an artificial person created by royal charter, prescription, or act of legislature, and having authority to preserve certain rights in perpetual succession.

The modern corporation in the U.S. (specifically those of for-profit companies) are governed by a board of directors elected by its shareholders. The board of directors votes for the president. The CEO is hired by the board of directors.

For-profit corporation is owned by its shareholders, who have equal claims to the profit of the corporation after debts paid, subject to corporate profits tax. Non-profit is not owned, has self-perpetuating directors, and is not subject to corporate profits tax. For-profit exists to benefit its shareholders, and non-profit does not. Thus, for-profit has a price per share but non-profit does not. Ideally, for-profit has value only because the company is dedicated to advancing the shareholder, either through dividends or through share repurchase.

Non-profit, though the name is misleading, does not mean that it don't make profits. It means that it does not distribute profits to shareholders. Non-profit can make profits but the profit has to be kept in the company and spent on some defined purposes. Non-profit must have defined purposes. Otherwise, it will just sit on the accumulated profits forever.

### Shares and Dividends
The ownership of a company equals number of shares owned divided by total number of shares. Thus, share splits are essentially meaningless. The reason behind share split is the ease for people to buy it as they cannot buy fractional shares.

In the U.S., a corporation must be incorporated in a certain state. The state law defines the rights and responsibilities of shareholders and board of directors.

A dividend is a distribution of money from the company's earnings to its shareholders. There are two types of returns in stock investment: capital gains and dividends. In the long-term, dividends are the reason to invest in stocks. Ideally, if the company pays a dividend, the value of share should go down by the amount of the dividend per share. Thus, the company must define when and to whom the dividend (usually quarterly) is paid. The date is called ex-dividend date, and company values drop on the ex-dividend date.

### Common vs. Preferred Stock
Preferred stock has a specified dividend which does not grow through time. And like common stock, it does not have to be paid. In other words, the company is supposed to pay out a fixed dividend to the preferred stockholders, but it does not have to. However, it cannot pay a common stock dividend until it has paid up its preferred stock dividends.

Although preferred stock has defined dividend pay-out like bonds, it is different from corporate bonds. Company is contractually obligated to pay coupons and principal on the maturity date for corporate bonds, but does not have to pay out preferred stock dividends on time.

### Corporate Charter
The basic corporate charter says all common shareholders are equal. Here equality means that if the corporation pays out dividends, every share gets the same amount. And that's where the word equity comes in; it's equality of shareholders. thus The word equity refers to common stock.

The charter does not say that the firm ever has to raise debt, pay dividends, repurchase shares (another way to increase shareholder value), or issue warrants (a form of option), convertible debt or anything else; the board decides so. But the shareholders elect the board.

There are criticisms to shareholder democracy, noticeably by Berle and Means, as ownership is so widely scattered that working control can be maintained with but a minority interest. This leads to separation of ownership and control. This leads to regulatory effort to improve shareholder democracy. The Securities Exchange Act of 1934 established rules for proxy contests, which allows outside parties to solicit proxies from individual shareholders (i.e. ask for the permission to vote on their behalf) so that they can influences corporate governance.

### Classes of Shares
As permitted by state laws, companies can issue different classes of shares with different voting rights.

### Corporations Raise Money
There are mainly two ways of raising money for a corporation, apart from retaining earnings:
* Debt, either through loans or corporate debt;
* Issue new shares.

When new shares are issued, it *dilutes* existing shareholders, since they now own a smaller fraction of the company. But it creates new earning power for the company to offset that.

Firms really don't like to issue new shares, because public gives them a bad price, mistrusts management if doing so, and it is costly and difficult to issue shares. Stuart Mayers proposes "pecking order theory": firms like to raise money through retained earnings first, through borrowing second,  and equity is only at last resort.

Equity issues include issues of stock to employees via options and grants.

### Dilution
If the company sells new shares at market price, that generally does not lower the value of my shares because the company has the money and my shares worth the same amount as before.

Stock dividend, i.e. dividends paid in the form of newly-issued shares to current shareholders, does not provide any value to shareholders as the ownership of any shareholders does not change at all. The company that issues the stock dividends wishes to maintain the stock price so it creates "shareholder value", but it inflates the share price because now there are more shares against the same company.

### Share Repurchase
The opposite of dilution is share repurchase, when a firm buys its own shares in the market. The value of the firm should go down by the amount it spends, but the share does not lose value. Share repurchase reduces number of shares outstanding and each shareholder now owns a larger portion of the company.

Share repurchase is like paying dividends, but share repurchase provides tax break for investors. While tax rate on capital gains is equal to that on dividends, capital gain tax can be postponed.

Share repurchase instead of dividends has behavior finance consequences.

### Stock Price as PDV of Expected Dividends
In the long-term, investment is stocks is about dividends. The meaning of equity is the right to its dividends. By this logic, the price of a stock is the PDV of all future dividends. With Gordon Model, we have $$P/E=1/(r-g)$$. The efficient markets theory purports to explain why P/E varies across stocks in terms of $$r$$ and $$g$$. Lower P/E does not mean that the stock is a "bargain", it only means that earnings are rationally forecasted to decrease in the future (i.e. low $$g$$) or that risk is high (i.e. high $$r$$).

Value investing instructs to invest in only low P/E stocks.

### Why Do Firms Pay Dividends?
Even when there was a stronger tax advantage to capital gains, firms still paid dividends. Why?
* A lot of conservative investors live off dividends. Their rule of thumb is not to dip into the principal.
* Framing effect: dividends are framed as income.
* University endowments once required high-yield investments to provide income.
* Dividend signaling (doing something not for the intrinsic value, but to prove something about yourself): firms shows it can court bankruptcy by raising dividends.
* People don't believe in share repurchase compared to dividends.

The firm must balance between dividends and earnings, as earnings can vary widely but dividends should rise only steadily so that the company does not need to cut it in the future.

# Module 4

This module takes a look into the recent past, explores recessions, bubbles, the mortgage crisis, and regulation.

## Recessions
Recessions are substantially psychological, thus it is difficult for anybody, a central bank, central government to deal with them.

Inverted yield curve is a situation in which short term interest rates, like overnight rates or three-month rates, are above long-term interest rates. That's not the usual situation because obviously long term bonds are riskier because the payout is coming much later. So people generally demand a higher, and the market will give them a higher interest rate, but sometimes it's inverted. It's been shown statistically that that's a leading indicator of a recession.

### Excessive Reserves after the Financial Crisis
Government regulators impose reserve requires that banks have to keep a certain amount of cash on reserve to meet any sudden increase in withdrawals (bank runs). For most of the time in recent history, banks don't keep excessive reserve beyond what is required by regulators. However, during or after the financial crisis, we see large amount of excessive reserves held by banks.

The reasons are follows:
1. The interest rate is effectively 0%, thus reserve holding does not lose to risk-free investment opportunities.
2. Risky investment opportunities are subject to adverse selection and moral hazard problems. Lending during this period of time will only attract borrower with worse financial situations.

## Mortgage and Real Estate

### Commercial Real Estate Vehicles
#### Real Estate Partnership
Real Estate Partnership is a major example of Direct Participation Program, a class of investments that also includes oil and gas explorations and equipment leasing program. Real Estate Limited Partnership provides limited liability to partners.

Direct Participation Program are "flow-through vehicles", as profits become investors' income, thus it escapes corporate income tax. On the other hand, program losses can be claimed as deductions by investors.

IRS requires that DPP cannot be perpetual. Unlike corporations, it must have a finite life.

In order to buy in a real estate partnership, you need to be an "accredited investor", which is a term defined by Security and Exchange Commission in terms of wealth and income.

There are two types of partners in the Limited Partnership structure:
* General Partner: runs the business, must own at least 1%, and does not have limited liability. General partners or their associates runs the offering to sell units to investors.
* Limited Partner: passive investors with limited liability, rights to vote, and rights to replace general partner. They give general partner performance-based compensation.

#### REITs
Real Estate Investment Trust were created by US Congress in 1960s to allow small investors to access the real estate investment.

Before 1960, companies that own real estate were considered businesses, and thus subject to corporate income tax. So (to avoid corporate income tax,) until 1960, real estate are largely owned by partnerships, not suitable for small investors.

Now REITs become big and institutions invest in REITs too.

There are restrictions on REITs regarding their assets, income, and income payouts.

### Mortgage
The word "mortgage" comes from Latin, and it means "dead pledge". It came to English via French. Verb mortgage means committing property as a collateral for a loan.

Mortgage was difficult when property laws were not well-developed.

Mortgage market in the U.S. is big, with over 40 million mortgaged homes. Before 1930, mortgage was in a form of five-year or less loans that can be renewed or refinanced after two years. During the Great Depression this became an issue as the house was under water (i.e. outstanding loan amount is greater than the value of the house) and people were out of job. in 1934 the Federal Housing Administration was established to insure the lender from the default risk (basically the government betted on its own people). It also requires mortgages to be longer than 15 years.

The interest rate of 30-year mortgage correlates closely to the YTM of 10-year treasury bond. The reason of this correlation is because most of mortgages don't last for 30 years. They last for about 10 years. There is a spread between the two because 1) mortgages tend to be more risky than treasury even mortgages can be insured; 2) and mortgages need to be serviced.

The type of mortgages:
* conventional, fixed-rate mortgage (amortizing, long term);
* Adjustable rate mortgage;
* Price level adjusted mortgage (PLAM): payment is adjusted to inflation so it is constant in real term. This was popular when inflation and interest rate were high, like in the 1980s. PLAM may have negative amortization, meaning the payment for any period is less than the interest rate charged over the period, so that the outstanding balance of the loan increases. This is possible if the inflation rate is higher than the growth of principal (due to negative amortization), thus the real value of the mortgage still decreases.
* Dual rate mortgage (DRM) same as PLAM but interest rate floats;
* Shared appreciation mortgage, in which you pay out some of the appreciation of the house in lieu of the mortgage. It was popular in the UK before the financial crisis.
* First mortgages;
* Home equity loans, a loan based on the value of the house that you can take on later to get cash from your home.

Related terms to mortgages:
* Amortization: refers to spreading payments over multiple periods. In lending, amortization is the distribution of loan repayments into multiple cash flow installments, as determined by an amortization schedule. Each repayment installment consists of both principal and interest. Payments are divided into equal amounts for the duration of the loan, making it the simplest repayment model. A greater amount of the payment is applied to interest at the beginning of the amortization schedule, while more money is applied to principal at the end. Commonly it is known as EMI or Equated Monthly Installment.
* Balloon payment: a large payment due at the end of a balloon loan. A balloon loan typically features a relatively short term, and only a portion of the loan's principal balance is amortized over the term. At the end of the term, the remaining balance is due as a final repayment.

#### Private Mortgage Insurance (PMI)
The insurance that insures mortgages and is paid by the borrower (i.e. homeowner). Both Fannie and Freddie require mortgagors buy mortgage insurance if the down payment is less than 20%.

It draws controversy when home value increases, the mortgage balance becomes less than 80% of the home value, thus PMI is no longer needed, but mortgage lenders do not notify borrowers to cancel PMI.

### Mortgage Derivatives
#### Collateralized Mortgage Obligations (CMO)
Because mortgage has value, it can be sold to others. CMO is a pool of mortgages, sold as securities to investors, and thus the lender becomes only the servicer of the mortgages.

CMOs are divided into trenches in terms of prepayment risk, with most senior trench gets the highest payment priority over all other trenches if the mortgage defaults. In this way, CMOs can earn higher ratings than the underlying mortgages or directly-securitized mortgages.

Sequential-Pay CMOs: the first trench gets principal payments first; after it is paid off, the second trench gets principal payments.

#### Collateralized Debt Obligations (CDO)
Like a more generalized CMO, CDO holds securities, typically mortgage securities as its assets. CDOs divide the cash flow into a number of tranches in terms of *default risk*.

CDOs got into trouble during the financial crisis. It drew criticisms of rating agencies for not downgrading them (or giving them too high a rating), and its value dropped too much when the market panicked.

The problem withe CMOs and CDOs is that the mortgage originator does not care whether the mortgage it approves would default or not, and it makes more money by issuing more mortgages. Thus it is in mortgage originator's interest to approve mortgages for anybody, including those who cannot show evidence of a good record (i.e. the mortgage is called "liar's loan"). To address this problem, the European parliament requires mortgage originator must hold at least 5% of the mortgages it issues. The Dodd-Frank Act copies this idea, but provides exemption to Qualifying Residential Mortgages (QRMs), whose definition was established in 2014 (4 years later) in a complicated way.

## The Real Estate Bubble
Home price has been stable (inflation adjusted) for the past 100 years, except since 2000 when home price increased significantly, and crashed during the financial crisis.

Due to technology advancement, the building cost of homes has been falling for the last half century. If you go outside major cities, the cost of land is rather a small component of home prices, and the structure cost depreciates quite fast because of wear and going out of fashion.

Thus, in late 1990s to early 2000s, the home price is seen as always going up, resulting in a bubble.

There are evidences of feedback mechanism between expected return of real estate and home prices: when home prices go up, so does the expected return. There are correlations between home prices and number of housing units started: when home prices go up, so does start of new constructions, because constructors want to maximize their profit. Therefore, the cause and burst of the bubble has something to do with human nature.

In addition, people tend to think that house appreciation is high relative to mortgage interest rate, when home prices have been on the rise. For example, people think house will appreciate at 12% annually for the next 10 years while mortgage interest rate is at 6%, and home prices are at all time high. The spread was reduced to almost 0 when home prices collapsed.

The financial crisis is usually dated in 2007, but there were signs of bubble in the housing market in 2005 when media was discussing a home-buying mania in the American public.

The word "bubble" was first used in financial market in a stock market crash in the 1700s in France, and was widely used in academia since the financial crisis. However, bubble implies that it would pop completely at certain stage, but the market will not completely crash. It would be more appropriate to use "epidemic" as it comes and goes as well as being contagious.

## Financial Regulation
Regulation is substantially aimed at dealing with human problems, manipulation and deception, as well as making the system work better by addressing problems such as monopoly and externalities, e.g. "too big to fail".

Most regulation just prior to the financial crisis emphasized at microprudential, meaning it aimed to protect individuals from abuse. On the contrary, macroprudential is regulation to help prevent big crisis in the macro economy.

Business wants regulation, because without regulation, people are forced to do things in a competitive system that they think are bad for the society. Businesses are forced to the lowest common denominator which can be ugly. A good analogy is sport referee.

Five levels of financial regulation:
1. Within-firm regulation;
2. Trade groups;
3. Local government regulation;
4. National government regulation;
5. International regulation.

### Within-Firm Regulation
The Board of Directors acts like a regulator. Often companies have outside directors represent a broader community. Thus board of directors cannot be a full-time job. Usually companies have board of directors who have good reputations, so that they can prevent devious tricks from happening within the company.

One devious trick within a company is tunneling, expropriation by minority shareholders (figuratively, as by an underground tunnel). Tunneling is more common in civil law countries (where laws are made by parliament or congress) than in common law countries (where laws are made by congress but legal precedence set by courts act as law as well). Hence, the proportion of private and family-owned companies is higher in civil law countries. Tunneling is achieved by:

* Asset sales below market value;
* Contracts with higher cost for service;
* Excessive executive compensation;
* Loan guarantees for other corporations;
* Expropriation of corporate opportunities to outsiders;
* Dilutive share issues;
* Insider trading.

U.S. law enforces duty on board of directors to prevent tunneling. The law specifies the Duty of Care (act as a reasonable, prudent or rational person would), and Duty of Loyalty (prevent insiders from benefiting at the expense of shareholders). Common law countries give more judicial discretion to judge conformance with these duties, and so are more effective in preventing self-dealing transactions.

### Trade Groups
One example of trade groups is stock exchange. The first stock exchange in the U.S. agreed on fixed commission, and requires subscribers only to trade with other subscribers. The fixed commission is abolished in 1975.

### Local Regulation
U.S. started as a confederate of states, so state government is often referred as "local government".

Banking regulation belonged to the states until the National Banking Act of 1863. Stock regulation moved to the federal government when the SEC was founded. But corporate regulation is still at the local government level.

Blue sky laws, referring to regulations on the offering and sale of securities to protect the public from fraud, was enacted in Kansas in 1911, and was widely adopted by other states shortly after. It regulates brokers and advisers.

### National Regulation
National regulation started with SEC as part of Roosevelt's New Deal in 1934 to address the failure of local regulation. It was initially viewed by businesses as a radical, almost, socialist, institution. It is peculiar that it started in the U.S>, and imitated by other countries.

SEC Rules require every broker, stock exchange, and every security must register with SEC, but registration does not literally mean SEC approval.

#### Public vs. Private Securities
One major effort by the SEC is to define public vs. private securities. Public securities need to undergo approved process of issuance under SEC surveillance. Public companies must file quarterly public states. The EDGAR database holds filings of all public companies.

The Initial Public Offering (IPO) is an SEC procedure for going public.

#### Hedge Funds
It is for wealthy investors only, and is under SEC regulation.

Those structured as 3c1s (refers to some lines in the SEC regulation) can take no more than 99 investors, and they must be "accredited investors" as defined by the SEC (which means income of $200000 or investable assets of $1 million). Those structured as 3c7s can take 500 investors, but they must be "qualified purchasers" as defined by the SEC (which means individuals with net worth of at least $5 million or institutions with net worth of at least $25 million).

#### Insiders vs. Outsiders
Insiders are people with special access to information about a company. Inside information represents wealth, and SEC tries to define access to this wealth, by disclosure rules. Regulation FD (Full Disclosure) requires that when a company tells any material fact to an analyst, it must immediately tell the public.

Brokers and stock exchanges have built sophisticated system to detect insider trading.

#### Accounting Standards
Financial Accounting Standards Board (FASB) was officially recognized as the authoritative by SEC in 1973. Though it has the statutory right to make accounting standards, it prefers private sector to do it. FASB defines the Generally Accepted Accounting Principles (GAAP), used for EDGAR.

#### Brokerage Firms
A brokerage firm, or brokerage, is a financial institution that facilitates the buying and selling of financial securities between a buyer and a seller. It serves a clientele of investors who trade public stocks and other securities, usually through the firm's agent stockbrokers (which trades on the exchange floor). The brokerage usually holds the stock on behalf of the clients, and the shares are usually held in what's called "street name", unless the brokerage reports the names of stock holders to the company. Thus, when a brokerage firm fails, investors are at risk losing their holdings.

Securities Investor Protection Corporation (SIPC) was created by U.S. Congress in 1970 to protect customers of brokerage firms or clearing houses from failure, up to $500,000 per account, $100,000 for cash.

### International Regulation
Bank for International Settlements were created in 1930 by Hague Agreements. It is like a bank of banks, has 57 member central banks, who are national regulators. It is located in Basel, Switzerland. Basel Committee was created by the G10 in 1974 to coordinate banking regulation, and was renewed multiple times.

Financial Stability Board was created by G20 from the former Financial Stability Forum (founded by G7). It makes recommendations to the G20, which may end up adopted in many countries. It is located in Basel, Switzerland, too.

World economy dominates more and more, and so regulation will shift to international.

# Module 5

This module explores options and bond markets.

## Student Loan
When students pursue higher education, they take out a loan (i.e. issue bonds). It is also possible to switch to an equity model, where lenders gets a fraction of students' future income. It is a legitimate idea because such a loan can protect students for the outcome of the education.

## Forwards and Futures Markets
Forwards and futures are so-called derivatives. They are different from the spot market, the market for immediate delivery ("you pay the money, you get them.").

Forwards and futures are important to economics because the markets reflect time as well as quality. Thus, for a certain commodity, there is a different price for every different future delivery date.

Theoretically, there could be a future market for every kind of commodity. But in reality, some future markets are successful while some are not. There are not a good explanation to this, but it may have something to do with risk.

### Forward Contract
Forward is a contract between two parities to deliver at a future date (exercise date or maturity date) at a specified exercise price. Both sides are locked into the contract with no liquidity.

Grain is the origin of the forward contract, but there are forward contracts on other commodities, such as foreign exchange (FX) forwards. FX Forward is like a pair of zero-coupon bonds; therefore, forward rate reflects interest rates in the two currencies. Their parity is called Forward Interest Parity:

$$ \text{Forward Exchange Rate} (¥/\$) = \text{Spot Exchange Rate} (¥/\$) \times \frac{1 + r_{¥}}{1 + r_{\$}}$$

This is because to lock in the forward exchange rate, one can do the following without entering the FX Forward market:
1. Short sell the dollar amount;
2. Exchange dollars to yen at the spot exchange rate;
3. Invest yen amount into risk-free, interest-bearing asset with the same maturity date;
4. One the exercise date, close the pay back the short-sold dollar balance.

A Forward Rate Agreement, or FRA, is an agreement between two parties who want to protect themselves against future movements in interest rates or exchange rates.

$$Settlement = \frac{(L-R)\times D \times A}{(B\times 100) + L\times D}$$

where
* $$L$$: actual interest rate on contract date (which can also be in the future);
* $$R$$: contract rate;
* $$D$$: days in contract period;
* $$A$$: contract amount;
* $$B$$: 360 or 365 days (a year).

### Futures Contracts
Futures contracts are like forward contracts except that there is an intermediary, the exchange, and thus do not have to deal with each other's credits.

Futures contracts are standardized retail products, rather than custom products. There can be many different spot prices for a commodity based on different specifications, but futures contracts have very specific quantity, quality, price unit, tick size (minimum fluctuation), to improve liquidity.

Futures contracts rely on **margin calls** to guarantee performance.

>When an investor uses margin to buy ourselves securities, they are paid for using a combination of his or her own funds as well as money borrowed from a broker. The investor receives a margin call from a broker if the securities in the portfolio decrease in value past a certain point, after which the investor must either deposit more money into the account, or sell some of the assets.

The futures contract requires you to pay a settlement every day from your futures margin account, based on the change in futures market on that day. Thus, the price change in the futures market is reflected in your margin account. Thus, the risk of the futures contract is eliminated by the margin account.

First futures market began at Dojima, Osaka, Japan, in 1670s, for rice, and remained world's only futures market until 1860s.

Most futures contracts are closed out without delivery, as they are used to hedge risks in the fluctuations in the price of the commodity. Because the price of a commodity of a specification at a certain location tends to move along with futures price, people in the world can buy or sell futures contracts in Chicago Board of Trade (CBOT, now part of CME Chicago Mercantile Exchange) to insure against price change.

The futures curve, the curve between price and delivery date, tends to go upward. This nature of the futures curve is called *contango*. This means if you want delivery at a future date, it will cost you more. However, sometimes there are drops ("backwardation") in the futures curve. This is often due to expected delivery of large quantity of the commodity, such as harvest.

### Buying, Selling, and Settlement
When one "buys" a futures contract, one agrees with the exchange to a daily settlement procedure that is only loosely analogous to buying the commodity. However, one has no intention of taking delivery of the commodity. One must post initial margin with the futures commission merchant. Similar things happen on the "selling" side: seller has no intention of selling the commodity; one posts margin.

Every day, the exchange defines a price called the "settle" price, which is essentially the last trade on that day. Every day until expiration, a buyer's margin account is credited (or debited if negative) with the amount equal to the change in settle price times contract amount, thus the margin account always reflect the amount needed to buy the contract amount at the settle price.

If contract is cash settled, on the last day, the margin account is credited with the last settle price times the contract amount. If the contract is physical delivery, on the last day, buyer must receive commodity.

For example, a farmer in March is planting crop expected to yield 50000 bushels of corn. In other words, farmer is "long" 50000 bushels. Farmer "sells" 10 Chicago September corn contracts for $$\$2.335\times 50000 = \$ 116750$$, and the margin is posted. Corn products manufacturer, who plans to buy corn at harvest time, "buys" the ten contracts, posts margin, too. In September, both buyer and seller close out position. Then between March and September, changes in the margin account mean that the price of corn is effectively locked in at $2.335 per bushel for both.

### Fair Value in Futures Contract

$$P_{future} = P_{spot}(1 + r + s)$$

where $$r$$ is the interest rate, and $$s$$ is the storage cost, and thus $$r+s$$ is the cost of carry. Future price is normally above cash price (i.e. contango).

If the commodity is in storage, there is a profit opportunity (i.e. arbitrage) that will tend to drive to zero any difference from fair value. If the commodity is not in storage, then it is possible that the futures price is lower than fair value (i.e. "backwardation").

### SPI & FFR Futures
Stock Price Index (SPI) Futures (such as S&P 500 Index futures), a financial future developed much later. It is cash settled rather than physical delivery. Cash settlement makes financial futures possible, because on the last day of the contract, the margin account is settled with the exact price of the S&P 500 Index as the settle price. This makes arbitrage of futures easier, and settle price of futures on the last day to be equal to the spot price (with margin of error).

The settlement is equal to $250 times the index on the last day minus the futures price for the day before. And the fair value is $$F=P+P(r-y)$$ where $$P$$ is the stock price index, $$r$$ is the financing cost (i.e. interest rate), and $$y$$ is the dividend yield. The formula is the same if you treat $$-y$$ as the storage cost for stocks.

Federal Funds Rate (FFR) Futures was created by CBOT in 1988. The settlement price is 100 minus annualized federal funds rate, averaged over contract month. It's used widely to show expectations of actions of the Federal Open Market Committee, and there is no fair value for the futures because it is not storable.

## Options
Options contract provides a choice in the future. There are two kinds of options.
* A call option is a right to buy.
* A put option is a right to sell.

It's different from a forward contract to buy/sell something, because in a forward contract, you are not only having the option to buy/sell, you are committed to buy/sell.

When an option is bought, the buyer of the option compensates the seller, or the *writer*, of the option, because the writer of the option is now subject to the choice in the future of buyer, and so it has to be compensated for providing such a choice.

The essence of options is not that I buy the ability to vacillate, or to exercise free will. The choice one makes actually depends only on the underlying asset price. Thus, options are truncated claims on the asset.

The options contract has the following terms:
* Exercise date, or strike date: the day when option expires.
* Exercise price, or strike price: the price where you have the right to buy/sell.
* Definition of underlying asset and number of shares.

The first well-documented option came from Holland in the 1600s, but standardized options and options exchange was much later. Chicago Board Options Exchange, a spinoff from the Chicago Board of Trader in 1973, traded first standardized options.

Future exchange trades options on futures.

### Option Pricing
An option pricing has the following components:
* Ticker: the symbol of the underlying asset;
* Strike price;
* Last price: the price an option was last sold for;
* Bid price: the price to sell the option back to the market maker (i.e. the price the market maker is offering to buy back options, thus bidding);
* Ask price: the price to buy the option (i.e. the price the market maker is asking for to write an option).

### Why Options Exist
A theoretical reason is that options provide a way to trade risks. Economic theorist Kenneth Arrow argued that a major source of economic inefficiency is the absence of markets for risks. Options provide a way to trade the risk that an asset will go up or below any price, and thus, options market "completes the market", as argued by Stephen Ross, a financial theorist.

A behavioral reason is that options can serve as insurance against downwards risks on portfolio's long position, making people happier.

### Ubiquity of Options
* Limited Liability makes stocks into an option to have a share in company's growth.
* Mortgages involve an option to default in a non-recourse states where the lender can only evict borrower.

### Put-Call Parity
On the expiration date, the intrinsic value of an option is the difference between strike price and the stock price or 0:

![Option Value to Buyer](https://ebrary.net/imag/bef/corp_fin/image080.jpg)

There is a relationship between put and call option prices:

Put option price - call option price = present value of strike price + present value of dividends - price of stock, or

Price of stock = call price + pdv strike + pdv dividends - put price.

This parity equation must hold out except margin of error due to transaction cost. Otherwise, there is arbitrage opportunity.

Before the expiration date, the value of an option is always higher than the intrinsic value of the option due to the probability that it will increase in value before expiry.

![Realistic Option Value](https://upload.wikimedia.org/wikipedia/commons/1/1b/Option_value.gif)

The option price is estimated via a predictive formula such as black-Scholes or using a numerical method such as the Binomial model.

### Use Options to Hedge
To put a floor on one's holding of stock, one can buy a put on same number of shares. alternatively, one can just decide to sell whenever the price reaches the floor, even automatically using a stop-loss order. Doing the former means I must pay the option price, and doing the latter costs nothing. However, a put option gives you the choice to exercise or not, so the decision can be made after the price drop; but a stop-loss order would be executed as soon as the price drops below the limit price, so the decision must be made at the price drop. So if price drops further, a put option limits the loss; if price drops and rebound, you could choose not to execute the put option, but the stop-loss order would have been executed.

# Module 6
This module covers investment banking, underwriting processes, brokers, dealers, exchanges, and new innovations in financial markets.

## Investment Banks
Investment banks are different from commercial banks:
* Investment banks do not accept deposits, and have not been regulated as banks.
* Investment banks traditionally are not members of the Federal Reserve System, and do not normally access discount window (an instrument of monetary policy, usually controlled by central banks, that allows eligible institutions to borrow money from the central bank, usually on a short-term basis, to meet temporary shortages of liquidity caused by internal or external disruptions).
* Investment banks underwrites securities instead of making loans.

Traditional US bulge-bracket (a slang term to describe the largest and most profitable multi-national investment banks) firms have been merged with other banks (such as First Boston with Credit Suisse, Salomon Brothers with Citicorp, or Merrill Lynch with Bank of America), become a bank holding company after the Financial Crisis (regulated as a bank, such as Goldman Sachs and Morgan Stanley), or bankrupted (Lehman Brothers).

### Underwriting
Underwriting of securities is the issuance of shares and corporate debt. Investment bank is sort of like a consulting firm that provides advice on financing. Underwriter provides advice to the issuer, distribution of securities, sharing of risks of issue, and stabilization of aftermarket.

Underwriter also "certifies" the issue by putting its reputation behind the issue, which address the moral hazard problem that firms have incentive to issue shares when they know their earnings are only temporarily high. Investment banks do their best to price the new issuance correctly. Market share loss occurs to investment banks that repeatedly underprice or underprice issues.

Two kinds of offerings:
* Bought deal (synonym: Firm commitment offering): the underwriter agrees to buy all shares that are not sold.
* Best efforts: the underwriter says that if the issue is not sold, deal collapses.

The underwriting process is defined by regulation and tradition.
1. Prefiling period. The investment bank advises issuers about their choices, forms an agreement among underwriters, designates managers and agrees on fees. It also files registration statement with the SEC. A single investment bank may not be able to handle a big issue, so several investment banks form a syndicate to issue shares together.
2. Cooling-off period. The preliminary prospectus (red herring) is distributed.
3. Investment banks call prospective clients for indication of interest, perform due diligence of the issuer, and decide on offering price.
4. The underwriting syndicate agrees on how to split the offering, and produces the underwriting agreement.
5. Produce dealer agreement. Dealers purchase from underwriter at a discount from public price.
6. Decide on the effective date.
7. Investment banks support the price in the aftermarket, called "stabilization". It is a form of market manipulation by the underwriter near the time of the issue that is permitted by the SEC. During stabilization, the underwriting syndicate is legally allowed to conspire to "fix" prices in market until the entire issue is sold out.

### Initial Public Offering
An initial public offering is an offering of a shares in a company for the first time publicly. Prices tend to jump up immediately after an IPO is issued, so apparently there is money left upon the table. But it is a common practice in IPO to ensure shares can be sold quickly and prices would not drop shortly after IPO.

The long-run performance of IPOs tend to be poor. Although average IPOs earn +16% return on the first day, this return tends to be offset over the next three years (Jay Ritter).

The reason behind underpricing in IPO: Impressario Hypothesis (analogy to sellers of tickets to concerts). Underpricing creates higher demand, more interest, and attract the right people (e.g. in concerts you want young people, and in IPO you want mature, long-term investors over flippers).

### Rating Agencies
Instead of trusting investment banks on worthiness of companies, one could get the information from a rating agency. Rating Agencies are barriers to adverse selection. John L. Moody founded the first rating agency in 1909. Moody's started using letter grades, and AAA is the best rating.

Agencies would not accept money from the people they rated, but that broke down in 1970s when they found it difficult to keep up with the complexities of the securities.

Part of the financial crisis is the agency problem, as the rating agencies gave AAA to CDOs holding subprime mortgages. They are since under tighter regulations.

### Regulation Effort
The Glass-Steagall Act of 1933 after the Great Depression, created the modern concept of "investment bank". It separated commercial banks, investment banks, and insurance companies. It also created the FDIC.

In the 1990s the Glass-Steagall Act was relaxed and rescinded, which resulted in a wave of mergers among commercial banks, investment banks, and insurance companies.

Volcker Rule was incorporated into the Dodd-Frank Act that commercial banks are no longer allowed to own hedge funds or do proprietary trading.

## Professional Money Managers
Money managers are professionals who invests other people's money in stocks and bonds or other normal investable assets in the open market by taking over the management of their portfolio.

### Prudent Man
Regulation requires money managers to act like a "prudent man". The Employment Retirees Income Security Act of 1974 (ERISA) defines the prudent man rule, which requires fiduciaries to act:

>With the care, skill, prudence and diligence under the circumstances then prevailing that a prudent man acting in a like capacity and familiar with such matters would use in the conduct of an enterprise of a like character and with like aims.

Basically a prudent man would do what somebody else would do in this circumstance.

Dodd-Frank Act moved away from the prudent person rule, but use a prudential standard. The new idea is not to rely on fiduciaries to use their own judgment as to what is prudent, but to impose regulatory standards.

### Financial Advisors
Financial Advisors are anyone who advises others on the value of securities or advisability of investing, or who publishes analysis. It excludes bankers, lawyers, reporters, professors, and excludes Broker Dealers whose advice is only incidental to their business.

The National Securities Markets Improvement Act of 1996 (NSMIA) requires all advisors managing more than $30 million to register with SEC. Those managing less than $25 million must register with state securities regulator. The Act does not mention “prudent person” but does bar convicted felons from serving as an adviser.

### Financial Planners
Financial Planners perform comprehensive planning for life, rather than just picking stocks. They are not regulated, not licensed in most countries. But in the U.S., financial planners must be registered as a financial advisor first.

They are mostly regulated by associations. For example, in US, there isFinancial Planning Association.

Certified Financial Planner (CFP) designation is awarded by Certified Financial Planner Board of Standards, Inc.

### Mutual Fund and ETF
First mutual fund appeared in Holland in 1770s.

In 1920s, many investment companies existed to serve the public, but the classes of shares were set up to trick small investors. At that time, Massachusetts Investment Trust had only one class of investors, published portfolio, and could be redeemed on demand. It quickly became the model for mutual fund industry.

The definition of a mutual fund is specified by the SEC under regulation. Mutual fund cannot be bought at an exchange; it must be bought from the investment company or through "supermarket". The transaction happens at 4 pm after the market close.

An open-ended fund, meaning the creation and redemption is done through the fund company. A close-ended fund, meaning, once created (like an IPO), it circulates on the exchange and cannot be directly redeemed at the fund company. While close-ended fund can be traded during trading hours, its value can diverge from its net-asset value (NAV).

The Exchange-Traded Fund (ETF) appeared much later in 1993 with the introduction of Standard & Poor Deposit Receipt (SPDR, spiders), which invests in S&P 500 index. It is traded in exchanges. Compared to mutual fund, its management fee is low. ETF addresses the divergence of NAV of the underlying portfolio by introducing authorized participants, a group of financial firms that can automatically create and redeem units in large amount, so that any premium or discount between the ETF and its NAV can be arbitraged away.

## Brokers and Dealers
Brokers act on behalf of others as their agent for which they earn a commission.

Dealers always act for themselves, in other words, they act as principals in the transaction (seller or buyer) for which they make a markup. Dealers stand to buy and sell at posted prices, bid and asked, in order to profit from the bid-ask spread rather than commission.

A firm doing business as a broker or dealer must register with SEC as Broker-Dealer. A person can never be a broker and a dealer in the same transaction. In other words, never make both a commission and a trade on the same trade.

There are rarely any real estate dealers in the U.S., because dealers pay ordinary income tax on capital gains, and real estate is usually held for a much longer period in between to qualify for long-term capital gain if the owner is not a dealer.

Good dealers do not churn (encourage clients to buy and sell frequently), and SEC penalizes "rogue brokers" who churn.

## Exchanges
The traditional four markets are:

1. First stock exchange in the US is NYSE (1792).
2. Second market is NASDAQ which is computerized to replace "pink sheets".
3. NASDAQ small cap.
4. Large institutions trade amongst themselves without the use of a securities firm.

Exchanges provide standards and codes of ethics for broker members, standards for stocks, and they must be registered and regulated by the SEC.

There are markets to lend (not sell) stocks for short sellers.

Listings matter in the sense that people tend to trader locally-listed stocks, and stocks co-move with co-listed stocks. It is a familiarity bias.

### Limit Order Book
A book with all outstanding bid (buy) and ask (sell) orders, consist of number of shares and price per share, ordered by price. Computerized exchange made it possible for people to access them.

### Payment for Order Flow
Brokers receive order from customers. But in order to fulfill them, brokers transfer orders to a stock exchange, who executes the order to find a counter party for the trade.

However, there is someone who is willing to pay brokers to get the orders, and such payment is called payment for order flow. Thus, brokers "sell" the order flow to "crossing networks" who profit from the order flow. However, the orders may not be executed at the best price. But SEC does not forbid it, but only requires brokers to disclose it.

### Kinds of Orders
* Market order
* Limit order
* Stop loss order

Market orders can be dangerous for thinly-traded stocks. ECN may not allow market orders.

A limit order sells at any price at or above the price given. But a stop loss order sells at any price at or below the price given.

## Public Finance
Public finance is the finance of the government. The government is just another organization that confronts markets and lenders like others and it always has been so going back into the history.

### Government Debt
Default is the failure to pay. It is different from repudiation, when a government announces that the debt is never going to be paid. There is another concept called odious debt, when a new government announces that the debt from the past government is due to corruption so it is moral not to pay it. Because repudiation damages the country's credibility for future investment, more often is that governments inflate the currency so that the real value of government debt drops.

When a country defaults on its sovereign debt, usually it is solved by negotiation between creditors and the government. To prevent a hold-up problem that a government does not pay and negotiate, there's been a movement in sovereign debt towards adding what they call collective action clauses, a contract signed by investors in sovereign bonds to agree to anything that's voted by a super-majority of the other investors in the same bond.

### Government's Involvement in Corporations
Governments all around the world regulate business. Some governments will own shares in private businesses. Private businesses know they may be nationalized in the future.

One major reason behind corporate profit tax is the recognizing of limited liability. A corporation can create more loss to a society than its share value, and thus government must cover the loss in order to honor limited liability. One example is the Tokyo Electric Power Company (TEPCO) in the Fukushima disaster. The Japanese government covered the damage caused by the reactors, and owned a major stake (i.e. nationalized) in TEPCO.

Bankruptcy laws make government a shareholder in all businesses. Government may pay the damages for bankrupted companies. Chapter 7 in bankruptcy laws is for liquidation, and Chapter 11 is for reorganization.

With introduction of personal bankruptcy, every individual is like an enterprise with lenders as shareholders. The government acts as a partial shareholder because it allows individuals to take risks then declare bankruptcy if it goes badly.

### Municipal Finance
Because states in the U.S. are sovereign, there is no state bankruptcy law made by the federal government. But there are laws about bankruptcies of municipal government at the federal level.

The basic motivation behind financing with local debt besides local tax is the change in population. Sometimes there is a reasonable prospect of future population inflow. With steady population growth, cities should borrow to finance the construction to get ready for them, and the debt can be repaid by additional local tax from new population.

States have constitutional prohibitions against deficit spending on the current account. They generally adopt state income tax. States can still run deficit on capital account, so there are still state bond.

Revenue bonds: similar to equity, local government can issue revenue bonds that tie to revenue of the financed project (such as toll road). But it is only allowed when cities are doing a project that yields them improvements.

Chapter 9 defines bankruptcy for municipals.

### Government Social Insurance
Insurance is a private business, but government is involved in it. Social (government) insurance originates in Germany. Sickness insurance is instituted in 1883, accident insurance in 1884, old-age insurance in 1889. Unemployment originates in UK in 1911.

* Progressive taxes: low tax when your income drops;
* Free public education and services;
* Social security;
* Health insurance: medicare, medicaid. US is the only major developed country without a comprehensive health insurance.
* Workers compensation (US before 1920): insurance to protect employees against accidents in the workspace.

Income tax was not effective in the US until 1913 because it was difficult for the government to measure people's income due to corruption and incapacity of officers. Withholding of income tax is another important invention of income tax system to address lack of saving issues at tax time.

Survivors Insurance is government-sponsored life insurance.

# Module 7

This module includes lectures about nonprofits and corporations, and your career in finance.

## Nonprofits
The defining character of nonprofits is the non-distribution constraint. Profits are not distributed as dividends because there are no owners or shareholders. Nonprofits are tax exempt.

However, nonprofits give bonuses just as for-profits, because they have to compete with for-profits. 42% of all nonprofits have a formal executive bonus system in place (2007-2008), and the percent is increasing.

Board of trustees appoints own successors.

It may not be much different to you compared to setting up a for-profit, but it gives you benefit such as moral high ground, tax savings, etc.

## Cooperatives
Cooperatives are not non-profit in the sense that they may distribute profits. But it is not the same as corporation. The voting system for cooperatives is one person one vote.

## Benefit Corporation
This is a new corporate form, first created in Maryland in 2010. Note that securities law is at the federal level, but corporate law is still at the state level, so states must adopt them in state legislation.

A benefit corporation is halfway between for-profit and non-profit. Its company charter must state a social or environmental purpose, and is required to pursue that as well as profit.
