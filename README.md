# CarND-Path-Planning-Project                       
## Overview                         
The goal of this project is to build a path planner that creates smooth, safe trajectories for the car to follow. The highway track has other vehicles, all going different speeds, but approximately obeying the 50 MPH speed limit.

The car transmits its location, along with its sensor fusion data, which estimates the location of all the vehicles on the same side of the road.                                        



## Prerequisites                              
* cmake >= 3.5                     
* make >= 4.1                                 
* gcc/g++ >= 5.4                                  
* libuv 1.12.0                               



## Run the code
1. clone this repo
2. mkdir build && cd build
3. cmake && make
4. ./path_planning



## Performance                           
The performance of the car can reach following requirements:                   
1. The car is able to drive at least 4.32 miles without incident.                             
2. The car drives according to the speed limit.                      
3. The speed of the car is lower than 50 MPH.
4. Max Acceleration and Jerk are not Exceeded.
5. Car does not have collisions.
6. The car stays in its lane, except for the time between changing lanes.
7. The car is able to change lanes
![Image text](https://github.com/Yunying-Chen/CarND-Path-Planning-Project/blob/master/img/sim_img.png)    



## Reflection
The path planning algorithms start at src/main.cpp， the main algorithms consist of two parts behavior and trajectory, the details are as follows:                        
1. Behavior (Line 265 to Line 358)                        
In this part, based on the telemetry and sensor fusion data, the decision of how to behave can be made. It will check several situations:                           
* If there is a car in front and if the distance is smaller than 30m? If yes, slow down and check if it is safe to change line.
* Check the left lane if availdable for the car to change lane.
* If left lane is unavailable check the right lane.
* If the distance between the front car and the car is too close, brake harder.
* If the lane is safe and clear, the car should speed up to  around 49.5 MPH but no higher than 50 MPH.



2. Trajectory (Line 360 to Line 456)                                           
In this part, based on the speed and lane output from the behavior, car coordinates and past path points the trajectory will be calculated and it works as follows:                   
Get the last two points from the previous trajectory. If it is null, calculate it by using the current car coordinates.(Line 368 to Line 391) With the next three coordinates from 30m, 60m and 90m(Line 392 to Line 402), after being transformed to the local car coordinates(Line 404 to Line 412), the coordinates is used to initialize the Spline calculation.(Line 413 to Line 415)                           
In order to ensure the continuity on the trajectory, the path points from the previous path are added to the spline adjustment.(Line 421 to Line 426)  The rest of the points are calculated by evaluating the spline and transforming the output coordinates from 30m ahead to local coordinates.    
