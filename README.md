# Autonomous Minidrone Line Tracking & Precision Auto-Landing

**Developer:** Vrushabhkumar Khade  
**Department:** Automation and Robotics, MES Wadia College of Engineering (Team Aviate)  

## Project Overview
This repository contains a robust MATLAB/Simulink flight control architecture for an autonomous multirotor minidrone. The system is designed to autonomously track a red line, navigate sharp corners without yaw-spinning, coast safely over track gaps, and execute a precision landing on a separate red circular target. 

## Objectives
* **Eliminate Continuous Yaw-Spinning:** Replace proportional heading loops with a fixed-yaw lateral strafing architecture.
* **Proactive Cornering:** Detect sharp track bends before reaching them using a 4-quadrant edge-windowing system.
* **Gap Navigation:** Implement a coasting state to safely glide over track breaks without losing orientation.
* **Precision Landing:** Use topological edge-detection and centroid-hunting to center perfectly on the landing circle.

## Key Features
* **4-Way Window Direction-Switching:** Splits the $120 \times 160$ camera feed into forward, back, left, and right quadrants to dynamically detect side lines and snap the drone's travel direction.
* **Dynamic PI Cross-Track Control:** Uses a Proportional-Integral controller to eliminate steady-state drift and keep the drone dead-center on the line.
* **Topological Edge-Detection Landing:** The drone distinguishes between a thick track line and the landing circle by verifying that red pixels exist (`area > 150`) but do not touch the camera edges (`edge_pixels < 25`).
* **Multi-Rate System Architecture:** Safely crosses data between the $0.2\text{ s}$ camera processing loop and the $0.005\text{ s}$ high-speed flight control loop using properly configured Rate Transition blocks.
