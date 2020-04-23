---
title: Tool Pipeline
subtitle: Getting started with our tool pipeline.
description: How to get setup with our tool
layout: page
show_sidebar: false
---

# Introduction

We have broken the instructions into two sections. First we introduce how to get started with the test generation. Second we introduce how to get started with the test execution. To get started with execution you own tests is relatively simple, and we have provided both a docker image as well as the raw instructions to get started with it. Unfortunately getting started with the test execution is more complex and requires complex dependencies on both the graphics card as well as the computers network card and thus we have not provided a docker image for this section. We have provided all the instructions which can be used on a powerful machine to get up and running (We ran our tests on a machine with 64 gigs of ram, i9-10900X, and a Titan RTX with 24 gigs of onboard memory).

## Before Starting

Before you get started make sure you have cloned the repository. You can do that by running:

```bash
git clone git@github.com:hildebrandt-carl/RobotTestGeneration.git
```

# Test Generation

The test generation generated tests which you could then execute on an autonomous system. To get started with this you can either use a docker image or install the system on your own machine. More details on each are given below:

## Docker Image

TODO

## Installing on your Own Machine

To install the test generation tool pipeline on your own machine you can do the following: 

### Prerequisites

You need to have the following installed in order to run the application:

```
$ sudo apt-get install python3-pip python3-tk -y
$ pip3 install --upgrade pip --user
$ pip3 install psutil --user
$ pip3 install numpy --user
$ pip3 install matplotlib --user
$ pip3 install scipy --user
$ pip3 install sympy --user
$ pip3 install sklean --user
```

### Generating Initial Tests

We start by generating the tests. The point of this is to generate the initial sets of tests. This includes the tests for all the search strategies namely:

* Random (random)
* Approximate Kinematic (maxvel)
* Full Kinematic (kinematic)

You can generate the tests using the available script. The script which are available alow you to generate either the full set of initial tests, or just a limited number of test sets. The full set of initial tests lets you view how the number fo valid trajectories change per the trajectory complexity. The limited number of test sets allows you to generate just  the tests required to run the entire tool chain. The scripts are named as follows:

* initial_all_run.sh

**NOTE:** These scripts were designed and run on a computer with a 20 core CPU. I recommend changing the number of python scripts launched to be less than or equal to the number of CPU cores you have available.

To run a script you use
```
$ cd ~/Desktop/RobotTestGeneration/TestGeneration
$ ./initial_all_run.sh
```

All tests will be output into `RobotTestGeneration/TestGeneration/Results` folder. We want to move them to a final folder. Once your tests are done running. You can do this by checking the list of process. Move the results into the final results folder
```
$ mv Results FinalResults
$ cd FinalResults
$ mkdir initial_run_flown
$ mv `ls -A | grep -v initial_run_flown` ./initial_run_flown
```

Feel free to call `initial_run_flown` anything you wish. Just remember it for later as this folder name is required.









# Test Execution








# Work Flow














## Prerequisits

You need to have the following installed in order to run the application:

```
$ sudo apt-get install python3-pip python3-tk -y
$ pip3 install --upgrade pip --user
$ pip3 install psutil --user
$ pip3 install numpy --user
$ pip3 install matplotlib --user
$ pip3 install scipy --user
$ pip3 install sympy --user
$ pip3 install sklean --user
```

## Generate the initial tests

We start by generating the tests. The point of this is to generate the initial sets of tests. This includes the tests for all the search stratergies namely:

* Random (random)
* Approximate Kinematic (maxvel)
* Full Kinematic (kinematic)

You can generate the tests using the available script. The script which are available alow you to generate either the full set of initial tests, or just a limitied number of test sets. The full set of initial tests lets you view how the number fo valid trajectories change per the trajectory complexity. The limited number of test sets allows you to generate just  the tests required to run the entire tool chain. The scripts are named as follows:

* initial_all_run.sh

**NOTE:** These scripts were designed and run on a computer with a 20 core CPU. I recommend changing the number of python scripts launched to be less than or equal to the number of CPU cores you have available.

To run a script you use
```
$ cd ~/Desktop/RobotTestGeneration/TestGeneration
$ ./initial_all_run.sh
```

