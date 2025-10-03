## OpenROAD-Flow-Scripts: Physical Design, Post-Route SPEF and Verilog Netlist Generation for VSDBabySoC

### config.mk File:
```
export DESIGN_NICKNAME = vsdbabysoc
export DESIGN_NAME = vsdbabysoc
export PLATFORM    = sky130hd
export DESIGN_HOME = /home/saad/OpenROAD-flow-scripts/flow/designs
# export VERILOG_FILES_BLACKBOX = $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/IPs/*.v
# export VERILOG_FILES = $(sort $(wildcard $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/*.v))
# Explicitly list the Verilog files for synthesis
export VERILOG_FILES = $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/vsdbabysoc.v \
                       $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/rvmyth.v \
                       $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/clk_gate.v

export SDC_FILE      = $(DESIGN_HOME)/$(PLATFORM)/$(DESIGN_NICKNAME)/vsdbabysoc_synthesis.sdc

export vsdbabysoc_DIR = $(DESIGN_HOME)/$(PLATFORM)/$(DESIGN_NICKNAME)

export VERILOG_INCLUDE_DIRS = $(wildcard $(vsdbabysoc_DIR)/include/)

export ADDITIONAL_GDS = $(wildcard $(vsdbabysoc_DIR)/gds/*.gds)
export ADDITIONAL_LEFS = $(wildcard $(vsdbabysoc_DIR)/lef/*.lef)
#export ADDITIONAL_LIBS = $(wildcard $(vsdbabysoc_DIR)/lib/*.lib)
# export PDN_TCL = $(DESIGN_HOME)/$(PLATFORM)/$(DESIGN_NICKNAME)/pdn.tcl


# Clock Configuration
#export CLOCK_PERIOD = 11.00
export CLOCK_PORT = CLK
export CLOCK_NET  = $(CLOCK_PORT)


# Pin Order and Macro Placement Configurations
export FP_PIN_ORDER_CFG = $(vsdbabysoc_DIR)/pin_order.cfg
export MACRO_PLACEMENT_CFG = $(vsdbabysoc_DIR)/macro.cfg

# Floorplanning Configuration
export DIE_AREA   = 0 0 1600 1600
export CORE_AREA  = 20 20 1590 1590

# Placement Configuration
export PLACE_PINS_ARGS = -exclude left:0-600 -exclude left:1000-1600 -exclude right:* -exclude top:* -exclude bottom:*

# Tuning for Timing and Buffers
export TNS_END_PERCENT     = 100
export REMOVE_ABC_BUFFERS  = 1

# CTS tuning
export CTS_BUF_DISTANCE = 600
export SKIP_GATE_CLONING = 1

# Magic Tool Configuration
export MAGIC_ZEROIZE_ORIGIN = 0
export MAGIC_EXT_USE_GDS    = 1

# export CORE_UTILIZATION=0.1
```

### 1. Run Synthesis
```shell
cd OpenROAD-flow-scripts/flow
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk clean_all
```
```shell
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk synth
```
<img width="1209" height="766" alt="Screenshot 2025-10-01 at 9 03 11 PM" src="https://github.com/user-attachments/assets/5f5d9fe5-2867-449d-a7c8-39e97dd0b0bc" />

* Synthesis netlist
```shell
gvim results/sky130hd/vsdbabysoc/base/1_2_yosys.v
```
<img width="1208" height="772" alt="Screenshot 2025-10-01 at 9 03 35 PM" src="https://github.com/user-attachments/assets/e9403cad-7888-45f4-b0e2-482ce6c4db9b" />

* Synthesis Stats
```shell
gvim reports/sky130hd/vsdbabysoc/base/synth_stat.txt
```
<img width="1209" height="765" alt="Screenshot 2025-10-01 at 9 04 30 PM" src="https://github.com/user-attachments/assets/b3b470c2-4b00-46e2-abc5-06ab28755b1d" />


### 2. Run FLoorplan
```shell
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk floorplan
```
<img width="1207" height="772" alt="Screenshot 2025-10-01 at 9 06 38 PM" src="https://github.com/user-attachments/assets/f41ae111-4808-4c56-8ffa-840c5cad42c7" />

<img width="1206" height="770" alt="Screenshot 2025-10-01 at 9 06 03 PM" src="https://github.com/user-attachments/assets/4be252ee-7566-479c-adb7-d09eca7df4c1" />

