---
title: Research Question 1
subtitle: Does the introduction of a KD model improve the ability to generate feasible and valid trajectories?
layout: page
show_sidebar: false
tabs: our_tabs
---

## Assessing the Generation Process

To assess the benefits of incorporating the kinematic and dynamic model we generated tests using no kinematic model, tests that used a maximum velocity to approximate the kinematic model, and the kinematic model. The test sets used are found in the `FinalResults/initial_run_flown` folder. Using a python script we can analyze the raw data to answer RQ1. The python scripts are found in the `AnalyzeResults` section. To run the python script which analyzes the benefits of incorporating the kinematic and dynamic model run the following python commands:

```bash
git clone https://github.com/hildebrandt-carl/RobotTestGeneration.git
cd RobotTestGeneration/TestGeneration/AnalyzeResults
python3 RQ1_GEN.py
```

This will generate a figure that displays the total number of tests generated using each of the different techniques as we increase the trajectory length. (Figure 5 in the [paper](../paper/))

<div style="text-align:center" markdown="1">
![smaller](../img/gen_metrics.png)
</div>

## Assessing of Performance Metrics Obtained

To assess the performance metrics obtained when flying the quadrotor on the tests which were generated using the kinematic and dynamic model without any scoring function, we flew trajectories of length 10. To view the raw data of these tests you can look inside the following folder:

```
RobotTestGeneration/TestGeneration/FinalResults/initial_run_flown/initial_MIT_seed10_length10_nodes250_res4_beamwidth5_totaltime7200_simtime90_searchtype_kinematic_scoretype_random/
```

We wrote a python script that parses this data to extract the maximum deviation, maximum acceleration as well as the total time taken when executing the tests. To run that script navigate to the `AnalyzeResults` folder:

```bash
git clone https://github.com/hildebrandt-carl/RobotTestGeneration.git
cd RobotTestGeneration/TestGeneration/AnalyzeResults
python3 RQ1_METRICS.py
```

This will generate a figure that shows the distribution of the maximum deviation, maximum acceleration and total time for the test set. (Figure 6 in the [paper](../paper/))

<div style="text-align:center" markdown="1">
![smaller](../img/metrics.png)
</div>
