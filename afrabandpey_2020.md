# Afrabandpey et al. (2020): A predictive surrogate partition

**Reference:** Afrabandpey, H., Peltola, T., Piironen, J., Vehtari, A., and Kaski, S. (2020). *A decision-theoretic approach for model interpretability in Bayesian framework*. **Machine Learning**, 109, 1855–1876. [Paper](https://doi.org/10.1007/s10994-020-05901-8). Relevant material: Section 3, especially the KL projection and CART formulations.

**Published idea.** Construct an interpretable surrogate by balancing predictive agreement with a flexible reference model against complexity. The paper uses KL divergence in its general formulation and includes BART reference models with tree surrogates.

## Gaussian mean-approximation sketches

Fix reference covariates $x_1,\ldots,x_n$. Let $f^{(b)}(x_i)$ be the full BART conditional mean in draw $b$, and define

$$
\bar f_i=\frac1B\sum_{b=1}^{B}f^{(b)}(x_i).
$$

Let $\mathcal C$ be a candidate set of small trees with nonempty leaves on the reference sample. Write $\ell_T(i)$ for the leaf containing $x_i$, and $K(T)$ for the number of leaves. Choose a complexity penalty $\lambda\geq0$.

### Version 1: Approximate the posterior mean

Choose a tree and one value $a_\ell$ per leaf:

$$
(\widehat T,\widehat a)\in\arg\min_{T\in\mathcal C,a}
\left\{
\frac1n\sum_i[\bar f_i-a_{\ell_T(i)}]^2
+\lambda K(T)
\right\}.
$$

For a fixed tree, each optimal leaf value is the average of $\bar f_i$ within that leaf. This is the simple squared-error specialization for reporting one predictive tree.

### Version 2: Use one partition across posterior draws

Keep the tree fixed across draws, but calculate a separate fitted value within each leaf and draw:

$$
\bar f_\ell^{(b)}(T)=\frac1{n_\ell(T)}
\sum_{i:\ell_T(i)=\ell}f^{(b)}(x_i).
$$

Select

$$
\widehat T\in\arg\min_{T\in\mathcal C}
\left\{
\frac1{Bn}\sum_{b=1}^{B}\sum_i
\left[f^{(b)}(x_i)-\bar f_{\ell_T(i)}^{(b)}(T)\right]^2
+\lambda K(T)
\right\}.
$$

This is a proposed common-partition formulation motivated by predictive projection. It chooses a single partition while allowing its group summaries to vary with the posterior function draw.

## Algorithm sketch

1. Obtain full-ensemble function draws at the reference covariates.
2. Choose Version 1 or Version 2 and the candidate tree set.
3. Calculate each candidate's approximation error and add its complexity penalty.
4. Return a minimizing tree, its leaf rules, and the group summaries.

For treatment-effect partitions, replace the function draws by draw-specific treatment contrasts.
