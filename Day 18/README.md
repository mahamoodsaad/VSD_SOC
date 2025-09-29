# Advanced Physical Design using OpenLane
#### ASIC Flow
Very Large-Scale Integration (VLSI) enables the fabrication of chips—tiny silicon devices containing millions or billions of transistors that power everything from smartphones and computers to cars and medical equipment.
ASICs (Application-Specific Integrated Circuits) are chips designed for a dedicated purpose, unlike general-purpose processors. A prime example is a smartphone processor, optimized for low power and high performance, often with specialized cores for graphics, AI, and imaging.

Designing an ASIC is a complex process supported by EDA (Electronic Design Automation) tools and usually takes months to complete. The standard flow includes:

**Design Entry** – Describe the logic in an HDL (Verilog/SystemVerilog) at the RTL or behavioral level.

**Functional Verification** – Validate correctness through simulation or formal methods, both at RTL and netlist level.

**Synthesis**– Convert the HDL into a gate-level netlist of standard logic cells.

**Physical Implementation (PnR)** – Transform the netlist into a chip layout with steps like floorplanning, placement, clock tree synthesis, and routing.

**Signoff** – Final checks for functionality, timing, power, and manufacturability before fabrication.

Additional steps such as scan chain insertion and test pattern generation are essential for testing the fabricated chip against defects.

#### OpenLANE ASIC design flow:
<img width="472" height="261" alt="Screenshot 2025-09-28 at 3 07 54 PM" src="https://github.com/user-attachments/assets/bd021ed2-c1b3-4da7-bf29-e25f8622fed4" />

OpenLane is an automated, open-source framework that transforms RTL designs into manufacturable layouts by integrating all major stages of the digital IC design process.

**Front-End:** RTL synthesis is performed using Yosys and ABC, followed by static timing analysis (STA) with OpenSTA. Design for Testability (DFT) is added to ensure fault coverage, and Yosys also supports Logic Equivalence Checking (LEC) for correctness.

**Physical Design:** The OpenROAD App handles floorplanning, placement, clock tree synthesis (CTS), optimization, and global routing. TritonRoute performs detailed routing, while custom scripts manage antenna diode insertion.

**Verification & Signoff:** DEF2SPEF enables RC extraction, with STA re-run to confirm timing. Magic and Netgen provide physical verification through Design Rule Checking (DRC) and Layout vs. Schematic (LVS) comparisons.

**Output:** The final deliverables are GDSII/LEF files, ready for fabrication, supported by the SkyWater PDK, which supplies process-specific libraries and technology data.

## OpenLANE Synthesis Flow Commands for picorv32a design:
* Clone the required files and project setup:
```shell
git clone https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd
```

* Build and install the OpenPDKs for the Sky130:

```bash
git clone https://github.com/RTimothyEdwards/open_pdks.git
cd open_pdks
./configure --enable-sky130-pdk
make
sudo make install
```

* Export the PDK_ROOT variable to point to sky130A PDK
```shell
export PDK_ROOT=/home/spatha/soc-design-and-planning-nasscom-vsd/Desktop/work/tools/openlane_working_dir/pdks
```
<img width="1304" height="747" alt="Screenshot 2025-09-24 at 12 59 22 PM" src="https://github.com/user-attachments/assets/ce01f397-88e9-4d1e-b4ef-882d645bce7a" />

* Change directory to the OpenLANE flow working directory
```shell
cd ~/soc-design-and-planning-nasscom-vsd/Desktop/work/tools/openlane_working_dir/openlane
```

* Launch the OpenLANE interactive shell
```shell
docker
```
```shell
./flow.tcl -interactive
```

<img width="1083" height="755" alt="Screenshot 2025-09-24 at 1 30 27 PM" src="https://github.com/user-attachments/assets/8a29075b-2bb3-41d9-bbcd-f340ad481a5a" />

* Prep the design
```shell
prep -design picorv32a
```
<img width="1348" height="801" alt="Screenshot 2025-09-24 at 4 12 48 PM" src="https://github.com/user-attachments/assets/f56ca14d-a98b-4ed5-831c-7dcf8b3c227e" />

* Run Synthesis

```shell
run_synthesis
```

<img width="1236" height="651" alt="Screenshot 2025-09-26 at 12 28 15 PM" src="https://github.com/user-attachments/assets/8df9b8d6-48a7-4cec-a530-13c4c835f980" />
<img width="1196" height="723" alt="Screenshot 2025-09-26 at 12 30 43 PM" src="https://github.com/user-attachments/assets/b97deea4-7778-4dbb-80e9-373a8fcb0d52" />
<img width="1158" height="708" alt="Screenshot 2025-09-26 at 12 32 49 PM" src="https://github.com/user-attachments/assets/d5c9786e-84d1-4993-891b-d5c430c736fd" />

