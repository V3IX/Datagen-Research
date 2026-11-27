market offers vast range of tools, many of which do similar things, but upon deeper analyzing, and technicalities they are not.
Example: [[GARCH]], [[Resampling (boostrap)]], 
one of those does not match others - why?
if we want to compare methods, we need to qualify those into some pipeline.

so we will set 
$$ score_k = \sum_m w_m R(\hat{\theta}_k; path_m)$$
$m$ - path index, [[plain history]] gives $m = 1$, [[Resampling (boostrap)]] gives $B \leq m^m$, ...
$w_m$ - weights that each path gets
$R$ - evaluation method, sharpe ratio, portfolio return, ...
$k$ - numbers the candidate strategies youre choosing between - each strategy for same comparison should get same path and weights
$\hat{\theta}_k$ - fitted parameters of strategy $k$ ([[overfitting]])
$path_m$ - time series of returns, or other data strategy $k$ gets

then we end up with list of scores $(score_1, score_2, \dots, score_k)$, each for some strategy. we define rule, that will pick $\hat{k}$ - best strategy, out of given scores
$$ \hat{k} = rule(score_1, score_2, \dots, score_k)$$

Then to avoid famous [[overfitting]] we check quality of $\hat{k}$
$$quality(method) = R(\hat{\theta}_{\hat{k}}; D_{real, \ held-out})$$
For how we will approach this see [[Architecture overview]]
