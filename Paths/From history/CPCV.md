# CPCV — Combinatorial Purged Cross-Validation

Source: López de Prado, *Advances in Financial Machine Learning* (2018), ch. 12.

A validation method on [[Validation on historical data|historical data]]. Instead of one train/test split (as in [[plain history]]), it builds many splits and combines their test results into several complete backtest paths. The output is a **distribution** of out-of-sample performance instead of a single number.

## Setup

- $T$ observations in chronological order, partitioned into $N$ contiguous groups $G_1, \dots, G_N$.
- In each split, $k$ groups are used for testing and $N-k$ for training.

Split matrix — the set of binary row vectors of length $N$; each row is one split, each column one time group, $v_n = 1$ means $G_n$ is a test group:

$$M = \Big\{ v \in \{0,1\}^N \;:\; \sum_{n=1}^{N} v_n = k \Big\}$$

Number of splits:

$$|M| = \binom{N}{k}$$

Each group is a test group in $\binom{N-1}{k-1}$ splits, so the number of complete backtest paths is

$$\varphi(N,k) = \frac{k}{N}\binom{N}{k} = \binom{N-1}{k-1}$$

## Example: $N = 6$, $k = 2$

$\binom{6}{2} = 15$ splits, $\varphi = \binom{5}{1} = 5$ paths.

![[Pasted image 20261111000829.png]]

Reading the figure: columns are splits, rows are groups. A coloured cell means the group is tested in that split; the number is the path the result is assigned to. Each path takes, for every group $G_n$, the test result from a different split, so every path covers the whole timeline exactly once.

## Purging

A label is determined over a time interval: observation $i$ uses $[t_{i,0},\, t_{i,1}]$. A training observation whose label interval overlaps a test group carries information about the test period. Remove training observation $i$ if

$$[t_{i,0},\, t_{i,1}] \;\cap\; \bigcup_{n:\, v_n = 1} G_n \neq \varnothing$$

## Embargo

Serial correlation can extend beyond the label interval. After each test group, additionally drop the next $h = \lceil \alpha T \rceil$ training observations (typically $\alpha \approx 0.01$).

## Procedure

1. Choose $N$ and $k$.
2. For each of the $\binom{N}{k}$ splits: purge, embargo, fit on the training groups, predict on the test groups.
3. Assemble the $\varphi$ paths.
4. Compute the metric on each path → $\{\widehat{SR}_1, \dots, \widehat{SR}_\varphi\}$. Report the mean and the dispersion.

## Trade-offs

- **Cost:** $\binom{N}{k}$ model fits. $N = 10, k = 2$ → 45 fits, 9 paths.
- **Training size:** each split trains on $\frac{N-k}{N}$ of the data, minus purged and embargoed observations. Larger $k$ → more paths, less training data.
- **Paths are not independent:** they share training data and the same underlying history. $\varphi$ paths $\neq$ $\varphi$ independent histories.
- **Still one history:** CPCV measures variability across splits of one realised history, not across possible histories.