```shell
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_floorplan
```
<img width="1209" height="773" alt="Screenshot 2025-10-01 at 9 41 20 PM" src="https://github.com/user-attachments/assets/f9856507-51c5-4019-a163-19ba2a823862" />

### 3. Run Placement
```shell
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk place
```
<img width="1203" height="767" alt="Screenshot 2025-10-01 at 9 44 19 PM" src="https://github.com/user-attachments/assets/efc01f5a-7ae5-4e9b-9596-f5a7dc435009" />

```shell
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk place
```
<img width="1207" height="765" alt="Screenshot 2025-10-01 at 9 44 48 PM" src="https://github.com/user-attachments/assets/0fcab272-92ee-407a-adde-fd04218ecdf8" />

### 4. Run CTS
```shell
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk cts
```
<img width="1207" height="758" alt="Screenshot 2025-10-01 at 9 59 24 PM" src="https://github.com/user-attachments/assets/cee7e18e-9d80-4827-a134-4d4829f96018" />

```shell
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_cts
```
<img width="1208" height="765" alt="Screenshot 2025-10-01 at 10 00 28 PM" src="https://github.com/user-attachments/assets/a085992d-5a27-4432-968a-4b192f28d071" />

* Setup Timing Report:
<img width="1212" height="769" alt="Screenshot 2025-10-01 at 10 14 04 PM" src="https://github.com/user-attachments/assets/fc5509d1-accd-4e14-9542-3b9ec1f36aad" />
All paths have positive slack, the design meets setup timing requirements.

* Hold Timing report:
<img width="1213" height="768" alt="Screenshot 2025-10-01 at 10 15 18 PM" src="https://github.com/user-attachments/assets/2cfafb0f-3ddb-4440-8c78-a0d744bbf5a2" />
All paths have positive slack, the design meets hold timing requirements.

* CTS final report:

