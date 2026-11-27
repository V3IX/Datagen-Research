From [[Problem overview]]:
$$  score_k = \sum_m w_m R(\hat{\theta}_k; path_m) $$
$m$ and $path_m$ - chosen by [[Path]], output of function that will create data paths - this is our main task
$w_m$ - scenario weighting. For [[plain history]] paths, its straight forward, but for generated ones, is core question of our paper - do we lower weight of less "real" scenario? why? how do we know its less real?
$R$ - evaluation method, important part, since having it wrong, we will output <!--c:ni9zc-->wrong strategies<!--/c:ni9zc-->
I dont think we should stick to only one, like portfolio return, but have multiple evaluators, like return, maximum drawdown, etc, and create our own evaluation method connecting those
$\hat{\theta}_k$ - fitted params of strategy k. Since k are different strategies, they will have differences in parameters they are taking
<!--co:ni9zc by:me at:2026-11-27T17:40:14.168Z status:open quote:"wrong strategies"
me (2026-11-27T17:40:14.168Z): wrong strategies? what does it mean?
-->


By strategy k we mean, <!--c:whdkx-->deterministic<!--/c:whdkx--> algorithm, that will decide what to buy/sell given market data. <!--co:whdkx by:me at:2026-11-27T17:40:35.939Z status:open quote:"deterministic"
me (2026-11-27T17:40:35.939Z): should strategies be deterministic? do we lose anything if they are?
-->This algorithm takes fitted parameters $\hat{\theta}$ and market data as an input, and produces sell/buy indicators, that we simulate on market and evaluate returns