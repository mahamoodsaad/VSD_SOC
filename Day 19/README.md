# Library Cell Design using Magic Layout and ngspice Characterization
## Cell Design and Characterization Flows

In integrated circuit (IC) design, a **library** is a collection of standard cells defined by parameters such as size, functionality, threshold voltage, and other physical/electrical properties. These libraries are essential to the ASIC flow, supporting synthesis, placement, and timing analysis.
<img width="662" height="308" alt="Screenshot 2025-09-29 at 6 11 49 PM" src="https://github.com/user-attachments/assets/961ccad0-92f0-423e-ba63-c84746058db5" />
<img width="661" height="340" alt="Screenshot 2025-09-29 at 6 12 02 PM" src="https://github.com/user-attachments/assets/c2078f85-a5fc-47f8-98f7-5a70ddfe3faa" />

### Inputs

* **PDKs (Process Design Kits)**
* **DRC & LVS rules**
* **SPICE models**
* **Library & user-defined specifications** (e.g., cell height, supply voltage, number of metal layers, pin locations, drawn gate length)

---

### Standard Cell Design Flow

1. **Circuit Design** – Develop the transistor-level schematic.
2. **Layout Design** – Create the physical layout adhering to DRC rules.
3. **Parasitic Extraction** – Extract parasitics from the layout for accurate modeling.
4. **Characterization** – Analyze performance (timing, power, noise).

**Outputs:**

* **CDL** – Circuit Description Language (netlist from schematic)
* **LEF** – Library Exchange Format (abstract layout info for PnR)
* **GDSII** – Final layout database for fabrication
* **.cir** – Extracted SPICE netlist
* **.lib** – Characterized library files with timing, power, and noise data
---

### Standard Cell Characterization Flow

A typical characterization process involves:

1. Reading SPICE models and technology files
2. Loading the extracted SPICE netlist
3. Recognizing functional behavior of the cell
4. Identifying subcircuits
5. Applying power sources
6. Stimulating the cell with test vectors
7. Setting output load capacitances
8. Running simulations with proper commands
<img width="602" height="330" alt="Screenshot 2025-09-29 at 6 12 55 PM" src="https://github.com/user-attachments/assets/940d89ef-eb93-4fbd-a9c7-dbfe339f6368" />
<img width="592" height="273" alt="Screenshot 2025-09-29 at 6 13 14 PM" src="https://github.com/user-attachments/assets/a5228f8c-2f22-430f-9f83-aa57a32c3c63" />

#### Propogation Delay
The time difference between the input signal reaching 50% of its final value and the output reaching 50% of its final value.
<img width="606" height="317" alt="Screenshot 2025-09-29 at 6 14 29 PM" src="https://github.com/user-attachments/assets/b3b5c778-699d-4cf5-8c8f-686ead0b7552" />

#### Transition Time
The time it takes for a signal to transition between logic states, typically measured between 10–90% or 20–80% of the voltage levels.
<img width="599" height="303" alt="Screenshot 2025-09-29 at 6 15 05 PM" src="https://github.com/user-attachments/assets/05bfc603-a06a-4b5f-9465-465956b799af" />

# Designing Library Cell using magic layout and ngspice charcterization:

Clone the required mag files and spicemodels of inverter,pmos and nmos sky130.
```shell
cd ~/soc-design-and-planning-nasscom-vsd/Desktop/work/tools/openlane_working_dir/openlane/

git clone https://github.com/nickson-jose/vsdstdcelldesign.git
```
<img width="1206" height="530" alt="Screenshot 2025-09-28 at 4 45 44 PM" src="https://github.com/user-attachments/assets/96cd7838-24f5-40ce-87dd-8563f9557c66" />

```shell
magic -T sky130A.tech sky130_inv.mag &
```
<img width="1249" height="776" alt="Screenshot 2025-09-28 at 4 46 05 PM" src="https://github.com/user-attachments/assets/893dbdc6-f311-4c4e-9b7c-5b8aae7f8185" />

## 16-Mask CMOS Fabrication Process – Summary

The **16-mask CMOS process** is a widely used semiconductor manufacturing method for building ICs. It involves repeated cycles of **photolithography, ion implantation, deposition, and etching**, each step defined by a mask that shapes a particular device feature.

### Key Steps

1. **Substrate Preparation**

   * Start with a high-quality silicon wafer, cleaned to eliminate impurities.

2. **Well Formation**

   * **N-Well**: Phosphorus/arsenic implantation for PMOS bodies.
   * **P-Well**: Boron implantation for NMOS bodies.
   * Twin-well processes allow independent optimization of NMOS and PMOS.

3. **Gate Formation**

   * Grow/deposit thin **gate oxide** for channel insulation.
   * Deposit and pattern **polysilicon** to form gate electrodes.

4. **Lightly Doped Drain (LDD) & Source/Drain Formation**

   * LDD regions implanted to reduce hot-carrier effects.
   * Heavier dopants implanted for **source/drain** regions of NMOS/PMOS.
   * Proper alignment ensures correct channel formation.

5. **Contact & Interconnect Formation**

   * **Etching** opens contact holes (vias) to source, drain, and gate.
   * Deposit **metal layers** (Al or Cu) for interconnects.
   * Pattern and etch metal for routing.
   * Additional higher-level metal layers may be added for complex designs.

6. **Final Steps**

   * Deposit a **passivation layer** (SiO₂ or Si₃N₄) to protect the circuit.
   * Perform **wafer-level testing**, dice the wafer, and package functional chips.

---
## Spice extraction for Inverter in Magic

