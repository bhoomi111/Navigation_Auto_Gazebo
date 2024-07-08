## Description
This repository contains a project focused on autonomous navigation using ROS and Gazebo simulation environment. It includes setups for robot models, environment configurations, and navigation algorithms.

## Code Analysis & Explanations
### Nodes
#### pos_info:
Subscribes to odom_combined topic from robot_pose_ekf.
Provides refined pose estimates using Extended Kalman Filtering (EKF) with IMU data.
#### depth_info:
Publishes depth information from Turtlebot's RGB-D Kinect sensor.
Used for basic obstacle detection directly in Turtlebot's path.
#### scan_info:
Subscribes to \scan topic from laserscan_nodelet_manager.
Converts RGB-D Kinect data to 'fake' laser readings.
Provides depth information for leftmost, rightmost, and middle readings in the horizontal axis.
Used in bot_control for preemptive obstacle detection.
#### bot_control:
Main node for motion control, obstacle avoidance, and pathfinding.
Subscribes to /auto_ctrl/scan_info, /auto_ctrl/depth_info, and /auto_ctrl/pos_info.
Implements algorithms from algo.h (primarily A* search) for navigating while avoiding obstacles.
### Algorithm file algo.h
Contains A* search and Flood Fill algorithms.
Utilizes an internal 2D array map for computing navigation paths.
### Launch files
#### world.launch:
Launches Gazebo environment using defined worlds.
Includes empty_world.launch with modifications for compatibility (<remap from="tf" to="gazebo_tf"/>).
#### empty_world.launch:
Original from ROS with added remap for tf to avoid conflicts with robot_pose_ekf.
#### bot.launch:
Main launch file for running robot_pose_ekf and associated nodes (pos_info, depth_info, scan_info, bot_control).
