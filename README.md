# Datagen — Research

Research repository for a paper asking: **in which situations does choosing trading strategies on synthetic market data beat choosing them on historical data alone?** The paper is the entry point for Datagen, a synthetic market data product.

## Why this paper

Most trading today is done by automated strategies chosen on historical data. That data is:

- **limited** — a few decades, with only a handful of crashes and regime changes;
- **biased** — one realised path, dominated by a single market environment (e.g. the US over the last 50 years).

Strategies chosen on it tend to be overfitted: their backtested performance overstates what they do on unseen data. Existing answers stay inside the one history — careful splitting (hold-out, walk-forward, CPCV), resampling (bootstrap), and multiple-testing corrections (Deflated Sharpe Ratio).

Synthetic data is the other option: generate many plausible histories (GARCH, regime switching, jump diffusion, deep generative models, agent-based models) and choose strategies on those. Whether this actually leads to better strategies — and when — has not been settled. A generator fitted to one history cannot contain more information than that history, so any gain has to come from the structure the generator adds. That makes the answer conditional, not yes/no.

## Research question

Compared with selection on historical data (plain history, CPCV, bootstrap), **under which conditions** does selection on synthetic data choose strategies that perform better on held-out real data — and under which does it do worse?

Candidate conditions (hypotheses, to be tested):

- length of available history (long vs short, e.g. young or small markets);
- whether a regime change happens between selection and use;
- how much the strategies depend on rare events (tails, crashes);
- how many strategies are compared, and how flexible they are;
- which generator is used, and how scenarios are weighted.

## Approach (draft)

1. Choose strategies with each method: plain history, CPCV, bootstrap, synthetic data (several generators and weighting schemes).
2. Evaluate the chosen strategies on held-out real data, across several markets and time windows.
3. Compare methods per condition and test whether the differences are significant.

## Main limitation

A generator only adds what its assumptions add. Where those assumptions are wrong, synthetic data can make selection worse, not better — which is why a large part of these notes is about the generators themselves.

## Notes

**Validation on historical data** — what is done today, and its limits

- [Overfitting](overfitting.md) — the overfitting gap, selection bias, why finance is especially exposed
- [Validation on historical data](Validation%20on%20historical%20data.md) — overview of methods
- [Plain history](plain%20history.md) — single chronological train/test split
- [CPCV](CPCV.md) — Combinatorial Purged Cross-Validation
- [Resampling (bootstrap)](Resampling%20(boostrap).md) — reusing data by sampling with replacement

**Parametric generators** — models that produce synthetic price series

- [Parametric economic generators](Paremetric%20economic%20generators.md) — overview and comparison
- [GARCH](GARCH.md) — time-varying volatility
- [Regime switching](Regime%20switching.md) — hidden market regimes (calm / crisis)
- [Jump diffusion](Jump%20diffusion.md) — sudden discontinuous moves (Merton, Kou, Hawkes)

Suggested reading order: top to bottom.

## Opening the notes

The notes are an [Obsidian](https://obsidian.md) vault. They also render on GitHub, except for two community plugins they rely on:

- **LaTeX Suite** (`obsidian-latex-suite`) — for writing the math;
- **Document Comments** (`document-comments`) — comments appear as `<!--co:...-->` blocks in the raw files.

The `.obsidian/` folder is not tracked, so install these plugins yourself after cloning.

## Status

Work in progress. Research questions and approach are drafts. Notes are written in English; each ends with open questions.
