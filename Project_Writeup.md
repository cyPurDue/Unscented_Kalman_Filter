# Unscented Kalman Filter Project Starter Code
Self-Driving Car Engineer Nanodegree Program

### 1. Project Overview
In this project utilize an Unscented Kalman Filter to estimate the state of a moving object of interest with noisy lidar and radar measurements. Passing the project requires obtaining RMSE values that are lower that the tolerance outlined in the project rubric. 

### 2. Follows the Correct Algorithm
2.1 "Your Sensor Fusion algorithm follows the general processing flow as taught in the preceding lessons." 
To successfully write code and make it work, I have generally followed the procedure in the unscented Kalman filter class. The ukf.cpp starts with initialization, then do the prediction process with generating points, and finally do the update part. In the tools.cpp, I have added the RMSE algorithm the same as last project (EKF). In the ukf.h, I have added a few variables in the class to better handle the data processing. Overall, the code runs without bug and shows the result properly.

2.2 "Your Kalman Filter algorithm handles the first measurements appropriately." 
This is handled by checking is_initialized_ bool variable, and the logic is the same as in EKF. 

2.3 "Your Kalman Filter algorithm first predicts then updates."
This is also shown in the ukf.cpp. In the UKF::ProcessMeasurement() function, it does the prediction first, and then updates.

2.4 "Your Kalman Filter can handle radar and lidar measurements."
This is also handled in the ukf.cpp. Everytime related to using measurement data, check the sensor_type_ and see if it is RADAR or LASER. Also in the update part, Radar and Lidar are separated in the update part, so the code can handle both measurements.  

### 3. Meets Accuracy Criteria
After the first run of code, the result was not good to meet the accuracy criteria, RMSE <= [.09, .10, .40, .30]. As learned in the class, I have dumped the NIS data and took a look at it, which shows that mostly the NIS value are much below the value 7.8. This means the process noise that we guessed was too high, so I have first divided the "std_a_" and "std_yawdd_" values by 10, giving =3 for each. Then the RMSE value has dropped obviously, and the NIS value increases within a reasonable range. Then I have tweaked these two values, basically from 3 down to 0.2 for each, and finally found using "std_a_ = 1" "std_yawdd_ = 0.65" gives an optimal result. 

After trying different pairs of value, my latest RMSE = [0.0653, 0.0830, 0.3278, 0.2274], which are all below the criteria. Also I have plotted the latest NIS data for both radar and lidar (omitted the 1st value because it is too big in radar and showing it affects the plot resolution). Checking those we can see, in NIS_radar.png, the plot is as expected, with most parts below 7.8 and some peaks over it. Checking NIS_lidar.png, the plot is also within expectation, with most parts below 7.8 (also a little below the radar plot), and 2 spikes over it. This also verifies the result, and both NIS plots are also within the same folder.

### 4. Appendix
-- copied from original README to set up enviroment.

This project involves the Term 2 Simulator which can be downloaded [here](https://github.com/udacity/self-driving-car-sim/releases)

This repository includes two files that can be used to set up and intall [uWebSocketIO](https://github.com/uWebSockets/uWebSockets) for either Linux or Mac systems. For windows you can use either Docker, VMware, or even [Windows 10 Bash on Ubuntu](https://www.howtogeek.com/249966/how-to-install-and-use-the-linux-bash-shell-on-windows-10/) to install uWebSocketIO. Please see [this concept in the classroom](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/0949fca6-b379-42af-a919-ee50aa304e6a/lessons/f758c44c-5e40-4e01-93b5-1a82aa4e044f/concepts/16cf4a78-4fc7-49e1-8621-3450ca938b77) for the required version and installation scripts.

Once the install for uWebSocketIO is complete, the main program can be built and ran by doing the following from the project top directory.

1. mkdir build
2. cd build
3. cmake ..
4. make
5. ./UnscentedKF

Tips for setting up your environment can be found [here](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/0949fca6-b379-42af-a919-ee50aa304e6a/lessons/f758c44c-5e40-4e01-93b5-1a82aa4e044f/concepts/23d376c7-0195-4276-bdf0-e02f1f3c665d)

Note that the programs that need to be written to accomplish the project are src/ukf.cpp, src/ukf.h, tools.cpp, and tools.h

The program main.cpp has already been filled out, but feel free to modify it.

Here is the main protcol that main.cpp uses for uWebSocketIO in communicating with the simulator.


INPUT: values provided by the simulator to the c++ program

["sensor_measurement"] => the measurment that the simulator observed (either lidar or radar)


OUTPUT: values provided by the c++ program to the simulator

["estimate_x"] <= kalman filter estimated position x
["estimate_y"] <= kalman filter estimated position y
["rmse_x"]
["rmse_y"]
["rmse_vx"]
["rmse_vy"]

---

## Other Important Dependencies
* cmake >= 3.5
  * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1 (Linux, Mac), 3.81 (Windows)
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools](https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./UnscentedKF` Previous versions use i/o from text files.  The current state uses i/o
from the simulator.

## Editor Settings

We've purposefully kept editor configuration files out of this repo in order to
keep it as simple and environment agnostic as possible. However, we recommend
using the following settings:

* indent using spaces
* set tab width to 2 spaces (keeps the matrices in source code aligned)

## Code Style

Please stick to [Google's C++ style guide](https://google.github.io/styleguide/cppguide.html) as much as possible.

## Generating Additional Data

This is optional!

If you'd like to generate your own radar and lidar data, see the
[utilities repo](https://github.com/udacity/CarND-Mercedes-SF-Utilities) for
Matlab scripts that can generate additional data.

## Project Instructions and Rubric

This information is only accessible by people who are already enrolled in Term 2
of CarND. If you are enrolled, see [the project page](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/0949fca6-b379-42af-a919-ee50aa304e6a/lessons/c3eb3583-17b2-4d83-abf7-d852ae1b9fff/concepts/f437b8b0-f2d8-43b0-9662-72ac4e4029c1)
for instructions and the project rubric.

## How to write a README
A well written README file can enhance your project and portfolio.  Develop your abilities to create professional README files by completing [this free course](https://www.udacity.com/course/writing-readmes--ud777).

