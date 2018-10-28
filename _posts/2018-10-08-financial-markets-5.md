---
layout: post
title: Financial Markets - 5
date: '2018-10-08 00:45'
tags:
 - "Financial Markets"
---

This is fifth of the note series I took when studying for [Financial Markets](https://www.coursera.org/learn/financial-markets-global/home/welcome) by Prof. Robert Shiller, offered by Coursera. This covers the content for Week 5.

This module explores options and bond markets.

# Student Loan
When students pursue higher education, they take out a loan (i.e. issue bonds). It is also possible to switch to an equity model, where lenders gets a fraction of students' future income. It is a legitimate idea because such a loan can protect students for the outcome of the education.

# Forwards and Futures Markets
Forwards and futures are so-called derivatives. They are different from the spot market, the market for immediate delivery ("you pay the money, you get them.").

Forwards and futures are important to economics because the markets reflect time as well as quality. Thus, for a certain commodity, there is a different price for every different future delivery date.

Theoretically, there could be a future market for every kind of commodity. But in reality, some future markets are successful while some are not. There are not a good explanation to this, but it may have something to do with risk.

## Forward Contract
Forward is a contract between two parities to deliver at a future date (exercise date or maturity date) at a specified exercise price. Both sides are locked into the contract with no liquidity.

Grain is the origin of the forward contract, but there are forward contracts on other commodities, such as foreign exchange (FX) forwards. FX Forward is like a pair of zero-coupon bonds; therefore, forward rate reflects interest rates in the two currencies. Their parity is called Forward Interest Parity:

$$ Forward Exchange Rate (¥/\$) = Spot Exchange Rate (¥/\$) \times \frac{1 + r_{¥}}{1 + r_{\$}}$$

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

## Futures Contracts
Futures contracts are like forward contracts except that there is an intermediary, the exchange, and thus do not have to deal with each other's credits.

Futures contracts are standardized retail products, rather than custom products. There can be many different spot prices for a commodity based on different specifications, but futures contracts have very specific quantity, quality, price unit, tick size (minimum fluctuation), to improve liquidity.

Futures contracts rely on **margin calls** to guarantee performance.

>When an investor uses margin to buy ourselves securities, they are paid for using a combination of his or her own funds as well as money borrowed from a broker. The investor receives a margin call from a broker if the securities in the portfolio decrease in value past a certain point, after which the investor must either deposit more money into the account, or sell some of the assets.

The futures contract requires you to pay a settlement every day from your futures margin account, based on the change in futures market on that day. Thus, the price change in the futures market is reflected in your margin account. Thus, the risk of the futures contract is eliminated by the margin account.

First futures market began at Dojima, Osaka, Japan, in 1670s, for rice, and remained world's only futures market until 1860s.

Most futures contracts are closed out without delivery, as they are used to hedge risks in the fluctuations in the price of the commodity. Because the price of a commodity of a specification at a certain location tends to move along with futures price, people in the world can buy or sell futures contracts in Chicago Board of Trade (CBOT, now part of CME Chicago Mercantile Exchange) to insure against price change.

The futures curve, the curve between price and delivery date, tends to go upward. This nature of the futures curve is called *contango*. This means if you want delivery at a future date, it will cost you more. However, sometimes there are drops ("backwardation") in the futures curve. This is often due to expected delivery of large quantity of the commodity, such as harvest.

## Buying, Selling, and Settlement
When one "buys" a futures contract, one agrees with the exchange to a daily settlement procedure that is only loosely analogous to buying the commodity. However, one has no intention of taking delivery of the commodity. One must post initial margin with the futures commission merchant. Similar things happen on the "selling" side: seller has no intention of selling the commodity; one posts margin.

Every day, the exchange defines a price called the "settle" price, which is essentially the last trade on that day. Every day until expiration, a buyer's margin account is credited (or debited if negative) with the amount equal to the change in settle price times contract amount, thus the margin account always reflect the amount needed to buy the contract amount at the settle price.

If contract is cash settled, on the last day, the margin account is credited with the last settle price times the contract amount. If the contract is physical delivery, on the last day, buyer must receive commodity.

For example, a farmer in March is planting crop expected to yield 50000 bushels of corn. In other words, farmer is "long" 50000 bushels. Farmer "sells" 10 Chicago September corn contracts for $$\$2.335\times 50000 = \$ 116750$$, and the margin is posted. Corn products manufacturer, who plans to buy corn at harvest time, "buys" the ten contracts, posts margin, too. In September, both buyer and seller close out position. Then between March and September, changes in the margin account mean that the price of corn is effectively locked in at $2.335 per bushel for both.

## Fair Value in Futures Contract

$$P_{future} = P_{spot}(1 + r + s)$$

where $$r$$ is the interest rate, and $$s$$ is the storage cost, and thus $$r+s$$ is the cost of carry. Future price is normally above cash price (i.e. contango).

If the commodity is in storage, there is a profit opportunity (i.e. arbitrage) that will tend to drive to zero any difference from fair value. If the commodity is not in storage, then it is possible that the futures price is lower than fair value (i.e. "backwardation").

## SPI & FFR Futures
Stock Price Index (SPI) Futures (such as S&P 500 Index futures), a financial future developed much later. It is cash settled rather than physical delivery. Cash settlement makes financial futures possible, because on the last day of the contract, the margin account is settled with the exact price of the S&P 500 Index as the settle price. This makes arbitrage of futures easier, and settle price of futures on the last day to be equal to the spot price (with margin of error).

The settlement is equal to $250 times the index on the last day minus the futures price for the day before. And the fair value is $$F=P+P(r-y)$$ where $$P$$ is the stock price index, $$r$$ is the financing cost (i.e. interest rate), and $$y$$ is the dividend yield. The formula is the same if you treat $$-y$$ as the storage cost for stocks.

Federal Funds Rate (FFR) Futures was created by CBOT in 1988. The settlement price is 100 minus annualized federal funds rate, averaged over contract month. It's used widely to show expectations of actions of the Federal Open Market Committee, and there is no fair value for the futures because it is not storable.

# Options
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

## Option Pricing
An option pricing has the following components:
* Ticker: the symbol of the underlying asset;
* Strike price;
* Last price: the price an option was last sold for;
* Bid price: the price to sell the option back to the market maker (i.e. the price the market maker is offering to buy back options, thus bidding);
* Ask price: the price to buy the option (i.e. the price the market maker is asking for to write an option).

## Why Options Exist
A theoretical reason is that options provide a way to trade risks. Economic theorist Kenneth Arrow argued that a major source of economic inefficiency is the absence of markets for risks. Options provide a way to trade the risk that an asset will go up or below any price, and thus, options market "completes the market", as argued by Stephen Ross, a financial theorist.

A behavioral reason is that options can serve as insurance against downwards risks on portfolio's long position, making people happier.

## Ubiquity of Options
* Limited Liability makes stocks into an option to have a share in company's growth.
* Mortgages involve an option to default in a non-recourse states where the lender can only evict borrower.

## Put-Call Parity
On the expiration date, the intrinsic value of an option is the difference between strike price and the stock price or 0:

![](https://ebrary.net/imag/bef/corp_fin/image080.jpg)

There is a relationship between put and call option prices:

Put option price - call option price = present value of strike price + present value of dividends - price of stock, or

Price of stock = call price + pdv strike + pdv dividends - put price.

This parity equation must hold out except margin of error due to transaction cost. Otherwise, there is arbitrage opportunity.

Before the expiration date, the value of an option is always higher than the intrinsic value of the option due to the probability that it will increase in value before expiry.

![](https://upload.wikimedia.org/wikipedia/commons/1/1b/Option_value.gif)

The option price is estimated via a predictive formula such as black-Scholes or using a numerical method such as the Binomial model.

## Use Options to Hedge
To put a floor on one's holding of stock, one can buy a put on same number of shares. alternatively, one can just decide to sell whenever the price reaches the floor, even automatically using a stop-loss order. Doing the former means I must pay the option price, and doing the latter costs nothing. However, a put option gives you the choice to exercise or not, so the decision can be made after the price drop; but a stop-loss order would be executed as soon as the price drops below the limit price, so the decision must be made at the price drop. So if price drops further, a put option limits the loss; if price drops and rebound, you could choose not to execute the put option, but the stop-loss order would have been executed.
