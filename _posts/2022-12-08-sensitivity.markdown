---
layout: post
title:  "Stage 4: Sensitivity Analysis"
date:   2022-12-08 12:00:00 -0500
categories: project1
---

Once our causal estimand of interest has been estimated, we can check the robustness of our estimate by performing **sensitivity analysis**. Because our data is simulated, we *<u>know</u>* that we don't have to deal with unmeasured confounding - this follows from the ignorability assumption that the authors have provided. Therefore, we cannot use the sensitivity analysis of Manski, Cornfield, Rosenbaum, or VanderWeele, as these all aim to explain how sensitive the estimand is to *some* unmeasured, confounding varible $$U$$.

Instead, we will discuss how our analysis depends on our initial assumptions. We made some major simplifying assumptions, namely that the *Sample Average Treatfment of the Treated* can be well-approximated by the population-wide *Average Treatment Effect*. This assumption was not justified in any way, but simply made in order to make identification feasible at the time by using backdoor adjustment. Clearly, the assumption is most likely not satisfied, unless the observational data has the form of an *RCT*. Instead, we should try our hand with more advanced identification techniques - we note, that given the longitudinal form of the data, *difference-in-difference* might be an interesting identification technique to try. It would the be interesting to check how the expression for the causal estimand would change change with this technique, and further how the final estimand would vary.



<br/>
#### Other Estimation Methods
Finally, unlike our identification that relies on major assumptions on the randomness/exchangeability of treatments, our estimation method *BART* is actually quite flexible. Bart is able to model non-linearities and multi-way interactions, as briefly touch upon in the previous post.
However, in the spirit of ensemble methods (like BART itself), it would be interesting to see how different estimation methods compare. I.e. would *IPW* yield somewhat similar estiames as BART?
