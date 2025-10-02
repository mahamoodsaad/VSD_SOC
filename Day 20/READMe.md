# Pre-layout timing analysis and importance of good clock tree

* Fix up small DRC errors and verify the design is ready to be inserted into our flow
Conditions to Verify Before Finalizing a Custom Standard Cell Layout

1. **Port Alignment** – All input and output pins must be placed at the intersections of horizontal and vertical routing tracks.
2. **Cell Width Requirement** – The cell width must be an **odd multiple** of the horizontal track pitch.
3. **Cell Height Requirement** – The cell height must be an **even multiple** of the vertical track pitch.

open the custom inverter layout:

```shell
# Change directory to vsdstdcelldesign
cd Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign
# Command to open custom inverter layout in magic
magic -T sky130A.tech sky130_inv.mag &
```

<img width="1198" height="764" alt="Screenshot 2025-09-28 at 10 30 27 PM" src="https://github.com/user-attachments/assets/92a0b217-f6d4-4125-bd3d-04ffe770efd7" />

set grid as tracks of locali layer:

```shell
help grid
grid 0.46um 0.34um 0.23um 0.17um
```

<img width="1215" height="763" alt="Screenshot 2025-09-28 at 11 33 57 PM" src="https://github.com/user-attachments/assets/7e52d9dc-c282-47ce-9841-5dfe11ead7bb" />

**Condition 1 verified:**
<img width="1212" height="764" alt="Screenshot 2025-09-28 at 11 35 01 PM" src="https://github.com/user-attachments/assets/0dd4b336-7638-4de8-ada3-bc91f961d9f8" />

**Condition 2 verified:**

Horizontal track pitch = 0.46 µm
Width of standard cell = 1.38 µm 
<img width="1214" height="764" alt="Screenshot 2025-09-28 at 11 39 12 PM" src="https://github.com/user-attachments/assets/ca9dbd7c-56d2-4449-b6f2-3291de2b5054" />

**Condition 3 verified:**

Vertical track pitch = 0.34 µm
Height of standard cell = 2.72 µm
<img width="1215" height="767" alt="Screenshot 2025-09-28 at 11 40 36 PM" src="https://github.com/user-attachments/assets/d32385e1-eb5d-41e2-8071-49d9a028fee5" />

* Save the finalized layout with custom name and open it

```shell
save sky130_vsdinv.mag
```
open the newly saved layout:
```shell
magic -T sky130A.tech sky130_vsdinv.mag &
```
<img width="1213" height="766" alt="Screenshot 2025-09-28 at 11 42 02 PM" src="https://github.com/user-attachments/assets/583eac77-89d2-459f-b944-027f3fc76b7c" />

* Generate Lef from the Layout
```shell
lef write
```
<img width="1213" height="768" alt="Screenshot 2025-09-28 at 11 43 09 PM" src="https://github.com/user-attachments/assets/c0dd7504-cbec-45fa-8f45-5c55875bf38b" />

Newly created lef file:

```shell
gvim sky130_vsdinv.lef
```
<img width="1214" height="767" alt="Screenshot 2025-09-28 at 11 43 52 PM" src="https://github.com/user-attachments/assets/3614ff34-d870-4def-8c34-225695a53371" />

* Copy the newly generated lef and associated required lib files to 'picorv32a' design 'src' directory

```shell
cp sky130_vsdinv.lef ~/soc-design-and-planning-nasscom-vsd/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/
cp libs/sky130_fd_sc_hd__* ~/soc-design-and-planning-nasscom-vsd/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/
ls ~/soc-design-and-planning-nasscom-vsd/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/
```

* Edit config file to change lib file and add the new extra lef into the openlane flow

```shell
set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib"
set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib"
set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]
```
<img width="1214" height="765" alt="Screenshot 2025-09-28 at 11 54 24 PM" src="https://github.com/user-attachments/assets/3f8947bc-7632-490d-ac7b-78e9580ddf63" />

* Run openlane flow synthesis with newly inserted custom inverter cell

```shell
cd ~/soc-design-and-planning-nasscom-vsd/Desktop/work/tools/openlane_working_dir/openlane

export PDK_ROOT=/home/saad/soc-design-and-planning-nasscom-vsd/Desktop/work/tools/openlane_working_dir/pdks

alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'

docker
```

