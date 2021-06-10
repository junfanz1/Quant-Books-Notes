Stochastic Volatility Modeling
===

Char 2 Local Volatility - Notes
====

Junfan Zhu

(`junfanz1.github.io`)

2021 - 06 - 10

![Book](https://user-images.githubusercontent.com/56275127/121436137-a4d0b600-c945-11eb-862c-f00afe6e1efd.png)

This is a short notes based on Chapter 2 of the book. 

__Stochastic Volatility Modeling (Chapman and Hall/CRC Financial Mathematics Series) 1st Edition, by Lorenzo Bergomi__

> [Book Link](https://www.amazon.com/Stochastic-Volatility-Modeling-Financial-Mathematics/dp/1482244063)

---

Table of Contents
---


<!-- TOC -->

- [Stochastic Volatility Modeling](#stochastic-volatility-modeling)
- [Char 2 Local Volatility - Notes](#char-2-local-volatility---notes)
  - [Table of Contents](#table-of-contents)
- [Chapter 2. Local Volatility](#chapter-2-local-volatility)
- [1. Introduction - Local Volatility as a market model](#1-introduction---local-volatility-as-a-market-model)
  - [1.1. SDE of local volatility model](#11-sde-of-local-volatility-model)
- [2. From prices to local volatilities](#2-from-prices-to-local-volatilities)
  - [2.1. Dupire Formula](#21-dupire-formula)
  - [2.2. No-arbitrage conditions](#22-no-arbitrage-conditions)
- [3. From local volatilities to implied volatilities](#3-from-local-volatilities-to-implied-volatilities)
  - [3.1. The smile near the forward](#31-the-smile-near-the-forward)
  - [3.2. A power-law-decaying ATMF skew](#32-a-power-law-decaying-atmf-skew)
  - [3.3. Expanding around a constant volatility](#33-expanding-around-a-constant-volatility)
- [4. Dynamics of local volatility model](#4-dynamics-of-local-volatility-model)
  - [4.1. Skew Stickness Ratio (SSR)](#41-skew-stickness-ratio-ssr)
  - [4.2. The $R=2$ rule](#42-the-r2-rule)
- [5. Future skews and volatilities of volatilities](#5-future-skews-and-volatilities-of-volatilities)
  - [5.1. Comparison with stochastic volatility models](#51-comparison-with-stochastic-volatility-models)
- [6. Delta and carry P&L](#6-delta-and-carry-pl)
  - [6.1. The local volatility delta](#61-the-local-volatility-delta)
  - [6.2. Consistency of $\Delta^{\mathrm{SS}}$ and $\Delta^{\mathrm{MM}}$](#62-consistency-of-deltamathrmss-and-deltamathrmmm)
  - [6.3. Local volatility as simplest market model](#63-local-volatility-as-simplest-market-model)
  - [6.4. Conclusion](#64-conclusion)
- [7. Vega Hedge](#7-vega-hedge)
- [8. Markov-functional models (MFM)](#8-markov-functional-models-mfm)
  - [8.1. Relationship of Gaussian copula to multi-asset local volatility prices](#81-relationship-of-gaussian-copula-to-multi-asset-local-volatility-prices)
  - [8.2. Using Uncertain Volatility Model (UVM) to price transaction costs](#82-using-uncertain-volatility-model-uvm-to-price-transaction-costs)
- [9. Conclusion](#9-conclusion)
  - [9.1. From prices to local volatilities](#91-from-prices-to-local-volatilities)
  - [9.2. From Implied Volatilities to Local Volatilities](#92-from-implied-volatilities-to-local-volatilities)
  - [9.3. From Local Volatilities to Implied Volatilities](#93-from-local-volatilities-to-implied-volatilities)
  - [9.4. Dynamics of Local Volatility Model](#94-dynamics-of-local-volatility-model)
  - [9.5. Future Skews and Volatilities of Volatilities](#95-future-skews-and-volatilities-of-volatilities)
  - [9.6. Delta and Carry P&L](#96-delta-and-carry-pl)
  - [9.7. Vega Hedge](#97-vega-hedge)
  - [9.8. Markov-Functional Models](#98-markov-functional-models)

<!-- /TOC -->

Chapter 2. Local Volatility
===

# 1. Introduction - Local Volatility as a market model

## 1.1. SDE of local volatility model

Black Scholes

$$\frac{d P}{d t}+(r-q) S \frac{d P}{d S}+\frac{\sigma(t, S)^{2}}{2} S^{2} \frac{d^{2} P}{d S^{2}}=r P \tag{2.2}$$

where local volatility for spot $S$ and $t$ is reflected in the difference of option prices with strikes straddling $S$ and maturities straddling $t$.

$$\sigma(t, S)^{2}=\left.2 \frac{\frac{d C}{d T}+q C+(r-q) K \frac{d C}{d K}}{K^{2} \frac{d^{2} C}{d K^{2}}}\right|_{K=S, ~T=t} \tag{2.3, Dupire}$$

# 2. From prices to local volatilities

## 2.1. Dupire Formula

Consider diffusive dynamics for $S_{t}$:

$$
d S_{t}=(r-q) S_{t} d t+\sigma_{t} S_{t} d Z_{t}
$$

where $\sigma_{t}$ is arbitrary process. By only using vanilla option prices, how precisely can we characterize $\sigma_{t} ?$ 

Price of call option:

$$
C(K, T)=e^{-r T} E\left[\left(S_{T}-K\right)^{+}\right]
$$

The dynamics of $S_{t}$ on the interval $[T, T+d T]$ determines how much prices of options of maturities $T$ and $T+d T$ differ. 

Itô expansion for $\left(S_{T}-K\right)^{+}$ over $[T, T+d T]$ :

$$
d\left(S_{T}-K\right)^{+} =\quad \frac{d\left(S_{T}-K\right)^{+}}{d S_{T}}\left((r-q) S_{T} d T+\sigma_{T} S_{T} d Z_{T}\right) \quad+\frac{1}{2} \frac{d^{2}\left(S_{T}-K\right)^{+}}{d S_{T}^{2}} \sigma_{T}^{2} S_{T}^{2} d T$$

$$=\theta\left(S_{T}-K\right)\left((r-q) S_{T} d T+\sigma_{T} S_{T} d Z_{T}\right)+\frac{1}{2} \delta\left(S_{T}-K\right) \sigma_{T}^{2} S_{T}^{2} d T \tag{2.4}
$$

where $\theta(x)$ is the Heaviside function: $\theta(x)=1$ for $x>0, \theta(x)=0$ for $x<0$, and $\delta$ is the Dirac delta function.

$$E\left[\sigma_{T}^{2} \mid S_{T}=K\right]=\frac{E\left[\sigma_{T}^{2} \delta\left(S_{T}-K\right)\right]}{E\left[\delta\left(S_{T}-K\right)\right]}=2 \frac{\frac{d C}{d T}+q C+(r-q) K \frac{d C}{d K}}{K^{2} \frac{d^{2} C}{d K^{2}}} \tag{2.6}$$

Dupire equation expresses the local volatility as a function of derivatives of call option prices. It's a general relationship linking the expectation of the instantaneous variance conditional on the spot price to the maturity and strike derivatives of vanilla option prices.

## 2.2. No-arbitrage conditions

Local volatility given by the Dupire equation is well-defined for any arbitrage-free smile.

In all fairness, the type of arbitrage strategy we have outlined – entering a position and keeping it until maturity to pocket the (positive) arbitrage profit – is a bit unrealistic as it does not take into account mark-to-market P&L and the discomfort that comes with it, in the case of a large position.

Imagine we bought yesterday a butterfly spread that had negative market value and today’s market value is even more negative: we have lost money on yesterday’s position. Our management may demand that we cut our position – at a loss – despite our plea that we will eventually make money if allowed to hold on to our position, that the arbitrage has actually become more attractive, and that we should in fact increase the size of our position.


# 3. From local volatilities to implied volatilities

## 3.1. The smile near the forward

Local volatility 

$$\sigma(t, S)=\bar{\sigma}(t)+\alpha(t) x+\frac{\beta(t)}{2} x^{2}$$

where moneyness is $x=\ln \left(S / F_{t}\right)$

ATMF(At-the-money forward) skew:

$$\begin{aligned} \mathcal{S}_{T}=&\left.\frac{d \widehat{\sigma}_{K T}}{d \ln K}\right|_{F_{T}} &=\frac{1}{T} \int_{0}^{T} \frac{t}{T} \alpha(t) d t \\ &\left.\frac{d^{2} \widehat{\sigma}_{K T}}{d \ln K^{2}}\right|_{F_{T}} &=\frac{1}{T} \int_{0}^{T}\left(\frac{t}{T}\right)^{2} \beta(t) d t \end{aligned}$$

## 3.2. A power-law-decaying ATMF skew

$$\left\{\begin{array}{l}\alpha(t)=\alpha_{0}\left(\frac{\tau_{0}}{t}\right)^{\gamma} \quad t>\tau_{0} \\ \alpha(t)=\alpha_{0} \quad t \leq \tau_{0}\end{array}\right.$$

$$
\mathcal{S}_{T} \simeq \frac{1}{2-\gamma} \alpha_{0}\left(\frac{\tau_{0}}{T}\right)^{\gamma}
$$

The long-term ATMF skew thus decays with the same exponent $\gamma$ as the local volatility function. For typical equity smiles, $\gamma \simeq \frac{1}{2}$.

## 3.3. Expanding around a constant volatility

$$\widehat{\sigma}_{K T}=\frac{1}{T} \int_{0}^{T} d t \int_{-\infty}^{+\infty} d y \frac{e^{-\frac{y^{2}}{2}}}{\sqrt{2 \pi}} \sigma\left(t, F_{t} e^{\frac{t}{T} x_{K}+\sigma_{0} \sqrt{\frac{(T-t) t}{T}} y}\right) \tag{2.42}$$


# 4. Dynamics of local volatility model

## 4.1. Skew Stickness Ratio (SSR)

Normalize $\frac{d \widehat{\sigma}_{F_{T} T}}{d \ln S_{0}}$ by the ATMF skew of maturity $T, \mathcal{S}_{T}$, thus defining a dimensionless number $\mathcal{R}_{T}$ which we call the Skew Stickiness Ratio (SSR) for maturity $T$ :

$$
\mathcal{R}_{T}=\frac{1}{\mathcal{S}_{T}} \frac{d \widehat{\sigma}_{F_{T} T}}{d \ln S_{0}} \tag{2.61}
$$

$\mathcal{R}_{T}$ quantifies how much the ATMF volatility for maturity $T$ responds to a move of the spot, in units of the ATMF skew. $\mathcal{R}_{T}$ will be given in Chapter 9 a more general definition as the regression coefficient of the ATMF volatility on $\ln S$, normalized by the ATMF skew:
$$
\mathcal{R}_{T}=\frac{1}{\mathcal{S}_{T}} \frac{\left\langle d \widehat{\sigma}_{F_{T} T} d \ln S_{0}\right\rangle}{\left\langle\left(d \ln S_{0}\right)^{2}\right\rangle} \tag{2.62}
$$

$\mathcal{R}_{T}$ essentially quantifies the spot/volatility covariance in the model at hand.
In the local volatility model, $\widehat{\sigma}_{F_{T} T}$ is a function of $(t, S)$, hence expression $(2.62)$ for $\mathcal{R}_{T}$ simplifies to $(2.61) .^{13}$ Practitioners routinely refer to two archetypical regimes:

- The "sticky-strike" regime corresponds to $\mathcal{R}_{T}=1$. As the spot moves, implied volatilities __for fixed strikes__ near the money stay frozen - the ATMF volatility slides along the smile.
- The "sticky-delta" regime corresponds to $\mathcal{R}_{T}=0 .$ The whole smile experiences a translation alongside the spot: volatilities __for fixed log-moneyness__ are frozen.

## 4.2. The $R=2$ rule

$$
\mathcal{R}_{T}=2
$$

This is an important result. The rule that the rate at which the ATMF implied volatility moves when the spot moves is twice the ATMF skew - or that the SSR equals $2-$ is in fact exact for local volatilities that are a function of $S / F_{t}$ only.
The reason is symmetry condition $(2.78) .$ 

$$\widehat{\sigma}_{S \frac{F_{T}}{F_{t}}, T}\left(K \frac{F_{t}}{F_{T}}\right)=\widehat{\sigma}_{K T}(S) \tag{2.78}$$

$F_{t}$ is the forward associated to $S$, not the reference spot $S^{\star} .$ For a local volatility of the form 

$$\sigma(t, S) \equiv \sigma\left(\frac{S}{F_{t}}\right)$$

, equation (2.42) reads:

$$
\widehat{\sigma}_{K T}(S)=\frac{1}{T-t} \int_{t}^{T} d u \int_{-\infty}^{+\infty} d y \frac{e^{-\frac{y^{2}}{2}}}{\sqrt{2 \pi}} \sigma\left(\frac{S}{S^{*}} e^{\frac{u-t}{T-t} \ln \left(\frac{K F_{t}}{S F_{T}}\right)+\frac{\sqrt{\sigma_{0}^{2}(T-u)(u-t)}}{\sqrt{T-t}} y}\right)
$$
where we have set the initial time equal to $t .$ One can check that, replacing in this expression $S$ with $K \frac{F_{t}}{F_{T}}$ and $K$ with $S \frac{F_{T}}{F_{t}}$ and making the change of variables $u \rightarrow T+t-u$ leaves the integrand unchanged and yields:
$$
\widehat{\sigma}_{S \frac{F_{T}}{F t}, T}\left(K \frac{F_{t}}{F_{T}}\right)=\widehat{\sigma}_{K T}(S)
$$

# 5. Future skews and volatilities of volatilities

## 5.1. Comparison with stochastic volatility models

Our analysis has concentrated on the price as generated by the local volatility model at $t=0$. As we risk-manage our forward-start or barrier option together with its vanilla hedge, the daily P&L of the hedged position. In case gammas and cross-gammas of the hedged position are sizeable, these P&Ls will be large and unpredictable, randomly polluting our final P&L - this is the price we pay for using the local volatility model.

In contrast, the stochastic volatility models are time-homogeneous. Future skews and future volatilities of volatilities are determined by the model's parameters and are commensurate with their values at $t=0$ - they are not altered by recalibration to the term structure of VS volatilities.

When using the local volatility model, it is then essential to realize that the model price at $t=0$ incorporates assumptions on future skews and break-even levels of spot/volatility gammas and cross-gammas that cannot be locked and will change as the model is recalibrated to future market smiles.

# 6. Delta and carry P&L

## 6.1. The local volatility delta

Delta hedged position 

$$P \& L=-\frac{1}{2} S^{2} \frac{d^{2} P^{\mathrm{LV}}}{d S^{2}}\left(\frac{\delta S^{2}}{S^{2}}-\sigma^{2}(t, S) \delta t\right) \tag{2.94}$$

Practically, we will be using the local volatility model in a way that it wasn't meant to be used - that is, recalibrating daily the local volatility function on market smiles. Is this nonsensical? Is it nevertheless possible to express simply our carry P&L? Do payoff-independent break-even levels for volatilities of implied volatilities and correlations of spot and implied volatilities exist?

## 6.2. Consistency of $\Delta^{\mathrm{SS}}$ and $\Delta^{\mathrm{MM}}$

$\Delta^{\mathrm{MM}}$ is the "real" delta of the local volatility model: it is the number of units of $S$ that need to be held in the hedge portfolio, alongside a position $\frac{d \mathcal{P}}{d O_{K T}}$ in vanilla options of strike $K$, maturity $T$. 

$\Delta^{\mathrm{SS}}$ is tied to a specific representation of vanilla option prices - in terms of Black-Scholes lognormal implied volatilities. Should we use it? What is its connection to $\Delta^{\mathrm{MM}}$ ?

Our hedge portfolio $\Pi$ comprises $\Delta^{\mathrm{MM}}=\frac{d \mathcal{P}}{d S}$ units of the underlying and $\frac{d \mathcal{P}}{d O_{K} T}$ vanilla options of strike $K$, maturity $T$ :

$$
\Pi=\frac{d \mathcal{P}}{d S} S+\frac{d \mathcal{P}}{d O_{K T}} \bullet O_{K T}
$$

Typically, the convention on an exotic desk is to use delta-hedged - rather than naked - vanilla options, where the delta hedge is the vanilla option's Black-Scholes delta computed using the option's implied volatility $\widehat{\sigma}_{K T}$. The composition of our hedge portfolio can thus be rewritten differently:

$$
\Pi=\left[\frac{d \mathcal{P}}{d S}+\frac{d \mathcal{P}}{d O_{K T}} \cdot \frac{d P_{K T}^{\mathrm{BS}}}{d S}\right] S+\frac{d \mathcal{P}}{d O_{K T}} \bullet\left[O_{K T}-\frac{d P_{K T}^{\mathrm{BS}}}{d S} S\right]
$$

What does $\frac{d \mathcal{P}}{d S}+\frac{d \mathcal{P}}{d O_{K T}} \bullet \frac{d P_{K T}^{\mathrm{BS}}}{d S}$ correspond to? It is the sensitivity of $\mathcal{P}$ to a
simultaneous move of $S$ and a variation of vanilla option prices generated by their Black-Scholes deltas. Saying that a vanilla option's price varies by its Black-Scholes delta times the variation of $S$ is equivalent to saying that its Black-Scholes implied volatility stays fixed. This is the sticky-strike delta: $\frac{d \mathcal{P}}{d S}+\frac{d \mathcal{P}}{d O_{K T}} \bullet \frac{d P_{K T}^{\mathrm{BS}}}{d S}=\Delta^{\mathrm{SS}}$,
which we can rewrite as:

$$
\Delta^{\mathrm{MM}}+\frac{d \mathcal{P}}{d O_{K T}} \bullet \frac{d P_{K T}^{\mathrm{BS}}}{d S}=\Delta^{\mathrm{SS}}
$$

Thus $\Delta^{\mathrm{SS}}$ is equal to the market-model delta $\Delta^{\mathrm{MM}}$ augmented by the BlackScholes deltas of the hedging vanilla options. Once Black-Scholes deltas of the hedging options are accounted for, we recover $\Delta^{\mathrm{MM}}$ as the aggregate delta in our hedge portfolio.

Had we used a different representation of vanilla option prices, ${ }^{21}$ we would have obtained a different "sticky-strike" delta. Yet, once the hedge porfolio is broken down into underlying + __naked__ vanilla options the delta is always equal to $\Delta^{\mathrm{MM}}$. 

$\Delta^{\mathrm{SS}}$ thus has no special status. It is simply the delta an exotic desk should trade if - as is customary - the delta hedges of delta-hedged vanilla options used as vega hedges are their Black-Scholes deltas.

## 6.3. Local volatility as simplest market model

Stochastic volatility market model is a market model driven by diffusive processes.

Def by joint SDEs for the spot and vanilla option prices:

$$
d S_{t} =(r-q) S_{t} d t+\sigma_{t} S_{t} d W_{t}^{S} 
$$

$$
d O_{K T, t} =r O_{K T, t} d t+\lambda_{K T, t} d W_{t}^{K T}
$$

$$
O_{K T, t=T}=\left(S_{T}-K\right)^{+}
$$

## 6.4. Conclusion

---

The local volatility model is a diffusive market model - a somewhat special one as (a) it is a one-factor model, (b) it possesses a Markov representation in terms of $t, S$

It can be used for risk-managing options, recalibrating on a daily basis the local volatility function to market smiles. The gamma/theta P&L is well-defined:
break-even levels for volatilities and correlations of $S$ and $\widehat{\sigma}_{K T}$ exist and are payoff-independent.

Obviously, one may wish that break-even correlations were different than $100 \%$ and that volatilities of implied volatilities could be controlled exogenously and not be dictated by the smile used for calibration, but this is how much we can get with a one-factor model.

---

The fact that volatilities of implied volatilities, as generated by the model, are neither chosen by the user, nor even deterministic but depend at each point in time on the then-prevailing market smile is however an issue. There is no guarantee that these levels will be adequate with respect to realized levels. Moreover, they will vary unpredictably: in case future market smiles happen to be flat, so that $\frac{d \Sigma_{K T}^{\mathrm{LV}}}{d S} \simeq 0$, hence $\nu_{K T}=0$, we will find ourselves risk-managing our exotic option with a model that is locally pricing vanishing volatilities of implied volatilities.

---

In practice, unlike delta hedging, vega hedging is typically not performed on a daily basis, because of larger bid/offer costs. When rehedging frequencies for delta and vega hedges differ, what gamma/theta P\&L do we materialize? See Section 9.11.3, page 383 .

---

The "local volatility delta" $\Delta^{\mathrm{LV}}$, computed with a fixed local volatility function, has no special significance or usefulness.

---

The very notion of a vanilla option's delta does not make sense in the context of a market model, such as the local volatility model. A market model takes as inputs vanilla option prices, in addition to the spot. The former are treated as hedge instruments, on the same footing as the underlying itself. Just as the notion of the delta of one asset with respect to a different asset in a multi-asset model makes no sense, the delta of a vanilla option in a market model is an irrelevant notion.

# 7. Vega Hedge

In the Black-Scholes model there is only one vega, as there is only one volatility parameter: any option can be used to vega-hedge any other option. The situation improves somewhat in the Black-Scholes model with deterministic time-dependent volatility $\sigma(t)$. The relationship linking $\sigma(t)$ to implied volatilities $\widehat{\sigma}_{T}$ is:

$$
\sigma^{2}(t)=\left.\frac{d}{d T}\left(T \widehat{\sigma}_{T}^{2}\right)\right|_{T=t}
$$

Thus an option's sensitivity to $\delta \sigma(t)$ can be hedged - within the model - by trading narrow calendar spreads of vanilla options. Equivalently, given the sensitivity of an option to $\sigma(t)$ for all $t$ up to its maturity, we can derive the maturity distribution of vanilla options to be used as hedges.

# 8. Markov-functional models (MFM)

In the local volatility model $S_{t}$ is generally a function of the path of $W_{t}$ up to time $t$, where $W_{t}$ is the driving Brownian motion. Are there special forms of the local volatility function such that $S_{t}$ can be written as a function of $t$ and $W_{t}$, hence can be simulated without any time-stepping? The Black-Scholes model is one example:

$$
S_{t}=S_{0} e^{\left(r-q-\frac{\sigma^{2}}{2}\right) t+\sigma W_{t}}
$$

$$
S_{t}=f\left(t, W_{t}\right)
$$
The condition that the drift of $S$ be $r-q$ translates into the following PDE for $f$ :
$$
\frac{d f}{d t}+\frac{1}{2} \frac{d^{2} f}{d x^{2}}=(r-q) f
$$
and the instantaneous (lognormal) volatility of $S$ is given by:
$$
\sigma(t, S)=\left.\frac{d \ln f}{d x}\right|_{x=f^{-1}(t, S), t}
$$
which makes it clear that it is a local volatility model.

Assume we are given the market smile for maturity $T$ : we can price digital options for all strikes, which gives access to the cumulative distribution function of $S_{T}$, $\mathcal{F}(S)$. Denoting $\mathcal{N}$ the cumulative distribution of the centered normal distribution, $f(t=T, x)$ is given by:
$$
f(T, x)=\mathcal{F}^{-1}\left(\mathcal{N}\left(\frac{x}{\sqrt{T}}\right)\right) \tag{2.128}
$$

$f(t=T, x)$ is monotonic by construction and so is $f(t, x)$ for $t<T$.

In the context of equities MFMs are very rarely used: one usually needs to calibrate a set of maturities simultaneously. In fixed income markets, on the other hand, MFMs are natural, as cap/floors on LIBOR rates, swaptions, have maturities that match the fixing date of the underlying rate with Markov-functional interest rate models. This is also the case of futures in commodity markets and VIX futures – see Section 7.8.2 for an example of an MFM in this context.

## 8.1. Relationship of Gaussian copula to multi-asset local volatility prices

MFMs can be used for European options on a basket of equities $S^{i}$. Let us call $T$ the option's maturity: one draws the (correlated) Gaussian random variables $W_{T}^{i}$, applies the mapping in (2.128) and evaluates the payoff. This is exactly equivalent to using the marginal densities supplied by the market smile for maturity $T$ for each asset, and then using a Gaussian copula function to generate the multivariate density for the $S_{T}^{i}$.

This is worth noting as, usually, given a multivariate density $\rho$ generated by an arbitrary copula function, one is unable to characterize the dynamics that underlies $\rho:$ it is not even clear that there exists a diffusive process that is able to generate $\rho$ one then has no idea of what the gamma/theta break-even levels of his/her position are: the model is unusable.

Because MFMs are a particular instance of local volatility, in the case of a multiasset European option, pricing with a Gaussian copula thus exactly boils down to using a particular multi-asset local volatility model calibrated on implied volatilities of the option's maturity, with constant correlations, equal to the correlations of the Gaussian copula.

Because we only calibrate implied volatilities of maturity T , there exist many other local volatility functions that achieve exact calibration. Prices generated by these other volatility functions will differ from the Gaussian copula price.

## 8.2. Using Uncertain Volatility Model (UVM) to price transaction costs

The UVM can be used to price-in transaction costs on the delta-hedge.

Consider selling a call option on a security $S$, risk-managed in the Black-Scholes model with an implied volatility $\widehat{\sigma}$, but assume that bid/offer costs are incurred as we adjust our delta - say on a daily basis. Assume the relative bid-offer spread is $k$ :
we pay $\left(1+\frac{k}{2}\right) S$ to buy one unit of the security, and receive $\left(1-\frac{k}{2}\right) S$ when we sell it.

The P&L over $[t, t+\delta t]$ of a delta-hedged short option position reads as in $(1.5)$ page 4 , with the additional contribution of bid/offer costs:
$$
P \& L=-\frac{S^{2}}{2} \frac{d^{2} P}{d S^{2}}\left(\frac{\delta S^{2}}{S^{2}}-\widehat{\sigma}^{2} \delta t\right)-\frac{k}{2} S|\delta \Delta|
$$
where $\delta \Delta$ is the variation of our delta during $\delta t$; we use an absolute value as the impact of bid/offer is always a cost. $\delta \Delta$ is generated by (a) time advancing by $\delta t,(\mathrm{~b})$ $S$ moving by $\delta S$. Since $\delta S$ if or order $\sqrt{\delta t}$ we keep the latter contribution only: $\delta \Delta=\frac{d^{2} P}{d S^{2}} \delta S$.
$$
\begin{aligned}
P \& L &=-\frac{S^{2}}{2} \frac{d^{2} P}{d S^{2}}\left(\frac{\delta S^{2}}{S^{2}}-\widehat{\sigma}^{2} \delta t\right)-\frac{k}{2} S\left|\frac{d^{2} P}{d S^{2}} \delta S\right| \\
&=-\frac{S^{2}}{2} \frac{d^{2} P}{d S^{2}}\left(\frac{\delta S^{2}}{S^{2}}-\widehat{\sigma}^{2} \delta t+\varepsilon_{\Gamma} k\left|\frac{\delta S}{S}\right|\right)
\end{aligned}
$$
where $\varepsilon_{\Gamma}$ is the sign of $\frac{d^{2} P}{d S^{2}}$

If the realized volatility of $S$ is indeed $\widehat{\sigma}$, the gamma and theta contributions cancel out and the third piece will make our P\&L persistently negative. Can we find an implied volatility $\widehat{\sigma}^{*}$ such that risk-managing the option position at $\widehat{\sigma}^{*}$ generates, on average, zero P\&L? $\widehat{\sigma}^{*}$ must be such that:

$$
\left\langle\frac{\delta S^{2}}{S^{2}}\right\rangle-\widehat{\sigma}^{* 2} \delta t+\varepsilon_{\Gamma} k\left\langle\left|\frac{\delta S}{S}\right|\right\rangle=0
$$

Assuming a lognormal dynamics for $S$ with realized volatility $\widehat{\sigma}$ and $\delta t$ small, $\frac{\delta S}{S}=\widehat{\sigma} \sqrt{\delta t} Z$ where $Z$ is a standard normal variable, thus $\left\langle\frac{\delta S^{2}}{S^{2}}\right\rangle=\widehat{\sigma}^{2} \delta t$ and
$\left\langle\left|\frac{\delta S}{S}\right|\right\rangle=\gamma \widehat{\sigma} \sqrt{\delta t}$ with $\gamma=\sqrt{\frac{2}{\pi}} \cdot \widehat{\sigma}^{*}$ is given by:

$$
\widehat{\sigma}^{*}=\sqrt{\widehat{\sigma}^{2}+\varepsilon_{\Gamma} k \gamma \frac{\widehat{\sigma}}{\sqrt{\delta t}}}
$$

Depending on the sign of the option's gamma, we need to use either $\widehat{\sigma}_{\Gamma+}^{*}$ or $\widehat{\sigma}_{\Gamma-}^{*}$, given by:

$$
\widehat{\sigma}_{\Gamma+}^{*}=\sqrt{\widehat{\sigma}^{2}+k \gamma \frac{\widehat{\sigma}}{\sqrt{\delta t}}} \quad \widehat{\sigma}_{\Gamma-}^{*}=\sqrt{\widehat{\sigma}^{2}-k \gamma \frac{\widehat{\sigma}}{\sqrt{\delta t}}} \tag{2.139}
$$

with $\widehat{\sigma}_{\Gamma+}^{*} \geq \widehat{\sigma}_{\Gamma-}^{*}$

__Observations:__

- $\gamma$ depends on the distribution we assume for daily returns. It equals $\sqrt{\frac{2}{\pi}}$ for Gaussian returns. In practice, daily returns of equities are better modeled with a Student distribution - see Chapter 10 for examples of actual distributions of index returns. In this respect, $\gamma=\sqrt{\frac{2}{\pi}}$ is an over-estimation.
- The period $\delta t$ of our delta rehedging schedule appears explicitly in (2.139). As $\delta t \rightarrow 0$, rehedging costs become prohibitive, to the point where $\widehat{\sigma}_{\Gamma-}^{*}$ does not exist anymore. When rehedging is frequent, or bid/offer spreads are large, rather than use the Black-Scholes delta and charge for the costs this incurs, it is preferable to go back to square one and cast the delta-hedging strategy as a stochastic control problem that maximizes a utility function which balances the costs generated by hedging with the benefit of a reduced uncertainty of our final P\&L. This was done by Mark Davis, Vassilios Panas and Thaleia Zhariphopoulou in [35]. In practice, an exponential utility function $e^{-\lambda P \& L}$ is well suited as the ensuing delta strategy is independent on the initial wealth. 

# 9. Conclusion

## 9.1. From prices to local volatilities

Given a full volatility surface $\widehat{\sigma}_{K T}$ that complies with the following no-arbitrage conditions:

$$
\begin{aligned}
e^{q T_{1}} C\left(\alpha F_{T_{1}}, T_{1}\right) & \leq e^{q T_{2}} C\left(\alpha F_{T_{2}}, T_{2}\right) \\
\frac{d^{2} C(K, T)}{d K^{2}} & \geq 0
\end{aligned}
$$

there exists one volatility function $\sigma(t, S)$ given by the Dupire formula (2.3):
$$
\sigma(t, S)^{2}=\left.2 \frac{\frac{d C}{d T}+q C+(r-q) K \frac{d C}{d K}}{K^{2} \frac{d^{2} C}{d K^{2}}}\right|_{K=S,~T=t}
$$
More generally, for any stochastic volatility model that recovers the market smile, whose instantaneous volatility is $\sigma_{t}$, the following condition holds:
$$
E\left[\sigma_{t}^{2} \mid S_{t}=S\right]=\sigma(t, S)^{2}
$$

In the absence of arbitrage, implied volatilities of vanilla options obey the convex order condition:
$$
T_{2} \widehat{\sigma}_{\alpha F_{T_{2}}, T_{2}}^{2} \geq T_{1}{\widehat{\sigma}}_{\alpha F_{T_{1}}, T_{1}}^{2}
$$
This condition is shared by any family of convex payoffs $f\left(S_{T}\right)=h\left(\frac{S_{T}}{F_{T}}\right)$.


## 9.2. From Implied Volatilities to Local Volatilities

When there are no cash-amount dividends, local volatilities are obtained directly as a function of implied volatilities through (2.19):
$$
\sigma(t, S)^{2}=\left.\frac{\frac{d f}{d t}}{\left(\frac{y}{2 f} \frac{d f}{d y}-1\right)^{2}+\frac{1}{2} \frac{d^{2} f}{d y^{2}}-\frac{1}{4}\left(\frac{1}{4}+\frac{1}{f}\right)\left(\frac{d f}{d y}\right)^{2}}\right|_{y=\ln \left(\frac{S}{F_{t}}\right)}
$$

When cash-amount dividends are present, there exists (a) an exact solution based on a mapping from $S_{t}$ to an asset that does not jump across dividend dates,
(b) an approximate solution that allows one to use the same formula as in the nodividend case, except the definition of $y$ changes so that, in particular, it complies with the matching conditions for implied volatilities across dividend dates.

## 9.3. From Local Volatilities to Implied Volatilities

Given a local volatility function $\sigma(t, S)$, implied volatilities satisfy the following condition: $(2.32)$ :

$$\widehat{\sigma}_{K T}^{2}=\frac{E_{\sigma(S, t)}\left[\int_{0}^{T} e^{-r t} \sigma(t, S)^{2} S^{2} \frac{d^{2} P_{\widehat{\sigma}} K T}{d S^{2}} d t\right]}{E_{\sigma(S, t)}\left[\int_{0}^{T} e^{-r t} S^{2} \frac{d^{2} P_{\widehat{\sigma}} K T}{d S^{2}} d t\right]}$$

For weakly local volatilities $\widehat{\sigma}_{K T}$ is given, at order one in the perturbation with respect to a time-dependent volatility $\sigma_{0}(t)$ by $(2.40)$ :

$$
\widehat{\sigma}_{K T}^{2}=\frac{1}{T} \int_{0}^{T} d t \int_{-\infty}^{+\infty} d y \frac{e^{-\frac{y^{2}}{2}}}{\sqrt{2 \pi}} u\left(t, F_{t} e^{\frac{\omega_{t}}{\omega_{T}} x_{K}+\frac{\sqrt{\left(\omega_{T}-\omega_{t}\right) \omega_{t}}}{\sqrt{\omega_{T}}} y}\right)
$$

where $\omega_{t}=\int_{0}^{t} \sigma_{0}^{2}(u) d u$. When expanding around a constant volatility $\sigma_{0}$, this simplifies to $(2.42)$
$$
\widehat{\sigma}_{K T}=\frac{1}{T} \int_{0}^{T} d t \int_{-\infty}^{+\infty} d y \frac{e^{-\frac{y^{2}}{2}}}{\sqrt{2 \pi}} \sigma\left(t, F_{t} e^{\frac{t}{T} x_{K}+\frac{\sqrt{(T-t) t}}{\sqrt{T}} y}\right)
$$

For a local volatility function of the form
$$
\sigma(t, S)=\bar{\sigma}(t)+\alpha(t) x+\frac{\beta(t)}{2} x^{2}, x=\ln \frac{S}{F_{t}}
$$
the ATMF skew and curvature are given, at order one in $\alpha$ and $\beta$ by $(2.48)$ and $(2.49)$ :
$$
\begin{aligned}
\mathcal{S}_{T}=&\left.\frac{d \widehat{\sigma}_{K T}}{d \ln K}\right|_{F_{T}} &=\frac{1}{T} \int_{0}^{T} \frac{t}{T} \alpha(t) d t \\
&\left.\frac{d^{2} \widehat{\sigma}_{K T}}{d \ln K^{2}}\right|_{F_{T}} &=\frac{1}{T} \int_{0}^{T}\left(\frac{t}{T}\right)^{2} \beta(t) d t
\end{aligned}
$$
If $\alpha(t)$ decays as a power law, so does $\mathcal{S}_{T}$ for large $T$, with the same exponent: the exponent of the decay of $\alpha(t)$ can be read off the market smile.

For $T \rightarrow 0$ we have the exact result:
$$
\frac{1}{\widehat{\sigma}(T=0, K)}=\frac{1}{\ln \frac{K}{S}} \int_{S}^{K} \frac{1}{\sigma(T=0, \mathcal{S})} \frac{d \mathcal{S}}{\mathcal{S}}
$$

---

## 9.4. Dynamics of Local Volatility Model

Given a fixed local volatility function of the above form, as the spot moves, implied volatilities move. At first order in $\alpha(t)$ the sensitivity of the ATMF volatility to a spot move is given by
$$
\frac{d \widehat{\sigma}_{F_{T} T}}{d \ln S_{0}}=\frac{1}{T} \int_{0}^{T} \alpha(t) d t
$$
which can be expressed using the term-structure of the ATMF skew:
$$
\frac{d \widehat{\sigma}_{F_{T} T}}{d \ln S_{0}}=\mathcal{S}_{T}+\frac{1}{T} \int_{0}^{T} \mathcal{S}_{t} d t
$$

How the ATMF volatility moves when the spot moves is quantified by the Skew Stickiness Ratio (SSR) - a dimensionless number defined as:
$$
\mathcal{R}_{T}=\frac{1}{\mathcal{S}_{T}} \frac{d \widehat{\sigma}_{F_{T} T}}{d \ln S_{0}}
$$
For a weakly local volatility function:
$$
\mathcal{R}_{T}=1+\frac{1}{T} \int_{0}^{T} \frac{\mathcal{S}_{t}}{\mathcal{S}_{T}} d t
$$

which implies $\lim _{T \rightarrow 0} \mathcal{R}_{T}=2$, an exact result shared by all diffusive models. For an ATMF skew decaying as a power law with exponent $\gamma$, in the local volatility model, for large $T$, for a weakly local volatility function:
$$
\mathcal{R}_{T} \rightarrow \frac{2-\gamma}{1-\gamma}
$$

For typical equity smiles $\gamma=\frac{1}{2}$, thus $\mathcal{R}_{\infty}=3$.

We have the exact result that $\lim _{T \rightarrow 0} \mathcal{R}_{T}=2 .$ In addition, for a local volatility function that is a function of $\frac{S}{F_{t}}$ only, $\mathcal{R}_{T}=2, \forall T$

Expressions above for $\left.\frac{d \widehat{\sigma}_{K T}}{d \ln K}\right|_{F_{T}},\left.\frac{d^{2} \widehat{\sigma}_{K T}}{d \ln K^{2}}\right|_{F_{T}}, \mathcal{R}_{T}$ are obtained in an order-one
expansion around a constant volatility $\sigma_{0} .$ See $(2.60)$ and $(2.65)$ for formulas in an expansion around a deterministic volatility $\bar{\sigma}(t)$.

In the local volatility model, implied volatilities are a function of $S .$ The instantaneous volatility of the ATMF volatility is thus proportional to the instantaneous volatility of $S$, which is equal to $\widehat{\sigma}_{F_{t} t} \cdot \operatorname{vol}\left(\widehat{\sigma}_{F_{T} T}\right)$ is given by:
$$
\operatorname{vol}\left(\widehat{\sigma}_{F_{T} T}\right)=\mathcal{R}_{T} \mathcal{S}_{T} \frac{\widehat{\sigma}_{F_{t} 0}}{\widehat{\sigma}_{F_{T} T}}=\left(1+\frac{1}{T} \int_{0}^{T} \frac{\mathcal{S}_{t}}{\mathcal{S}_{T}} d t\right) \mathcal{S}_{T} \frac{\widehat{\sigma}_{F_{t} t}}{\widehat{\sigma}_{F_{T} T}}
$$

Thus, for short maturities, $\operatorname{vol}\left(\widehat{\sigma}_{F_{T} T}\right)=2 \mathcal{S}_{T}$ while for long maturities: $\operatorname{vol}\left(\widehat{\sigma}_{F_{T} T}\right)=$
$\frac{2-\gamma}{1-\gamma} \mathcal{S}_{T}$, for a flat term structure of ATMF volatilities where $\gamma$ is the characteristic exponent of the decay of the ATMF skew.

At order one in $\alpha(t)$ the ATMF skew is related to the weighted average of the instantaneous covariance of $\ln S$ and the ATMF volatility for the residual maturity:
$$
\mathcal{S}_{T}=\frac{1}{\widehat{\sigma}_{T}^{2} T} \int_{0}^{T} \frac{T-t}{T}\left\langle d \ln S_{t} d \widehat{\sigma}_{F_{T} T}(t)\right\rangle
$$
This relationship is derived more generally in Chapter 8, Section 8.4. One consequence of this formula is that, with respect to time-homogeneous stochastic volatility models, a local volatility model calibrated to the same smile generates larger SSRs and weaker future skews.

## 9.5. Future Skews and Volatilities of Volatilities

In the local volatility model, skews observed at future dates for a given residual maturity are typically weaker than spot-starting skews. For a spot-starting skew $\mathcal{S}_{\theta}(\tau=0)$ that decays as a function of residual maturity $\theta$ with characteristic exponent $\gamma$, the skew at a future date $\tau$ for the same residual maturity $\theta$ scales like, for small $\theta$ :
$$
\mathcal{S}_{\theta}(\tau) \propto\left(\frac{\theta}{\tau}\right)^{\gamma} \mathcal{S}_{\theta}(\tau=0)
$$

Investigating model-generated future skews is useful for assessing localvolatility prices of options that are subject to forward-smile risk. One should bear in mind that these future skews - and future levels of volatility of volatility - cannot be locked and will vary as the model is recalibrated to market smiles. Thus, unpredictable gamma/theta carry P&Ls will impact substantially the P&L of a hedged position, in case residual gammas and cross-gammas are sizeable.

## 9.6. Delta and Carry P&L

The carry P\&L of a delta and vega-hedged option position in the local volatility model can be expressed in the usual gamma-theta form, with payoffindependent break-even levels for spot variance, spot/volatility covariances and volatility/volatility covariances. The local volatility model is a genuine diffusive market model.

The carry P\&L of a delta-hedged, vega-hedged option position can be equivalently expressed either in terms of spot and implied volatilities (2.105), or as a function of spot and option prices (2.107). The "real" delta of the local volatility model is given by the derivative of the price with respect to $S$, keeping vanilla option prices fixed. The delta computed by keeping fixed implied volatilities is called the sticky-strike delta. Regardless of the particular parametrization used for vanilla option prices, once the deltas of the hedging vanilla options are included, the "real" delta is recovered.

The delta of the local volatility model - computed with a fixed local volatility function - has no particular significance or usefulness. Furthermore the delta of a vanilla option in the local volatility model is an irrelevant notion. More generally, the issue of outputting a delta of one asset (a vanilla option) on another (the spot) is irrelevant in a market model.

## 9.7. Vega Hedge

Which portfolio of vanilla options immunizes our derivative position at order one against all perturbations of the local volatility function? Denote by $\mu(\tau, K)$ the density of vanilla options of maturity $\tau$, strike $K$, in the hedging portfolio. $\mu(\tau, K)$ is given by formula (2.121):
$$
\mu(\tau, K)=-\frac{1}{K^{2}} \mathcal{L} \phi(\tau, K)
$$
where $\phi(t, S)$ is the conditional dollar gamma, given by formula (2.118):
$$
\phi(t, S)=E_{\sigma}\left[S^{2} \frac{d^{2} P}{d S^{2}}(t, S, \bullet) \mid S, t\right]
$$
and operator $\mathcal{L}$ is defined by:
$$
\mathcal{L} f=\frac{d f}{d t}+(r-q) S \frac{d f}{d S}+\frac{1}{2} S^{2} \frac{d^{2}}{d S^{2}}\left(\sigma^{2}(t, S) f\right)-r f
$$
In the case of a flat local volatility function, $\phi(t, S)$ is easily computed in a Monte Carlo simulation - see expression (2.123).

## 9.8. Markov-Functional Models

- In Markov-functional models, $S_{t}$ is a function of a process $W_{t}: S_{t}=f\left(t, W_{t}\right)$, where $W_{t}$ is typically a Brownian motion or an Ornstein-Ühlenbeck process. Markovfunctional models are special instances of local volatility and can be calibrated at most to the smile of a single maturity. Smiles for intermediate maturities depend on the choice for the underlying process $W_{t}$
- Pricing a multi-asset European derivative using a Gaussian copula together with marginals supplied by each asset's respective vanilla smile is equivalent to pricing with a multi-asset local volatility model, with the correlation matrix equal to the Gaussian copula's correlation matrix.