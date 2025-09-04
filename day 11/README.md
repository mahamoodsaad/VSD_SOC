## SPICE Overview
SPICE (Simulation Program with Integrated Circuit Emphasis) was created at UC Berkeley in the 1970s to model electronic circuits before fabrication. It uses an input file called a SPICE deck, originally structured as punch-card “cards.”

## SPICE Input
A SPICE deck includes a netlist of components and nodes, device models with parameters, initial conditions, circuit inputs (stimuli), and commands for the type of simulation to run.

## Why Use SPICE
Designers use SPICE to verify that gates, bias networks, and feedback loops function correctly. It predicts performance with DC and transient analyses, checks delays and bandwidth, and estimates power usage across supply, process, and temperature variations. It also allows parametric and Monte Carlo studies to explore tolerances and variability.

## CMOS Inverter
An inverter uses PMOS and NMOS transistors. The PMOS connects to VDD and the NMOS to GND, with gates tied to the input and drains tied to the output. When the input is high, NMOS pulls the output low; when the input is low, PMOS drives the output high.

<img width="306" height="287" alt="Screenshot 2025-09-03 at 4 14 02 PM" src="https://github.com/user-attachments/assets/62dc5cb5-6c9e-4171-a1f8-e2bb3874801a" />


## Inverter Analysis with SPICE
SPICE verifies inverter functionality, measures propagation delay, and calculates power. The load capacitor (CL) models the next stage or parasitic load. Results include voltage transfer curves and ID–Vout plots for NMOS devices at different input voltages.

<img width="653" height="403" alt="Screenshot 2025-09-03 at 4 15 00 PM" src="https://github.com/user-attachments/assets/22e702ff-8026-4b7f-9b29-6e01ae29102a" />

## Delay Tables
Digital cell delays depend on input slew and output load, stored in 2D or 3D lookup tables. A 2D LUT maps slew and load to delay, while a 3D LUT adds related output load.

<img width="650" height="346" alt="Screenshot 2025-09-03 at 4 15 20 PM" src="https://github.com/user-attachments/assets/2a750321-e136-4fe7-85c7-632485629c28" />
<img width="653" height="339" alt="Screenshot 2025-09-03 at 4 15 39 PM" src="https://github.com/user-attachments/assets/4e527860-d9bc-4e0c-a875-53f78c1ef9df" />


## NMOS Basics
An NMOS transistor consists of a p-type substrate with n+ source and drain regions, plus a gate electrode separated by SiO₂. With VGS=0, no channel forms and the device is off. When VGS exceeds the threshold voltage (Vt), an inversion layer creates a conductive channel.

<img width="318" height="276" alt="Screenshot 2025-09-03 at 4 16 21 PM" src="https://github.com/user-attachments/assets/aee921ef-4fd9-4a74-8ef5-7be8a2e0cd05" />

## Body Effect
If the source-to-body voltage (VSB) is greater than zero, the threshold voltage increases. This widens the depletion layer, requiring higher VGS for strong inversion. This is called the body or substrate bias effect.

<img width="654" height="310" alt="Screenshot 2025-09-03 at 6 58 03 PM" src="https://github.com/user-attachments/assets/5ff6b5bc-b90d-487a-964a-0b434cebde9c" />
<img width="652" height="274" alt="Screenshot 2025-09-03 at 6 58 18 PM" src="https://github.com/user-attachments/assets/350e45ab-fd4a-4e1d-b0fe-f65e87724fb0" />
<img width="653" height="306" alt="Screenshot 2025-09-03 at 6 58 31 PM" src="https://github.com/user-attachments/assets/e31405ea-6001-4158-bfcd-89fa4c6fee23" />

## Linear Region
For VGS ≥ Vt and small VDS, the NMOS acts like a voltage-controlled resistor. 

<img width="654" height="374" alt="Screenshot 2025-09-03 at 7 12 28 PM" src="https://github.com/user-attachments/assets/a9c7ca49-3baf-4ab2-bfab-762d5434e253" />
<img width="650" height="351" alt="Screenshot 2025-09-03 at 7 12 42 PM" src="https://github.com/user-attachments/assets/c6b593df-c912-4dfa-8d82-c4845bc455dd" />


## Saturation Region
When VDS > VGS – Vt, the channel pinches off near the drain. Current depends mainly on gate overdrive (VGS – Vt) and only weakly on VDS, with channel-length modulation adding a slope.
<img width="656" height="306" alt="Screenshot 2025-09-03 at 7 13 06 PM" src="https://github.com/user-attachments/assets/b96750e4-79ce-49bd-b562-911a7291dbe2" />
<img width="653" height="338" alt="Screenshot 2025-09-03 at 7 13 18 PM" src="https://github.com/user-attachments/assets/83aaec4b-3d2d-4e89-bdb4-79996f2ee410" />
<img width="656" height="334" alt="Screenshot 2025-09-03 at 7 13 32 PM" src="https://github.com/user-attachments/assets/7fd11515-5c1b-4a52-a5f9-b4e26ed3539a" />


## Levels of Simulation
Simulation can occur at multiple levels. Process simulators predict how fabrication affects devices. Circuit simulators like SPICE model voltages and currents. Logic simulators verify digital behavior, while architecture simulators study performance at the instruction and memory level.

## Sky130 Lab
Cloning the Sky130 workshop repo provides SPICE files. Running ngspice day1_nfet_idvds_L2_W5.spice and plotting -vdd#branch shows ID–VDS characteristics of an NMOS device with W=5 µm, L=2 µm.
<img width="1034" height="602" alt="Screenshot 2025-09-03 at 2 07 22 PM" src="https://github.com/user-attachments/assets/f417052f-b77b-4d53-b65a-ab6e56003cd8" />