```bash
pwd

#extraction command to extract .ext format
extract all

# Before converting ext to spice this command enable the parasitic extraction also
ext2spice cthresh 0 rthresh 0

#convert ext to spice
ext2spice
```
<img width="1275" height="801" alt="Screenshot 2025-09-28 at 4 49 17 PM" src="https://github.com/user-attachments/assets/f44ea495-1104-4222-a701-01125e1c9415" />

* spice file contents:
<img width="691" height="504" alt="Screenshot 2025-09-28 at 4 50 50 PM" src="https://github.com/user-attachments/assets/3a535c66-33db-45e8-a2d3-7289d6c4b1b7" />

* Edited spice file ready for ngspice simulation:
<img width="1210" height="773" alt="Screenshot 2025-09-28 at 6 51 08 PM" src="https://github.com/user-attachments/assets/4326ebec-db3c-4284-bd08-e53ee94384b4" />

**Post-layout ngspice simulations**
```bash
# Command to directly load spice file for simulation to ngspice
ngspice sky130_inv.spice

# Now that we have entered ngspice with the simulation spice file loaded we just have to load the plot
plot y vs time a
```
<img width="1210" height="767" alt="Screenshot 2025-09-28 at 6 54 22 PM" src="https://github.com/user-attachments/assets/242e6211-3df9-4131-9608-98cd51ca6c9a" />

## Rise Transition Time Calculation:
**Rise Time = T<sub>80%</sub> − T<sub>20%</sub>**
Reference values for VDD = 3.3V
20% of output voltage = 0.20 x 3.3V = 660 mV
80% of output voltage = 0.80 x 3.3V = 2.64 V
<img width="1214" height="768" alt="Screenshot 2025-09-28 at 7 05 55 PM" src="https://github.com/user-attachments/assets/b68069f0-7bab-425f-a213-d0e0f8fa6c6c" />
<img width="1215" height="775" alt="Screenshot 2025-09-28 at 7 07 01 PM" src="https://github.com/user-attachments/assets/61dd0c9c-ec6b-4665-a991-250b7bb00a85" />

## Fall Transition Time Calculation:
**Fall Time = T<sub>20%</sub> − T<sub>80%</sub>**
Reference values (for VDD = 3.3 V)
20% of output voltage: 0.20 x 3.3V = 660 mV
80% of output voltage: 0.80 x 3.3V = 2.64 V
<img width="1209" height="766" alt="Screenshot 2025-09-28 at 7 07 54 PM" src="https://github.com/user-attachments/assets/b5fd7981-ffc1-42b7-9439-c5797d66ee3f" />
<img width="1203" height="764" alt="Screenshot 2025-09-28 at 7 09 46 PM" src="https://github.com/user-attachments/assets/91e8cc6b-e0b1-4335-8112-822f295dc7c8" />

### Find problem in the DRC section of the old magic tech file for the skywater process and fix them

Link to Sky130 Periphery rules: https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html

```shell
wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz

tar xfz drc_tests.tgz

cd drc_tests

ls -al
```
<img width="1216" height="760" alt="Screenshot 2025-09-28 at 7 12 40 PM" src="https://github.com/user-attachments/assets/0eded0d6-a81b-4ee5-8cfa-7b40ef62caa9" />

```shell
gvim .magicrc
```

<img width="1219" height="776" alt="Screenshot 2025-09-28 at 7 13 08 PM" src="https://github.com/user-attachments/assets/03e85582-2e47-4151-9e9b-6da3687d24cc" />

```shell
magic -d XR &
```

* load the poly file by using the cmd 'load poly' on tkcon window:
<img width="1200" height="766" alt="Screenshot 2025-09-28 at 7 17 46 PM" src="https://github.com/user-attachments/assets/6f1ae025-c097-4930-9805-476eb889eca7" />

* Shows no drc violation:
<img width="1200" height="769" alt="Screenshot 2025-09-28 at 7 18 24 PM" src="https://github.com/user-attachments/assets/0579f501-7e62-48c0-8969-834406852b11" />

* New commands inserted in `sky130A.tech` file to update drc:

<img width="1211" height="764" alt="Screenshot 2025-09-28 at 7 25 48 PM" src="https://github.com/user-attachments/assets/c14efaa2-539c-4e3f-83d2-30247f6a930b" />
<img width="1218" height="771" alt="Screenshot 2025-09-28 at 7 28 23 PM" src="https://github.com/user-attachments/assets/884e8060-7af9-4514-af50-a8b95deed396" />

* Loading updated tech file

```shell
tech load sky130A.tech
drc check
drc why
```
<img width="1211" height="767" alt="Screenshot 2025-09-28 at 7 32 42 PM" src="https://github.com/user-attachments/assets/6d645103-7c5b-4b08-8faa-c1e2a14dc9d1" />

---
To properly enforce the **poly.9 design rule**—which requires poly resistors to maintain a minimum spacing of **0.48 µm (480 nm)** from both diffusion regions (*alldiff*) and non-resistor poly (*allpolynonres*)—additional DRC checks were integrated into the `sky130A.tech` file.

In the original setup, violations went unreported even when spacing was below the required 0.48 µm, suggesting that the rule was either absent or incorrectly defined. To fix this, two new spacing rules were introduced:

* one between **npres** and **alldiff**
* another between **npres** and **allpolynonres**

Both rules were defined with the **touching_illegal** condition and assigned rule IDs corresponding to **poly.9**.

After modifying the `.tech` file, the following steps were performed in Magic’s TkCon window:

1. `tech load sky130A.tech` – reload the updated technology file.
2. `drc check` – rerun the design rule checker.
3. `drc why` – inspect flagged violations.

As confirmed in the final Magic screenshot, the updated DRC now correctly reports errors when spacing falls below 0.48 µm, validating that the **poly.9 rule is properly implemented and functional**.