All tests will be output into `RobotTestGeneration/TestGeneration/Results` folder. We want to move them to a final folder. Once your tests are done running. You can do this by checking the list of process. Move the results into the final results folder
```
$ mv Results FinalResults
$ cd FinalResults
$ mkdir initial_run_flown
$ mv `ls -A | grep -v initial_run_flown` ./initial_run_flown
```

Feel free to call `initial_run_flown` anything you wish. Just remember it for later as this folder name is required.


## Fly with WorldEngine

First install the prereck
```
$ sudo apt-get install python-pip
$ sudo apt install python-rosinstall python-rosinstall-generator python-wstool build-essential
$ sudo apt-get install python-catkin-tools
$ sudo apt install python-wstool
$ pip install catkin_pkg --user
$ sudo apt-get install python-scipy
```

We are going to be running a python matlab interface and will need to install matlab.engine in python. For that to work we need to have mathlab installed. You then need to install matlab. Open matlab and run the following command in the matlab terminal:
```
matlabroot
```

Copy the result of this and then run the following in a normal terminal:
```
$ {matlabroot}/extern/engines/python
# Which in my case will be:
$ /usr/local/MATLAB/R2019b/extern/engines/python
$ sudo python setup.py install
```

To start you need to compile world engine. To do that run the following:
```
$ ~/Desktop/RobotTestGeneration/WorldEngineSimulation
$ rosdep install --from-paths src --ignore-src -r -y
$ catkin build
```

The next stage is to use the a modified version of FlightGoogles simulator known as the WorldEngineSimulation. To do that we need simply need to run a script inside the WorldEngineSimulation. The script works by assuming you have saved the initial sets of tests as follows `RobotTestGeneration/TestGeneration/FinalResults/<your_directory>`. You can run the script as follows:

```
$ cd ~/Desktop/RobotTestGeneration/WorldEngineSimulation
$ ./run_mit_25001.sh <your_directory> <search_type> <score_type> <save_prefix> <trajectory_length> <search_time> <controller_type>
```

After having run the initial set of tests the parameters available are:

* your_directory -> initial_run_flown
* search_type -> random; maxvel; kinematic
* score_type -> random
* save_prefix -> initial
* trajectory_length -> 5; 10
* search_time -> (not required) 3600
* controller_type -> (not required - leave blank for all) -42 -2 -1 2 5 10

The `score_type` and `save_prefix` for at this stage of the test generation can only be a single value. They will be used later on. We however want to run the WorldEngineSimulator on all combinations of `search_type` and `trajectory_length`. To make this process faster if your computer is able to handle more than 1 simulation I have provided three scripts so that you can run them in parallel. **NOTE:** you will not be able to run three of the same script at a time as they use static network addresses and will clash. Thus an example of running the simulation in parrallel would be:

Remember we need to run it on all combinations so after this run is complete dont forget to run the trajectories of length 10.

```
$ ./run_mit_25001.sh "initial_run_flown" "random" "random" "initial" "10" "7200"
$ ./run_mit_25002.sh "initial_run_flown" "maxvel" "random" "initial" "10" "7200"
$ ./run_mit_25003.sh "initial_run_flown" "kinematic" "random" "initial" "10" "7200"
```

The simulators output will be automatically put into the correct folders inside of `RobotTestGeneration/TestGeneration/FinalResults/<your_directory>`.

## Analyzing Results

The next thing we need to do is to parse all the resulting data to get the details from each of the runs. To do that we use the files which can be found in the `TestGeneration/AnalyzeResults` folder.

First we want to parse the resulting data to extract high level metrics from it. We do that using the file `processResults.py`. For each of the test sets we need to extract high level information from them individually. To do that we can run the following commands:

```
maindir="~/RobotTestGeneration/TestGeneration/FinalResults/<your_directory>
maindir="/home/autosoftlab/Desktop/RobotTestGeneration/TestGeneration/FinalResults/initial_run_flown/"
$ python3 processResults.py --main_directory ${maindir} --searchtype "kinematic" --scoretype "random" --fileprefix "initial" --trajectorylength "10" --searchtime "7200"
```

# RQ1

To answer RQ1 you can run the following:
```
$ python3 RQ1_GEN.py
$ python3 RQ1_METRICS.py
```


# RQ2 

So for in our case we need to run:
```
$ cd ~/Desktop/RobotTestGeneration/TestGeneration 
$ ./handcraft_lim_run.sh 
```

