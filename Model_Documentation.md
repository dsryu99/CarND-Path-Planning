# **Path Planning Model Documentation**
---

## **Source Code Design**
### Vehicle Class
This is the main class to plan the vehicle's behavior for highway driving. We employ finite state machine (FSM) method to deal with the transition of multiple states of the ego vehicle.
The main member functions are as follows:

- choose_next_state() : Through FSM, the next state of the ego vehicle is computed.
- successor_states() : Provides the possible next states given the current state for the FSM.
- generate_path_points() : Based on the next state decided, valid trajectories are generated. For safe and smooth driving, points are interpolated through spline fitting.
- generate_predictions() : Generate predictions for non-ego vehicles te be used in trajectory generation for the ego vehicle.
- generate_trajectory() : Given a possible next state, generate the appropriate trajectory to realize the next state.
- get_kinematics() : 	Gets next timestep kinematics (position, velocity, acceleration) for a given lane. Tries to choose the maximum velocity and acceleration, given other vehicle positions and accel/velocity constraints.
- constant_speed_trajectory() : Generate a constant speed trajectory.
- keep_lane_trajectory() : Generate a keep lane trajectory.
- lane_change_trajectory() : Generate a lane change trajectory.
- prep_lane_change_trajectory() : Generate a trajectory preparing for a lane change.
- get_vehicle_behind() : Returns a true if a vehicle is found behind the current vehicle, false otherwise.
- get_vehicle_ahead() : Returns a true if a vehicle is found ahead of the current vehicle, false otherwise.

#### Behavior Planning
 States of the ego vehicle are as follows:
- "CS" - Constant Speed
- "KL" - Keep Lane
- "LCL" / "LCR" - Lane Change Left / Lane Change Right
- "PLCL" / "PLCR" - Prepare Lane Change Left / Prepare Lane Change Right

#### Trajectory Generation
The known map waypoints are not sufficient to estimate the location of points in terms of safe driving. Thus, we need to interpolate the position of those points.
For that purpose, we can employ polynomial fitting or spline fitting. In this project, spline fitting is used to generate points. I used [this spline tool](http://kluge.in-chemnitz.de/opensource/spline/) for C++.

### Cost Computation
We use two cost functions, i.e., global distance cost function and inefficiency cost function.
The main functions related to cost are as follows:
- calculate_cost() : computes the cost for a trajectory
- goal_distance_cost() : Cost increases based on distance of intended lane (for planning a lane change) and final lane of trajectory. Cost of being out of goal lane also becomes larger as vehicle approaches goal distance.
- inefficiency_cost() : Cost becomes higher for trajectories with intended lane and final lane that have slower traffic.
- lane_speed() : provides the expected speed of the predicted trajectory.
- get_helper_data() : provides some preprocessing of vehicle data that is useful in the cost functions. They include intended_lane, final_lane, and distance_to_goal.
