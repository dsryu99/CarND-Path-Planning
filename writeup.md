# **Path Planning**
---

**Path Planning Project**

The goals / steps of this project are the following:
* In this project your goal is to safely navigate around a virtual highway with other traffic that is driving +-10 MPH of the 50 MPH speed limit.
* You will be provided the car's localization and sensor fusion data, there is also a sparse map list of waypoints around the highway.
* The car should try to go as close as possible to the 50 MPH speed limit, which means passing slower traffic when possible, note that other cars will try to change lanes too.
* The car should avoid hitting other cars at all cost as well as driving inside of the marked road lanes at all times, unless going from one lane to another.
* The car should be able to make one complete loop around the 6946m highway.
* Since the car is trying to go 50 MPH, it should take a little over 5 minutes to complete 1 loop.
* Also the car should not experience total acceleration over 10 m/s^2 and jerk that is greater than 10 m/s^3.

[//]: # (Image References)

[image1]: ./image/Valid_Trajectories.jpg "Valid Trajectories"

## Rubric Points
###Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/1020/view) individually and describe how I addressed each point in my implementation.  

---
### Compilation

#### 1. The code compiles correctly.

My code compiles without errors with `cmake` and `make`.

### Valid Trajectories

![alt text][image1]

#### 1. The car is able to drive at least 4.32 miles without incident.
The top right screen of the simulator shows the current/best miles driven without incident. Incidents include exceeding acceleration/jerk/speed, collision, and driving outside of the lanes. Each incident case is also listed below in more detail. I also uploaded [the simulation video](https://www.youtube.com/watch?v=v-pf2Y82rWs&t=17s).

#### 2. The car drives according to the speed limit.
The car doesn't drive faster than the speed limit. Also the car isn't driving much slower than speed limit unless obstructed by traffic. Whenever checking the next state using the FSM, the ego vehicle checks the speed limit and also tries to speed up as possible.

#### 3. Max acceleration and jerk are not exceeded.
The car does not exceed a total acceleration of 10 m/s^2 and a jerk of 10 m/s^3. The predicted trajectories of the ego vehicle are based on the interpolating points via the spline fitting. Consequently, its driving is much safer and smooth.

#### 4. Car does not have collisions.
The car must not come into contact with any of the other cars on the road. Whenever checking the next state using the FSM, the distances between the predicted trajectories of the ego and non-ego vehicles are checked. Based on the distances, the velocity of the ego vehicle is updated in order to avoid the collisions.

#### 5. The car stays in its lane, except for the time between changing lanes.
The car doesn't spend more than a 3 second length out side the lanes during changing lanes, and every other time the car stays inside one of the 3 lanes on the right hand side of the road. In order to shorten the time of chaning lanes, the interval between two path points is set to 30 meters. In order for the ego vehicle to stay inside one of the 3 lanes, d value of Frenet coordinate is employed.

#### 6. The car is able to change lanes.
The car is able to smoothly change lanes when it makes sense to do so, such as when behind a slower moving car and an adjacent lane is clear of other traffic. The trajectories for non-ego vehicles are generated. Based on such trajectories, the ego vehicle can identify when it can safely change lanes. Changing lanes is also smooth by using the interpolated points.

### Reflection

#### 1. There is a reflection on how to generate paths.
The code model for generating paths is described in detail. This can be part of the README or a separate doc labeled "Model Documentation".