```shell
gvim /home/saad/OpenROAD-flow-scripts/flow/reports/sky130hd/vsdbabysoc/base/4_cts_final.rpt
```
```
==========================================================================
cts final report_tns
--------------------------------------------------------------------------
tns max 0.00

==========================================================================
cts final report_wns
--------------------------------------------------------------------------
wns max 0.00

==========================================================================
cts final report_worst_slack
--------------------------------------------------------------------------
worst slack max 6.11

==========================================================================
cts final report_clock_min_period
--------------------------------------------------------------------------

==========================================================================
cts final report_clock_skew
--------------------------------------------------------------------------
Clock clk
   0.98 source latency core.CPU_valid_a4$_DFF_P_/CLK ^
  -0.92 target latency core.CPU_Dmem_value_a5[4][5]$_SDFFE_PP0P_/CLK ^
   0.00 CRPR
--------------
   0.06 setup skew


==========================================================================
cts final report_checks -path_delay min
--------------------------------------------------------------------------
Startpoint: core.CPU_rd_a2[1]$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_rd_a3[1]$_DFF_P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     1    0.22    0.00    0.00    0.00 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.03    0.01    0.01 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    16    0.46    0.45    0.42    0.43 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.45    0.01    0.44 ^ clkbuf_4_4__f_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     7    0.15    0.17    0.33    0.77 ^ clkbuf_4_4__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_4__leaf_CLK (net)
                  0.17    0.00    0.77 ^ clkbuf_leaf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    11    0.03    0.05    0.18    0.95 ^ clkbuf_leaf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_0_CLK (net)
                  0.05    0.00    0.95 ^ core.CPU_rd_a2[1]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.04    0.30    1.25 ^ core.CPU_rd_a2[1]$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         core.CPU_rd_a2[1] (net)
                  0.04    0.00    1.25 ^ core.CPU_rd_a3[1]$_DFF_P_/D (sky130_fd_sc_hd__dfxtp_1)
                                  1.25   data arrival time

                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     1    0.22    0.00    0.00    0.00 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.03    0.01    0.01 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    16    0.46    0.45    0.42    0.43 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.45    0.01    0.44 ^ clkbuf_4_4__f_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     7    0.15    0.17    0.33    0.77 ^ clkbuf_4_4__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_4__leaf_CLK (net)
                  0.17    0.00    0.77 ^ clkbuf_leaf_120_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     6    0.04    0.07    0.19    0.96 ^ clkbuf_leaf_120_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_120_CLK (net)
                  0.07    0.00    0.96 ^ core.CPU_rd_a3[1]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
                          0.00    0.96   clock reconvergence pessimism
                         -0.03    0.93   library hold time
                                  0.93   data required time
-----------------------------------------------------------------------------
                                  0.93   data required time
                                 -1.25   data arrival time
-----------------------------------------------------------------------------
                                  0.31   slack (MET)



==========================================================================
cts final report_checks -path_delay max
--------------------------------------------------------------------------
Startpoint: core.CPU_src1_value_a3[0]$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_Xreg_value_a4[9][28]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     1    0.22    0.00    0.00    0.00 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.03    0.01    0.01 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    16    0.46    0.45    0.42    0.43 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.45    0.01    0.44 ^ clkbuf_4_7__f_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     8    0.15    0.16    0.33    0.77 ^ clkbuf_4_7__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_7__leaf_CLK (net)
                  0.16    0.00    0.77 ^ clkbuf_leaf_19_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     9    0.03    0.05    0.18    0.95 ^ clkbuf_leaf_19_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_19_CLK (net)
                  0.05    0.00    0.95 ^ core.CPU_src1_value_a3[0]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
     8    0.03    0.32    0.50    1.45 ^ core.CPU_src1_value_a3[0]$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         core.CPU_src1_value_a3[0] (net)
                  0.32    0.00    1.45 ^ _05635_/A (sky130_fd_sc_hd__inv_1)
     2    0.01    0.08    0.10    1.55 v _05635_/Y (sky130_fd_sc_hd__inv_1)
                                         _00022_ (net)
                  0.08    0.00    1.55 v _11014_/B (sky130_fd_sc_hd__ha_1)
     2    0.01    0.09    0.25    1.80 v _11014_/COUT (sky130_fd_sc_hd__ha_1)
                                         _00004_ (net)
                  0.09    0.00    1.80 v _11007_/CIN (sky130_fd_sc_hd__fa_1)
     2    0.01    0.09    0.40    2.20 v _11007_/COUT (sky130_fd_sc_hd__fa_1)
                                         _00005_ (net)
                  0.09    0.00    2.20 v _07623_/A (sky130_fd_sc_hd__nor2b_1)
     4    0.02    0.33    0.30    2.50 ^ _07623_/Y (sky130_fd_sc_hd__nor2b_1)
                                         _02605_ (net)
                  0.33    0.00    2.50 ^ _07774_/A1 (sky130_fd_sc_hd__o21ai_0)
     2    0.02    0.29    0.33    2.83 v _07774_/Y (sky130_fd_sc_hd__o21ai_0)
                                         _02754_ (net)
                  0.29    0.00    2.84 v _07776_/A2 (sky130_fd_sc_hd__o21a_1)
     4    0.02    0.12    0.34    3.18 v _07776_/X (sky130_fd_sc_hd__o21a_1)
                                         _02756_ (net)
                  0.12    0.00    3.18 v _08183_/A2 (sky130_fd_sc_hd__a21boi_1)
     5    0.04    0.74    0.63    3.81 ^ _08183_/Y (sky130_fd_sc_hd__a21boi_1)
                                         _03154_ (net)
                  0.74    0.00    3.82 ^ _08522_/C (sky130_fd_sc_hd__nor4b_1)
     2    0.01    0.17    0.18    4.00 v _08522_/Y (sky130_fd_sc_hd__nor4b_1)
                                         _03485_ (net)
                  0.17    0.00    4.00 v _08523_/D (sky130_fd_sc_hd__nor4_1)
     1    0.01    0.61    0.51    4.51 ^ _08523_/Y (sky130_fd_sc_hd__nor4_1)
                                         _03486_ (net)
                  0.61    0.00    4.51 ^ _08550_/A (sky130_fd_sc_hd__nor4_2)
     3    0.01    0.15    0.17    4.68 v _08550_/Y (sky130_fd_sc_hd__nor4_2)
                                         _03513_ (net)
                  0.15    0.00    4.68 v _08553_/A2 (sky130_fd_sc_hd__o311ai_2)
    11    0.05    0.87    0.79    5.47 ^ _08553_/Y (sky130_fd_sc_hd__o311ai_2)
                                         _03516_ (net)
                  0.87    0.00    5.47 ^ _09766_/A1 (sky130_fd_sc_hd__o21ai_0)
     1    0.00    0.15    0.22    5.69 v _09766_/Y (sky130_fd_sc_hd__o21ai_0)
                                         _01100_ (net)
                  0.15    0.00    5.69 v core.CPU_Xreg_value_a4[9][28]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_1)
                                  5.69   data arrival time

                         11.00   11.00   clock clk (rise edge)
                          0.00   11.00   clock source latency
     1    0.22    0.00    0.00   11.00 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.03    0.01   11.01 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    16    0.46    0.45    0.42   11.43 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.45    0.01   11.44 ^ clkbuf_4_3__f_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    11    0.16    0.17    0.34   11.78 ^ clkbuf_4_3__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_3__leaf_CLK (net)
                  0.17    0.00   11.78 ^ clkbuf_leaf_41_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    10    0.03    0.05    0.18   11.96 ^ clkbuf_leaf_41_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_41_CLK (net)
                  0.05    0.00   11.96 ^ core.CPU_Xreg_value_a4[9][28]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_1)
                          0.00   11.96   clock reconvergence pessimism
                         -0.15   11.80   library setup time
                                 11.80   data required time
-----------------------------------------------------------------------------
                                 11.80   data required time
                                 -5.69   data arrival time
-----------------------------------------------------------------------------
                                  6.11   slack (MET)



==========================================================================
cts final report_checks -unconstrained
--------------------------------------------------------------------------
Startpoint: core.CPU_src1_value_a3[0]$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_Xreg_value_a4[9][28]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     1    0.22    0.00    0.00    0.00 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.03    0.01    0.01 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    16    0.46    0.45    0.42    0.43 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.45    0.01    0.44 ^ clkbuf_4_7__f_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     8    0.15    0.16    0.33    0.77 ^ clkbuf_4_7__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_7__leaf_CLK (net)
                  0.16    0.00    0.77 ^ clkbuf_leaf_19_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     9    0.03    0.05    0.18    0.95 ^ clkbuf_leaf_19_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_19_CLK (net)
                  0.05    0.00    0.95 ^ core.CPU_src1_value_a3[0]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
     8    0.03    0.32    0.50    1.45 ^ core.CPU_src1_value_a3[0]$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         core.CPU_src1_value_a3[0] (net)
                  0.32    0.00    1.45 ^ _05635_/A (sky130_fd_sc_hd__inv_1)
     2    0.01    0.08    0.10    1.55 v _05635_/Y (sky130_fd_sc_hd__inv_1)
                                         _00022_ (net)
                  0.08    0.00    1.55 v _11014_/B (sky130_fd_sc_hd__ha_1)
     2    0.01    0.09    0.25    1.80 v _11014_/COUT (sky130_fd_sc_hd__ha_1)
                                         _00004_ (net)
                  0.09    0.00    1.80 v _11007_/CIN (sky130_fd_sc_hd__fa_1)
     2    0.01    0.09    0.40    2.20 v _11007_/COUT (sky130_fd_sc_hd__fa_1)
                                         _00005_ (net)
                  0.09    0.00    2.20 v _07623_/A (sky130_fd_sc_hd__nor2b_1)
     4    0.02    0.33    0.30    2.50 ^ _07623_/Y (sky130_fd_sc_hd__nor2b_1)
                                         _02605_ (net)
                  0.33    0.00    2.50 ^ _07774_/A1 (sky130_fd_sc_hd__o21ai_0)
     2    0.02    0.29    0.33    2.83 v _07774_/Y (sky130_fd_sc_hd__o21ai_0)
                                         _02754_ (net)
                  0.29    0.00    2.84 v _07776_/A2 (sky130_fd_sc_hd__o21a_1)
     4    0.02    0.12    0.34    3.18 v _07776_/X (sky130_fd_sc_hd__o21a_1)
                                         _02756_ (net)
                  0.12    0.00    3.18 v _08183_/A2 (sky130_fd_sc_hd__a21boi_1)
     5    0.04    0.74    0.63    3.81 ^ _08183_/Y (sky130_fd_sc_hd__a21boi_1)
                                         _03154_ (net)
                  0.74    0.00    3.82 ^ _08522_/C (sky130_fd_sc_hd__nor4b_1)
     2    0.01    0.17    0.18    4.00 v _08522_/Y (sky130_fd_sc_hd__nor4b_1)
                                         _03485_ (net)
                  0.17    0.00    4.00 v _08523_/D (sky130_fd_sc_hd__nor4_1)
     1    0.01    0.61    0.51    4.51 ^ _08523_/Y (sky130_fd_sc_hd__nor4_1)
                                         _03486_ (net)
                  0.61    0.00    4.51 ^ _08550_/A (sky130_fd_sc_hd__nor4_2)
     3    0.01    0.15    0.17    4.68 v _08550_/Y (sky130_fd_sc_hd__nor4_2)
                                         _03513_ (net)
                  0.15    0.00    4.68 v _08553_/A2 (sky130_fd_sc_hd__o311ai_2)
    11    0.05    0.87    0.79    5.47 ^ _08553_/Y (sky130_fd_sc_hd__o311ai_2)
                                         _03516_ (net)
                  0.87    0.00    5.47 ^ _09766_/A1 (sky130_fd_sc_hd__o21ai_0)
     1    0.00    0.15    0.22    5.69 v _09766_/Y (sky130_fd_sc_hd__o21ai_0)
                                         _01100_ (net)
                  0.15    0.00    5.69 v core.CPU_Xreg_value_a4[9][28]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_1)
                                  5.69   data arrival time

                         11.00   11.00   clock clk (rise edge)
                          0.00   11.00   clock source latency
     1    0.22    0.00    0.00   11.00 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.03    0.01   11.01 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    16    0.46    0.45    0.42   11.43 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.45    0.01   11.44 ^ clkbuf_4_3__f_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    11    0.16    0.17    0.34   11.78 ^ clkbuf_4_3__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_3__leaf_CLK (net)
                  0.17    0.00   11.78 ^ clkbuf_leaf_41_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    10    0.03    0.05    0.18   11.96 ^ clkbuf_leaf_41_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_41_CLK (net)
                  0.05    0.00   11.96 ^ core.CPU_Xreg_value_a4[9][28]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_1)
                          0.00   11.96   clock reconvergence pessimism
                         -0.15   11.80   library setup time
                                 11.80   data required time
-----------------------------------------------------------------------------
                                 11.80   data required time
                                 -5.69   data arrival time
-----------------------------------------------------------------------------
                                  6.11   slack (MET)



==========================================================================
cts final report_check_types -max_slew -max_cap -max_fanout -violators
--------------------------------------------------------------------------

==========================================================================
cts final max_slew_check_slack
--------------------------------------------------------------------------
0.18770186603069305

==========================================================================
cts final max_slew_check_limit
--------------------------------------------------------------------------
1.4979510307312012

==========================================================================
cts final max_slew_check_slack_limit
--------------------------------------------------------------------------
0.1253

==========================================================================
cts final max_fanout_check_slack
--------------------------------------------------------------------------
1.0000000150474662e+30

==========================================================================
cts final max_fanout_check_limit
--------------------------------------------------------------------------
1.0000000150474662e+30

==========================================================================
cts final max_capacitance_check_slack
--------------------------------------------------------------------------
0.007412398234009743

==========================================================================
cts final max_capacitance_check_limit
--------------------------------------------------------------------------
0.021067000925540924

==========================================================================
cts final max_capacitance_check_slack_limit
--------------------------------------------------------------------------
0.3518

==========================================================================
cts final max_slew_violation_count
--------------------------------------------------------------------------
max slew violation count 0

==========================================================================
cts final max_fanout_violation_count
--------------------------------------------------------------------------
max fanout violation count 0

==========================================================================
cts final max_cap_violation_count
--------------------------------------------------------------------------
max cap violation count 0

==========================================================================
cts final setup_violation_count
--------------------------------------------------------------------------
setup violation count 0

==========================================================================
cts final hold_violation_count
--------------------------------------------------------------------------
hold violation count 0

==========================================================================
cts final report_checks -path_delay max reg to reg
--------------------------------------------------------------------------
Startpoint: core.CPU_src1_value_a3[0]$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_Xreg_value_a4[9][28]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

  Delay    Time   Description
---------------------------------------------------------
   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock source latency
   0.00    0.00 ^ pll/CLK (avsdpll)
   0.43    0.43 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.34    0.77 ^ clkbuf_4_7__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.18    0.95 ^ clkbuf_leaf_19_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.00    0.95 ^ core.CPU_src1_value_a3[0]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
   0.50    1.45 ^ core.CPU_src1_value_a3[0]$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
   0.11    1.55 v _05635_/Y (sky130_fd_sc_hd__inv_1)
   0.25    1.80 v _11014_/COUT (sky130_fd_sc_hd__ha_1)
   0.40    2.20 v _11007_/COUT (sky130_fd_sc_hd__fa_1)
   0.30    2.50 ^ _07623_/Y (sky130_fd_sc_hd__nor2b_1)
   0.33    2.83 v _07774_/Y (sky130_fd_sc_hd__o21ai_0)
   0.35    3.18 v _07776_/X (sky130_fd_sc_hd__o21a_1)
   0.63    3.81 ^ _08183_/Y (sky130_fd_sc_hd__a21boi_1)
   0.18    4.00 v _08522_/Y (sky130_fd_sc_hd__nor4b_1)
   0.51    4.51 ^ _08523_/Y (sky130_fd_sc_hd__nor4_1)
   0.18    4.68 v _08550_/Y (sky130_fd_sc_hd__nor4_2)
   0.79    5.47 ^ _08553_/Y (sky130_fd_sc_hd__o311ai_2)
   0.22    5.69 v _09766_/Y (sky130_fd_sc_hd__o21ai_0)
   0.00    5.69 v core.CPU_Xreg_value_a4[9][28]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_1)
           5.69   data arrival time

  11.00   11.00   clock clk (rise edge)
   0.00   11.00   clock source latency
   0.00   11.00 ^ pll/CLK (avsdpll)
   0.43   11.43 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.35   11.78 ^ clkbuf_4_3__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.18   11.96 ^ clkbuf_leaf_41_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.00   11.96 ^ core.CPU_Xreg_value_a4[9][28]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_1)
   0.00   11.96   clock reconvergence pessimism
  -0.15   11.80   library setup time
          11.80   data required time
---------------------------------------------------------
          11.80   data required time
          -5.69   data arrival time
---------------------------------------------------------
           6.11   slack (MET)



==========================================================================
cts final report_checks -path_delay min reg to reg
--------------------------------------------------------------------------
Startpoint: core.CPU_rd_a2[1]$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_rd_a3[1]$_DFF_P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

  Delay    Time   Description
---------------------------------------------------------
   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock source latency
   0.00    0.00 ^ pll/CLK (avsdpll)
   0.43    0.43 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.34    0.77 ^ clkbuf_4_4__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.18    0.95 ^ clkbuf_leaf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.00    0.95 ^ core.CPU_rd_a2[1]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
   0.30    1.25 ^ core.CPU_rd_a2[1]$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
   0.00    1.25 ^ core.CPU_rd_a3[1]$_DFF_P_/D (sky130_fd_sc_hd__dfxtp_1)
           1.25   data arrival time

   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock source latency
   0.00    0.00 ^ pll/CLK (avsdpll)
   0.43    0.43 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.34    0.77 ^ clkbuf_4_4__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.19    0.96 ^ clkbuf_leaf_120_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.00    0.96 ^ core.CPU_rd_a3[1]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
   0.00    0.96   clock reconvergence pessimism
  -0.03    0.93   library hold time
           0.93   data required time
---------------------------------------------------------
           0.93   data required time
          -1.25   data arrival time
---------------------------------------------------------
           0.31   slack (MET)



==========================================================================
cts final critical path target clock latency max path
--------------------------------------------------------------------------
0

==========================================================================
cts final critical path target clock latency min path
--------------------------------------------------------------------------
0

==========================================================================
cts final critical path source clock latency min path
--------------------------------------------------------------------------
0

==========================================================================
cts final critical path delay
--------------------------------------------------------------------------
5.6897

==========================================================================
cts final critical path slack
--------------------------------------------------------------------------
6.1137

==========================================================================
cts final slack div critical path delay
--------------------------------------------------------------------------
107.452062

==========================================================================
cts final report_power
--------------------------------------------------------------------------
Group                  Internal  Switching    Leakage      Total
                          Power      Power      Power      Power (Watts)
----------------------------------------------------------------
Sequential             4.39e-03   4.30e-04   9.26e-09   4.82e-03  36.9%
Combinational          9.03e-04   2.40e-03   9.95e-09   3.30e-03  25.2%
Clock                  2.77e-03   2.19e-03   2.33e-09   4.96e-03  37.9%
Macro                  0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
Pad                    0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
----------------------------------------------------------------
Total                  8.06e-03   5.01e-03   2.15e-08   1.31e-02 100.0%
```

