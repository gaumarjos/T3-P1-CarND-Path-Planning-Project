# CarND-Path-Planning-Project
Self-Driving Car Engineer Nanodegree Program

### Model Documentation
#### Finite State Machine
The path planning model is based on a FSM with 4 states:
- "keep": stays in the current lane, accelerating if the car is driving below the speed limit and keeping just below the speed limit if already there.
- "take action": when the car approaches another in front and the distance is below 30m, the car starts braking and evaluates a cost function to decide what to do, either stay behing, turn left or turn right. If, a result of the braking or because the car in front accelerated, the distance returns above 30m, it just rolls back to the "keep" state.
- "go left", "go right": the car accelerates and changes lane. When the maneuver is completed, the state goes back to "keep".

#### Cost function
A function named "environmentFrontRear" provides distance and speed information about the cars immediately ahead and back of the ego vehicle. These are taken as inputs by the cost function.

The decision about what to do when in "take action" state depends on the result of a cost function that returns a number for each lane (current, left and right) to choose the most convenient one (the lower the cost, the better the option is considered).

This decision takes into account:
- Lane: to avoid illegal lane changes that leave the drivable portion of the road.
- Space ahead: to ensure there's enough free space ahead, there's a high cost just in front and an exponentially decreasing cost after, that helps choosing the most free (hence, "promising") lane.
- Space at the back: to ensure the car is not cutting off another car.
- Speed ahead: the higher the speed of the vehicle ahead, the most "promising" that lanes become.

#### Braking
To avoid crashing into cars cutting off the ego vehicle, the braking intensity is dependent on the distance from the vehicle ahead.

#### Smoothing down lane changes
To provide a smooth and low jerk lane change, both an averaging with the current vehicle situation and a spline solution are used, as suggested in project documentation and walkthrough.
