# GoFa-CRB 15000-Project

## Problem 1: Developing a Digital Robot Model 

### 1A. DH Parameters + Forward Kinematics
Description:
- Place coordinate frames on the GoFa robot model in CAD file.
- Derive Denavit–Hartenberg (DH) parameters.
- Compute forward kinematics from base frame to end-effector frame.

Solution:
- Frames were assigned using the standard DH convention (Zi aligned with joint rotation axes).
- DH table was derived from the CAD model measurements.
- Forward kinematics was built by chaining transforms from joint 1 to joint 6, then to the end-effector.
- An additional offset transform was added to correct the CAD-measured end-effector offset.

### 1B. Jacobian Matrix (6x6)
Description:
- Derive the full geometric Jacobian in the base frame.
- Use MATLAB/Python to compute and verify the Jacobian.

Solution:
- Linear Jacobian Jv computed by symbolic differentiation of end-effector position p(q) w.r.t joint angles q.
- Angular Jacobian Jw formed from joint axes expressed in the base frame (Zi vectors from cumulative rotations).
- Verified correctness using numeric evaluation at test joint configurations and finite-difference checks.

### 1C. Writing Trajectory + Motion-Rate Control (Simulation)
Description:
- Create a trajectory for a writing task (team initials: “BLJT”).
- Apply motion-rate control (Jacobian-based) to track the trajectory.
- Simulate and record robot behaviour in MATLAB/Simulink.

Solution:
- Letters were generated as waypoint strokes using Hershey font, then converted into 3D points (x, y, z).
- Pen-lift segments were added by raising Z between letters to avoid unwanted lines.
- Letters were shifted along the x-axis and connected using via points for clean transitions.
- A smooth time-based trajectory was generated using mstraj and exported as traj_data.mat.
- Simulink tracking used:
- Forward-kinematics feedback (GoFa Kinematic block)
- Inverse Jacobian block to compute joint updates
- A fixed initial joint configuration (“ready-to-go” pose) for stable start.

## Problem 2: Operating a Physical Robot 

### 2A. RAPID Program in RobotStudio (Letters on A4)
Description:
- Write a RAPID program for the GoFa Education Station to draw the first letters of team members.

Solution:
- Each letter (B, L, J, T) was represented by persistent target points on an A4 workspace frame.
- Tool orientation was kept consistent using quaternion parameters.
- Pen engagement was controlled by Z (writing height vs lift height).
- Motions used:
- MoveL for straight strokes
- MoveC for curved strokes
- MoveJ for safe transitions
- Motion tuning used velocity presets (V100, Vdraw), zone fine, and the defined workobject for the drawing base.

### 2B. Track MATLAB Trajectory on RobotStudio / Robot
Description:
- Control the GoFa robot to follow the trajectory generated in Problem 1.

Solution:
- MATLAB trajectory execution was integrated through a ROS 2 bridge (instead of manual RAPID waypoint programming).
- MATLAB (ROS Toolbox) publishes FollowJointTrajectory goals with time-from-start.
- ROS 2 middleware delivers trajectories to an ABB ROS 2 driver.
- The driver streams compatible commands to the RobotStudio Virtual Controller for smooth interpolation and synchronized joint motion.
## Technical Challenges:
- While the kinematic calculations and RAPID motion programs are fully implemented and verified, the ROS 2 bridge intended to automate trajectory transfer via the ROS 2 middleware is currently under development. At present, trajectories are manually synchronized within the RobotStudio Virtual Controller.

## Demo Video
### Simulation of CRB 15000 in Simulink:
![Image](https://github.com/user-attachments/assets/9753742f-ed73-4d2f-a46b-27a2d3975a7b)
### Simulation of CRB 15000 in RobotStudio:
![Image](https://github.com/user-attachments/assets/176ca8d6-dc59-4e71-9295-07e1f4f54579)
### Demonstration with the real GoFa CRB 15000
![Image](https://github.com/user-attachments/assets/474979b5-c5e3-4d1f-ad4b-5bdf38d50d44)
### Demo Result
![Demo Result](https://github.com/user-attachments/assets/79e6f532-454c-4da4-8713-da859f2edda6)