```shell
./flow.tcl -interactive
package require openlane 0.9

prep -design picorv32a

# Add commands to lef file
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

run_synthesis
```
<img width="1055" height="348" alt="Screenshot 2025-09-30 at 6 31 09 PM" src="https://github.com/user-attachments/assets/b60e39ce-be97-4535-89f4-c6680f70c98d" />

* Remove/reduce the newly introduced violations with the introduction of custom inverter cell by modifying design parameters
<img width="1137" height="480" alt="Screenshot 2025-09-30 at 6 32 16 PM" src="https://github.com/user-attachments/assets/f96ce387-b670-447c-b5b6-a281dfc63d18" />

* Change parameters to improve timing and run synthesis:

```shell
prep -design picorv32a -tag new -overwrite

# include newly added lef to openlane flow merged.lef
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]

add_lefs -src $lefs

echo $::env(SYNTH_STRATEGY)

set ::env(SYNTH_STRATEGY) "DELAY 3"

echo $::env(SYNTH_BUFFERING)

echo $::env(SYNTH_SIZING)

set ::env(SYNTH_SIZING) 1

echo $::env(SYNTH_DRIVING_CELL)

run_synthesis
```
<img width="937" height="345" alt="Screenshot 2025-09-30 at 6 33 14 PM" src="https://github.com/user-attachments/assets/e3b943a8-f1ba-4689-8c58-12c6c52b02d5" />
<img width="873" height="326" alt="Screenshot 2025-09-30 at 6 33 26 PM" src="https://github.com/user-attachments/assets/9716f166-ba58-4deb-962d-73bfee7e7e36" />
(The area has increased and Slack has become 0)

* Run floorplan and placement
```shell
run_floorplan
```
```shell
init_floorplan
place_io
tap_decap_or
```
<img width="1515" height="506" alt="Screenshot 2025-10-01 at 11 58 03 AM" src="https://github.com/user-attachments/assets/01cebbff-c8f8-424c-9c7b-e83c9d7fab66" />

```shell
run_placement
```
<img width="1011" height="366" alt="Screenshot 2025-10-01 at 12 01 11 PM" src="https://github.com/user-attachments/assets/95649446-776d-4eb9-81fb-464fb21a8d9b" />

* Load placement def in magic in another terminal:

```shell
cd ~/soc-design-and-planning-nasscom-vsd/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/new/results/placement/

magic -T ~/soc-design-and-planning-nasscom-vsd/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```
<img width="1215" height="763" alt="Screenshot 2025-10-01 at 12 06 05 PM" src="https://github.com/user-attachments/assets/029e1b0a-56e9-4585-b84c-97968ed35395" />

```shell
expand
```
<img width="817" height="447" alt="Screenshot 2025-10-01 at 1 42 00 PM" src="https://github.com/user-attachments/assets/153ea493-e160-4b03-ab75-ed45d98a56ad" />

* Post-Synthesis timing analysis with OpenSTA tool

```shell
cd ~/soc-design-and-planning-nasscom-vsd/Desktop/work/tools/openlane_working_dir/openlane

export PDK_ROOT=/home/saad/soc-design-and-planning-nasscom-vsd/Desktop/work/tools/openlane_working_dir/pdks

alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
docker
```
```shell
./flow.tcl -interactive
package require openlane 0.9

prep -design picorv32a

# Include newly added lef to openlane flow
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
set ::env(SYNTH_SIZING) 1

run_synthesis
```
<img width="1005" height="421" alt="Screenshot 2025-10-01 at 1 42 34 PM" src="https://github.com/user-attachments/assets/b719b411-3ce5-4b0e-a3b4-e8bb44075ffd" />
<img width="1202" height="759" alt="Screenshot 2025-10-01 at 4 01 17 PM" src="https://github.com/user-attachments/assets/c56d2454-cf5a-4b98-ae73-301aaa93523c" />
<img width="1204" height="766" alt="Screenshot 2025-10-01 at 1 46 18 PM" src="https://github.com/user-attachments/assets/509dfe46-ba70-4fdb-bebd-560d9eded05e" />

* Run STA in another terminal:

