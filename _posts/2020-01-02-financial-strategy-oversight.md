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

1. Embargo: After finishing backtest, the strategy will run on data observed just after the end date of the backtest. If the results are consistent with the backtest outcome, we may proceed to the next pace. By `consistent` we mean the performance should be rather similar in some norm that is defined exactly to calculate the similarity.
2. Paper Trading: In this stage we run the strategy on live data. One of the reasons to implement this stage is to monitor the chronical gap between observation and positioning. This way we can have a more realistic idea of how well the strategy is performing. Note that the chronical gap may be occurred because of multiple factors like data parsing latencies, calculation latencies, execution delays, and so on.


```python

```
[pl]: https://saeidbadamchi.github.io
Check out the [quant research][quant-research] for more info on how to get the most out of Jekyll.

[quant-research]: https://quantresearch.org
