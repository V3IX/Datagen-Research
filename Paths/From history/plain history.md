# Plain history (hold-out split)

The simplest validation method on [[Validation on historical data|historical data]]. Data is split chronologically into two parts:

$$D = D_{\text{train}} \cup D_{\text{test}}, \qquad |D_{\text{train}}| \approx (0.70\text{–}0.80)\,|D|$$

$D_{\text{train}}$ is the earlier part, $D_{\text{test}}$ the later part.

## Procedure

1. Develop, fit and compare candidate strategies on $D_{\text{train}}$ only. Inspecting performance here is allowed.
2. Choose one (or a few) final candidates.
3. Evaluate them **once** on $D_{\text{test}}$. That result is the estimate of out-of-sample performance.

## Rules

- **Chronological, not random.** Shuffling leaks future information into training (autocorrelation, overlapping labels).
- **The test set is used once.** Modifying a strategy after seeing its test result turns $D_{\text{test}}$ into training data. Choosing the best of $m$ candidates on $D_{\text{test}}$ reintroduces the selection bias described in [[overfitting]].
- **Gap at the boundary.** If a label is computed over $h$ future bars, the last $h$ observations of $D_{\text{train}}$ overlap $D_{\text{test}}$ and must be dropped. This is purging, see [[CPCV]].

## Limitations

- **One estimate, one path.** A 5-year test window contains one crisis or none; the variance of the estimate is high and unknown.
- **One regime.** The result says how the strategy did in that window, not in general.
- **Wasted data.** ~25% is never used for fitting, ~75% is never used for testing.

These limitations motivate [[CPCV]].