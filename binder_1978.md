# Binder (1978): Partition selection through pairwise loss

**Reference:** Binder, D. A. (1978). *Bayesian cluster analysis*. **Biometrika**, 65(1), 31–38. [Paper](https://doi.org/10.1093/biomet/65.1.31).

**Published idea.** Evaluate a partition by the cost of separating observations that belong together and merging observations that belong apart. Choose an action that minimizes posterior expected loss.

## Proposed BART sketch

Fix reference covariates $x_1,\ldots,x_n$. Let $A_{ij}(T)$ equal one when $x_i$ and $x_j$ share a leaf in tree $T$, and zero otherwise. From $B$ saved draws with $H$ component trees each, calculate

$$
P_{ij}=\frac{1}{BH}\sum_{b=1}^{B}\sum_{h=1}^{H}A_{ij}(T_h^{(b)}).
$$

The underlying random partition is that of a uniformly selected component tree from a posterior ensemble draw. Choose positive costs $c_{\mathrm{split}}$ for separating a pair and $c_{\mathrm{merge}}$ for merging a pair.

For a candidate tree $T$, the posterior expected pairwise loss is

$$
R(T)=\sum_{i<j}\left[
c_{\mathrm{split}}P_{ij}\{1-A_{ij}(T)\}
+c_{\mathrm{merge}}(1-P_{ij})A_{ij}(T)
\right].
$$

Choose a candidate set $\mathcal C$ of interpretable covariate-rule trees, then select

$$
\widehat T\in\arg\min_{T\in\mathcal C}R(T).
$$

The simplest candidate set is the saved component trees. Newly constructed small trees can also be included. Optimization is over complete tree partitions, so all pairwise assignments are jointly consistent.

## Algorithm sketch

1. Calculate $P$ from the saved component trees.
2. Specify the two costs and the candidate tree set $\mathcal C$.
3. Score each candidate using $R(T)$.
4. Return a minimizing tree and its leaf rules.

**Connection to Dahl:** With equal costs and the same candidate set, minimizing this risk gives the same minimizers as Dahl's squared-distance criterion.