This will crate a set of handcrafted tests.

Move the handcrafted tests into the results folder (DONT USE THESE COMMANDS YET) - BASICALLY MOVE EVERYTHING INTO handcrafted_run_flown) To do that you can run:
```
$ cd ~/Desktop/RobotTestGeneration/TestGeneration/FinalResults
$ mv ../Results ./handcrafted_run_flown
```

Waypoint controller tests
```
$ cd ~/Desktop/RobotTestGeneration/WorldEngineSimulation
$ ./run_mit_25001.sh "handcrafted_run_flown" "kinematic" "edge" "handcrafted" "10" "3600" "-1"
$ ./run_mit_25002.sh "handcrafted_run_flown" "kinematic" "edge90" "handcrafted" "10" "3600" "-1"
$ ./run_mit_25003.sh "handcrafted_run_flown" "kinematic" "edge180" "handcrafted" "10" "3600" "-1"
```

Minimum Snap Controller Tests
```
$ cd ~/Desktop/RobotTestGeneration/WorldEngineSimulation
$ ./run_mit_25001.sh "handcrafted_run_flown" "kinematic" "edge" "handcrafted" "10" "3600" "-42"
$ ./run_mit_25002.sh "handcrafted_run_flown" "kinematic" "edge90" "handcrafted" "10" "3600" "-42"
$ ./run_mit_25003.sh "handcrafted_run_flown" "kinematic" "edge180" "handcrafted" "10" "3600" "-42"
```

Unstable Waypoint Controller
```
$ cd ~/Desktop/RobotTestGeneration/WorldEngineSimulation
$ ./run_mit_25001.sh "handcrafted_run_flown" "kinematic" "edge" "handcrafted" "10" "3600" "-2"
$ ./run_mit_25002.sh "handcrafted_run_flown" "kinematic" "edge90" "handcrafted" "10" "3600" "-2"
$ ./run_mit_25003.sh "handcrafted_run_flown" "kinematic" "edge180" "handcrafted" "10" "3600" "-2"
```

Velocity Controller Controller
```
$ cd ~/Desktop/RobotTestGeneration/WorldEngineSimulation
$ ./run_mit_25001.sh "handcrafted_run_flown" "kinematic" "edge" "handcrafted" "10" "3600" "2"
$ ./run_mit_25002.sh "handcrafted_run_flown" "kinematic" "edge90" "handcrafted" "10" "3600" "2"
$ ./run_mit_25003.sh "handcrafted_run_flown" "kinematic" "edge180" "handcrafted" "10" "3600" "2"
```


Now you need to process them using:
```
$ maindir="/home/autosoftlab/Desktop/RobotTestGeneration/TestGeneration/FinalResults/handcrafted_run_flown/"
$ cd ~/Desktop/RobotTestGeneration/TestGeneration/AnalyzeResults 
$ python3 processResults.py --main_directory ${maindir} --searchtype "kinematic" --scoretype "edge" --fileprefix "handcrafted" --trajectorylength "10" --searchtime "3600"
$ python3 processResults.py --main_directory ${maindir} --searchtype "kinematic" --scoretype "edge90" --fileprefix "handcrafted" --trajectorylength "10" --searchtime "3600"
$ python3 processResults.py --main_directory ${maindir} --searchtype "kinematic" --scoretype "edge180" --fileprefix "handcrafted" --trajectorylength "10" --searchtime "3600"
```

You can use the graph deviation file for this:

Plot the deviation
```
$ python3 RQ2.py
```


# RQ3

Need to figure out how to generate a learned value for each controller

based on the inital test runs we are interested in two a folders:
initial_MIT_seed10_length10_nodes250_res4_beamwidth5_totaltime3600_simtime90_searchtype_kinematic_scoretype_random

We need to generate a model for each of the types in here. To generate a model we run:
```
$ maindir="/home/autosoftlab/Desktop/RobotTestGeneration/TestGeneration/FinalResults/initial_run_flown/"
$ python3 FindTrends.py --maindirectory ${maindir} --searchtype "kinematic" --scoretype "random" --fileprefix "initial" --trajectorylength "10" --searchtime "7200" --saveprefix "len10"
```

This will produce a set of models in the models directory named:

