# Sivaganesan, Müller and Huang (2017): A predictive subgroup sketch

**Reference:** Sivaganesan, S., Müller, P., and Huang, B. (2017). *Subgroup finding via Bayesian additive regression trees*. **Statistics in Medicine**, 36(15), 2391–2403. [Paper](https://doi.org/10.1002/sim.7276). Relevant material: Sections 2–3, especially equation (4).

**Published idea.** Fit BART for the response, then choose a covariate-defined subgroup by maximizing a utility based on predicted treatment benefit, subgroup size, and simplicity. Higher response values are taken to indicate greater benefit.

## Predictive quantities and utility

Let $f^{(b)}(x,t)$ be a draw of the full BART conditional mean, with treatment $t\in\{0,1\}$. At reference covariates $x_1,\ldots,x_n$, estimate the posterior predictive mean contrast by

$$
\bar\tau_i=\frac1B\sum_{b=1}^{B}
\left[f^{(b)}(x_i,1)-f^{(b)}(x_i,0)\right].
$$

For a candidate subgroup $A$, let $n_A$ count its reference observations and set

$$
\bar\tau_A=\frac1{n_A}\sum_{i:x_i\in A}\bar\tau_i,
\qquad
\bar\tau_{\mathrm{all}}=\frac1n\sum_i\bar\tau_i.
$$

Using simplified notation, the paper's posterior expected utility is

$$
U(A)=\frac{(n_A-n_{\min})_+^d}{(1+c)^{p_A-1}}
\left[\bar\tau_A-\bar\tau_{\mathrm{all}}-\delta_0\right],
\qquad U(\varnothing)=0.
$$

Here, $(z)_+=\max(z,0)$, $p_A$ counts the covariates defining $A$, $n_{\min}$ is a minimum size threshold, and $\delta_0$ is the required elevation above the overall effect. Positive constants $c,d$ control the simplicity penalty and size reward.

## Minimal proposed partition adaptation

Specify a collection of candidate subgroup rules $\mathcal A$, such as intersections of covariate thresholds. Choose

$$
A^\star\in\arg\max_{A\in\mathcal A\cup\{\varnothing\}}U(A).
$$

Use the empty action when no candidate has positive utility. For a positive-utility selection, report the partition

$$
\widehat\pi=\{A^\star,\mathcal X\setminus A^\star\}.
$$

Otherwise, report the one-group partition $\{\mathcal X\}$. This adaptation completes the selected subgroup with its complement; it need not be a tree with exactly two leaves.

## Algorithm sketch

1. Calculate full-ensemble treatment contrasts at the reference covariates.
2. Average the contrasts within each candidate subgroup and over the full reference sample.
3. Calculate $U(A)$ and select a positive-utility maximizer, if one exists.
4. Report its covariate rule and complement, or the whole population as one group.
