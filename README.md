# Building-a-Controller
Overview:
In this project, our goal is to implement a cascaded PD controller with an
structure similar to the control architecture shown in the image below.
# Controller:
# 1. Implemented body rate control in C++.
This method determines the commanded 3-axis moment for the vehicle. As input, this
method receives the current 3-axis body rate pqr and desired 3-axis body rate pqrCmd.
Both inputs are supplied as a 3-tuple using the V3F data type.
To get the difference between the rate and desired body rates, using the V3F type,
# 2. Implement roll pitch control in C++.
This method determines the roll and pitch angle rates for the vehicle. As input, this
method receives a commanded acceleration as a V3F (roll and pitch only), the vehicle
attitude as a Quaternion and a collective thrust command. This controller is
implemented as a P controller.
In order to convert between the world frame and body frame, a rotation matrix will be
required. 
# 3. Implement altitude controller in C++.
This method determines the vertical thrust required given the current vehicle attitude,
commanded vs. actual altitude, commanded vs. actual vertical velocity, feed forward
vertical acceleration command and current timestep.
This controller is implemented as a full PID controller. I am globally (at a global class
level) tracking the integrated altitude error which is also used as input data.
First, calculate proportional, integral and differential components:

# 4. Implement lateral position control in C++.
This method determines the commanded acceleration in x/y given the current vs
desired lateral position, current vs. desired lateral velocity and lateral feed forward
acceleration.
This method will use a PD controller to output lateral acceleration commands in the x/
y direction that will be used as input to the Roll Pitch Controller. The Roll Pitch
Controller then will convert those accelerations into to b_x_c and b_y_c.
Ultimately, this will generate lateral acceleration by changing the vehicle's body
orientation. 
# 5. Implement yaw control in C++.
This method determines the yaw rate in rad/s given a current and commanded yaw,
given in radians. This controller will be implemented as a P controller.
First, we need to make sure that the rate part in radians that is less than a full
rotation
# 6. Implement calculating the motor commands given commanded thrust and moments in C++.
This method converts the commanded moments and vertical thrusts, which were
outputs from the other controllers into a single set of thrust commands, one per
motor.
For the first part, I needed to determine the 4 thrusts overall, 3 for the moments and 1
for the vertical thrust.
 
# Flight Evaluation
1. Your C++ controller is successfully able to fly the provided test trajectory
and visually passes inspection of the scenarios leading up to the test
trajectory.

# Position control gains
kpPosXY = 25
kpPosZ = 50
KiPosZ = 20
# Velocity control gains
kpVelXY = 10
kpVelZ = 20
Upon completion we got the following feedback:
Simulation #1401 (../config/4_Nonidealities.txt)
PASS: ABS(Quad1.PosFollowErr) was less than 0.100000 for at least 1.500000 seconds
PASS: ABS(Quad2.PosFollowErr) was less than 0.100000 for at least 1.500000 seconds
PASS: ABS(Quad3.PosFollowErr) was less than 0.100000 for at least 1.500000 seconds
Scenario #5: Tracking Trajectories
For the final scenario, there were 2 quads that are following a figure 8 trajectory.
This scenario passed, as shown by the result:
Simulation #89 (../config/5_TrajectoryFollow.txt)
PASS: ABS(Quad2.PosFollowErr) was less than 0.250000 for at least 3.000000 seconds
