
Belongs to the same group as [[Validation on historical data]]: a non-parametric statistical method for reusing the data we have.

Given data $X = \{x_1, x_2, \dots, x_n\}$, a bootstrap sample $X_b$ is $n$ elements drawn from $X$ uniformly **with replacement**, so some elements appear several times and others not at all.

Choose the number of bootstrap samples $B$ (typically $10^3$–$10^4$). There are $n^n$ possible ordered samples, so $B$ is practically unlimited. A sample could also be made longer than $n$, increasing possibilities (which guarantees at least one repeat), but I don't see a reason to.

For each sample $b = 1, \dots, B$:

$$\hat{\theta}_b = \arg\max_{\theta} R(\theta;\, X_b)$$

where $X_b$ is the $b$-th bootstrap sample (for brevity, the train/test split is left out of this notation).

We end up with $B$ models. Averaging their results $R(\hat{\theta}_b;\, X_b)$ smooths out extreme values.

Notes:
- Averaging reduces variance, not bias: each $R(\hat{\theta}_b;\, X_b)$ is scored on the data it was fitted on, so it is still overfitted. The honest score uses the out-of-bag points, the ones not in $X_b$ (about $(1 - 1/n)^n \approx e^{-1} \approx 37\%$ of $X$).
- Drawing single elements independently destroys time order (e.g. volatility clustering). For time series, whole blocks of consecutive observations are resampled instead (block bootstrap).
