## Floorplan and Placement of VSDBabySoC in OpenROAD

#### Creating Config File:

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

# export CORE_UTILIZATION=0.1  # Reduce this value to allow more whitespace for routing.
```
### Run Synthesis:

```shell
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk synth
```

#### * Synthesis Stats:

<img width="923" height="623" alt="Screenshot 2025-09-23 at 10 15 47 PM" src="https://github.com/user-attachments/assets/40e5ce6a-7e8a-47b0-a35d-966e0d4ddad4" />


#### * Synthesis check:

<img width="928" height="611" alt="Screenshot 2025-09-23 at 10 15 32 PM" src="https://github.com/user-attachments/assets/233205f3-f4ca-4ef8-a27f-48b0e8ec3e0c" />


#### * Synthesis Log file:

<img width="924" height="625" alt="Screenshot 2025-09-23 at 10 15 00 PM" src="https://github.com/user-attachments/assets/7c5d5355-7cf3-49b5-9ec0-9bdcc5fe0b73" />


#### * Synthesis netlist:

<img width="921" height="626" alt="Screenshot 2025-09-23 at 10 13 50 PM" src="https://github.com/user-attachments/assets/066d70ba-cbe5-4f78-9d08-2a15164705cb" />


#### Run Floorplan and floorplan result in GUI:

```shell
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk floorplan
```

```shell
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_floorplan
```

<img width="937" height="582" alt="Screenshot 2025-09-22 at 8 34 53 PM" src="https://github.com/user-attachments/assets/5c1c4b37-30bf-46f1-9723-07abf442afff" />

#### Run Placement:

```shell
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk place
```

<img width="629" height="432" alt="Screenshot 2025-09-22 at 8 37 34 PM" src="https://github.com/user-attachments/assets/be210408-3d95-41ee-b867-12c9e671ce89" />

#### Placement Result in GUI:
```shell
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_place
```

<img width="618" height="434" alt="Screenshot 2025-09-22 at 8 38 30 PM" src="https://github.com/user-attachments/assets/a92415f0-4af4-41eb-9fe2-b1ee3896fde9" />

#### * Placement Density Heatmap:

<img width="939" height="580" alt="Screenshot 2025-09-22 at 8 39 59 PM" src="https://github.com/user-attachments/assets/6a0d6e4e-6c56-4c77-baf5-533de835ffb0" />

#### * Pin Density Heatmap:

<img width="941" height="577" alt="Screenshot 2025-09-22 at 9 17 11 PM" src="https://github.com/user-attachments/assets/5a87af9f-a520-47d2-b598-e5c0d85afdd5" />
