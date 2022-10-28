---
layout: post
title:  "Stage 2: Identification of Causal Effects"
date:   2022-10-27 15:27:01 -0500
categories: project1
---

In the [previous post](/ece-888/project1/2022/10/05/problem-definition), we described the causal estimation problem, defined the random variables that we are working with, and specified the causal effect we wish to estimate. In this post, we will then perform *identification* of the causal effect.
To do this, we will first need to make a number of assumptions.

<br/>


### Assumptions
The organizers of the 2022 ACIC Data Challenge have conveniently provided the following assumptions that the generated data will satisfy:

- **Ignorability** : 

    Ignorability means that the data will be free of *unmeasured confounding*. In the specific context we are considering, it means that there are no unmeasured covariates that relates to both the treatment assignment and the change in the untreated potential outcome $$ Y_{it}(0) $$ from the non-intervention period to the intervention period:

    $$ Y_{it_{1}}(0) - Y_{it_{0}}(0) \perp Z_{i} \mid W_{i} \text{  for all  } t_{1} : p_{t} = 1, t_{0} : p_{t} = 0 $$

    where $$ p_{t} $$ was defined in the previous post, and $$ W_{i} $$ is the vector of all the measured covariates related to patient $$ i $$.

- **Overlap** : 

    We assume overlap for the treatment group (which includes positivity), i.e.:

    $$ 0 < P(Z_{i} = 1 \mid X_{i}) < 1 \text{ for all } i : Z_{i} = 1 $$

    This assumptions states, that for all configurations of the vector of covariates $$ X_{i} $$ there is a positive probability for patient $$ i $$ to receive treatment. This ensures that all neighborhoods of the confounder space contain patients that have received treatment.



- **SUTVA** : 

    This assumption ensures that all patients receive a single treatment, and that the outcome for each patient does not depend on the assigned treatment of other patients.

<br/>





### Identification
Finally, we are ready to perform identification. Recall that the estimand we are interested in is the Sample Average Treatment effect of the Treated (SATT), defined as follows:

$$ \dfrac{1}{\sum^{4}_{t=3}{N_{t}}} \sum^{4}_{t=3}{\sum_{i: z_{i} = 1}{\left[Y_{it}(1) - Y_{it}(0)\right]}}  $$

However, as we are only looking at the subgroup of patients that were treated, $$ Y_{it}(0) $$ is a counterfactual, as every patient $$ i $$ has $$ Z_{i} = 1 $$. Thus, the expression we have is a *causal estimand*, and we must somehow convert it into a *statistical estimand*. 

However, we do not *yet* have the tools in this course to convert this SATT to a statistical estimand, thus we will make some major simplifying assumptions. We will assume that the SATT can be well approximated by the vanilla *Average Treatment Effect* (ATE):

$$ \dfrac{1}{\sum^{4}_{t=3}{N_{t}}} \sum^{4}_{t=3}{\mathbb{E}\left[Y_{it}(1) - Y_{it}(0) \right]}  $$

$$ = $$

$$ \dfrac{\sum^{4}_{t=3}}{\sum^{4}_{t=3}{N_{t}}} {\left(\mathbb{E}\left[Y_{it}(1)\right] - \mathbb{E}\left[Y_{it}(0)\right]\right)} $$

<br/>
Then, as can be verified from the DAG, we can use backdoor adjustment (which relies on consistency) to get:

$$ \mathbb{E}\left[Y_{it}(a)\right]  = 
        \mathbb{E}\left[\mathbb{E}\left[Y_{it}(a) \mid Z_{i} = a, Y_{i2}, W_{i}\right]\right]  =
        \mathbb{E}\left[\mathbb{E}\left[Y_{it} \mid Z_{i} = a, Y_{i2}, W_{i}\right]\right]$$

<br/>
for $$ t \in \{3, 4\} $$, where $$ W_{i} $$ again stands for all the observable covariates $$ X_j $$ and $$ V_i $$, and where we used SUTVA to perform the last step.
We insert to get our final estimand:

$$ ATE = \dfrac{\sum^{4}_{t=3}}
         {\sum^{4}_{t=3}{N_{t}}} {\left(\mathbb{E}\left[\mathbb{E}\left[Y_{it} \mid Z_{i} = 1, Y_{i2}, W_{i}\right]\right] - \mathbb{E}\left[\mathbb{E}\left[Y_{it} \mid Z_{i} = 0, Y_{i2}, W_{i}\right]\right]\right)} $$


<br/>

This final estimand contains two examples of iterated expectations over the covariates $$ W_{i} $$ as well as the outcome at timestep 2.