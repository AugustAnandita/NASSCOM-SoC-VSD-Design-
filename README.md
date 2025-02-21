# NASSCOM-SoC-VSD-Design 
This repository summarizes the contents and labs covered in 5 days of the NASSCOM SoC VSD Design Workshop.

![VSD](https://github.com/user-attachments/assets/4b6efa5a-ebe1-4698-b387-c6ba6be2985d)


# WORKSHOP TOPICS

## Table of Contents
- [Course Content](#course-content)
- [Day 1: How to Talk to Computers](#day-1-how-to-talk-to-computers)
- [Day 2: Good Floorplan vs Bad Floorplan](#day-2-good-floorplan-vs-bad-floorplan)
- [Day 3: Design a Library Cell using Magic Layout and Ngspice Characterization](#day-3-Design-a-Library-Cell-using-Magic-Layout-and-Ngspice-Characterization)
- [Day 4: Timing Analysis with Real clocks using openSTA](#day-4-Timing-Analysis-with-Real-clocks-using-openSTA)
- [Day 5: Final steps for RTL2GDS using tritonRoute and openSTA](#day-5-Final-steps-for-RTL2GDS-using-tritonRoute-and-openSTA)

## Course Content

### Day 1
- Introduction to IC Design components and terminologies
- Software application to hardware execution
- RTL2GDS OpenLANE ASIC Flow
- Open-source EDA tools familiarization

### Day 2
- Chip floorplanning
- Placement
- Standard cell design
- Standard cell characterization

### Day 3
- 16 Mask CMOS fabrication process
- Design and characterize library cell CMOS inverter

### Day 4
- Introduction and generation of LEF files using Magic tool
- Custom cells in OpenLANE
- Fixing slack violations
- Clock Tree Synthesis (CTS)

### Day 5
- Power distribution network
- Routing
- SPEF extraction
- GDSII

---


# Day 1 : Inception of open-source EDA, OpenLANE and Sky130 PDK 

### IC Design Terminology

#### Key Components:
- **Package**: A case that surrounds the circuit material to protect it from damage and allows for mounting electrical contacts connecting it to the PCB.
- **Die**: A small block of semiconductor material where the circuit is fabricated.
- **Core**: The actual area of the IC where logic resides.
- **Pads**: Interfaces between the internal signals of a chip and external pins, connected via wire bonds.

 ![F1](https://github.com/user-attachments/assets/de1f657a-1cda-46fd-b2a5-57428b741a65)

#### Simple Definitions:
- **Package** = Protective case with pins
- **Die** = Tiny brain of the chip
- **Core** = The part where the processing happens
- **Pads** = Connection points between the chip and the outside world

![F1_2](https://github.com/user-attachments/assets/087ef17a-c801-46d4-9642-620468c1fcdf)


### Application to Hardware Flow
1. **Operating System (OS)**
   - Manages hardware
   - Provides functions in C, C++, Java, etc.
2. **Compiler**
   - Converts OS functions into hardware-specific instructions
3. **Assembler**
   - Translates instructions into binary (machine language)

### Open-Source ASIC Design Implementation

#### Required Components:
- **RTL Designs**: Register-Transfer Level circuit descriptions
- **EDA Tools**: Electronic Design Automation tools
- **PDK Data**: Process Design Kits

#### Historical Context
- **1979**: Lynn Conway and Carver Mead separated design from fabrication
- Introduced λ-based design rules
- Wrote "Introduction to VLSI Systems"



#### Industry Evolution:
- **Fabless Companies**: Design-focused
- **Pure Play Fabs**: Manufacturing-focused

#### PDK Components:
- Device models
- Technology information
- Design rules
- Digital standard cell libraries
- I/O libraries

### RTL2GDS OpenLANE ASIC Flow
![F1_3](https://github.com/user-attachments/assets/5443a379-9304-4397-bd0b-509bb9060636)

#### Integrated Tools:
- OpenROAD
- Yosys
- Magic
- Netgen
- Fault
- OpenPhySyn
- CVC
- SPEF-Extractor
- CU-GR
- KLayout

![F1_4](https://github.com/user-attachments/assets/f22e6e95-da21-4af8-83e5-9fb4ad4bd30c)

### Lab Work - Task 1: Running 'picorv32a' Design

#### OpenLANE Configuration Priority:
1. sky130_xxxxx_config.tcl
2. config.tcl
3. Default values

#### Step-by-Step Execution:

1. **Navigate to OpenLANE directory**
```bash
cd openLANE
```

2. **Start Docker environment**
```bash
docker
```

3. **Start OpenLANE interactive mode**
```bash
./flow.tcl -interactive
```

4. **Load OpenLANE package**
```bash
package require openlane 0.9
```

5. **Prepare design**
```bash
prep -design picorv32a
```

6. **Run synthesis**
```bash
run_synthesis
```

#### Generated Outputs:

![F1_5](https://github.com/user-attachments/assets/97443f89-97a8-4285-81f6-55f1a25e66c7)
![F1_6](https://github.com/user-attachments/assets/bd855ba0-1dd6-46d4-a8db-7643d264ddaa)
![F1_7](https://github.com/user-attachments/assets/a889185c-fcdb-4201-98fd-f0d0c887363e)
![F1_8](https://github.com/user-attachments/assets/efbfe702-66ee-41b9-9781-3eb5c213f451)
![F1_9](https://github.com/user-attachments/assets/59fc1371-1f94-466d-a222-d1bb77b6a5e8)


#### Flip-Flop Ratio Calculation:
- **Number of Flip-Flops**: 1613
- **Total Number of Cells**: 14876


## Day 2: Good Floorplan vs Bad Floorplan

### Introduction to Floorplanning


![F2_1](https://github.com/user-attachments/assets/cae3123b-e4c0-4599-b795-2b5dae1d053e)

![F2_2](https://github.com/user-attachments/assets/278f10d5-06f7-441f-b463-7437282c249c)
### Key Concepts:

#### Aspect Ratio
- Ratio of height to width of core area
- Aspect ratio of 1 indicates square shape

#### Utilization Factor
- Ratio of netlist area to core area
- Recommended range: 0.5 to 0.7

#### Preplaced Cells
- Fixed location cells
- Examples: memory blocks, clock gating cells, comparators

#### Decoupling Capacitors
- Used with preplaced cells
- Compensates voltage drop
- Charges to supply voltage

#### Power Planning
- Provides power to all components
- Creates power grid network
- Mitigates voltage droop

### Implementation Steps

1. **Run Floorplan**
```bash
run_floorplan
```
**Load Floorplan in Magic**
```bash
magic -T $PDK/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```

![F2_3](https://github.com/user-attachments/assets/6f7af553-e4fa-4269-858e-24d1fb5b6027)
![F2_4](https://github.com/user-attachments/assets/ac888477-4ad9-4efe-b543-fe7709c69d42)
![F2_5](https://github.com/user-attachments/assets/ad6a4f3f-c2c7-47e3-b68a-a36ca249278c)



2. **Run Placement**
```bash
run_placement
```

![F2_6_b](https://github.com/user-attachments/assets/df177928-4c15-4926-aa3e-635345d443de)

3. **Load Placement in Magic**
```bash
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/16-02_07-35/results/placement/

magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```

### Placement Results

![F2_7_c](https://github.com/user-attachments/assets/f285cb94-9393-41d3-ae7d-05d2160631c2)
![F2_8](https://github.com/user-attachments/assets/12c25669-d8ff-44ce-b347-ea6bffc701b5)



---


## Day 3: Design a Library Cell using Magic Layout and Ngspice Characterization



1. Clone custom inverter standard cell design from github repository
 ```bash  
# Change directory to openlane
cd Desktop/work/tools/openlane_working_dir/openlane

# Clone the repository with custom inverter design
git clone https://github.com/nickson-jose/vsdstdcelldesign

# Change into repository directory
cd vsdstdcelldesign

# Copy magic tech file to the repo directory for easy access
cp /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech .

# Check contents whether everything is present
ls

# Command to open custom inverter layout in magic
magic -T sky130A.tech sky130_inv.mag &
```
Screenshot of commands run






![L3_1](https://github.com/user-attachments/assets/f9f004c5-9787-48e5-8446-5e8dfad08304)


![L3_2](https://github.com/user-attachments/assets/eec29e86-d751-4ddf-983e-e09187b0eeaa)

![L3_3](https://github.com/user-attachments/assets/f8bd122e-87a6-4283-983c-be7988e62594)


![L3_4](https://github.com/user-attachments/assets/71e98688-b066-4386-8df5-3b5284b19ba3)


![L3_5](https://github.com/user-attachments/assets/e4f50360-3d6e-498d-89ea-4e195eb65e2f)

Screenshot of tkcon window after running above commands
![L3_6](https://github.com/user-attachments/assets/241a79e8-b750-4bb8-88fe-bff6d06d0f91)
Spice extraction of inverter in magic.
Commands for spice extraction of the custom inverter layout to be used in tkcon window of magic
```bash
# Check current directory
pwd

# Extraction command to extract to .ext format
extract all

# Before converting ext to spice this command enable the parasitic extraction also
ext2spice cthresh 0 rthresh 0

# Converting to ext to spice
ext2spice
```
Final edited spice file ready for ngspice simulation
```bash
* SPICE3 file created from sky130_inv2.ext - technology: sky130A  
.option scale=0.01u  
.include ./libs/pshort.lib  
.include ./libs/nshort.lib  

M1000 Y A VPWR VPWR pshort_model.0 w=37 l=23  
+ ad=1443 pd=152 as=1517 ps=156  
M1001 Y A VGND VGND nshort_model.0 w=35 l=23  
+ ad=1435 pd=152 as=1365 ps=148  

VDD VPWR 0 3.3V  
VSS VGND 0 0V  
Va A VGND PULSE(0V 3.3V 0 0.1ns 0.1ns 2ns 4ns)  

C0 A Y 0.05fF  
C1 Y VPWR 0.11fF  
C2 A VPWR 0.07fF  
C3 Y 0 2fF  
C4 VPWR 0 0.59fF  

.tran 1n 20n  

.control  
run  
.endc  
.end  
```
Post-layout ngspice simulations.
Commands for ngspice simulation
```bash
# Command to directly load spice file for simulation to ngspice
ngspice sky130_inv.spice

# Now that we have entered ngspice with the simulation spice file loaded we just have to load the plot
plot y vs time a
```
![L3_7](https://github.com/user-attachments/assets/3c4537ba-b432-412e-a295-22467954079f)
Screenshot of generated plot
![L3_8](https://github.com/user-attachments/assets/06144dd5-887a-4d96-8647-2a99adb7df0c)
![L3_9](https://github.com/user-attachments/assets/ed79e9af-96d0-46ab-8b48-7bce8481698b)
![L3_10](https://github.com/user-attachments/assets/43b1ff53-d09b-4230-948e-79a5309937b7)
![L3_11](https://github.com/user-attachments/assets/baeb22b8-8e3a-4bb5-aa4f-530e9234bae7)


# DAY 4 : Timing Analysis with Real clocks using openSTA

# Contents:

* Timing modeling using delay tables

* Lab steps to convert grid info to track info

* Lab steps to convert magic layout to std cell LEF

* Introduction to timing libs and steps to include new cell in synthesis

* Introduction to delay tables

* Delay table usage Part 1

* Delay table usage Part 2

* Lab steps to configure synthesis settings to fix slack and include vsdinv

* Timing analysis with ideal clocks using openSTA

* Setup timing analysis and introduction to flip-flop setup time

* Introduction to clock jitter and uncertainty

* Lab steps to configure OpenSTA for post-synth timing analysis

* Lab steps to optimize synthesis to reduce setup violations

* Lab steps to do basic timing ECO

* Clock tree synthesis TritonCTS and signal integrity

* Clock tree routing and buffering using H-Tree algorithm

* Crosstalk and clock net shielding

* Lab steps to run CTS using TritonCTS

* Lab steps to verify CTS runs

* Timing analysis with real clock using openSTA

* Setup timing analysis using real clocks

* Hold timing analysis using real clocks

* Lab steps to analyze timing with real clocks using OpenSTA

* Lab steps to execute OpenSTA with right timing libraries and CTS assignment

* Lab steps to observe impact of bigger CTS buffers on setup and hold timing

# Theory

![image](https://github.com/user-attachments/assets/f7ba9de0-b838-43c4-a516-e8310c43c7de)

![image](https://github.com/user-attachments/assets/3d6a5f77-f34d-4517-aa48-1f3bd5ce06b2)

![image](https://github.com/user-attachments/assets/bb45c21e-5294-46bc-8aad-71ed2ab20063)

Assume c1=c2=c3=c4=25fF and Cbuf1=Cbuf2=30fF

Total Cap at node 'A'=> 60fF

Total Cap at node 'B'=> 50fF

Total Cap at node 'C'=> 50fF

Example:

![image](https://github.com/user-attachments/assets/377c7b53-a3a1-4183-8b14-772bac3a84dd)

![image](https://github.com/user-attachments/assets/a8410720-9968-4b20-b4da-d8750f1a41f1)


# Timing analysis using openSTA

![image](https://github.com/user-attachments/assets/55c90d6f-33ed-4c61-933f-b0d44554f6c0)

![image](https://github.com/user-attachments/assets/a069a99a-6e40-4748-bae5-814295cffb45)

![image](https://github.com/user-attachments/assets/52b4a7f1-cc6a-4193-8e50-783f905431c7)

![image](https://github.com/user-attachments/assets/85ab955d-00a5-4031-8fef-fbc7f141118d)


# Clock tree routing

![image](https://github.com/user-attachments/assets/3607b07e-2070-440f-bf71-76bdbbd20058)

![image](https://github.com/user-attachments/assets/17482635-52c2-4499-849f-d2beb99ec2f2)

![image](https://github.com/user-attachments/assets/d637280b-7ee7-47c7-b9bc-2c7b43e92000)


![image](https://github.com/user-attachments/assets/3ef991a2-7a0e-433d-b4d8-a22e45cfa89b)

![image](https://github.com/user-attachments/assets/1fa24129-b10d-47c4-bfc7-eccd6469060b)

# Crosstalk and Clock net shielding

![image](https://github.com/user-attachments/assets/5e71f7cf-a138-4b42-8f74-9e885f5a37ee)

![image](https://github.com/user-attachments/assets/8485bcda-6164-4b58-b7da-414fa71d4a0c)

# Setup and Hold timing analysis

![image](https://github.com/user-attachments/assets/e7e2a7b3-24ee-4bfd-8ab2-77034e1e4266)

![image](https://github.com/user-attachments/assets/c31f0805-bde6-4e44-80e1-2a98d4b6b7d6)

![image](https://github.com/user-attachments/assets/d51b6bdb-0952-45ca-b094-1bdb723ea9ca)

![image](https://github.com/user-attachments/assets/5e37f635-fed4-4ff2-b2e3-17f6ac643bb9)

# Day 4 Lab

# Lab steps to convert grid info to track info

![image](https://github.com/user-attachments/assets/3204cebd-08fa-4df7-87fd-25d488c36b9f)


![image](https://github.com/user-attachments/assets/37eb9689-c599-429d-a7dc-79919b17c208)

![image](https://github.com/user-attachments/assets/f8b3ca5c-db3a-466e-8ad9-bcdcea29c5d7)

![image](https://github.com/user-attachments/assets/7b770fba-f038-4992-b53d-d8093ad05917)

# Lab steps to convert magic layout to std cell LEF
![image](https://github.com/user-attachments/assets/d5f5f21b-4abc-41f6-ae9e-c0ce5c80eb30)

![image](https://github.com/user-attachments/assets/0930bbcb-1a92-410c-8eb6-b0120598fddb)

![image](https://github.com/user-attachments/assets/9871511c-aa32-4b71-b6ce-94de178b692d)

![image](https://github.com/user-attachments/assets/5b10938a-acdc-4b09-b27e-55d3d2e7fd80)

![image](https://github.com/user-attachments/assets/53332981-2982-4a76-b328-5f94aff24f41)

# sky130_vsdinv.lef

![image](https://github.com/user-attachments/assets/dd5ea8c8-ee15-4bda-8aa2-1ec294b3f75e)

# Introduction to timing libs and steps to include new cell in synthesis

![image](https://github.com/user-attachments/assets/f189a358-93c3-405c-956a-d59867769ba4)

![image](https://github.com/user-attachments/assets/45994b4e-79ab-4bf7-85b3-c7c1addfaea1)

# sky_130_fd_sc_hd_typical.lib

![image](https://github.com/user-attachments/assets/94aeb299-f4b4-4d54-9a97-d8ee68394a6d)

# sky_130_fd_sc_hd_slow.lib

![image](https://github.com/user-attachments/assets/8af1630b-52b2-4660-8b44-6a972abc6459)

# sky_130_fd_sc_hd_fast.lib

![image](https://github.com/user-attachments/assets/6d4c8ca6-c849-45d4-a0c2-8a6358aa94c4)


![image](https://github.com/user-attachments/assets/997fdc16-ae9f-4cb5-9479-52b07868de94)

![image](https://github.com/user-attachments/assets/55135d9d-079d-4129-aac5-1c931e96cebc)


# config.tcl file

![image](https://github.com/user-attachments/assets/4ce0a43b-a951-4109-8ae2-54512b7ce08e)

# Openlane Execution

 ./flow.tcl -interactive

 package require openlane 0.9

 prep -design picorv32a -tag 17-02_06-53 -overwrite 

set lefs [glob $::env(DESIGN_DIR)/src/*.lef]

add_lefs -src $lefs

![image](https://github.com/user-attachments/assets/960eddd5-a668-4fe8-ade3-277879fefde5)


![image](https://github.com/user-attachments/assets/e7369078-2335-4c13-badc-d1ed2d4e149d)

# run_synthesis

![image](https://github.com/user-attachments/assets/b99f45e5-1367-401d-90f3-c341cbb7eecd)

# README.md file

![image](https://github.com/user-attachments/assets/6d8d7792-1439-4064-b2e9-b59848547ce2)

# To fix slack 

![image](https://github.com/user-attachments/assets/f7047d22-7af9-4011-b065-cf6fe1f51a6d)

 * run_synthesis

 * init_ floorplan 
 
 * pace_io
 
 * tap_decap_or

![image](https://github.com/user-attachments/assets/b4ff39dd-372e-4c1b-a867-cc70800b4a81)


![image](https://github.com/user-attachments/assets/be8e623f-750f-44f6-b240-cead719fdd13)

![image](https://github.com/user-attachments/assets/fe659c91-f98d-41fc-926d-24a6ae1cc60e)

![image](https://github.com/user-attachments/assets/6b4e9bfa-c8aa-4754-9f54-d18788520895)

# Post Synthesis timing analysis with openSTA tool

Commands:

#Change directory to openlane

cd Desktop/work/tools/openlane_working_dir/openlane

#Command to invoke OpenSTA tool with script

sta pre_sta.conf

# invole sta tool by using "sta" command

![image](https://github.com/user-attachments/assets/7671521c-7d5b-4935-aa7a-2f2bb303061d)

# prep_sta.conf file 

![image](https://github.com/user-attachments/assets/6dd401e9-be16-4b47-b8dc-e92003ae6f44)

# picorv32a.sdc file 

![image](https://github.com/user-attachments/assets/2c253062-6573-4899-bcbc-de28deacd638)

# Make timing ECO fixes to remove all violations

![image](https://github.com/user-attachments/assets/fb4a61fa-847c-4ef7-b008-9ff3c8854f0b)

![image](https://github.com/user-attachments/assets/d545221a-c535-4103-ba7a-7260e96f5d7a)

![image](https://github.com/user-attachments/assets/b6f82549-bd3a-4611-b2af-5b3613177fc4)

# cell getting Replaced

![image](https://github.com/user-attachments/assets/e54aa4ac-40a5-4cfa-9160-d67c7c4a7c96)

# Before & After Slack

![image](https://github.com/user-attachments/assets/80fa4fcc-2abb-44c8-b331-49f9a6b6f95c)


![image](https://github.com/user-attachments/assets/a871efd0-776b-4f80-a83a-f5eadd172f85)

It is reduced from -5.39 to -5.33

# Replace the old netlist with the new netlist generated after timing ECO fix and implement the floorplan, placement and cts

![image](https://github.com/user-attachments/assets/a31cd4f6-2668-4fb1-b6ad-4cb8190947e9)

# Verified that the netlist is overwritten

![image](https://github.com/user-attachments/assets/6bc257f6-08bd-4f49-92a7-588c1a373758)

# Load the design and run necessary stages

![image](https://github.com/user-attachments/assets/a7b80956-64b9-4a61-867a-36e942dc7a29)


* run_synthesis

  ![image](https://github.com/user-attachments/assets/1cf5e974-9321-4f9b-b94f-4e8a15f99db2)

*init_floorplan

![image](https://github.com/user-attachments/assets/ffedc28e-e7aa-42bc-9109-5fdb258ab29f)

* place_io

![image](https://github.com/user-attachments/assets/dae953d2-05cc-4004-810f-e04729dc0d26)

* tap_decap_or

![image](https://github.com/user-attachments/assets/8f7590bc-d66f-4463-a51f-8dbf0a4234cc)

* run_placement
  

# CTS

run_cts

![image](https://github.com/user-attachments/assets/4ecd354b-e6e5-479c-a71b-63cdcaef7aa1)

# cts.tcl file

![image](https://github.com/user-attachments/assets/73bdccf8-3bfc-498b-97cf-2dabfc9090fc)


![image](https://github.com/user-attachments/assets/59c17bcb-a508-475c-b4f3-3c2aa566bc24)

#  or_cts.tcl file 

![image](https://github.com/user-attachments/assets/40ea8d8e-b945-40d7-b94b-4f7c355c3a62)

![image](https://github.com/user-attachments/assets/a26c0468-d8c5-4f47-92e5-4cb62fe4702d)

# sky130_fd_sc_hd__typical.lib file

![image](https://github.com/user-attachments/assets/a2dbd35e-5ac6-4b37-97cf-1321df931dc5)


# Post-CTS OpenROAD timing analysis

# openroad 

![image](https://github.com/user-attachments/assets/9b50d096-0d77-4dd6-a222-f43aa4ef0b04)

![image](https://github.com/user-attachments/assets/cee86f90-fe48-4958-8266-7592b0445073)

![image](https://github.com/user-attachments/assets/c907c781-7aed-440b-9c1a-56a4cc21935e)

![image](https://github.com/user-attachments/assets/2c89a9ed-4ff0-46bb-8890-ba8ee32c442d)

![image](https://github.com/user-attachments/assets/0def4e7d-2abf-4c6a-83f0-14b4b9a65872)

![image](https://github.com/user-attachments/assets/cf68f17b-96f9-4dfa-a1fa-8fff1816ab52)

# Post-CTS OpenROAD timing analysis

![image](https://github.com/user-attachments/assets/a5069124-3e93-4895-896a-1a9bf4ab7642)

![image](https://github.com/user-attachments/assets/158e55d4-faeb-418d-8d7e-2d121aab9316)



# Day 5 :Final steps for RTL2GDS using tritonRoute and openSTA

# Contents :

* Routing and design rule check (DRC)

* Introduction to Maze Routing and Lee's algorithm

* Lee's Algorithm conclusion

* Design Rule Check

* Power Distribution Network and routing

* Lab steps to build power distribution network

* Lab steps from power straps to std cell power

* Basics of global and detail routing and configure TritonRoute

* TritonRoute Features

* TritonRoute feature 1 - Honors pre-processed route guides

* TritonRoute Feature2 & 3 - Inter-guide connectivity and intra- & inter-layer routing

* TritonRoute method to handle connectivity

* Routing topology algorithm and final files list post-route

# Theory :

![image](https://github.com/user-attachments/assets/eeb4a2ce-27f6-4f36-9338-6aef1440805d)

![image](https://github.com/user-attachments/assets/d37a6214-5209-4dc8-b626-f053985b2fa0)

![image](https://github.com/user-attachments/assets/1e9b5481-c63d-40e6-b665-10d9a07907c7)

# Design rule check (DRC)

![image](https://github.com/user-attachments/assets/5095ce67-774a-4022-a81a-ae4e25c98b56)

![image](https://github.com/user-attachments/assets/df7e8e76-7857-4694-b327-ba41aad6123f)

![image](https://github.com/user-attachments/assets/85980b02-fc93-4722-b44b-375698f34118)

![image](https://github.com/user-attachments/assets/869629f7-99b9-4e5f-a777-2f121e34ba81)

# Routing

![image](https://github.com/user-attachments/assets/5d74f127-3a77-4adb-beb3-b38db4d8fb86)

![image](https://github.com/user-attachments/assets/c2275484-ce26-4c87-bfcb-e7a22bdd43d5)

![image](https://github.com/user-attachments/assets/8975e411-375e-489f-b74d-ad15f40035cf)

![image](https://github.com/user-attachments/assets/f3320f9f-8947-4374-b6d4-4562874ed973)

![image](https://github.com/user-attachments/assets/bf26306e-3978-4115-983a-fdc00a0eb654)

# Routing Topology Algorithm

![image](https://github.com/user-attachments/assets/0f9f670e-28e9-4fa0-9e59-6ae83442d54f)

# Day 5 LAB :

# Perform generation of Power Distribution Network (PDN)

gen_pdn

![image](https://github.com/user-attachments/assets/8db660e4-bf04-404f-b221-88dba83c09ab)


![image](https://github.com/user-attachments/assets/c51cba24-56f6-4bfe-9946-cf492b734a02)


![image](https://github.com/user-attachments/assets/ee69cee8-13b0-4a3e-8bf4-8b90a72af1dc)


![image](https://github.com/user-attachments/assets/9b98f757-fa68-465d-9fce-0d328d8cf5bd)


![image](https://github.com/user-attachments/assets/fb52dd3f-66f1-476b-9c82-0f7e1528731d)


![image](https://github.com/user-attachments/assets/7c29b2d4-a23c-456f-9324-c8417d8b23a9)

![image](https://github.com/user-attachments/assets/c0f07989-983b-4957-a73e-eaf502344cf8)

# Perform detailed routing using TritonRoute and explore the routed layout

run_routing

Load routed def in magic

![image](https://github.com/user-attachments/assets/bb47803f-8e99-4d6b-9b63-504bf6148be0)

![image](https://github.com/user-attachments/assets/90275467-90a7-4876-bd10-d749a6354346)

![image](https://github.com/user-attachments/assets/0600ffb2-7b16-40b4-b166-e22993f3abe0)

fastroute.guide file

![image](https://github.com/user-attachments/assets/2f5818b6-3ce9-4db7-b07f-f204dab6234e)

# Post-Route parasitic extraction

Parasitics command is already run and spef is extracted

In runs folder, where routing outputs are dumped and we'll find the SPEF there.

# Acknowledgements

Kunal Ghosh, Co-founder, VSD Corp. Pvt. Ltd.
