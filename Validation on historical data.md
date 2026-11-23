
**Goal:** choose a strategy and estimate how it will perform on unseen data, without the estimate being inflated by [[overfitting]].

**Basic loop:** split history into training windows (fit and select) and testing windows (evaluate). Choose the best strategy whose performance holds on the test windows.

The same methods are used for trading strategies and for training ML models.

## Methods

- [[plain history]] — single chronological hold-out split.
- **Walk-forward** — train on $[t_0, t]$, test on $(t, t+\Delta]$, move $t$ forward by $\Delta$ and repeat (expanding or rolling window). Closest to how a strategy is run live. Early periods are trained on little data.
- **Standard k-fold CV** — not valid for time series as is: training folds contain data from after the test fold, and overlapping labels leak information.
- [[CPCV]] — all combinations of test groups, with purging and embargo; produces several complete backtest paths.

| Method | Backtest paths | All data used for testing | Leakage control |
|---|---|---|---|
| Plain history | 1 | no | gap at boundary |
| Walk-forward | 1 | almost (not the first window) | gap at each step |
| CPCV | $\binom{N-1}{k-1}$ | yes | purging + embargo |

## Shared limitation

All of these methods reorganise **the same single history**. They reduce selection bias and give better estimates of variance, but they cannot add information that is not in the data: no new crises, no new regimes. This is the gap synthetic data is meant to address.

## Question

1. ​<!--c:mwrtp-->Input → output<!--/c:mwrtp-->
2. ​<!--c:z0p9z-->Assumptions about the data<!--/c:z0p9z-->
3. How much data it needs, and which parameters someone sets by hand
4. What it can produce that history can’t, and what it never can
5. When it breaks
6. How its authors show it works: looks real vs. works
7. Where it sits in your harness, and what gets fitted on the training window
8. How hard it is to use
9. Which strategy types it favors or punishes
10. Which knob controls scenario diversity, and what happens when you turn it up
<!--co:mwrtp by:me at:2026-11-11T00:32:16.533Z status:open quote:"Input → output"
me (2026-11-11T00:32:16.533Z): input: historical data (no way!!!!)\noutput: maybe-not overfitten strategy
-->
<!--co:z0p9z by:me at:2026-11-11T00:33:37.054Z status:open quote:"Assumptions about the data"
me (2026-11-11T00:33:37.054Z): maches what we will be using strategy on
-->