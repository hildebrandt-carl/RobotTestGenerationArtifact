---
title: Research Question 2
subtitle: Does the introduction of a scoring model improve the ability to generate stressful trajectories?
layout: page
show_sidebar: false
tabs: our_tabs
---

## Assessing the Scoring Models

To assess the scoring models we created 3 handcrafted scoring models and a learned scoring model. We then compared this to the initial tests with no scoring model. The raw data for scoring models and learned scoring experiments are in the folders `handcrafted_run_flown` and `learned_run_flown`, while the initial tests are found in the `initial_run_flown` tests. There full path directories are:

```bash
RobotTestGeneration/TestGeneration/FinalResults/initial_run_flown
RobotTestGeneration/TestGeneration/FinalResults/handcrafted_run_flown
RobotTestGeneration/TestGeneration/FinalResults/learned_run_flown
```

Using this data we provide a script which parses the data and computes the maximum deviation from the optimal trajectory. It then displays the data as a ratio of the mean maximum deviation when no scoring model was used to the current mean maximum deviation. To run that script you can run:

```
git clone git@github.com:hildebrandt-carl/RobotTestGeneration.git
cd RobotTestGeneration/TestGeneration/AnalyzeResults
python3 RQ2.py
```

Using this we can see the following output (Figure 7 in the [paper](../paper/)). This output shows us that using a scoring model creates stress that on average are more stressful in than using no scoring model.

<div style="text-align:center" markdown="1">
![](../img/maxdev_ratio.png)
</div>
