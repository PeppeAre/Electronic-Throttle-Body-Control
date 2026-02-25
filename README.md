# Analysis and Control of an Electronic Throttle Body System

This repository contains the simulation models, analysis scripts, and technical documentation developed for the **"Nonlinear Dynamics & Control"** course. The project focuses on the modeling, analysis, and control of an Electronic Throttle Body (ETB), a critical automotive component used to regulate air mass flow in internal combustion engines.

## 📝 Project Overview

The ETB is a highly non-linear system. While standard linear controllers are often used in the industry, they can struggle when operating far from their linearization point or when subjected to physical wear and external disturbances. This project explores the system's dynamics and compares a traditional linear approach with a robust non-linear control strategy.

**Key Non-linearities addressed:**
* **Stiffness:** Cubic hardening return spring modeled as $k_1\theta + k_2\theta^3$.
* **Damping:** Non-linear aerodynamic damping modeled as $b_2\dot{\theta}|\dot{\theta}|$.

## 📂 Repository Structure

* **`ETB_NLDC.mlx` (MATLAB Live Script):** This script contains the rigorous Open-Loop analysis of the system. It includes the calculation of the equilibrium point (the Limp Home position) and the stability analysis using the Phase Plane method and Lyapunov theory. It demonstrates how parameter variations (e.g., increasing damping or decreasing stiffness) change the system's topology from a stable spiral to a stable node.
* **`ETB_NLDC.slx` (Simulink Model):** The core simulation environment. It features a parallel architecture allowing both controllers to run simultaneously on identical plant models. 
    * *Interactive Masks:* The subsystems use editable masks to easily inject external matched disturbances (sinusoidal) and apply parametric uncertainties (up to 30%) without altering the underlying blocks.
* **`NLDC_Report.pdf` & `NLDC_Presentation.pdf`:** Comprehensive technical report and summary slides detailing the mathematical modeling, control synthesis, and quantitative performance comparisons.

## ⚙️ Control Strategies Compared

### 1. State Feedback Controller (SFC)
Designed based on a linearized "Small Signal Model" around the equilibrium point.
* **Pros:** Simple implementation and generates a smooth, continuous control signal that is mechanically gentle on the DC motor and gears.
* **Cons:** Performance degrades significantly in robustness tests. It exhibits static steady-state errors under parametric uncertainty and fails to reject external sinusoidal oscillations.

### 2. Sliding Mode Controller (SMC)
A non-linear control strategy designed explicitly to handle the system's inherent non-linearities and enforce robustness.
* **Pros:** Superior tracking accuracy. Guarantees practically zero error even in "worst-case" scenarios combining dynamic references, 30% parametric mismatch, and external disturbances.
* **Cons & Practical Solution (Chattering Mitigation):** Ideal SMC uses a discontinuous switching function (relay/sign) that causes high-frequency *chattering* in the control effort. In physical Hardware-In-the-Loop (HIL) applications, this risks overheating the motor coils and causing mechanical wear. To solve this, the implementation utilizes a "Boundary Layer" technique, replacing the pure sign function with a saturation function. This successfully mitigates chattering, accepting a negligible steady-state error to preserve actuator health.

## 🚀 How to Run

1. Clone the repository.
2. Open MATLAB 2024 or newer(Ensure Simulink and Stateflow toolboxes are installed).
3. Run the `ETB_NLDC.mlx` script to load the nominal system parameters and look at the analysis of the system and computation for linear controlller.
4. Open `ETB_NLDC.slx`(ensure you have the folder "immagini" at the same path).
5. Double-click the Controller subsystems to toggle the `Enable Disturbance` and `Enable Uncertainty` checkboxes.
6. Run the simulation and open the `Plot` subsystem to view the comparative scopes for Position ($x_1$), Velocity ($x_2$), Error, and Control Effort ($u$).
