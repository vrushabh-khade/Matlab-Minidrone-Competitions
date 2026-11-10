# Requirements & Prerequisites

To run this simulation without Coder or Type mismatch errors, ensure your environment meets the following specifications:

## Software
* **MATLAB** (R2023a or newer recommended)
* **Simulink**

## Required MathWorks Toolboxes
* Aerospace Blockset (Required for Minidrone simulation environment)
* Computer Vision Toolbox (For image thresholding and blob matrix operations)
* MATLAB Coder & Simulink Coder (For strictly-typed C++ code generation)
* Control System Toolbox

## System Configuration
* Ensure all Rate Transition blocks between the vision system and flight controller have **Initial conditions** set to `0`.
* Uncheck "Ensure deterministic data transfer (maximum delay)" on the Rate Transition blocks to allow instant 0.005s flight loop updates.
