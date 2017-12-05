# Kidnapped Vehicle Project - Particle Filter Implementation in C++
#### In this project a Particle Filter is implemented in C++ to localize a vehicle using LIDAR sensor data observations and Map landmarks data into a Particle Filter for Localization of a moving car.

### Particle Filter is a sequence of these Major Steps:
#### 1. Initialize - Create new particles according to the `num_particles` parameter with a gaussian distribution around the GPS `(x,y)` position and IMU `yaw_angle`.
#### 2. Prediction - Calculate the new position of each particle according to the input `velocity` and `yaw_rate` while introducing noise to the position estimate. *(instead of introducing sensor noise)
#### 3. Weights Update - Incorporate the new sensor data to the predicted state, by updating the weights of each particle with respect to the LIDAR observations from the vehicle:
1. Homogeneous Transformation: Rotate and translate the map axis to the vehicle's, to be able to convert the vehicle's observations from vehicle coordinates to map coordinates; to be able to associate each observation with a landmark.

2. Nearest Neighbor: Use the transformed observations and compare each with the Map's landmarks to figure which is the closest landmark `id` and associate the observation and landmark together for the weight calculation.

3. Multivariate Gaussian Probability Function: Use this function to calculate the weight of the particles by multiplying the probability of all the observations with the associated landmarks to get the particle's weight.

#### 4. Resample - Using a resampling algorithm `resampling_wheel` or the C++ function `std::discrete_distribution<>` to resample a whole new set of particles equal to `num_particles`, however the recurrence probability of each particle is directly proportional to its weight, so that particles that are close to the true location - having higher weights - are prominent, while particles that are far away die out.

---

## Running the Code
This project involves the Term 2 Simulator which can be downloaded [here](https://github.com/udacity/self-driving-car-sim/releases)

Execute the following in the top directory of the project to run the particle filter:

1. ./clean.sh
2. ./build.sh
3. ./run.sh

## Particle Filter Process
Here is the main protcol that main.cpp uses for uWebSocketIO in communicating with the simulator.

INPUT: values provided by the simulator to the c++ program

// sense noisy position data from the simulator

["sense_x"] 

["sense_y"] 

["sense_theta"] 

// get the previous velocity and yaw rate to predict the particle's transitioned state

["previous_velocity"]

["previous_yawrate"]

// receive noisy observation data from the simulator, in a respective list of x/y values

["sense_observations_x"] 

["sense_observations_y"] 


OUTPUT: values provided by the c++ program to the simulator

// best particle values used for calculating the error evaluation

[best_particle_x"]

["best_particle_y"]

["best_particle_theta"] 


## The directory structure of this repository is as follows:

```
root
|   build.sh
|   clean.sh
|   CMakeLists.txt
|   README.md
|   run.sh
|
|___data
|   |   
|   |   map_data.txt
|   
|   
|___src
    |   helper_functions.h
    |   main.cpp
    |   map.h
    |   particle_filter.cpp
    |   particle_filter.h
```


### The following is a screenshot from running the simulator: 
- The green lines represent the ground truth.
- The blue lines represent the particle filter's best estimation.

<p align="center">
<img align="center" src="./data/Screen Shot 2017-12-06 at 12.13.35 AM.png" alt="alt text">
</p>

### The following is after 2,443 time steps at the end of this project at errors:
X ~ 0.120
Y ~ 0.109
Yaw ~ 0.004

<p align="center">
<img align="center" src="./data/Screen Shot 2017-12-06 at 12.14.34 AM.png" alt="alt text">
</p>

---

*For More Information check the [source files](./src) to review the Particle Filter algorithm. 