### 4. Run Routing
```shell
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk route
```
<img width="1202" height="714" alt="Screenshot 2025-10-01 at 10 34 13 PM" src="https://github.com/user-attachments/assets/83d9533d-20cf-46cf-822c-5f13d5c7935f" />

```shell
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_route
```
<img width="1211" height="773" alt="Screenshot 2025-10-01 at 10 41 17 PM" src="https://github.com/user-attachments/assets/1d094898-ebe1-48a6-89e3-420ee16e6a3f" />

### 5. Convert `.odb` to `.def` in OpenROAD

```shell
cd ~/OpenROAD-flow-scripts
source env.sh
cd flow
openroad
read_db /home/saad/OpenROAD-flow-scripts/flow/results/sky130hd/vsdbabysoc/base/5_2_route.odb
write_def /home/saad/OpenROAD-flow-scripts/flow/results/sky130hd/vsdbabysoc/base/5_2_route.def
```
<img width="814" height="350" alt="Screenshot 2025-10-01 at 10 45 31 PM" src="https://github.com/user-attachments/assets/b6279bab-18b6-4ac8-a4d3-5f2d67525a19" />

```shell
gvim /home/saad/OpenROAD-flow-scripts/flow/results/sky130hd/vsdbabysoc/base/5_2_route.def
```
<img width="1209" height="757" alt="Screenshot 2025-10-01 at 10 47 45 PM" src="https://github.com/user-attachments/assets/900b1124-26f3-4b2c-b30e-a76d2cb4422c" />

