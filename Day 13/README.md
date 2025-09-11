# CMOS Switching Threshold and Dynamic Simulations

This section demonstrates **CMOS inverter static and dynamic analysis** using SPICE simulations, focusing on Voltage Transfer Characteristics (VTC), transient response, and switching threshold voltage (Vm).

---

## CMOS Inverter SPICE Deck

To simulate a CMOS inverter in SPICE, define:  

- **Component Connectivity**: PMOS (M1), NMOS (M2), supply (Vdd), ground (Vss), input (Vin), output (Vout).  
- **Component Values**: Transistor dimensions (W/L), supply voltage, load capacitance (Cload).  
- **Node Naming**: in, out, vdd, vss, plus transistor terminals.  
- **Simulation Commands**: `.dc`, `.tran`, `.op`, and technology model include statements.  

### Example SPICE Deck
<img width="651" height="305" alt="Screenshot 2025-09-10 at 7 02 52 PM" src="https://github.com/user-attachments/assets/cb0283a1-9446-4a8c-9d52-aadd46e20adf" />

<img width="653" height="309" alt="Screenshot 2025-09-10 at 7 03 08 PM" src="https://github.com/user-attachments/assets/8cfe81a4-5988-472e-af41-d65a82f8c808" />


* CMOS Inverter
M1 out in vdd vdd pmos W=0.375u L=0.25u
M2 out in 0   0   nmos W=0.375u L=0.25u
Cload out 0 10f

Vdd vdd 0 2.5
Vin in  0 2.5

## Labs Sky130 SPICE simulation for CMOS
day3_inv_vtc_Wp084_Wn036.spice
<img width="956" height="512" alt="Screenshot 2025-09-03 at 2 10 54 PM" src="https://github.com/user-attachments/assets/c8e72cd5-7d0d-4da9-b64b-5c436036dce9" />

Voltage Transfer Characteristics (VTC) of a CMOS Inverter:

day3_inv_tran_Wp084_Wn036.spice
<img width="960" height="512" alt="Screenshot 2025-09-03 at 2 12 41 PM" src="https://github.com/user-attachments/assets/da2567aa-ccb2-443c-abed-6f389f69a629" />

output waveform of transient analysis of a CMOS inverter, illustrating rise time delay and fall time delay:

# Static Behavior Evaluation — CMOS Inverter Robustness

The robustness of a CMOS inverter is defined by several key characteristics:

- **Switching Threshold Voltage (Vm)**
- **Noise Margins**
- **Power Supply Variation Tolerance**
- **Device Parameter Variations**

---

## Switching Threshold Voltage (Vm)

The **Switching Threshold Voltage (Vm)** is the input voltage at which the output voltage equals the input:
Vin = Vout
- At **Vm**, both NMOS and PMOS are **ON in saturation**.  
- This point corresponds to **maximum voltage gain** in the inverter transfer curve.  
- Vm strongly influences **noise margin** and inverter robustness.
<img width="647" height="310" alt="Screenshot 2025-09-10 at 7 08 09 PM" src="https://github.com/user-attachments/assets/19d38976-6151-410a-9adf-4ed4681eafe4" />

### Regions of operation:

Different regions of the curve correspond to the transistor operating regions:
PMOS Linear / NMOS OFF
PMOS Linear / NMOS Saturation
PMOS Saturation / NMOS Saturation — This is where Vm is located.
PMOS Saturation / NMOS Linear
PMOS OFF / NMOS Linear
<img width="716" height="368" alt="Screenshot 2025-09-10 at 7 09 05 PM" src="https://github.com/user-attachments/assets/8a4cda4e-9f4a-4e4e-938d-e2e61f5f46ae" />

## 📊 Effect of Wp/Wn Ratio on Inverter Performance

Varying the **Wp/Wn ratio** directly impacts:  
- Rise Delay  
- Fall Delay  
- Switching Threshold Voltage (Vm)
- 
<img width="718" height="370" alt="Screenshot 2025-09-11 at 6 08 40 PM" src="https://github.com/user-attachments/assets/edcc7005-93f0-4e88-9688-98321d4d8203" />

| Wp/Wn Ratio | Rise Delay | Fall Delay | Switching Threshold (Vm) |
|-------------|------------|------------|--------------------------|
| 1:1         | Longer     | Shorter    | ~0.98 V                  |
| ~2:1        | Balanced (~80 ps each) | Balanced (~80 ps each) | ~1.2 V |

---

### Key Observation
- When **Wp/Lp ≈ 2 × Wn/Ln**, the inverter achieves **balanced rise and fall delays** (~80 ps).  
- At this ratio, the **switching threshold Vm ≈ 1.2 V**, providing symmetric switching behavior.  

---

### Duty Cycle Consideration
- If rise and fall delays are **well-matched**, no duty cycle correction is required.  
- If delays are **mismatched** (due to PMOS/NMOS Ron differences), **duty cycle distortion** occurs.  
  - In such cases, **clock buffers** incorporate duty cycle correction circuits to restore a 50% duty cycle.  
