---
layout: post
title: Financial Markets - 2
date: '2018-09-22 13:29'
---

This is second of the note series I took when studying for [Financial Markets](https://www.coursera.org/learn/financial-markets-global/home/welcome) by Prof. Robert Shiller, offered by Coursera. This covers the content for Week 2.

This module dives into some details of behavioral finance, forecasting, pricing, debt, and inflation.

# Practical Financial Innovation
## Limited Liability
In order to encourage investing, investors must be protected against failures of invested companies. Investors' liability to their invested companies should not exceed the amount they put in.

Limited Liability New York State 1811: divide up an enterprise into shares, and not shareholder is liable for more than he or she puts in.

Limited Liability was not obviously a good idea: it creates [agency problems](https://www.investopedia.com/terms/a/agencyproblem.asp) (conflict of interest inherent in any relationship where one party is expected to act in another's best interests) for shareholders, who might pursue risky strategies at the expense of bondholders.

But why Limited Liability was so successful?
* Investors' overestimation of minuscule probability of loss beyond initial investment will discourage investment.
* Lottery effect: with limited liability, an investment in a corporation was a throwaway item, like a lottery ticket.
* Allowed for investors to hold a highly diversified portfolio.

## Inflation Indexed Debt
History shows many examples of nominal debt being wiped out in real terms by high inflation. But debt indexed against real terms or commodity is protected against fluctuations in currency valuation.

Indexed debt was first attempted in Massachusetts to fund the Revolutionary War, but it didn't appear in the US until 1997. Still there is no private indexed debt today.

## Unidad de Fomento
The Unidad de Fomento (UF, literally: unit of development) is a Unit of account that is used in Chile. The exchange rate between the UF and the Chilean peso is now (today) constantly adjusted for inflation so that the value of the Unidad de Fomento remains constant on a daily basis during low inflation.

In economics, money is often defined in terms of the three functions it provides: a store of value, a unit of account and a medium of transaction. You can separate out those functions. You can then have a separate unit of account that is not money. So Chile invented something called the UF, a Unidad de Fomento, and they allowed its value to be tied to the Consumer Price Index.

## Real Estate Risk Management Devices
Value of homes is a major source of risk in real estate. There are home insurance, casualty insurance against accidents in your home, or fire insurance, but there is no protection against fluctuations of home value.

One way to protect you against collapse is to short the housing market in your own city. Chicago Mercantile Exchange offers home price futures and options. Another way is through equity-protected mortgages, which associates the remaining balance of the housing mortgages with the value of the house. If the house value decreases, the balance is corrected downwards.

Another big risk everyone bears is human capital. How to protect yourselves from the risk of your own skills? One way is to short a company in your industry, assuming it correlates with your skills. Another way is through some insurance payout when people lose their job and switch to another permanent job with lower wages.

# Market Forecasting
There are many theories to forecast the market movement.
* The efficient market hypothesis states that asset prices fully reflect all available information.
* The random walk hypothesis states that the markets movement at a time is independent from the last, thus it is impossible to forecast the market movement. $$x_t=x_{t-1} + \epsilon_t$$ where $$x_t$$ is the forecast, $$x_{t-1}$$ is the prior knowledge, and $$\epsilon_t$$ is the noise at time $$t$$ that is totally unforecastable.
* First-order autoregressive (AR-1) model: $$x_t = \bar{x} + \rho(x_{t-1} - \bar{x}) + \epsilon_t$$. With $$ -1 \lt \rho \lt 1$$, $$x_t$$ is going to revert to the mean more than pure random walk. The advice would be to stay out of the market when it's higher than the mean, and get back in the market when it is lower than the mean.

There are three variants of the efficient market hypothesis: "weak", "semi-strong", and "strong" form.
* The weak form of the EMH claims that prices on traded assets (e.g., stocks, bonds, or property) already reflect all past publicly available information.
* The semi-strong form of the EMH claims both that prices reflect all publicly available information and that prices instantly change to reflect new public information.
* The strong form of the EMH additionally claims that prices instantly reflect even hidden "insider" information.

The efficient market hypothesis forms the basis of CAPM and mathematical finance.

## Intuition of Market Efficiency
The difference in the speed of information can be exploited to make money in the market. However, thanks to technology the speed of information is much faster at cheaper cost, thus it is harder to make money from information. Thus, market must be more "efficient" to adjust to information.

There are doubts about the efficient market hypothesis after the 2008-09 financial crisis.

## Price as Present Discounted Value of Expected Dividends
According to Gordon model, if earnings equal to dividends and if dividends grow at long-turn rate of $$g$$, then $$PV = \frac{E}{r - g}$$ or $$P/E = \frac{1}{r - g}$$.

So the efficient market hypothesis must explain why P/E varies across stocks. Low P/E does not mean that the stock is a "bargain", it only means that earnings are rationally forecasted to decrease in the future. A high P/E means market expects the growth of future earnings is fast.

However, there exist differences in P/E across industries or countries that are difficult to explain.

# Behavior Finance
The research of the role of psychology in finance. Aspects of Psychology play a role in many economic institutions:
* Insurance and loss aversion
* Corporate stocks and gambling
* Bonds and money illusion
* Banks and trust
* Central banks and bubbles
* Investment banks and framing
* Exchange and sensation seeking
* Options and salience

## Prospect Theory
It was proposed by Kahneman and Tverksy in Econometrica (a most prestigious mathematical economics journal) in 1979. They replace the core theory of economics, expected utility theory, with a constructive alternative. It consists with two changes:

1. The utility function is replaced with a value function.
2. The probabilities is replaced subjective probabilities determined by a weighting function in terms of the actual probabilities.

In economics, the utility function exhibits diminishing marginal utility everywhere, so it is concaved down. The value function (a function between gain/loss and value) is indeed concaved down in the gain part. The loss part, however, is concaved up, and it is not smooth at the origin. This means people are willing to take more risk to escape the loss part.

![Value Function](https://upload.wikimedia.org/wikipedia/commons/thumb/4/4e/Valuefun.jpg/350px-Valuefun.jpg)

The other thing is, that the utility function is not usually between gains as losses in economics. Rather, your utility is determined by how much you have. The value function is about the reaction to an opportunity on any given day. So the origin of the value function forms a reference point of your utility, and the function represents the "value" of different utilities relative to that reference point.

The weight function is the relationship between the stated probability and the decision weight. In a completely rational world, it is a straight line between 0 and 1 (inclusive) because the decision weight must equal to the stated probability. In reality, it is a curve of slope less than 1, and it does not cover very small and large probabilities (i.e. close to 0 or 1). This means people either ignore it or exaggerate it when the probability is low or high.

## Overconfidence
Most people think they are better than average. Wishful thinking bias is a term of psychology that people overestimate of the probability of the things that they identify with and want to see happen.

## Cognitive Dissonance
It is a term used by social psychologists that refers to the mental conflict that occurs when one's beliefs are discovered to be wrong. People show avoidance behavior to evidences that disprove their beliefs.

## Mental Compartments
CAPM assumes the whole portfolio to be taken into consideration. Compartments refer to investors having two or more portfolios, one "safer" and one risky that they can have fun with.

## Attention Anomalies
Psychologists have long understood that you can't pay attention to everything. You have to decide what you pay attention to. A lot of people pay attention to things that are irrelevant to them, such as day-to-day movement of the stock market.

Second, there's a social basis for attention and that is that you tend to pay attention to the same things that other people pay attention. This could result in mispricing of stocks, as many investors pay attention to a specific news that may drive the stock price overly high or low.

## Anchoring
Anchoring refers to a tendency in ambiguous situations to allow one's decisions to be affected by some anchor. For example, stock prices are anchored to past values, or to other stocks in the same country.

## Representativeness Heuristic
People judge by similarity to familiar types, without regard to base rate probabilities. It refers to the tendency to see patterns in what is really random walk.

## Disjunction Effect
The disjunction effect is the inability to make decisions in advance in anticipation of future information. People don't anticipate their emotions later. So, they can't work through a decision tree correctly. This can explain the reaction of stock market to news and how people make stock strategies to trade on news.

## Magical Thinking
Discovered by B. F. Skinner in 1948, it shows repeated events can develop superstitions (strong irrational beliefs) with fallacious attributions of causal relationships between actions and events. Stock market responses to events may have similar origins in similar way.

## Quasi-Magical Thinking
It describes the cases in which people act as if they erroneously believe that their action influences the outcome, even though they do not really hold that belief. People may realize that a superstitious intuition is logically false, but act as if it were true because they do not exert an effort to correct the intuition. For example, people vote because they think their vote may have an impact of the outcome (whose probability is minuscule), or people bet more on coin not yet tossed, or pay more for lottery ticket in which they choose the number.

Newcomb’s paradox describes the case that people sometimes change their behavior when they learn about a prediction which has been made about the future.

## Culture and Social Contagion
Culture and "collective memory" influences people's thinking. There are moral anchors for the markets in the form of human stories, and people's memory is formed by stories.

## Antisocial Personality Disorder
It is a sociopathy defined in Diagnostic and Statistical Manual 5:
* Identity: egocentric, self-esteem from personal gain
* Self-direction: absence of prosocial internal standards
* Lack of empathy, incapacity for intimacy
* Manipulative, deceitful, callous, hostile
* Irresponsible, impulsive, risk-taking
