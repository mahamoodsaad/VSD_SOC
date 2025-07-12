## VSDBabySoC Post-Synthesis Simulation

**Reading the main vsdbabysoc.v RTL file in yosys**

<img width="752" height="506" alt="Screenshot 2025-07-02 at 11 49 31 AM" src="https://github.com/user-attachments/assets/a66507ac-34b6-4fbd-898d-465930ea70b5" />

**Reading the rvmyth.v file**

<img width="750" height="511" alt="Screenshot 2025-07-02 at 11 51 55 AM" src="https://github.com/user-attachments/assets/23f1eef3-05c7-42b7-a963-6b3422a79b86" />

**Reading the clk_gate.v file**

<img width="750" height="508" alt="Screenshot 2025-07-02 at 11 53 20 AM" src="https://github.com/user-attachments/assets/82bf0a35-36de-408e-ac4b-dc75a59af38d" />

**Loading the Liberty Files for Synthesis**

<img width="751" height="506" alt="Screenshot 2025-07-02 at 11 55 30 AM" src="https://github.com/user-attachments/assets/98f71874-9c28-452a-9ae6-3f553f3f17da" />

**Running Synthesis**

<img width="751" height="499" alt="Screenshot 2025-07-02 at 11 58 57 AM" src="https://github.com/user-attachments/assets/9f64298d-3daa-4612-b094-ddcd6a7480b3" />

**Mapping D FF with standard cells**

<img width="751" height="500" alt="Screenshot 2025-07-02 at 11 59 56 AM" src="https://github.com/user-attachments/assets/cfedd9c7-6a2f-45fd-b2c7-f2e125f43df6" />

**Check Statistics in yosys**

```shell
=== vsdbabysoc ===

   Number of wires:               4736
   Number of wire bits:           6210
   Number of public wires:        4736
   Number of public wire bits:    6210
   Number of ports:                  7
   Number of port bits:              7
   Number of memories:               0
   Number of memory bits:            0
   Number of processes:              0
   Number of cells:               5920
     $scopeinfo                      8
     avsddac                         1
     avsdpll                         1
     sky130_fd_sc_hd__a2111oi_0     10
     sky130_fd_sc_hd__a211o_2        1
     sky130_fd_sc_hd__a211oi_1      26
     sky130_fd_sc_hd__a21boi_0       4
     sky130_fd_sc_hd__a21o_2         1
     sky130_fd_sc_hd__a21oi_1      672
     sky130_fd_sc_hd__a221o_2        1
     sky130_fd_sc_hd__a221oi_1     163
     sky130_fd_sc_hd__a22o_2         4
     sky130_fd_sc_hd__a22oi_1      123
     sky130_fd_sc_hd__a311oi_1       4
     sky130_fd_sc_hd__a31o_2         1
     sky130_fd_sc_hd__a31oi_1      344
     sky130_fd_sc_hd__a32oi_1        2
     sky130_fd_sc_hd__a41oi_1       26
     sky130_fd_sc_hd__and2_2        12
     sky130_fd_sc_hd__and3_2         1
     sky130_fd_sc_hd__clkinv_1     597
     sky130_fd_sc_hd__dfxtp_1     1144
     sky130_fd_sc_hd__lpflow_inputiso0p_1      1
     sky130_fd_sc_hd__mux2i_1       12
     sky130_fd_sc_hd__nand2_1      839
     sky130_fd_sc_hd__nand3_1      249
     sky130_fd_sc_hd__nand3b_1       1
     sky130_fd_sc_hd__nand4_1       41
     sky130_fd_sc_hd__nor2_1       403
     sky130_fd_sc_hd__nor3_1        35
     sky130_fd_sc_hd__nor4_1         2
     sky130_fd_sc_hd__o2111ai_1     20
     sky130_fd_sc_hd__o211a_1        1
     sky130_fd_sc_hd__o211ai_1      49
     sky130_fd_sc_hd__o21a_1         6
     sky130_fd_sc_hd__o21ai_0      866
     sky130_fd_sc_hd__o21ba_2        1
     sky130_fd_sc_hd__o21bai_1      18
     sky130_fd_sc_hd__o221a_2        1
     sky130_fd_sc_hd__o221ai_1       7
     sky130_fd_sc_hd__o22ai_1      155
     sky130_fd_sc_hd__o2bb2ai_1      1
     sky130_fd_sc_hd__o311ai_0       2
     sky130_fd_sc_hd__o31ai_1        3
     sky130_fd_sc_hd__o32ai_1        1
     sky130_fd_sc_hd__o41ai_1        1
     sky130_fd_sc_hd__or2_2         12
     sky130_fd_sc_hd__or3_2          1
     sky130_fd_sc_hd__or4_2          1
     sky130_fd_sc_hd__xnor2_1       13
     sky130_fd_sc_hd__xor2_1        32
```

**Run simulation and view waveforms in GTKWave:**

<img width="964" height="278" alt="Screenshot 2025-07-11 at 9 23 04 PM" src="https://github.com/user-attachments/assets/03a14ede-9ea3-475b-92e0-4b034e9da6d7" />

*Post sysnthesis simulation matches the pre synthesis simulation.