### VSDBabySoC post_route SPEF generation

## Step 1: Launch OpenROAD
```bash
cd ~/OpenROAD-flow-scripts
source env.sh
cd flow/
openroad
```

## Step 2: Load Design and Technology Files
```shell
read_lef /home/saad/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/lef/sky130hd.lef
read_lef /home/saad/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/lef/avsdpll.lef
read_lef /home/saad/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/lef/avsddac.lef
```
```shell
read_liberty /home/saad/OpenROAD-flow-scripts/flow/platforms/sky130hd/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
```shell
read_def /home/saad/OpenROAD-flow-scripts/flow/results/sky130hd/vsdbabysoc/base/5_2_route.def
```
<img width="1207" height="763" alt="Screenshot 2025-10-01 at 10 50 03 PM" src="https://github.com/user-attachments/assets/79ee20f3-a2c2-4d40-bc8d-af9dcf39f9ee" />

## Step 3: RC Extraction and Output Generation
```tcl
define_process_corner -ext_model_index 0 /home/saad/OpenROAD-flow-scripts/external-resources/open_pdks/sky130/openlane/rules.openrcx.sky130A.nom.calibre
```
```shell
extract_parasitics -ext_model_file /home/saad/OpenROAD-flow-scripts/external-resources/open_pdks/sky130/openlane/rules.openrcx.sky130A.nom.calibre
```
<img width="1805" height="1141" alt="Screenshot 2025-10-02 at 3 49 34 PM" src="https://github.com/user-attachments/assets/5bac8753-6ab6-44fb-8a33-f0185f40013c" />

## Step 4: Write SPEF File
```shell
write_spef /home/saad/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/vsdbabysoc.spef
```
<img width="1822" height="1155" alt="Screenshot 2025-10-02 at 3 52 49 PM" src="https://github.com/user-attachments/assets/94bc4fb1-1cbe-4708-acd1-730d818480ba" />

## Step 5: Write Post-Placement Verilog Netlist
```shell
write_verilog /home/saad/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/vsdbabysoc_post_place.v
```
<img width="1823" height="1152" alt="Screenshot 2025-10-02 at 3 54 22 PM" src="https://github.com/user-attachments/assets/684c34db-36d7-47e9-955a-0f6fdca387af" />



