---
layout: post
title:  "Stage 1: Problem Definition"
date:   2022-10-05 15:27:01 -0500
categories: project1
---

This project aims to create a model for policy evaluation using causal inference. More specifically, the goal of the project is to create a (late) submission to [the American Causal Inference Conference 2022 Data Challenge](acic2022). This year, the challenge attempts to determine the effect of U.S. health care system interventions that aim to lower Medicare expenditures, by estimating the impact of an intervention on the monthly spending for the average treated patient. The datasets includes data from ~ 300,000 patients from 500 practices over 4 years - or approximately 1.2 million patient-year records per dataset. The dataset contains a number of practices, where intervention is applied to all patients in chosen practices. The first 2 years, no practices receive intervention - then for the 3rd and 4th year, the selected practices receive the intervention. It is noted, that the supplied data is **generated**, but informed by a real intervention. Thus, the intervention for which we actually try to estimate the effect of, is a *generic intervention*.

The project will examine the effectiveness of [Bayesian Additive Regression Trees](BART) to estimate the causal impacts. As the results for the ACIC 2022 Data Challenge have already been released, it is already known that BART (with some additions) performed well - we will try to replicate this result.


### Impact
The ACIC Data Challenge was created to shed light on the effectiveness of different approaches to causal inference in particular observational study settings. The challenge helps researchers understand which methods are best suited to estimate the impacts of policy changes by comparing the different causal methods that are used to solve the generic policy evaluation problem. Then, the penultiame goal is to improve the effectiveness of policy-making by enchancing the quality of evidence obtained from such program evaluations.

Personally, I hope that recreating the results of BART in the ACIC Data Challenge will introduce me to how Machine Learning can be used to estimate **causal impacts** from data.


### Random Variables
We define random variables $$ Y_{it} $$ and $$ Z_{i} $$ for all patients $$ i $$ and timesteps $$ t $$, where $$ Y_{it} $$ denotes the realised outcome for patient $$ i $$ at time $$ t $$, and the random variable $$ Z_{i} $$ is an indicator for whether patient $$ i $$ is in the treatment group or control group - the former is modelled by $$ Z_{i} = 1 $$ and the latter by $$ Z_{i} = 0 $$.
Then, let $$ Y_{it}(1) $$ and $$ Y_{it}(0) $$ denote the potential outcomes were patient i to receive the treatment or control condition respectively at time t. The observed outcome for patient i can then also be expressed as:

$$ Y{it} = Z_{i} p_{t} Y_{it}(1) + (1 − Z_{i} p_{t}) Y_{it}(0)  $$

where	$$ p_{t} $$ is equal to 1 if time $$ t $$ is in the intervention period.

Finally, we define random variables $$ X_{j1}, ..., X_{j9} $$ to model unordered practice-level covariates for each practice j, and $$ V_{i1}, ..., V_{i5} $$ for each patient $$ i $$ to model continuous, unordered categorical, and binary patient-level covariates.


### Causal Effects
The goal of the ASIC 2022 Data Challenge is to determine the average effect of the intervention on monthly spending per patient, but only for treated patients in the study. In other words, the goal is to determine the sample average treatment effects on the treated (SATTs). This is to be done for a number of different groupings of treated patients. Formally, the effects are defined as follows:

**Average Treatment Effect of the Treated (ATT)** : 

$$ E_{(i \vert z_{i} = 1)} (Y_{it}(1) - Y_{it}(0)) $$


**Sample average treatment effect on the treated** : 

$$ \dfrac{1}{\sum^{4}_{t=3}{N_{t}}} \sum^{4}_{t=3}{\sum_{i: z_{i} = 1}{(Y_{it}(1) - Y_{it}(0))}} $$




[acic2022]: https://acic2022.mathematica.org/
[BART]: https://www.tandfonline.com/doi/abs/10.1198/jcgs.2010.08162