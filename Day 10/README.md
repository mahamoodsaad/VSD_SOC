# Timing Graphs using openSTA
### OpenSTA Installation
<img width="1184" height="866" alt="Screenshot 2025-08-12 at 1 45 48 PM" src="https://github.com/user-attachments/assets/1c34aa0f-73ab-4e66-9ecc-8790b0234031" />

### VSDBabySoC basic timing analysis

* **TCL Script to run  min/max timing checks on the SoC:**
<img width="487" height="428" alt="Screenshot 2025-08-21 at 11 07 03 AM" src="https://github.com/user-attachments/assets/a0d541f6-2057-4ea2-aa43-5bbcb0b7301a" />


* **After running the script, the following timing analysis is reported**
<img width="903" height="636" alt="Screenshot 2025-08-21 at 11 06 32 AM" src="https://github.com/user-attachments/assets/502c19bf-8d48-4873-b29d-1fe9aa6de080" />

### VSDBabySoC PVT Corner Analysis:
* **Script to perform STA across the PVT corners:**
```shell
 set list_of_lib_files(1) "sky130_fd_sc_hd__tt_025C_1v80.lib"
 set list_of_lib_files(2) "sky130_fd_sc_hd__ff_100C_1v65.lib"
 set list_of_lib_files(3) "sky130_fd_sc_hd__ff_100C_1v95.lib"
 set list_of_lib_files(4) "sky130_fd_sc_hd__ff_n40C_1v56.lib"
 set list_of_lib_files(5) "sky130_fd_sc_hd__ff_n40C_1v65.lib"
 set list_of_lib_files(6) "sky130_fd_sc_hd__ff_n40C_1v76.lib"
 set list_of_lib_files(7) "sky130_fd_sc_hd__ss_100C_1v40.lib"
 set list_of_lib_files(8) "sky130_fd_sc_hd__ss_100C_1v60.lib"
 set list_of_lib_files(9) "sky130_fd_sc_hd__ss_n40C_1v28.lib"
 set list_of_lib_files(10) "sky130_fd_sc_hd__ss_n40C_1v35.lib"
 set list_of_lib_files(11) "sky130_fd_sc_hd__ss_n40C_1v40.lib"
 set list_of_lib_files(12) "sky130_fd_sc_hd__ss_n40C_1v44.lib"
 set list_of_lib_files(13) "sky130_fd_sc_hd__ss_n40C_1v76.lib"

 read_liberty /home/saad/OpenSTA/examples/timing_libs/avsdpll.lib
 read_liberty /home/saad/OpenSTA/examples/timing_libs/avsddac.lib

 for {set i 1} {$i <= [array size list_of_lib_files]} {incr i} {
 read_liberty /home/saad/OpenSTA/examples/timing_libs/$list_of_lib_files($i)
 read_verilog /home/saad/OpenSTA/examples/BabySoC/vsdbabysoc.synth.v
 link_design vsdbabysoc
 current_design
 read_sdc /home/saad/OpenSTA/examples/BabySoC/vsdbabysoc_synthesis.sdc
 check_setup -verbose
 report_checks -path_delay min_max -fields {nets cap slew input_pins fanout} -digits {4} > /home/saad/OpenSTA/examples/BabySoC/STA_OUTPUT/min_max_$list_of_lib_files($i).txt

 exec echo "$list_of_lib_files($i)" >> /home/saad/OpenSTA/examples/BabySoC/STA_OUTPUT/sta_worst_max_slack.txt
 report_worst_slack -max -digits {4} >> /home/saad/OpenSTA/examples/BabySoC/STA_OUTPUT/sta_worst_max_slack.txt

 exec echo "$list_of_lib_files($i)" >> /home/saad/OpenSTA/examples/BabySoC/STA_OUTPUT/sta_worst_min_slack.txt
 report_worst_slack -min -digits {4} >> /home/saad/OpenSTA/examples/BabySoC/STA_OUTPUT/sta_worst_min_slack.txt

 exec echo "$list_of_lib_files($i)" >> /home/saad/OpenSTA/examples/BabySoC/STA_OUTPUT/sta_tns.txt
 report_tns -digits {4} >> /home/saad/OpenSTA/examples/BabySoC/STA_OUTPUT/sta_tns.txt

 exec echo "$list_of_lib_files($i)" >> /home/saad/OpenSTA/examples/BabySoC/STA_OUTPUT/sta_wns.txt
 report_wns -digits {4} >> /home/saad/OpenSTA/examples/BabySoC/STA_OUTPUT/sta_wns.txt
 }
```
<img width="965" height="358" alt="Screenshot 2025-08-22 at 2 41 05 PM" src="https://github.com/user-attachments/assets/32d175a1-6192-438f-897f-6e8fa1a20fe7" />

* **After executing the above script, The following timing reports were generated:**
<img width="965" height="247" alt="Screenshot 2025-08-22 at 2 41 20 PM" src="https://github.com/user-attachments/assets/9f163d33-af13-429b-8156-15db0eaa5b64" />

* **Timing Plots Across PVT Corners:**
<img width="1150" height="540" alt="Screenshot 2025-08-22 at 3 22 53 PM" src="https://github.com/user-attachments/assets/bc13561c-c1af-4d91-8588-e8ec34e37216" />
<img width="1003" height="389" alt="Screenshot 2025-08-22 at 3 23 11 PM" src="https://github.com/user-attachments/assets/953f8407-fe60-4c83-873d-ff11c3ea90ea" />
<img width="1001" height="354" alt="Screenshot 2025-08-22 at 3 23 23 PM" src="https://github.com/user-attachments/assets/059e127c-879b-44f5-b52e-29df2635f621" />
<img width="1009" height="461" alt="Screenshot 2025-08-22 at 3 23 32 PM" src="https://github.com/user-attachments/assets/520a72d2-d7d5-4860-8325-464a381019ef" />
