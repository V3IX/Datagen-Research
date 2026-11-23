
Any model whose parameters are chosen from historical data — a trading strategy, an ML model, a deep network — is fitted by optimising a score on that data. **Overfitting** is when the chosen parameters capture noise specific to the sample instead of structure that persists. Performance on the data used for selection then overstates performance on data the model has not seen.

## Formal view

Let $D_{\text{in}}$ be the data used for fitting and selection, $D_{\text{out}}$ unseen data from the same process, $\theta$ the parameters and $R$ a performance measure (e.g. Sharpe ratio).

$$\hat\theta = \arg\max_{\theta} R(\theta;\, D_{\text{in}})$$

The overfitting gap:

$$\Delta = R(\hat\theta;\, D_{\text{in}}) - \mathbb{E}\big[R(\hat\theta;\, D_{\text{out}})\big]$$

In expectation $\Delta > 0$. It grows with <!--c:rucn7--><!--c:7kcgm-->model flexibility<!--/c:7kcgm--><!--/c:rucn7--> and with the number of <!--c:4xisg-->configurations tried<!--/c:4xisg-->; it shrinks with the amount of data.
<!--co:4xisg by:me at:2026-11-11T00:03:49.525Z status:open quote:"configurations tried"
me (2026-11-11T00:03:49.525Z): the more, the more overfitten the model is, causing $R(\\hat{\\theta};D_{in})$ to maximize
-->
<!--co:rucn7 by:me at:2026-11-11T00:05:24.178Z status:resolved quote:"model flexibility"
me (2026-11-11T00:05:24.178Z): ?
-->
<!--co:7kcgm by:me at:2026-11-11T00:07:11.263Z status:open quote:"model flexibility"
me (2026-11-11T00:07:11.263Z): ?
-->

## Selection is fitting too

A backtest runs a strategy's rules over historical data and records the result the strategy would have produced, e.g. its Sharpe ratio. It is the score $R(\theta;\, D_{\text{out}})$, computed for one strategy.

​<!--c:0y481-->Overfitting is not limited to training. Choosing the best of many backtests is itself an optimisation over $D_{\text{out}}$.<!--/c:0y481-->
<!--co:0y481 by:me at:2026-11-11T00:11:38.870Z status:open quote:"Overfitting is not limited to training. Choosing the best of many backtests is itself an optimisation over $D_{\text{in}}$."
me (2026-11-11T00:11:38.870Z): interesting problem, when we use training data multiple times (lets say first 3 strategies perform badly, and we must pass them) our test data starts being used more than once, and this contradicts it
-->

Choosing among $K$ backtested strategies is the same $\arg\max$ as above, with $\theta$ ranging over the $K$ strategies instead of over the parameters of one. The more strategies are tried, the more chances noise has to produce a high score. The same applies to test data: if it is checked after every rejected strategy, it takes part in the selection and becomes part of $D_{\text{in}}$.

Size of the effect: take $K$ strategies with no edge, i.e. true Sharpe ratio $0$. Each backtest Sharpe ratio is then pure noise, approximately i.i.d. $\mathcal{N}(0, \sigma^2)$, with $\sigma \approx 1/\sqrt{\text{years}}$ for annualised Sharpe. The best one is expected to show

$$\mathbb{E}\big[\max_k \widehat{SR}_k\big] \approx \sigma \sqrt{2 \ln K}$$

Where $\sqrt{2 \ln K}$ comes from (for standard normals $Z_1, \dots, Z_K$; multiply by $\sigma$ at the end):

1. The maximum sits roughly at the level $x$ that, on average, only one of the $K$ draws exceeds: $K \cdot P(Z > x) \approx 1$.
2. The normal tail decays as $P(Z > x) \approx e^{-x^2/2}$ (a slowly varying factor $\frac{1}{x\sqrt{2\pi}}$ is dropped).
3. $K e^{-x^2/2} \approx 1 \;\Rightarrow\; x^2/2 \approx \ln K \;\Rightarrow\; x \approx \sqrt{2 \ln K}$.

This is only the leading-order growth rate. Dropping the factor in step 2 makes it overestimate: for $K = 1000$ it gives $3.72$, while the exact value is $\approx 3.24\,\sigma$. It also assumes the $K$ backtests are independent.

Example: 10 years of data, 1000 tried strategies, no real edge anywhere → the winner shows an annualised Sharpe of about $1.0$. Correction for this: Deflated Sharpe Ratio (Bailey & López de Prado, 2014)<!--c:c7z31-->.<!--/c:c7z31-->
<!--co:c7z31 by:me at:2026-11-11T00:14:52.317Z status:open quote:"."
me (2026-11-11T00:14:52.317Z): by chosing best way of training strategies (chosing parameters) we even do overfitting
-->

## Why finance is especially exposed

- **Low signal-to-noise** — real edges are small relative to noise, so noise is easy to fit.
- **Non-stationarity** — $D_{\text{out}}$ may come from a different regime than $D_{\text{in}}$. The gap then mixes overfitting with distribution shift.
- **One history** — a single realised path with few crises, which cannot be re-sampled.

These are the limits that [[Validation on historical data]] methods try to work around, and the motivation for synthetic data.