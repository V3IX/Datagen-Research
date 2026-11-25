# GARCH

Generalized Auto Regressive Conditional Heteroskedasticity. One of the [[Paremetric economic generators]].

A non-linear stochastic process that generates time series with time-varying volatility. It replaces the constant-volatility assumption of geometric Brownian motion with something closer to real markets, so the output resembles market behaviour statistically more closely (volatility clustering).

GARCH(1,1): today's variance depends on yesterday's shock and yesterday's variance.

$$\sigma_t^2 = \omega + \alpha\,\epsilon_{t-1}^2 + \beta\,\sigma_{t-1}^2$$

- $\omega > 0$ — baseline variance level
- $\alpha \ge 0$ — reaction to the previous market shock
- $\beta \ge 0$ — persistence of volatility (the key GARCH term)
- $\alpha + \beta < 1$ — required for the process to be stationary; then the long-run variance is $$\bar\sigma^2 = \dfrac{\omega}{1 - \alpha - \beta}$$ The closer $\alpha + \beta$ is to $1$, the longer volatility shocks last. At $\alpha + \beta = 1$ (IGARCH) the long-run variance does not exist.

Realised shock, fed into both the return and the variance equation:

$$\epsilon_t = \sigma_t\, z_t, \qquad z_t \sim \mathcal{N}(0,1) \text{ i.i.d.}$$

Market return:

$$r_t = \mu + \epsilon_t$$

Generating a series: start from $\sigma_0^2 = \bar\sigma^2$, then for each $t$: compute $\sigma_t^2$ → draw $z_t$ → $\epsilon_t$ → $r_t$.
