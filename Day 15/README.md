# Static Behaviour Evaluation — CMOS Inverter Robustness

This section explores how **power supply variation** and **device variation** affect the static robustness of CMOS inverters. Robustness is evaluated using switching threshold (Vm), noise margins, and Voltage Transfer Characteristics (VTCs) from SPICE simulations.

---

## Power Supply Variation

### Overview
Scaling the supply voltage (`Vdd`) directly impacts the inverter’s static behavior:
- Shifts the switching threshold **Vm**
- Reduces **noise margins**
- Alters overall **robustness**

<img width="654" height="415" alt="Screenshot 2025-09-12 at 12 06 50 PM" src="https://github.com/user-attachments/assets/dcbccc04-6414-4380-b58d-28ccb9eaa611" />


**Switching Threshold (Vm)**  
- As `Vdd` decreases, Vm shifts closer to the center of the supply range.  
- Lower supply → smaller margins → weaker noise immunity.  

**Noise Margins**  
- Reduced at lower `Vdd`, making the circuit more sensitive to disturbances.  

**Performance Impact**  
- Lower supply: saves **static & dynamic power** but reduces robustness.  
- Higher supply: improves noise margins but **increases power dissipation**.  

### Trade-offs
Power supply scaling balances **low-power operation** with **noise robustness**.  
This is critical in modern **low-voltage CMOS design**.

---

## SPICE Example

<img width="959" height="510" alt="Screenshot 2025-09-03 at 2 15 10 PM" src="https://github.com/user-attachments/assets/55509f0b-4389-4273-9dfc-323068951f81" />

* CMOS Inverter with supply variation

# Static Behaviour Evaluation — CMOS Inverter Robustness (Device Variation)

## Overview
Device variation is a critical factor that influences the **robustness** of a CMOS inverter.  
It arises from **fabrication non-idealities** such as **etching variation** and **oxide thickness variation**, which alter the effective transistor dimensions and performance.

---

## Etching Variation
Etching defines the **physical structures** in a CMOS layout (Width `W`, Length `L`, diffusion regions, and interconnects).  
During fabrication, small deviations can occur between designed and actual values:

- **P-diffusion region** → sets PMOS gate width  
- **N-diffusion region** → sets NMOS gate width  
- **Poly-silicon layer** → defines channel length `L` (linked to the technology node, e.g., 20 nm, 30 nm, 45 nm)  
- **Other features**: metal layers and interlayer contacts  

### Impact
- Actual `W` and `L` differ from intended values  
- Drain current (`Id ∝ W/L`) changes  
- Results in variations in:  
  - **Switching Threshold (Vm)**  
  - **Noise Margins**  
  - **Overall robustness and performance**  
---

## Inverter Chain Example

<img width="655" height="369" alt="Screenshot 2025-09-12 at 12 11 54 PM" src="https://github.com/user-attachments/assets/4ff740e0-b6af-4a27-a01b-6e30eb2e1d96" />

Inverter chains (series of CMOS inverters) are often used to study:
- **Propagation delay**
- **Cumulative variation effects**
- **Robustness across multiple stages**

**Layout layers in an inverter chain:**
- Poly (Gate)  
- P-Diffusion  
- N-Diffusion  
- VDD / VSS rails

<img width="654" height="332" alt="Screenshot 2025-09-12 at 12 12 13 PM" src="https://github.com/user-attachments/assets/e9d1cc19-0f90-495c-956d-7a30d394fa0f" />

---

## Oxide Thickness Variation
Another key source of variation comes from **oxide thickness** (`tox`) of the MOS gate.  

- Ideally, gate oxide has a uniform thickness.  
- In reality, deviations occur during fabrication.  

## SPICE Example
<img width="960" height="516" alt="Screenshot 2025-09-03 at 2 20 15 PM" src="https://github.com/user-attachments/assets/bee779f4-210a-4e35-8b35-72f7d77eab57" />


