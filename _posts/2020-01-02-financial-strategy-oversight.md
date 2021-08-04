---
title: "Financial ML Strategy Oversight (LifeCycle)"
categories:
  - blog
  - financial machine learning
tags:
  - Finance
  - Machine Learning
---
If you are familiar with machine learning application in finance, you may have heard the name Marcos López de Prado who has focused in the past twenty year expanding the financial machine learning discipline. He is one of the pioneers in introducing and implementing the concept of meta-strategy paradigm which similarizes the process of strategy generation to a production line which has extensive aspects in implementation.
He argues that an investment company should operate like a production line and produce vast amount of strategies, implement them and monitor the performance of each strategy through their life cycle. If you wish to know more about the production line paradigm refer to post about [Strategy Production Line][pl] so you can find exact material about the subject. Here we discuss the lifecycle that each strategy is gone through after being implemented.

1. **Embargo**: After finishing backtest, the strategy will run on data observed just after the end date of the backtest. If the results are consistent with the backtest outcome, we may proceed to the next pace. By `consistent` we mean the performance should be rather similar in some norm that is defined exactly to calculate the similarity.
2. **Paper Trading**: In this stage we run the strategy on live data. One of the reasons to implement this stage is to monitor the chronical gap between observation and positioning. This way we can have a more realistic idea of how well the strategy is performing. Note that the chronical gap may be occurred because of multiple factors like data parsing latencies, calculation latencies, execution delays, and so on.
3. **Graduation**: After successfully handling the tasks in previous steps, now is the time to manage a real position. Remember that you can setup the position by strategy solely or as part of an ensemble.
4. **Re-allocation**: Now that we have enough information about how well the strategy has managed a real position, we can decide whether or not to change the share of strategy out of the total fund the manages. So typically when an strategy reaches the **Re-allocation** stage, we may increase the allocation.
5. **Decommission**: Finally there is a rule each strategy is some day outdated and will no longer provides us with expected profit. In this stage, we start to decrease the allocation of funds to the strategy. Since there are better strategies waiting in the production line (specifically step 2 and 3) to increase their share of allocation, we may never get empty-handed unless the strategy station of the production line or maybe other stations are causing problems.

[pl]: https://saeidbadamchi.github.io
Check out the [quant research][quant-research] for more info on how to get the most out of Jekyll.

[quant-research]: https://quantresearch.org
