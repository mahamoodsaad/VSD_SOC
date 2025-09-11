# Static Behavior Evaluation: CMOS Inverter Robustness and Noise Margin

## Introduction to Noise Margin
Noise margin is the maximum noise voltage a CMOS circuit can tolerate without logic errors.

<img width="655" height="384" alt="Screenshot 2025-09-11 at 6 14 14 PM" src="https://github.com/user-attachments/assets/05c0c96b-71aa-4117-8971-2e6115d8586f" />

- **Ideal case:** An inverter with infinite slope switches abruptly at `Vdd/2`.
- **Real case:** Practical inverters have finite slope, creating a **transition region**.

This transition region defines where input levels are ambiguous, and robustness depends on how much margin exists before noise can flip logic states.

Noise margins are derived from the **Voltage Transfer Characteristic (VTC)** of the inverter.
<img width="655" height="381" alt="Screenshot 2025-09-11 at 6 16 48 PM" src="https://github.com/user-attachments/assets/af5a4f85-0a38-409a-8011-b4b2003c1118" />

**Key Thresholds**  
- `VIL`: Input Low Threshold Voltage (slope = −1)  
- `VIH`: Input High Threshold Voltage (slope = −1)  
- `VOH`: Valid Output High level  
- `VOL`: Valid Output Low level  

**Noise Margins**  
- `NMH = VOH − VIH` → tolerance for noise on logic **1**  
- `NML = VIL − VOL` → tolerance for noise on logic **0**

**Undefined Region**  
Between `VIL` and `VIH`, the input is undefined. Noise in this zone may cause unstable or invalid outputs.

> Designers aim to maximize `NMH` and `NML` for **robust and reliable circuits**.

---

## Sky130 Lab
Below image is the waveform of VTC curve to get the Noise Margin:

<img width="961" height="512" alt="Screenshot 2025-09-03 at 2 13 47 PM" src="https://github.com/user-attachments/assets/852876a3-aa84-41c2-ad1d-f7855ec0e3e4" />
