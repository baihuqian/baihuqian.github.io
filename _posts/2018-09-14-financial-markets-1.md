---
layout: post
title: "Financial Markets - 1"
date: '2018-09-14 20:55'
---

This is first of the note series I took when studying for [Financial Markets](https://www.coursera.org/learn/financial-markets-global/home/welcome) by Prof. Robert Shiller, offered by Coursera. This covers the content for Week 1.

This module covers the basics of financial markets, insurance, and CAPM (Capital Asset Pricing Model).

A fundamental principle of human life is collaboration. There aren't that many things that you can do as an individual that are useful, you have to join an organization. When you join a enterprise or an organization, you have to do things for the organization and you have to take a different perspective. You have to help manage a productive venture that involves many people. So this is a course about understanding how the institutions work and how we can predict what will happen.

Behavior finance is more important than what people might think. The market involves people, and the human nature is interesting to study.

Finance is a technology. Technologies can be used and misused.

# Risk and its Measure
## VaR
VAR in finance means two things:
1. Variance;
2. Value at risk.

When "A" is not capitalized, it means only "value at risk". It appears after the stock market crash of 1987.

It's a measure to quantify risk of of an investment or of a portfolio and it's quoted in units of dollars for a given probability and time horizon. For example, if it says 1% one-year VaR of 10 million, it means that there is a 1% chance that the portfolio will lose 10 million in one year.

## Stress Test
The term "Stress Test" is originally referred to a medical procedure to test for cardiovascular fitness. It now refers to procedures to test the risks associated with financial firms or portfolios. The idea of a stress test is that, let's look at a portfolio not just by its historical returns and how variable they are, but let's look at the details of the portfolio and ask what vulnerabilities there are for various kinds of financial crisis. Because what actually stresses firms the most are crisis, which is not just normal variation but some extreme events.  

The stress test is usually ordered by government to see how some firm will stand up to a financial crisis. For example, the Dodd-Frank Act of 2010 requires Federal Reserve to do annual stress tests for nonbank financial institutions it supervises for at least three different scenarios, e.g. financial crisis, sudden change in exchange rates of the dollar, or short-term liquidity crisis. EU, UK, China, etc. all perform stress tests now.

## Market Risk vs Idiosyncratic Risk
The risk of an investment can be described as the variation in returns. If we plot returns for an asset class (e.g. a stock) and the entire market on a 2D plane, and fit them with a straight line, we can get

$$ y = \beta \cdot x + \alpha $$

where $$x$$ is the market return, $$y$$ is the return of the asset class.

The CAPM implies that the expected return of an asset class is determined from its beta. **Beta** is the regression slope coefficient when the return on the asset class is regressed on the return of the market.

So the variance of the return on an asset class is equal to it's beta squared times the variance of the market return and that's called systematic risk, plus the variance of the residual and the regression, or the idiosyncratic risk.

Slope beta tells how much a particular asset class co-moves with the market and thus as a measure of the systematic risk. The idiosyncratic risk is the risk that the point will lie above or below that line. And you can see, there's a lot of idiosyncratic risk for a particular asset class.

## Distribution and Outliers
In life, many things follow the normal distribution. Normal distribution implies that it is highly unlikely, or close to impossible for large deviation from the mean to happen. However, outliers exist in the stock market. For example, if we use Cauchy Distribution, which contains a fat tail, large deviations can be observed from random sampling.

Central Limit Theorem implies that average of large number of independent identically distributed shocks (whose variation is finite) are approximately normally distributed. However, it can fail if the underlying shocks are fat tailed, or the underlying shocks lose their independence.

## Covariance
Covariance quantifies correlation between two asset classes. A covariance of 0 means they are independent; positive number means they tend to move towards the same direction; and negative number means they tend to move towards opposite direction.

Covariance can be used to quantify beta:

$$ \beta_i = \frac{Cov(r_i, r_{market})}{VAR(r_{market})}$$

So the average $$\beta$$ of individual asset classes that makes up the market must be 1 because covariance to itself is the variance.

# Insurance
Risk pooling is the source of all values in insurance. If $$n$$ policies, each has *independent* probability $$p$$ of a claim, then the number of claims follows binomial distribution. The standard deviation, i.e. risk, of the fraction of policies that result in a claim is $$\sqrt{p(1-p)/n}$$. By the law of large numbers, pooling large number of risks together is no longer risky anymore (variance approaches to 0).

Sometimes this assumption does not work, due to two reasons:
1. Moral hazard, meaning people taking more risk knowing they're insured. It is partially dealt with deductions and co-insurance.
2. Selection bias, meaning the insurance company may not know all the factors related to risks compared to its customers, e.g. health insurance attracts more to patients. It is dealt with by group policies, by testing and referrals, and by mandatory government insurance.

Insurance is regulated at local level, at least in the US. Insurance companies set up state charter to do business in different states, and there're no national insurance companies.

Health insurance is designed to overcome moral hazard problem of doctors earning fees for procedures make more money if people are sick. HMO (Health Maintenance Organization) doctors are salaried, each patient has a "primary" who serves as gatekeeper.

Insurance industry is heavily regulated to enforce both insurance company and people to do the "right thing". For example, flood insurance became mandatory so that people can stop building houses on flood plains because the mandatory flood insurance costs a lot.

# Portfolio Management
An alternative to insurance in order to reduce risk is through diversification. You have to take on risks in order to get a good investment, so risk is inherent to investing.

All that should matter to an investor is the performance of the entire portfolio, and mean and variance of portfolio matter.

Capital Asset Pricing Model (CAPM) asserts that all investors hold their optimal portfolio. Due to the consequence of [the mutual fund theorem](https://en.wikipedia.org/wiki/Mutual_fund_separation_theorem), all investors hold the same portfolio of risky assets, the tangency portfolio. Therefore, the CAPM says that the tangency portfolio equals the market portfolio.

Individual investors are too small to buy many asset classes in the market, thus investment companies act as providers of diversification.

Due to diversification, investor cares about systematic risk (beta), not the idiosyncratic risk, because idiosyncratic risks from different asset classes cancel out.

Equity Premium Puzzle: in the past 200 years, the inflation-adjusted return of equity (stocks) was 6.6% a year, while short-term government bond was only 2.7% a year. So the premium of stocks over short term saving vehicles over 200 years was 3.9%. Then why does anyone invest in the short term government bonds?

Under the CAPM assumptions with the optimal portfolio owned by everyone, the return of an asset class can be computed as follows:

$$r_i = r_f + \beta_i(r_{market} - r_f)$$

where $$r_f$$ is the risk-free return. This means higher return is associated with higher beta, which means higher risk.

## Optimal Portfolio
Assume there is a risky asset with expected return $$r_1$$, and a risk-free asset of return $$r_f$$. By investing $$x$$ into risky asset:

* Expected return: $$r = xr_1 + (1 - x)r_f$$;
* Variance: $$x^2var(r_1)$$;
* Standard deviation: $$\sigma = \mid \frac{r-r_f}{r_1-r_f} \mid \sigma(r_1)$$.

The insight here is that the standard deviation of the portfolio is linear to the expected return of the portfolio. One can achieve any kind of expected return by exposing to risk. One way to do so is to short the risk-free asset and invest the proceeds in the risky asset. By doing so the expected return will increase at the cost of elevated standard deviation.

Assume there are two risky assets with expected return $$r_1$$ and $$r_2$$ respectively. By investing $$x$$ into the first asset:

* Expected return: $$r = xr_1 + (1 - x)r_2$$;
* Variance: $$x^2var(r_1) + (1 -x )^2var(r_2) + 2x(1 - x)cov(r_1, r_2)$$.

## Short Sale
In order to own a negative quantity of an asset class, one could you use short sale: borrow the asset from brokerage and sell it, escrow the proceeds. Then you receive the proceeds and owe the asset class.

However, in the tangency portfolio of CAPM there is not shorted asset class. The reason is that everyone holds the same tangency portfolio and there is no one to provide the shares to short.

## Efficient Portfolio Frontier
Efficient portfolio frontier expresses the relationship between the standard deviation of the portfolio and the return of the portfolio.

The section below the minimum variance point is called dominated, and you don't ever want to be there. The dominated section means that there exists a portfolio of the same asset classes with the same risk but higher returns.

## Gordon Growth Model
Gordon Growth Model, also known as Dividend Discount Model, is a method for calculating the intrinsic value of a stock, exclusive of current market conditions. Assuming there is an asset which yields a return (e.g. dividend) of $$x$$ amount in a year, and yearly growth rate of the return is $$g$$ (could be negative). The future yield of this asset will be $$x(1+g)$$ after year 2, $$x(1+g)^2$$ after year 3, etc.

Gordon growth model computes the present value of this asset by discounting all future yields into current value:

$$ PV = \frac{x}{1+r} + \frac{x(1+g)}{(1+r)^2} + \frac{x(1+g)^2}{(1+r)^3} + ... = \frac{x}{r-g}$$

Where $$r$$ is the discount rate, which can be riskless interest rate if the asset is riskless (i.e. $$g$$ is fixed). $$r$$ will be higher if there is risk in the asset.