* len5_speed-1_minsnap0_poly_features.npy && len5_speed-1_minsnap0_regression_mode.npy
* len5_speed-2_minsnap0_poly_features.npy && len5_speed-2_minsnap0_regression_mode.npy
* len5_speed1_minsnap0_poly_features.npy && len5_speed1_minsnap0_regression_mode.npy
* len5_speed2_minsnap0_poly_features.npy && len5_speed2_minsnap0_regression_mode.npy
* len5_speed5_minsnap0_poly_features.npy && len5_speed5_minsnap0_regression_mode.npy
* len5_speed10_minsnap0_poly_features.npy && len5_speed10_minsnap0_regression_mode.npy

Move the models into a final_models folder by running:
```
$ cd /home/autosoftlab/Desktop/RobotTestGeneration/TestGeneration/AnalyzeResults
$ mv Models ../FinalModels
```

Then we generate a test for each of the system types to do that run:
```
./learned_model_run.sh
```

Now move the new tests into the final test folder using:
```
$ cd /home/autosoftlab/Desktop/RobotTestGeneration/TestGeneration/
$ mv Results FinalResults/learned_run_flown
```

Now we want to fly each of these tests to do that you can run the following:
```
$ cd /home/autosoftlab/Desktop/RobotTestGeneration/WorldEngineSimulation
$ ./run_mit_25001.sh "learned_run_flown" "kinematic" "learned" "learned_speed10_minsnap0" "10" "3600" "10"
$ ./run_mit_25002.sh "learned_run_flown" "kinematic" "learned" "learned_speed2_minsnap0" "10" "3600" "2"
$ ./run_mit_25003.sh "learned_run_flown" "kinematic" "learned" "learned_speed-2_minsnap0" "10" "3600" "-2"

$ ./run_mit_25001.sh "learned_run_flown" "kinematic" "learned" "learned_speed-1_minsnap1" "10" "3600" "-42"
$ ./run_mit_25002.sh "learned_run_flown" "kinematic" "learned" "learned_speed-1_minsnap0" "10" "3600" "-1"
$ ./run_mit_25003.sh "learned_run_flown" "kinematic" "learned" "learned_speed5_minsnap0" "10" "3600" "5"
```

Now that you have generated all the execution files we need to analyze them to get the performance metrics
```
maindir="/Users/carlhildebrandt/Dropbox/UVA/Research/Work/RobotTestGeneration/TestGeneration/FinalResults/learned_run_flown/"
maindir="/home/autosoftlab/Desktop/RobotTestGeneration/TestGeneration/FinalResults/learned_run_flown/"
$ python3 processResults.py --main_directory ${maindir} --searchtype "kinematic" --scoretype "learned" --fileprefix "learned_speed-1_minsnap0" --trajectorylength "10" --searchtime "3600"
$ python3 processResults.py --main_directory ${maindir} --searchtype "kinematic" --scoretype "learned" --fileprefix "learned_speed-2_minsnap0" --trajectorylength "10" --searchtime "3600"
$ python3 processResults.py --main_directory ${maindir} --searchtype "kinematic" --scoretype "learned" --fileprefix "learned_speed2_minsnap0" --trajectorylength "10" --searchtime "3600"
$ python3 processResults.py --main_directory ${maindir} --searchtype "kinematic" --scoretype "learned" --fileprefix "learned_speed5_minsnap0" --trajectorylength "10" --searchtime "3600"
$ python3 processResults.py --main_directory ${maindir} --searchtype "kinematic" --scoretype "learned" --fileprefix "learned_speed10_minsnap0" --trajectorylength "10" --searchtime "3600"
$ python3 processResults.py --main_directory ${maindir} --searchtype "kinematic" --scoretype "learned" --fileprefix "learned_speed-1_minsnap1" --trajectorylength "10" --searchtime "3600"
```

Plot the deviation 
```
$ python3 RQ3_MIT.py
```


# Anafi

