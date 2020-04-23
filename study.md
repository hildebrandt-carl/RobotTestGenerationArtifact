---
title: Exploratory Study 
subtitle: Exploring the application of the proposed approach on commercial drones.
layout: page
show_sidebar: false
tabs: our_tabs
---

## Assessing the Anafi drone

We flew the Anafi drone and collected data from tests run in the real world. The raw data for these experiments is found in the following folder:

```bash
anafi_learned_run_flown/learned_anafi_sim_ANAFI_seed10_length10_nodes250_res4_beamwidth5_totaltime3600_simtime90_searchtype_kinematic_scoretype_learned/
```

The data can then be parsed using the provided script. This script parses the data and finds the test with the largest deviation. The test with the largest deviation is then displayed. The script can be run by running the following commands:

```bash
cd ~/Artifact/ReproducingResults
python3 RQ3_COMPARISON.py
```

This results in an example of the test which resulted in the largest deviation (Figure 8 in the [paper](../paper/)).

<div style="text-align:center" markdown="1">
![](../img/anafi.png)
</div>

## Simulation Compared to Outdoors

To assess the learned scoring models we compared the deviation created when no scoring model was used to the deviation produced by a test set which incorporated a learned scoring model. The results are then displayed as a ratio of mean maximum deviation of the no scoring model to the test set (notice how the no scoring model is centered at 1). The raw data for these tests are in the initial test folder with no scoring model, and the folder where the anafi's learned scoring model is. More specifically raw data can be found in 

```bash
initial_run_flown/initial_ANAFI_seed10_length10_nodes250_res4_beamwidth5_totaltime7200_simtime90_searchtype_kinematic_scoretype_random/
anafi_learned_run_flown/learned_anafi_sim_ANAFI_seed10_length10_nodes250_res4_beamwidth5_totaltime3600_simtime90_searchtype_kinematic_scoretype_learned/
```

We can then use a script provided in the repo which parses the information and computes the ratio of the maximum deviation. To run this script you can use the following commands:

```bash
cd ~/Artifact/ReproducingResults
python3 RQ3_ANAFI.py
```

The results from running this script can be seen in the output (Figure 9 in the [paper](../paper/)).

<div style="text-align:center" markdown="1">
![smaller](../img/maxdev_anafi.png)
</div>
