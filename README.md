# Datagen — Research

Research repository for a paper asking: **in which situations does choosing trading strategies on synthetic market data beat choosing them on historical data alone?** The paper is the entry point for Datagen, a synthetic market data product.

## Why this paper

Most trading today is done by automated strategies chosen on historical data. That data is:

- **limited** — a few decades, with only a handful of crashes and regime changes;
- **biased** — one realised path, dominated by a single market environment (e.g. the US over the last 50 years).

Strategies chosen on it tend to be overfitted: their backtested performance overstates what they do on unseen data. Existing answers stay inside the one history — careful splitting (hold-out, walk-forward, CPCV), resampling (bootstrap), and multiple-testing corrections (Deflated Sharpe Ratio).

Synthetic data is the other option: generate many plausible histories (GARCH, regime switching, jump diffusion, deep generative models, agent-based models) and choose strategies on those. Whether this actually leads to better strategies — and when — has not been settled. A generator fitted to one history cannot contain more information than that history, so any gain has to come from the structure the generator adds. That makes the answer conditional, not yes/no.

## Research question

Compared with selection on historical data (plain history, CPCV, bootstrap), **under which conditions** does selection on synthetic data (and synthetic data creation methods) choose strategies that perform better on held-out real data — and under which does it do worse?

Candidate conditions (hypotheses, to be tested):

- length of available history (long vs short, e.g. young or small markets);
- whether a regime change happens between selection and use;
- how much the strategies depend on rare events (tails, crashes);
- how many strategies are compared, and how flexible they are;
- which generator is used, and how scenarios are weighted.

## Main limitation

A generator only adds what its assumptions add. Where those assumptions are wrong, synthetic data can make selection worse, not better — which is why a large part of these notes is about the generators themselves. 
Generator is based on market data - so its output wont produce anything new - TO CHECK
Strategies may be overfit to current regime, but this might be intended. If we would be able to train robust strategy, that would survive every market scenario, its return would be lower than strategy that is overfit to current (USA leads, AI bubble ect) regime - TO CHECK

## Notes

After reding README, start at [[Problem overview]]

## Opening the notes

The notes are an [Obsidian](https://obsidian.md) vault. They also render on GitHub, except for two community plugins they rely on:

- **LaTeX Suite** (`obsidian-latex-suite`) — for writing the math;
- **Document Comments** (`document-comments`) — comments appear as `<!--co:...-->` blocks in the raw files.

The `.obsidian/` folder is not tracked, so install these plugins yourself after cloning.

## Status

Work in progress. Research questions and approach are drafts. Notes are written in English.
