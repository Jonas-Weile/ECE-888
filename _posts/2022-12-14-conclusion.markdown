---
layout: post
title:  "Stage 5: Conclusion"
date:   2022-12-14 12:00:00 -0500
categories: project1
---
To summarize the project, we attempted to build a late "submission" to the American Causal Inference Conference 2022 Data Challenge. We wanted to use BART to perform the estimation of the causal estimands, since BART has shown promising results in previous years, and to further test the effectiveness of the method.

We were able to perform identification, under some major assumptions, and setup to run BART on the data. Unfortunately, we were unable to actually perform the estimation. This was due to the size of the dataset, which was more than the BART implementation could handle with my limited RAM.

To continue the work on this project, I hope to reimplement BART myself in order to have complete control of the data usage/movement, and potentially utilize the disk to . This will allow me to perform the estimation on the full dataset. Further, I hope to read about difference-in-differences to be able to perform identification of the sample average treatment effect of the treated.