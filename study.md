---
title: Exploratory Study 
subtitle: Exploring the application of the proposed approach on commercial drones.
layout: page
show_sidebar: false
tabs: our_tabs
---

## Assessing the Anafi drone

We flew the Anafi drone and collected data from tests run in the real world. The [raw data](../raw-data/) is found inside the `anafi_learned_run_flown` folder. For this experiment, we were interested in a specific test suite inside the `anafi_learned_run_flown` folder. The specific [raw data](../raw-data/) we were interested in is:

```bash
anafi_learned_run_flown/learned_anafi_sim_ANAFI_seed10_length10_nodes250_res4_beamwidth5_totaltime3600_simtime90_searchtype_kinematic_scoretype_learned/
```

This [raw data](../raw-data/) can be parsed using the provided script. This script parses the data and finds the test with the largest deviation. The test with the largest deviation is then displayed. The script can be run using the following commands in your terminal:

```bash
cd ~/Artifact/ReproducingResults
python3 RQ3_COMPARISON.py
```

The result of these commands is the test that resulted in the most significant deviation when flying the drone. The figure shows both the expected trajectory as well as the trajectory of the drone (Figure 8 in the [paper](../paper/)).

<div style="text-align:center" markdown="1">
![](../img/anafi.png)
</div>

## Simulation Compared to Outdoors

To assess the learned scoring models, we compared the deviation created when no scoring model was used to the deviation produced by a test set that incorporated a learned scoring model. The results are displayed as a ratio of mean maximum deviation of the no scoring model to the test set with a scoring model. The [raw data](../raw-data/) for these tests are in the `initial_run_flown` folder, which used no scoring model. The folder `anafi_learned_run_flown` contains the Anafi learned scoring model test suite. More specifically, the [raw data](../raw-data/) can be found in: 

```bash
initial_run_flown/initial_ANAFI_seed10_length10_nodes250_res4_beamwidth5_totaltime7200_simtime90_searchtype_kinematic_scoretype_random/
anafi_learned_run_flown/learned_anafi_sim_ANAFI_seed10_length10_nodes250_res4_beamwidth5_totaltime3600_simtime90_searchtype_kinematic_scoretype_learned/
```

Using a provided script, we can parse the [raw data](../raw-data/) and computes the ratio of the maximum deviation. To run this script, you can use the following commands in terminal:

```bash
cd ~/Artifact/ReproducingResults
python3 RQ3_ANAFI.py
```

The results from running this script can be seen in the output (Figure 9 in the [paper](../paper/)).

<div style="text-align:center" markdown="1">
![smaller](../img/maxdev_anafi.png)
</div>


## Additional Analysis

We show that you can perform a similar study on the learned controllers from the MIT Flightgoggles drone. As this requires a different set of data from the Anafi drone, we included this as an additional script. The [raw data](../raw-data/)for this script is found in the `initial_run_flown` as well as the `learned_run_flown` folders. More specifically, they can be found in:

```
~/Artifact/RawData/initial_run_flown
~/Artifact/RawData/learned_run_flown
```

The script computes the ratio of the maximum deviation from the learned scoring model to the initial test set for the Flightgoggles drone. To run the script run the following command in the terminal:

```bash
cd ~/Artifact/ReproducingResults
python3 RQ3_MIT.py
```

Using this command, you will get the following output, which shows the improvement of the learned scoring model.

<div style="text-align:center" markdown="1">
![smaller](../img/rq3_additional.png)
</div>