```shell
cd Desktop/work/tools/openlane_working_dir/openlane
~/OpenSTA/build/sta pre_sta.conf
```
<img width="1014" height="230" alt="Screenshot 2025-10-01 at 1 58 10 PM" src="https://github.com/user-attachments/assets/98cb6a96-d7f9-4bc7-95bb-1edd5c47af10" />
<img width="1006" height="428" alt="Screenshot 2025-10-01 at 1 58 38 PM" src="https://github.com/user-attachments/assets/fd95523c-3ec0-4f19-a84a-eb8a0d69b904" />

* Timing ECO Fixes to Remove Violations
OR gate with drive strength 2 `sky130_fd_sc_hd__or3_2` is driving 4 fanout loads.
```bash
OR gate: sky130_fd_sc_hd__or3_2
Fanouts: 4
Net: _11873_
```
Replacing the OR3 gate of drive strength 2 with a drive strength 4

```bash
report_net -connections _11873_

help replace_cell

replace_cell _14770_ sky130_fd_sc_hd__or3_4

report_checks -fields {net cap slew input_pins fanout} -digits 4
```
<img width="1008" height="400" alt="Screenshot 2025-10-01 at 1 59 53 PM" src="https://github.com/user-attachments/assets/8f1bfd81-2b60-4015-b5e0-b007ca3c0d94" />

Now Replace the weak OR gate with a higher drive-strength version (sky130_fd_sc_hd__or4_4)

```bash
report_net -connections _11844_

replace_cell _14741_ sky130_fd_sc_hd__or4_4

report_checks -fields {net cap slew input_pins fanout} -digits 4
```
The OR gate of drive strength 2 driving OA gate has more delay
<img width="997" height="445" alt="Screenshot 2025-10-01 at 2 00 43 PM" src="https://github.com/user-attachments/assets/e47f3f95-b32f-4101-b6fe-ec27df9ba7ec" />

Analyze and optimize timing by replacing with OR gate of drive strength 4:
```bash
report_net -connections _11869_

replace_cell _14766_ sky130_fd_sc_hd__or4_4

report_checks -fields {net cap slew input_pins fanout} -digits 4
```
<img width="1000" height="296" alt="Screenshot 2025-10-01 at 2 01 01 PM" src="https://github.com/user-attachments/assets/ac219a0e-0468-4276-974c-e3a65d0060d9" />
<img width="1009" height="388" alt="Screenshot 2025-10-01 at 2 01 13 PM" src="https://github.com/user-attachments/assets/4773b249-e573-4c97-be4b-6e4d3f7bf2e3" />

* Replace the old netlist with the new netlist generated after timing ECO fix and implement the floorplan, placement and cts

```bash
cd  /home/saad/soc-design-and-planning-nasscom-vsd/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/29-09_16-36/results/synthesis

cp picorv32a.synthesis.v picorv32a.synthesis_old.v

ls
```
<img width="817" height="177" alt="Screenshot 2025-10-01 at 8 08 39 PM" src="https://github.com/user-attachments/assets/b40ab056-0965-4828-9e9d-10a85a7210a2" />

```bash
help write_verilog

write_verilog /home/saad/soc-design-and-planning-nasscom-vsd/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/29-09_16-36/results/synthesis/picorv32a.synthesis.v

exit
```

Verify the netlist is overwritten by checking that instance _14506_ is replaced with sky130_fd_sc_hd__or4_4
<img width="1013" height="582" alt="Screenshot 2025-10-01 at 8 00 52 PM" src="https://github.com/user-attachments/assets/eb8d10bd-860d-4910-ac8f-cf923d26fe4f" />

Adding the following two lines to config.tcl
```bash
set ::env(CTS_SQR_CAP) 0.024   ;# Square capacitance in pF/µm²
set ::env(CTS_SQR_RES) 0.075   ;# Square resistance in kΩ/µm²
```
```bash
prep -design picorv32a -tag 25-07_23-12 -overwrite
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

set ::env(SYNTH_STRATEGY) "DELAY 3"
set ::env(SYNTH_SIZING) 1

run_synthesis

init_floorplan
place_io
tap_decap_or

run_placement

unset ::env(LIB_CTS)

run_cts
```
<img width="1024" height="406" alt="Screenshot 2025-10-01 at 2 09 38 PM" src="https://github.com/user-attachments/assets/431a11a2-fe1c-4059-9a57-3af9573ba349" />
