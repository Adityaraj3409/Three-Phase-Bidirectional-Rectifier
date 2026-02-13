# Three-Phase Bidirectional PWM Rectifier with dq Vector Control (EV Charger)

A MATLAB/Simulink implementation of a grid-connected three-phase bidirectional rectifier using synchronous dq-axis vector control for Electric Vehicle (EV) charging and Vehicle-to-Grid (V2G) operation.
This project demonstrates advanced power electronics control including PLL synchronization, decoupled current control, and bidirectional power flow between the AC grid and battery-based DC bus.

## ⚡ Project Overview

Modern EV chargers require efficient AC-DC conversion with precise current control and the ability to operate bidirectionally.
This project models a Voltage Source Converter (VSC) with dq-axis control to regulate active and reactive power exchange with the grid.

The controller enables:

✅ Grid-to-Vehicle charging (G2V).  
✅ Vehicle-to-Grid discharging (V2G).  
✅ Unity power factor operation.  
✅ Stable DC bus regulation.  

## 🧠 Control Architecture
The system uses synchronous reference frame control.

🔹 Phase Locked Loop (PLL)
+ Synchronizes controller with grid voltage phase.  
+ Provides angle θ for abc↔dq transformations.

🔹 dq Current Control
Active and reactive currents are controlled independently:
+ Id → Controls active power / battery charging current.  
+ Iq → Controls reactive power (set to 0 for unity PF).

🔹 Modulation Stage  
+ dq → abc transformation  
+ PWM generation  
+ Gate signal creation for converter switches

## 📊 Simulation Results

The model demonstrates:  
+ Stable dq current tracking.  
+ Proper bidirectional power flow.  
+ Controlled battery charging/discharging.  
+ PLL synchronized operation.  
Smooth DC bus dynamics.
