# SPICE Simulations of NMOS and CMOS Circuits

To explore **SPICE simulations** of NMOS transistors and CMOS inverters across technology nodes, comparing long-channel and short-channel device behaviors, and analyzing their current–voltage characteristics.

---

##  NMOS Output Characteristics

**Device Parameters:**  
- Long Channel: W = 1.8 μm, L = 1.2 μm (W/L = 1.5)  
- Short Channel: W = 0.375 μm, L = 0.25 μm (W/L = 1.5)  

<img width="708" height="390" alt="Screenshot 2025-09-10 at 4 01 37 PM" src="https://github.com/user-attachments/assets/b72bce37-3a82-443b-9b85-d86807d400fd" />


### Operating Regions
- **Linear Region**:  
  - Id ∝ Vds  
  - Valid for Vds < (Vgs – Vt)  

- **Saturation Region**:  
  - Id depends on channel length modulation and Vds  
  - Valid for Vds ≥ (Vgs – Vt)  

---

##  Observation 1: Long vs. Short Channel Devices

- **Long Channel NMOS** → Drain current (Id) shows quadratic dependence on Vgs.  
- **Short Channel NMOS** → Id is quadratic at low Vgs, but becomes linear at high Vgs due to **velocity saturation**.  

<img width="710" height="359" alt="Screenshot 2025-09-10 at 4 02 04 PM" src="https://github.com/user-attachments/assets/4ecff8c6-50e9-43a4-9b3e-0fbfc83541cb" />
The left plot corresponds to a device with W = 1.8μm and L = 1.2μm (long-channel device), and the right plot corresponds to W = 0.375μm and L = 0.25μm (short-channel device).

 Velocity saturation limits carrier velocity at high electric fields, flattening the Id–Vgs curve.

---

##  Observation 2: Peak Current Comparison

- Long Channel (W = 1.8 μm, L = 1.2 μm): **Id_peak = 410 μA**  
- Short Channel (W = 0.375 μm, L = 0.25 μm): **Id_peak = 210 μA**  

<img width="715" height="368" alt="Screenshot 2025-09-10 at 4 03 46 PM" src="https://github.com/user-attachments/assets/6c9b7934-2230-434f-bcfe-3c6ef385c9b3" />

Short-channel devices switch faster and scale better, but exhibit lower peak current due to velocity saturation.

---

##  Lab Simulations with SKY130

### Id–Vds Characteristics
<img width="958" height="505" alt="Screenshot 2025-09-03 at 2 09 06 PM" src="https://github.com/user-attachments/assets/6a7a72a4-a545-4d5b-8334-9bebb4f28fbc" />


# CMOS Voltage Transfer Characteristics (VTC)

This section explains the behavior of MOSFETs as switches, CMOS inverter operation, and the derivation of the **Voltage Transfer Characteristic (VTC)** using load line analysis.

---

## MOSFET as a Switch

- **OFF State**  
  - Condition: |Vgs| < |Vth|  
  - Behavior: MOSFET acts as an **open switch** (infinite resistance).  

- **ON State**  
  - Condition: |Vgs| > |Vth|  
  - Behavior: MOSFET acts as a **closed switch** (finite resistance).  

<img width="713" height="429" alt="Screenshot 2025-09-10 at 4 11 02 PM" src="https://github.com/user-attachments/assets/ef8d7cce-6577-4bda-8ba1-85be72dc052b" />

---

## CMOS Inverter Representations

The CMOS inverter can be viewed in two levels of abstraction:  

1. **Transistor-Level**  
   - PMOS connected to Vdd  
   - NMOS connected to Vss  
   - Vin applied to both gates  
   - Vout taken at the common drain  

2. **Switch-Level**  
   - When Vin = Vdd → NMOS ON, PMOS OFF → Vout = 0  
   - When Vin = 0   → PMOS ON, NMOS OFF → Vout = Vdd  

<img width="712" height="400" alt="Screenshot 2025-09-10 at 4 11 21 PM" src="https://github.com/user-attachments/assets/a60b2a74-14a5-45f9-9a91-578962e1a496" />

 This model explains why CMOS inverters provide **sharp transitions** and **low static power consumption**.

---

## PMOS/NMOS Id–Vds Characteristics

The drain current of PMOS and NMOS transistors is plotted against drain voltage (**Id–Vds** curves).  
These characteristics form the basis for understanding how NMOS and PMOS devices interact in the inverter.

---

## Load Line Analysis for CMOS Inverter

To derive the **Voltage Transfer Characteristic (VTC)**:

**Step 1**: Convert PMOS gate-source voltage (VgsP) into an equivalent Vin.  
Replace internal node voltages with **Vin, Vdd, Vss, and Vout**.

<img width="712" height="347" alt="Screenshot 2025-09-10 at 4 11 46 PM" src="https://github.com/user-attachments/assets/686c736d-0639-4afb-b908-95beb902ac86" />

**Step 2 & Step 3**: Express PMOS and NMOS drain-source voltages in terms of **Vout**.  

<img width="712" height="386" alt="Screenshot 2025-09-10 at 4 12 03 PM" src="https://github.com/user-attachments/assets/e714cbeb-c6f0-4e39-a2bf-530aa78833a9" />

**Step 4**: Merge NMOS and PMOS load curves by equating their Ids with respect to Vout.  

<img width="717" height="393" alt="Screenshot 2025-09-10 at 4 12 22 PM" src="https://github.com/user-attachments/assets/dad835d5-6437-4742-a1ee-84b512d87747" />

Finally, sweep **Vin** and plot **Vout** to obtain the inverter **VTC curve**, which shows the switching behavior from logic HIGH to logic LOW.

---
