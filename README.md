# Three-Phase Bidirectional PWM Rectifier with dq Vector Control (EV Charger)

A MATLAB/Simulink implementation of a grid-connected three-phase bidirectional rectifier using synchronous dq-axis vector control for Electric Vehicle (EV) charging and Vehicle-to-Grid (V2G) operation.
This project demonstrates advanced power electronics control including PLL synchronization, decoupled current control, and bidirectional power flow between the AC grid and battery-based DC bus.

## ⚡ Project Overview

Modern EV chargers require efficient AC-DC conversion with precise current control and the ability to operate bidirectionally.
This project models a Voltage Source Converter (VSC) with dq-axis control to regulate active and reactive power exchange with the grid.

<img width="1919" height="824" alt="Screenshot 2026-02-13 193747" src="https://github.com/user-attachments/assets/878cc1fe-49c5-4b9d-bf33-54727834e5f5" />

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

## 🛠️ Technical Implementation

### ⚡ Phase Locked Loop (PLL)

<img width="784" height="562" alt="image" src="https://github.com/user-attachments/assets/d9c3aad5-71e7-48aa-b009-bd4cfc6a0b40" />


The PLL (3-phase) block receives the measured three-phase grid voltage Vabc and estimates:

+ Grid phase angle (θ=ωt)  
+ Grid frequency  
+ Synchronous reference frame angle  

Purpose:  
+ Synchronizes the controller with the grid voltage.
+ Aligns the d-axis with the grid voltage vector.

When the PLL is locked:  
+ Vq ≈ 0  
+ Vd = Grid voltage magnitude

### 🔄 abc → dq0 Transformation (Voltage)

The measured grid voltages Vabc are transformed into the rotating dq frame using the PLL angle.

Outputs:  
+ Vd → Direct-axis voltage (aligned with grid)
+ Vq → Quadrature-axis voltage  

Meaning:  
+ Vd represents active-power component.  
+ Vq represents reactive-power component.  
+ This transformation converts sinusoidal AC signals into DC-like signals, making PI control possible.  

### 🔄 abc → dq0 Transformation (Current)

Similarly, measured phase currents Iabc are transformed using the same PLL angle:

Outputs:  
+ Id → Active current component  
+ Iq → Reactive current component  

Control Interpretation:  
+ Id controls power flow (charging/discharging)
+ Iq controls power factor / reactive power

## ⚡ dq Control – Controller Explanation

<img width="1661" height="565" alt="image" src="https://github.com/user-attachments/assets/e5301922-08f7-4ac6-b701-f40fb77cdde9" />

### 1️⃣ Outer DC-Link Voltage Control Loop

The upper-left part of the diagram generates the active current reference (Idref​)

🔹 Working Principle  

The DC-link voltage is compared with a reference value:  
+ error(ev) ​= Vdcref ​− Vdc  ​
Where:  
+ Vdcref = 800V (constant block)  ​
+ Vdc ​= measured DC bus voltage

This error passes through a PI controller  

### 🎯 Purpose

+ Regulates DC bus energy.  
+ Controls active power flow between grid and battery.  
+ Automatically adjusts charging/discharging current.  

👉 If battery absorbs power → Idref > 0  
👉 If battery injects power → Idref ​< 0  

### 2️⃣ Inner d-Axis Current Controller (Active Power Control)

The middle section compares:
+ Id_ref  –  Id  
This current error goes into a PI controller to generate the voltage command:  
+ Vd∗ ​= PI (Idref − Id)

🔹 Meaning of Vd∗
+ Reference voltage along grid voltage axis.  
+ Directly controls active power.  

This is the fast inner loop responsible for tracking current quickly.  

### 3️⃣ Inner q-Axis Current Controller (Reactive Power Control)

The bottom loop controls reactive current:  
+ Iq_ref = 0

Error signal:  
+ eq​=Iqref−Iq​

PI output:  
+ Vq∗​=PI(eq​)

🎯 Why Iqref = 0 ?
+ Ensures unity power factor.  
+ Eliminates reactive power exchange with grid.

🧠 Basic PI Tuning Formulas (for L-Filter Grid Converter)  

For inner current loop:  
+ Kpi ​= L⋅ωc  ​
+ Kii ​= R⋅ωc​

Where:  
L = filter inductance  
R = filter resistance  
ωc​ = current loop bandwidth (rad/s)  

For outer voltage loop (slow loop):  
+ Kpv = Cdc × ​ωv ​/ Vdc  
+ Kiv  = Cdc × (ωv)*2​

Where:  
+ Cdc = DC-link capacitor  
+ ωv ​= voltage loop bandwidth (ωv ≈ (1/5 to 1/10) × ωc)


## 📊 Simulation Results

<img width="1915" height="901" alt="image" src="https://github.com/user-attachments/assets/5815257d-b27d-49f5-8562-0e8d4cf0ba3b" />



The model demonstrates:  
+ Stable dq current tracking.  
+ Proper bidirectional power flow.  
+ Controlled battery charging/discharging.  
+ PLL synchronized operation.  
Smooth DC bus dynamics.

## 👨‍💻 Author

Aditya Raj  
(Electrical Engineering Student at RGIPT)
