# Castillo and Ročková (2021): A median-tree sketch

**Reference:** Castillo, I., and Ročková, V. (2021). *Uncertainty Quantification for Bayesian CART*. **The Annals of Statistics**, 49(6), 3482–3509. [Paper](https://doi.org/10.1214/21-AOS2093). Relevant material: Section 3.2, Definition 4 and equation (29).

**Published idea.** On a shared dyadic tree hierarchy, retain internal nodes whose marginal posterior inclusion probability is at least one-half. These nodes determine a median tree.

For a node $v$ and the internal-node set $\mathcal I(T)$, write

$$
p_v=\Pr\{v\in\mathcal I(T)\mid D\},
\qquad
\widehat{\mathcal I}=\{v:p_v\geq 1/2\}.
$$

Within the shared hierarchy, inclusion of a child implies inclusion of its ancestors. The thresholded internal-node set therefore respects ancestry; adding its terminal children completes the tree.

## Proposed BART adaptation

Choose a fixed finite reference hierarchy $\mathcal H$ of covariate splits. Every node must represent the same region and split across draws. Candidate summary trees are valid prunings of this hierarchy.

For each posterior draw $b$, evaluate the full BART mean function

$$
f^{(b)}(x_i)=\sum_{h=1}^{H}g_h^{(b)}(x_i)
$$

at reference covariates $x_1,\ldots,x_n$. Approximate these values by a tree $\widetilde T^{(b)}$ within the reference hierarchy, using squared approximation error plus a leaf-count penalty. Use the same penalty across draws and require nonempty leaves on the reference sample.

Calculate node-inclusion frequencies in these projected trees:

$$
\widehat p_v=\frac1B\sum_{b=1}^{B}
\mathbf 1\{v\in\mathcal I(\widetilde T^{(b)})\}.
$$

Retain nodes with $\widehat p_v\geq1/2$ and complete the terminal leaves. These frequencies describe the projected trees induced by full BART draws.

## Algorithm sketch

1. Specify the reference hierarchy and common complexity penalty.
2. Fit a summary tree to each full BART function draw within that hierarchy.
3. Count internal-node inclusions across the summary trees.
4. Apply the one-half threshold and return the completed median tree.

**Attribution:** The median-node rule is from the paper. Projecting BART draws onto a reference hierarchy before applying that rule is the proposed adaptation here.