We have to do things slightly differently to fly the Anafi drone. We first have to move the data we want to fly with into the `AnafiSimulation/TestingAnafi/Outdoor` folder. To do that we can run the following commands:
```
# Clean out the current Anafi simulation folder
$ cd /home/autosoftlab/Desktop/RobotTestGeneration/AnafiSimulation/TestingAnafi/Outdoor/test
$ rm -rf maps
# Copy the new data into the Anafi simulation folder
$ cd /home/autosoftlab/Desktop/RobotTestGeneration/TestGeneration/FinalResults/initial_run_flown/initial_ANAFI_seed10_length10_nodes250_res4_beamwidth5_totaltime7200_simtime90_searchtype_kinematic_scoretype_random
$ cp -r maps ../../../../AnafiSimulation/TestingAnafi/Outdoor/test
```

Next we need to open two terminals and run the following in each:
Terminal 1:
```
$ sudo systemctl start firmwared.service
$ sphinx /opt/parrot-sphinx/usr/share/sphinx/drones/anafi4k.drone::stolen_interface=enp2f0:eth0:192.168.42.1/24 /home/autosoftlab/Desktop/RobotTestGeneration/AnafiSimulation/sphinxfiles/worlds/empty.world
```

Terminal 2:
```
$ cd ~/Desktop/RobotTestGeneration/AnafiSimulation/TestingAnafi/Outdoor
$ source ~/code/parrot-groundsdk/./products/olympe/linux/env/shell
$ python3 runtestwithpos.py --test_number 1 --test_type "simulation" 
```

Finally we wait until the drone comes to a complete stop. Then we kill the code running in Terminal 2. And inside the simulator we hit `ctrl + r`. We then repeat this for all tests in the simulator.

Once you are done each of the folders will contain a `simulation_output.txt`. We can then convert that file into the same format as our MIT drive by running the following code. In a new terminal run:
```
$ cd ~/Desktop/RobotTestGeneration/TestGeneration/AnalyzeResults
$ python3 convertAnafiToStandard.py --test_directory "/home/autosoftlab/Desktop/RobotTestGeneration/AnafiSimulation/TestingAnafi/Outdoor/test/" --test_type "simulation"
```

Copy the tests into the `initial_run_flown` folder

Each map will now have a bunch of `performance` files which we then can process using:
```
$ maindir="/home/autosoftlab/Desktop/RobotTestGeneration/TestGeneration/FinalResults/initial_run_flown/"
$ python3 processResults.py --main_directory ${maindir} --searchtype "kinematic" --scoretype "random" --fileprefix "initial" --trajectorylength "10" --searchtime "7200" --dronetype "ANAFI"
```

Now we want to generate more test for us to run on. To do that we:
```
$ cd ~/Desktop/RobotTestGeneration/TestGeneration/AnalyzeResults
$ maindir="/home/autosoftlab/Desktop/RobotTestGeneration/TestGeneration/FinalResults/initial_run_flown/"
$ python3 FindTrends.py --maindirectory ${maindir} --searchtype "kinematic" --scoretype "random" --fileprefix "initial" --trajectorylength "10" --searchtime "7200" --saveprefix "len10" --dronetype "ANAFI"
```


Now that we have the models move them into the final models folder. Use that generate new tests
```
$ cd ~/Desktop/RobotTestGeneration/TestGeneration
$ ./learned_model_run_anafi.sh
```


Move the learned folder to `anafi_learned_run_flown`
```
$ /home/autosoftlab/Desktop/RobotTestGeneration/TestGeneration/FinalResults
$ mv ../Results anafi_learned_run_flown
```

Then we now run those tests:
Fly them in the same manner


We then need to convert them to the standard format:
```
$ cd ~/Desktop/RobotTestGeneration/TestGeneration/AnalyzeResults
$ python3 convertAnafiToStandard.py --test_directory "/home/autosoftlab/Desktop/RobotTestGeneration/TestGeneration/FinalResults/anafi_learned_run_flown/learned_anafi_sim_ANAFI_seed10_length10_nodes250_res4_beamwidth5_totaltime3600_simtime90_searchtype_kinematic_scoretype_learned"
```

Finally we need to process them:
```
$ maindir="/home/autosoftlab/Desktop/RobotTestGeneration/TestGeneration/FinalResults/anafi_learned_run_flown/"
$ python3 processResults.py --main_directory ${maindir} --searchtype "kinematic" --scoretype "learned" --fileprefix "learned_anafi_sim" --trajectorylength "10" --searchtime "3600" --dronetype "ANAFI"
```

Plot the deviation 
```
$ python3 RQ3_Comparison.py
$ python3 RQ_ANAFI.py
```


