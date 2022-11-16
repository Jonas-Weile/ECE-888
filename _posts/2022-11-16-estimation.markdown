---
layout: post
title:  "Stage 3: Estimation of Causal Effects"
date:   2022-11-16 12:15:00 -0500
categories: project1
---

We are now ready to perform **estimation**  of our causal estimand of interest. To perform this estimation we will use **BART**[^1]. BART was chosen due to the simplicity of performing the modelling, as well as the promising performance results that BART has showcased in previous iterations of the ACIC Data Competition[^2].
The following description of BART will follow [(Hill, 2011)](#fn:3)[^3].

### BART
Bayesian Additive Regression Trees (BART) is a *bayesian*, *nonparametric* modelling procedure. BART takes as input the outcome, the treatment assignment, and confounding covariates, but requires **no parametric assumptions** about the relationship of these variables. The outcome $$Y$$ is modelled very generally as:

$$ Y = f(z, x) + \epsilon $$

<br/>
where $$z$$ is the received treatment, $$x$$ is the vector of all observed confounding covariates, and $$\epsilon \sim \mathcal{N}(0, \sigma^2)$$ are i.i.d., additive error terms. BART then estimates the function $$f(z, x)$$ as a sum of regression trees:

$$f(z, x) = \sum g(z, x, T_j, M_j)$$

<br/>
where the output of a regression trees takes the form $$g(z, x, T_j, M_j)$$ and are briefly presented below.

<br/>

#### Regression Trees
A tree model consists of a binary tree $$T$$ and a set of a set of parameters for terminal nodes $$M$$.
The binary tree $$T$$ consists of the tree structure itself and a number of decisions rules; one decision rule for each internal node of the tree. The decision rules takes as input a pair $$(z, x)$$ and then sends the pair either left or right down the tree, according to the boolean output of the decision rule. We adopt the convention that a pair $$(z, x)$$ is passed to the right when a decision rule evaluates to $$True$$, and passed to the left otherwise.
The set $$M$$ consists of parameters $$\mu_{i}$$ for each terminal node $$i$$.

A generic example of a tree model is shown in the figure below.

<img src="{{ site.baseurl }}/assets/figures/regtree.drawio.svg">


Then given a tree model $$(T, M)$$ and a pair $$(z, x)$$, the function $$g(z, x, T, M)$$ is defined by taking the input $$(z, x)$$, running it down the tree model, and finally returning the value $$\mu_{i}$$ associated with the terminal node $$i$$ that the input ends at. BART thus models the function $$f(z, x)$$ as a sum of the output of a large number of tree models.

<br/>

#### Regularization Priors
Besides the sum-of-trees, BART consists of a **regularization prior**. To simplify the specification of regularization priors, it is assumed that:

- All trees $$T_i$$ are i.i.d.
- All terminal nodes $$\mu_{i,b}$$ (node b of tree i) are i.i.d given the set of trees, $$T$$.
- The variance $$\sigma^2$$ is independent of all trees $$T_i$$.

Then, [(Chipman, George, and McCulloch, 2010)](#fn:1) defines three prior using three components:

1. a prior preference for trees $$T_j$$ with only a few bottom nodes.
2. a prior that shrinks each set $$M_j$$ toward zero.
3. a prior which suggests that $$\sigma$$ is smaller than that given by least squares.

Finally, with the prior specified, BART then trains by performing a sequence of draws from the posterior using Markov Chain Monte Carlo.
<br/>


### Implementation
We decided to use the pymc_bart implementation of BART to perform estimation. Unfortunately, the memory requirments for running pymc_bart on the full dataset from the challenge far exceeded the available memory on any of the available personal machines - even when looking at the practice level dataset. We note, that this seems to be a problem for some BART implementations in R as well, and the flexBART package developed as part of a submission to the ACIC2022 Data Challenge seems like an interesting way to circumvent these limitations.[^4] It was briefly considered to write the BART algortihm from scratch to deal with this problem, however time was scarce and would not allow this.
Thus, for now, we will *assume* that we have the statististical estimands from BART, and then use these to estimate our causal estimand - but with the hope that we can successfully perform the causal estimation soon, and update this blog post correspondingly.

<br/>

 
### Estimation
Assuming that our causal estimand can be written in the form required for BART, it is then simple to estimate $$f$$ by running BART on our data. Once we have an estimate for $$f$$, we simply plug this estimate into the equation for the ATE as derived in the previous post.

<br/>


### Results
As stated previously, we are not able to present any concrete results at this time, due to the problems with scaling pymc_bart. However, we hope to present some actual data very soon, once a solution to the problem with scaling has been found.
In the meantime, we draw from the conclusions of [Hill, 2011](#fn:3), namely that BART has been shown to produces more accurate estimates of average treatment effects (using the default priors) compared to a number of other methods while requiring no parametric assumptions.[^3] This makes BART easy to use for practitioners, as one does not need to come up with suitable priors to replace the defaults, nor even to perform cross-validation as several generic values of the number of trees has been shown to perform well.

<br/>


### References

[^1]: Chipman, H. A., George, E. I., and McCulloch, R. E. (2010). BART: Bayesian additive regression trees. The Annals of Applied Statistics, 4(1):266–298.

[^2]: Dorie, V.,  Hill, J., Shalit, U., Scott M., and Cervone, D. (2019). Automated versus Do-It-Yourself Methods for Causal Inference: Lessons Learned from a Data Analysis Competition. Statist. Sci., 34(1):43-68. [https://doi.org/10.1214/18-STS667](https://doi.org/10.1214/18-STS667).

[^3]: Hill, J.L., (2011). Bayesian nonparametric modeling for causal inference. Journal of Computational and Graphical Statistics, 20(1):217–240.

[^4]: Kokandakar, A. H., Kang, H.,  Deshpande, S. K., (2022). Sensitivity of Bayesian Casual Forests to Modeling Choices: A Re-analysis of the 2022 ACIC Data Challenge. [https://arxiv.org/abs/2211.02020](https://arxiv.org/abs/2211.02020).
