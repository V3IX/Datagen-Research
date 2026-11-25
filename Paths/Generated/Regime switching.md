# Regime switching

One of the [[Paremetric economic generators]]. Source: [riskhub — Regime switching models explained](https://riskhub.org/blogs/regime-switching-models-explained).

Markets behave differently at different times: long quiet stretches, then sudden announcements about NVIDIA CEO romance. A model with a single fixed set of parameters (or a single distribution) cannot describe both.

Regime switching models assume the market is always in one of several hidden **regimes**, each with its own mean, volatility and correlation, and moves between them by fixed probabilities. This one idea explains volatility clustering, fat tails and correlation spikes.

## Model

Hidden state $s_t \in \{1, \dots, K\}$; here $K = 2$ (1 = calm, 2 = crisis). Conditional on the regime, the return is normal with a regime-specific mean and variance:

$$r_t \mid (s_t = j) \sim \mathcal{N}(\mu_j, \sigma_j^2)$$

Example (daily): calm $\mu_1 = 0.05\%$, $\sigma_1 = 0.8\%$; crisis $\mu_2 = -0.15\%$, $\sigma_2 = 2.5\%$. Every day's return comes from **one** of these two bells; we never observe which, we can only infer it.

## Transition matrix

The core of the model. $p_{ij}$ is the probability of moving from regime $i$ today to regime $j$ tomorrow; each row sums to $1$.

$$P = \begin{bmatrix} p_{11} & p_{12} \\ p_{21} & p_{22} \end{bmatrix}, \qquad \text{e.g. } P = \begin{bmatrix} 0.98 & 0.02 \\ 0.10 & 0.90 \end{bmatrix}$$

Large diagonal entries mean regimes persist — this is where volatility clustering comes from.

Expected time spent in regime $i$ (mean of a geometric distribution):

$$\mathbb{E}[\text{duration}_i] = \frac{1}{1 - p_{ii}} \quad\Rightarrow\quad \text{calm: } 50 \text{ days, crisis: } 10 \text{ days}$$

Long-run share of time in each regime ($\pi P = \pi$, $\pi_1 + \pi_2 = 1$):

$$\pi_1 = \frac{p_{21}}{p_{12} + p_{21}} \approx 0.83, \qquad \pi_2 = \frac{p_{12}}{p_{12} + p_{21}} \approx 0.17$$

## Fat tails

Inside each regime returns are normal, but unconditionally they are a mixture of normals:

$$f(r) = \pi_1\, f_1(r) + \pi_2\, f_2(r)$$

Where $f_n(r)\sim \mathcal{N}(\mu_n, \sigma_n^2)$ 
The mixture has fat tails and excess kurtosis even though neither component does. For the example the overall volatility is $\approx 1.26\%$ — a single normal with that volatility underestimates crisis days and overestimates calm ones.

## Generating a series

Draw $s_0$ from $\pi$, then for each $t$: draw $s_t$ from row $s_{t-1}$ of $P$ → draw $r_t \sim \mathcal{N}(\mu_{s_t}, \sigma_{s_t}^2)$.

Regime correlations work the same way: one correlation matrix per regime (e.g. $0.3$ calm, $0.8$ crisis), which reproduces diversification breaking down in crises.

## Inferring the regime (Hamilton filter)

On real data, the regime is estimated day by day:

1. Predict: $P(s_t = j) = \sum_i P(s_{t-1} = i)\, p_{ij}$
2. Update with the observed return (Bayes), $f_j$ = normal density of regime $j$:
$$P(s_t = j \mid r_t) = \frac{P(s_t = j)\, f_j(r_t)}{\sum_k P(s_t = k)\, f_k(r_t)}$$

Example: yesterday $95\%$ calm; prediction gives $6.4\%$ crisis; after a $-2\%$ day the crisis probability rises to $\approx 31\%$. (The source gives $14.3\%$; its calm density $f_1$ is miscalculated — $\approx 1.87$, not $4.93$.)

Parameters ($\mu_j, \sigma_j, P$) are estimated by maximum likelihood via EM (Baum–Welch). **Filtered** probabilities use data up to $t$ (real time); **smoothed** probabilities use the whole sample (hindsight).

## Limitations

- The number of regimes $K$ is our choice; more regimes fit better in-sample and risk [[overfitting]].
- $P$ is constant: the model describes how long regimes last, not what triggers them. It cannot predict the start of a crisis.
- Within a regime volatility is constant — unlike [[GARCH]]. Markov-switching GARCH gives each regime its own GARCH process.
- Estimation can get stuck in local maxima and be unstable on rolling windows.

## Questions

- $p_{22}$ is estimated from the crisis days in the history — a handful of crises. How precise can it be, and what happens to the generated data if it is off by $0.05$?
- A bootstrap of the crisis periods and this model both produce "crisis days". What can this model produce that the bootstrap cannot?
