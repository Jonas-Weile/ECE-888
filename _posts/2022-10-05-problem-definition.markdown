---
layout: post
title:  "Stage 1: Problem Definition"
date:   2022-10-05 15:27:01 -0500
categories: project1
---

This project aims to create a model for policy evaluation using causal inference. More specifically, the goal of the project is to create a (late) submission to [the American Causal Inference Conference 2022 Data Challenge](https://acic2022.mathematica.org/). This year, the challenge attempts to determine the effect of U.S. health care system interventions that aim to lower Medicare expenditures, by estimating the impact of an intervention on the monthly spending for the average treated patient. The datasets includes data from ~ 300,000 patients from 500 practices over 4 years - or approximately 1.2 million patient-year records per dataset. The dataset contains a number of practices, where intervention is applied to all patients in chosen practices. The first 2 years, no practices receive intervention - then for the 3rd and 4th year, the selected practices receive the intervention. It is noted, that the supplied data is **generated**, but informed by a real intervention. Thus, the intervention for which we actually try to estimate the effect of, is a *generic intervention*.

The project will examine the effectiveness of [Bayesian Additive Regression Trees](https://www.tandfonline.com/doi/abs/10.1198/jcgs.2010.08162) to estimate the causal impacts. As the results for the ACIC 2022 Data Challenge have already been released, it is already known that BART (with some additions) performed well - we will try to replicate this result.

<br/>


### Impact
The ACIC Data Challenge was created to shed light on the effectiveness of different approaches to causal inference in particular observational study settings. The challenge helps researchers understand which methods are best suited to estimate the impacts of policy changes by comparing the different causal methods that are used to solve the generic policy evaluation problem. Then, the penultiame goal is to improve the effectiveness of policy-making by enchancing the quality of evidence obtained from such program evaluations.

Personally, I hope that recreating the results of BART in the ACIC Data Challenge will introduce me to how Machine Learning can be used to estimate **causal impacts** from data.

<br/>


### Random Variables
We define random variables $$ Y_{it} $$ and $$ Z_{i} $$ for all patients $$ i $$ and timesteps $$ t $$ (in years), where $$ Y_{it} $$ denotes the realised outcome for patient $$ i $$ at time $$ t $$ (i.e. monthly Medicare expenditures for patient $$ i $$ in year $$ t $$) and the random variable $$ Z_{i} $$ is an indicator for whether patient $$ i $$ is in the treatment group or control group - the former is modelled by $$ Z_{i} = 1 $$ and the latter by $$ Z_{i} = 0 $$.

Now, having defined random variables for the treatment and the outcome, we turn to modelling the remaining measured variables that are not of direct interest (also called the *covariates*). As mentioned earlier, the dataset is artificially generated and thus these covariates correspond to a number of generic measurements, but we do not know what each covariate actually measures. We define $$ X_{j1}, ..., X_{j9} $$ to model unordered practice-level covariates for each practice $$ j $$, and $$ V_{i1}, ..., V_{i5} $$ to model a mix of continuous, unordered categorical, and binary patient-level covariates for each patient $$ i $$. Further, let $$ X_{j} $$ and $$ V_{i} $$ be vectors of the corresponding covariates for practice $$ j $$ and patient $$ i $$, respectively.

<br/>


### Causal Effects
Using the defined random variables, we can express the causal effect of the individual-level binary treatment assigment, $$ Z_{i} $$. Let $$ Y_{it}(1) $$ and $$ Y_{it}(0) $$ denote the potential outcomes at time $$ t $$ were patient $$ i $$ to receive the treatment or control condition, respectively. Then, the individual-level causal effect of the treatment is defined as the *difference* between the two potential outcomes, $$ Y_{it}(1) - Y_{it}(0) $$. Further, the observed outcome $$ Y_{it} $$ as defined previously can then be reexpressed using the potential outcomes as:

$$ Y_{it} = Z_{i} p_{t} Y_{it}(1) + (1 − Z_{i} p_{t}) Y_{it}(0)  $$

where $$ p_{t} $$ is an indicator variable equal to 1 if time $$ t $$ is in the intervention period, and 0 otherwise.


The goal of the ASIC 2022 Data Challenge is to determine the average treatment effect of the intervention on monthly spending per patient. That is, for each patient $$ i $$, we wish to determine the expected (or average) treatment effect, given that the that patient was actually treated, at each year $$ t $$ in the intervention period. This causal estimand is defined as:

**Average Treatment Effect of the Treated (ATT)** : 

$$ E_{(i \vert z_{i} = 1)} (Y_{it}(1) - Y_{it}(0)) $$

However, we are only interested in the treatment effect for the actual patients in the study; we are not trying to generalize to a broader population. In other words, the goal is to determine the *sample* average treatment effects on the treated (SATT). Further, we are only interested in the $$ SATT $$ for the post-intervention years, thus we average the $$ SATT $$ over years 3 and 4. Formally, the effect is then defined as follows:

**Sample average treatment effect on the treated** : 

$$ \dfrac{1}{\sum^{4}_{t=3}{N_{t}}} \sum^{4}_{t=3}{\sum_{i: z_{i} = 1}{(Y_{it}(1) - Y_{it}(0))}} $$

where $$ N_{t} $$ is the total number of treatment-receiving patients that were observed during year $$ t $$.

<br/>


### Causal Relationships
Finally, we will now specify the causal relationship between the random variables we defined earlier by drawing the corresponding DAG. To create the DAG, we use information given by the organizers of the ACIC Data Challenge 2022, and further make some assumptions.

- From the desciption of the ACIC Data Challenge 2022, we know that *all* of the defined covariates are time-invariant. This means, that the intervention does not affect any covariates, and in general, that no variables affect the covariants.

- We represent all the patient-levet covariates $$ V_{i1}, ..., V_{i5} $$ and practice-level covariates $$ X_{j1}, ..., X_{j9} $$ as single vectors $$ V_{i} $$ and $$ X_{j} $$, respectively.

![DAG describing the causal relationship between variables.](/assets/figures/CausalRelationship.drawio.svg)

<br/>