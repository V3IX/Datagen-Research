list
- [[GARCH]]
- [[Regime switching]]
- [[Jump diffusion]]

funcions that aim to recreate market data, as similary as possible - they existence is not depending on fitting parameteres, we can change funcion variables, minimalizing difference between function output and focused stock - but those functions will work without this
Starting from most basic one, it will be brownian motion - random funcion, describing continuous, random position fluctuations caused by countless tiny collisions in a medium or unpredictable changes over time.
then we have [[GARCH]] witch changes volaitlity bechaviour - but it still has something constant, and this is volatilty change
[[Regime switching]] changes this, creating regimes


|Feature|GARCH (1,1)|Regime Switching|
|---|---|---|
|Volatility dynamics|Smooth, continuous decay|Discrete jumps between levels|
|Hidden state|None (variance is a function of past data)|Yes (latent regime )|
|Captures fat tails|Partially|Yes, via regime mixture|
|Mean returns|Usually constant|Can differ by regime (bull vs bear)|
|Estimation|MLE, straightforward|MLE via EM, harder to identify|
