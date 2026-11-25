# Jump diffusion

One of the [[Paremetric economic generators]]. Source: [riskhub — Jump diffusion models in financial markets](https://riskhub.org/blogs/jump-diffusion-models-in-financial-markets-246) (§2, §3, §5, §7.3, §10.2; option pricing parts skipped).

Brownian motion moves prices only continuously, in small steps. Real markets also make sudden discontinuous moves (1987: −22.6% in one day; 2015: the Swiss franc after the SNB dropped its peg). Jump diffusion = Brownian motion + jumps arriving at random times.

## Poisson process (when jumps happen)

$N_t$ counts jumps up to time $t$; $\lambda$ is the average number of jumps per unit of time.

$$P(N_t - N_s = k) = \frac{[\lambda(t-s)]^k}{k!}\, e^{-\lambda(t-s)}, \qquad k = 0, 1, 2, \dots$$

$\mathbb{E}[N_T] = \mathrm{Var}(N_T) = \lambda T$. Time between jumps is exponential with mean $1/\lambda$.

Compound Poisson (jumps with random sizes $Y_k$, i.i.d.):

$$J_t = \sum_{k=1}^{N_t} Y_k, \qquad \mathbb{E}[J_t] = \lambda t\, \mathbb{E}[Y], \qquad \mathrm{Var}(J_t) = \lambda t\, \mathbb{E}[Y^2]$$

## Merton model

$$dS_t = (r - \lambda\kappa)\, S_t\, dt + \sigma S_t\, dW_t + S_{t-}(e^J - 1)\, dN_t$$

- $\sigma$ — volatility of the continuous (Brownian) part
- $\lambda$ — jump intensity
- $J \sim \mathcal{N}(\mu_J, \sigma_J^2)$ — log jump size
- $\kappa = \mathbb{E}[e^J - 1] = e^{\mu_J + \sigma_J^2/2} - 1$ — average relative jump; $-\lambda\kappa$ in the drift cancels the average effect of jumps
- $S_{t-}$ — price just before the jump

The article uses the risk-free rate $r$ (option pricing). For generating real price paths, $r$ is replaced by the real drift $\mu$.

Log-price $X_t = \ln S_t$:

$$X_T = X_0 + \left(r - \tfrac{\sigma^2}{2} - \lambda\kappa\right)T + \sigma W_T + \sum_{k=1}^{N_T} J_k$$

Given the number of jumps, the log-return is normal:

$$X_T \mid (N_T = n) \sim \mathcal{N}\!\left(X_0 + \left(r - \tfrac{\sigma^2}{2} - \lambda\kappa\right)T + n\mu_J,\;\; \sigma^2 T + n\sigma_J^2\right)$$

So unconditionally it is a Poisson-weighted mixture of normals — same idea as the mixture in [[Regime switching]].

Moments of the log-return $X_T - X_0$:

$$\mathrm{Var} = \sigma^2 T + \lambda T(\mu_J^2 + \sigma_J^2)$$

$$\text{Skew} = \frac{\lambda T\, \mu_J(\mu_J^2 + 3\sigma_J^2)}{\left[\sigma^2 T + \lambda T(\mu_J^2 + \sigma_J^2)\right]^{3/2}}, \qquad \text{Excess kurt} = \frac{\lambda T(\mu_J^4 + 6\mu_J^2\sigma_J^2 + 3\sigma_J^4)}{\left[\sigma^2 T + \lambda T(\mu_J^2 + \sigma_J^2)\right]^2}$$

- $\mu_J < 0$ → negative skew (crashes bigger than rallies)
- skew decays as $1/\sqrt{T}$, excess kurtosis as $1/T$ → jumps matter most on short horizons; over long horizons returns look normal again (the article says both decay as $1/T$ — wrong by its own formulas)

## Generating a series (Monte Carlo)

Step $\Delta = t_{k+1} - t_k$, $Z_k \sim \mathcal{N}(0,1)$, $N_k \sim \text{Poisson}(\lambda\Delta)$:

$$X_{t_{k+1}} = X_{t_k} + \left(r - \tfrac{\sigma^2}{2} - \lambda\kappa\right)\Delta + \sigma\sqrt{\Delta}\, Z_k + \sum_{i=1}^{N_k} J_{k,i}$$

(The article allows at most one jump per step; drawing $N_k$ from Poisson is exact for any $\Delta$.)

## Kou model (asymmetric jumps)

Same dynamics, but the log jump size has a double exponential density — separate shapes for up and down jumps:

$$f_J(y) = p\,\eta_1 e^{-\eta_1 y}\,\mathbb{1}_{\{y \ge 0\}} + (1-p)\,\eta_2 e^{\eta_2 y}\,\mathbb{1}_{\{y < 0\}}$$

- $p$ — probability a jump is up; $p < 0.5$ for equities
- $1/\eta_1$, $1/\eta_2$ — average size of up / down jumps ($\eta_1 > 1$, $\eta_2 > 0$)
- heavier tails than normal jumps

$$\kappa = \frac{p\,\eta_1}{\eta_1 - 1} + \frac{(1-p)\,\eta_2}{\eta_2 + 1} - 1$$

## Hawkes process (self-exciting jumps)

In Merton $\lambda$ is constant, so jumps are independent: one crash does not make the next one more likely. Hawkes makes the intensity rise after every jump and decay back:

$$\lambda_t = \lambda_0 + \sum_{t_k < t} \alpha\, e^{-\beta(t - t_k)}$$

- $\lambda_0$ — baseline intensity (written $\mu$ in the article; renamed here to avoid a clash with drift)
- $\alpha$ — how much each jump raises intensity, $\beta$ — how fast it decays
- $\alpha / \beta < 1$ — required for stability (otherwise jumps trigger jumps without end)

## Questions

- Merton with $\lambda = 1$ per year: how many jumps does 20 years of history contain, and how precisely can $\mu_J$ and $\sigma_J$ be estimated from them?
- GARCH, regime switching and jump diffusion all produce fat tails. If you only see a histogram of returns, can you tell which one generated it? What would you look at instead?
