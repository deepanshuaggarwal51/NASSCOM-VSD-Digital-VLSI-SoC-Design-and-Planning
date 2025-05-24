<span style="color: rgb(255,0,0) ; font-weight: 'bold'">

# NASSCOM-VSD Digital VLSI SoC Design and Planning Workshop

</span>

This repository provides the documentation that is prepared while doing the workshop theory lectures and the labs. If you like this, please fork this repo and do your desired modifications. _Enjoy the reading!_

## Table of contents

<span style="color: rgb(150,0,255)">

I. [Inception of open-source EDA, openLANE, Sky130 PDK]()

1. [How to talk to computers?]()
    - [Introduction to QFN-48 package, chip, pads, core, die and IPs]()
    - [Introduction to RISC-V]()
    - [From software applications to hardware]()
2. [SoC design and openLANE]()
    - [Introduction to all components of open-source digital ASIC design]()
    - [Simplified RTL2GDS flow]()
    - [Introduction to openLANE and Strive chipsets]()
    - [Introduction to openLANE detailed ASIC design flow]()
3. [Get familiar to open-source EDA tools]()
    - [OpenLANE directory structure in detail](#openlane-directory-structure-in-detail)
    - [Design preparation step](#design-preparation-step)
    - [Review files after design prep and run synthesis](#review-files-after-design-prep-and-run-synthesis)
    - [OpenLANE project git link description](#openlane-project-git-link-description)
    - [Steps to characterize synthesis results](#steps-to-characterize-synthesis-results)

II. [Good floorplan vs bad floorplan and introduction to library cells](#ii-good-floorplan-vs-bad-floorplan-and-introduction-to-library-cells)

1. [Chip floor planning considerations](#1-chip-floor-planning-considerations)
    - [Utilization factor and aspect ratio]()
    - [Concept of pre-placed cells]()
    - [De-coupling capacitors]()
    - [Power planning]()
    - [Pin placement and logical cell placement blockage](#pin-placement-and-logical-cell-placement-blockage)
    - [Steps to run floorplan using openLANE](#steps-to-run-floorplan-using-openlane)
    - [Review floorplan files and steps to view floorplan](#review-floorplan-files-and-steps-to-view-floorplan)
2. [Library building and placement](#2-library-building-and-placement)
    - [Netlist binding and initial place design](#netlist-binding-and-initial-place-design)
    - [Optimize placement using estimated wire-length and capacitance](#optimize-placement-using-estimated-wire-length-and-capacitance)
    - [Final placement optimization](#final-placement-optimization)
    - [Need for libraries and characterization]()
    - [Congestion aware palcement using RePlAce](#congestion-aware-placement-using-replace)
3. [Cell design and characterization flow](#3-cell-design-and-characterization-flow)
    - [Inputs for cell design flow](#inputs-for-cell-design-flow)
    - [Circuit design steps](#circuit-design-steps)
    - [Layout design steps](#layout-design-steps)
    - [Typical Characterization flow](#typical-characterization-flow)
4. [General time characterization parameters](#4-general-time-characterization-parameters)
    - [Timing threshold definitions](#timing-threshold-definitions)
    - [Propagation delay and transition time](#propagation-delay-and-transition-time)

[III. Design library cell using MAGIC layout and ngspice characterization](#iii-design-library-cell-using-magic-layout-and-ngspice-characterization)
1. [Labs for CMOS inverter and ngspice simulations](#1-labs-for-cmos-inverter-and-ngspice-simulations)
    - [IO placer revision](#io-placer-revision)
    - [SPICE deck creation for CMOS inverter](#spice-deck-creation-for-cmos-inverter)
    - [SPICE simulation lab for CMOS inverter](#spice-simulation-labs-for-cmos-inverter)
    - [Switching threshold voltage $V_m$](#switching-threshold-voltage-)
    - [Static and dynamic simulation of CMOS inverter](#static-and-dynamic-simulation-of-cmos-inverter)
    - [Lab steps to git clone vsdstdcelldesign](#lab-steps-to-git-clone-vstcelldesign)
2. [Inception of layout and CMOS fabriaction process](#2-inception-of-layout-and-cmos-fabrication-process)
    - [Create active regions]()
    - [Formation of N-well and P-well]()
    - [Formation of gate terminal]()
    - [Lightly doped drain (LDD) formation]()
    - [Source and drian formation]()
    - [Higher level metal information]()
    - [Lab introduction to sky130 baisc layers layout and LEF using inverter]()
    - [Lab steps to create std cell layout and extract spice netlist]()

3. [Sky130 tech file labs](#3-sky130-tech-file-labs)

[IV. Pre-layout timing analysis and importance of good clock tree](#iv-pre-layout-timing-analysis-and-importance-of-good-clock-tree)
1. [Timing modelling using delay tables](#1-timing-modelling-using-delay-tables)
    - [Lab steps to convert grid info to track info](#lab-steps-to-convert-grid-info-to-track-info)
    - [Lab steps to convert magic layout to std cell LEF](#lab-steps-to-convert-magic-layout-to-std-cell-lef)
    - [Introduction to timing libs and steps to include new cell in synthesis](#introduction-to-timing-libs-and-steps-to-include-new-cell-in-synthesis)
2. [Timing analysis using ideal clocks using openSTA](#2-timing-analysis-using-ideal-clocks-using-opensta)
3. [Clock tree synthesis tritonCTS and signal integrity](#3-clock-tree-synthesis-tritoncts-and-signal-integrity)
4. [Timing analysis with real clocks using openSTA](#4-timing-analysis-with-real-clocks-using-opensta)

[V. Final steps for RTL2GDS using tritonRoute and openLANE](#v-final-steps-for-rtl2gds-using-tritonroute-and-openlane)
1. [Routing and design rule check (DRC)](#1-routing-and-design-rule-check-drc)
2. [Power distribution network (PDN) and routing](#2-power-distribution-network-pdn-and-routing)
3. [TritonRoute features](#3-tritonroute-features)

[Acknowledgements](#acknowledgements)

</span>

### I. Inception of open-source EDA, OpenLANE and sky130 PDK
#### 1. How to talk to computers
##### Introduction to QFN-48 package, chip, pads, core, die and IPs
**Arduino Board**: This is an arduino microcontroller board. The encircled area shows the chip(microprocessor) which is interfaced with other components of the board. The designing of this chip from abstract level all the way down to the fabrication is done by RTL to GDSll flow. Arduino consists of both a physical programmable circuit board (often referred to as a microcontroller) and a piece of software, or IDE (Integrated Development Environment) that runs on the computer, used to write and upload computer code to the physical board. The various chip components are

- **Pads**: Through which we can send the signal inside the chip.
- **Core**: Place where all the logic gates are placed.
- **Die**: It is the size of the entire chip. In particular, if $A$ be the area of the die, then $fA$ will be the area of the core where $0 < f <= 1$.

For example, **RISC-V SoC**: It consist of SRAM,SOC,ADC,DAC,SPI these all are called foundary IP's. All devices depends upon foundary where all chips are fabricated using deposition and lithography techniques and so on.

##### Introduction to RISC-V
##### From software applications to hardware

#### 2. SoC design and openLANE
##### Introduction to all components of open-source digital ASIC design
To design Digital ASIC, few tools or things which are required from the day one. These are

_RTL Design EDA tools PDK data_:  
What is RTL design? In digital circuit design, register-transfer level (RTL) is a design abstraction which models a synchronous digital circuit in terms of the flow of digital signals (data) between hardware registers, and the logical operations performed on those signals. For these designs many open source websites are available such as [librecores.org](https://librecores.org), [opencores.org](https://opencores.org), [github.com](https://github.com) etc.

What are the EDA tools? The Electronic Design Automation (EDA) tools are used to design and verify integrated circuits (ICs), printed circuit boards (PCBs), and electronic systems, in general. Many open source tools are available like Qflow, OpenROAD, OpenLANE, etc.

What is PDK Data? PDK is an abbreviation that stands for process design kit. It is an interface between fabrication and design. The PDK data is actually a collection of data files like
process design rules: DRC, LVS, REX Digital standerd cell libraries i/o libraries etc. These files are used to model a fabrication process for the EDA tools used to design an ICs. For example, in 2020, Google released the open source PDK for FOSS 130nm production with the skywater technology.

The cutting-edge sized for process are 7nm and lesser. In many applications, such an advance node is not required, and also the cost of an advanced node is also high as compared to 130nm processors. This 130nm processors are also fast processor. For example, intel: P4EE @3.46 GHz(Q4'o4), sky130_OSU (single cycle RV32i CPU) pipeline version can achieve more than 1 GHz clock.

##### Simplified RTL2GDS flow
##### Introduction to openLANE and STRIVE chipsets
OpenLane is an automated RTL to GDSII flow based on several components, including OpenROAD, Yosys, Magic, Netgen, CVC, SPEF-Extractor, KLayout and a number of custom scripts for design exploration and optimisation. It also provides a number of custom scripts for design exploration and optimisation. OpenLane abstracts the underlying open source utilities and allows users to configure all their behaviour with just a single configuration file. For more details, read the openLANE documentation at [openLANE](https://openlane.readthedocs.io/).

The main goal of the openLANE flow is to produce a clean GDSII with no human intervention (no-human-in-the-loop) with zero violations, such as> zero LVS (Layout vs schematic) violations, zero DRC (design rule check) violations and zero timing violations.

OpenLANE is tuned for skyWater130nm open PDK, but other PDKs can also be used (see custom PDKs with [openLANE](https://openlane.readthedocs.io/en/latest/usage/custom_pdk_builds.html)). It can be used to harden Macros and chips. There are basically two modes of operation
> Autonomous: As the name suggests, it automates the flow by automatically executing different stages in a predefined order and results in the final GDS file.

> Interactive: In the interactive mode, it runs iteratively, and we need to perform each stage in a particular order. We can also change any environmental variable on the fly and re-run a particular stage.

##### Introduction to openLANE detailed ASIC design flow

![alt text](pics/flow.webp)
Ref: This image is downloaded from [openLANE docs](https://openlane.readthedocs.io/en/latest/flow_overview.html)

#### 3. Get familiar to open-source EDA tools
##### OpenLANE directory structure in detail
The image below shows the openlane directory structure inside the openlane working directory:

![alt text](<pics/Screenshot from 2025-05-23 11-46-39.png>)

The two folders `drc_tests` and `vsdstdcelldesign` are added later by cloning the repositories and are explained in further sections.

Here we are working in the Sky130_fd_sc_hd PDK variant. where "sky130" is the process name or node name. "fd" stands for a foundry name (skywater foundry)."sc" means standard cell library files, and the last one "hd" stands for high density.

For example, a particular variant `sky130_fd_sc_hd` contains many technology files like verilog, spice, techlef, meglef, mag, gds, cdl, lib, lef, etc. as shown below (level-1 contents).

![alt text](<pics/Screenshot from 2025-05-23 12-03-32.png>)

##### Design preparation step
After entering into the openLANE working directory, we first type `docker` and then invoke the openLANE tool by executing the command `./flow.tcl -interactive` for interactive openLANE flow. Where the tag `-interactive` is used for the interactive mode, where we will do a step-by-step process. (see figure below)

![alt text](<pics/Screenshot from 2025-05-23 12-17-07.png>)

Now, we need to first prepare the configuration files and set the environment variables for the design which we are interested in. Before doing that, if we open the designs folder in openLane, there are many designs that are already built. (see figure below)

![alt text](<pics/Screenshot from 2025-05-23 12-25-33.png>)

For example, we open up the `picorv32a` design. In this design, we can see that there are many files. All of them are required to build the .gds file for picorv32a from its verilog file inside the `src`.

![alt text](<pics/Screenshot from 2025-05-23 12-46-05.png>)

The `config.tcl` file contains some details about the design and sets up some environment variables. For example, clock period, clock port, etc. Please note that the latest version of openLANE still has compatibility for `config.tcl`, but they recommend using `config.json`. (see [here](https://openlane.readthedocs.io/en/latest/reference/configuration_files.html))

![alt text](<pics/Screenshot from 2025-05-23 12-37-07.png>)

Now, we go back to the openlane command line tool window and prepare the files for the design flow of `picorv32a`. To do the design preparation for a design, we invoke the command `prep -design picorv32a`.

![alt text](<pics/Screenshot from 2025-05-23 12-50-33.png>)

##### Review files after design prep and run synthesis
After completing the preparation step in the previous subsection, there is a new directory `runs` is created inside the design folder.

![alt text](<pics/Screenshot from 2025-05-23 12-47-56.png>)

Within the `runs` directory, a directory with a date (in DD-MM format) is created. This detail is also printed during the preparation step run (see previous figure). We open up the recent run directory;

![alt text](<pics/Screenshot from 2025-05-23 12-53-35.png>)

Here, the `config.tcl` file is created to inform the user about what all the default parameters are taken or used in the design preparation step for picorv32a.  
The directory `reports` will contain the reports as we run the various stages (presently empty), and the `results` directory will contain the results, which will be explained in later sections.  
In the `tmp` directory, `merged.lef` file is available, which was created in preparation time. If we open this merged.lef file, we get all the wire or layer level and cell level information.

![alt text](<pics/Screenshot from 2025-05-23 13-00-43.png>)

Now, we come back to the openlane and run the very first step, which is synthesis. To do that, we will invoke the command `run_synthesis`, and it will take some time to run the synthesis, and finally, the synthesis will be completed.

![alt text](<pics/Screenshot from 2025-05-23 13-09-19.png>)

##### OpenLANE project git link description
Here, I am providing the two GitHub repo links for the openLANE:

> OpenLANE (OpenROAD project) -> [OpenLane 1](https://github.com/The-OpenROAD-Project/OpenLane)

> OpenLANE (efabless) -> [OpenLANE 2](https://github.com/efabless/openlane2)
> 
##### Steps to characterize synthesis results
IN this subsection, we characterize the synthesis result of the previous subsection and do the _d-flops ratio_ calculation.

$$ \text{d-flop ratio} = \frac{\text{Total \# of d-flops}}{\text{Total \# of cells}}$$

To do this, we first go into the `reports/synthesis` inside the runs directory of the designs and open up the `1-yosys_4.stat.rpt`;

![alt text](<pics/Screenshot from 2025-05-23 13-22-44.png>)

Therefore, the d-flop ratio is $ \text{d-flop ratio} = \frac{1613}{14876} \approx 0.11 $, which implies that there are $11\%$ d-flops.

### II. Good floorplan vs bad floorplan and introduction to library cells
#### 1. Chip floor planning considerations
##### Utilization factor and aspect ratio
##### Concept of pre-placed cells
##### De-coupling capacitors
##### Power planning
##### Pin Placement and logical cell placement blockage
##### Steps to run floorplan using openLANE
Before running the floorplan, we look at the various environment variables available in the configuration file of openLANE;

![floorplan_config](<pics/Screenshot from 2025-05-23 15-20-57.png>)

This large number of variables controls the entire flow run and can be changed on-the-fly as per the requirements.

The defaults for the floorplan stage are given in the `floorplan.tcl` as shown in the figure.

![floorplan_tcl](<pics/Screenshot from 2025-05-23 15-48-08.png>)

In the OpenLANE flow, the order of priority is 
> system default (floorplanning.tcl) -> config.tcl -> PDK variant.tcl (sky130A_sky130_fd_sc_hd_congig.tcl).

Now we run the floorplan by invoking `run_floorplan`;

![alt text](<pics/Screenshot from 2025-05-23 19-52-53.png>)
![alt text](<pics/Screenshot from 2025-05-23 19-53-47.png>)

The floorplan is successfully performed. Now, we review the floorplan files generated by the floorplan stage.

##### Review floorplan files and steps to view floorplan
The different files that got generated in the floorplan stage will be placed in the `results/floorplan`, where the directory has now got the structure as;

![alt text](<pics/Screenshot from 2025-05-23 20-00-39.png>)

Now, we take a look at the `picorv32a.floorplan.def` (design exchange format) file. We can see all information about the die area `DIEAREA (llx lly) (urx ury)`, unit distance in microns. We can divide 660685 and 671405 by thousand to convert from the database units to microns.

![alt text](<pics/Screenshot from 2025-05-23 20-09-44.png>)

In order to visualise the layout (floorplan), we use the `magic` tool with the `.lef` and `.def` files as

`magic -T <path_to_openlane_dir>/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def`.

One can press `s` to select the entire floorplan and press `v` to shift  it to the centre of the magic window.

![alt text](<pics/Screenshot from 2025-05-23 20-18-24.png>)

We can press `z` to zoom in, `shift + z` to zoom out and `ctrl + z` to zoom into a selected region; We observe that the input and output pins are placed equidistant as was expected (see `config.tcl` or `README.md` for more information!)

![alt text](<pics/Screenshot from 2025-05-23 20-23-32.png>)

and to select a particular cell, we hover the mouse over that cell and press `s`.

Alongside the magic GUI window, a magic command line console window also pops up, and whatever we are doing in the layout is dynamically being traced into the console. To get the information about any cell, such as the metal layer information, we just hover the mouse button over that cell, press `s` on the keyboard, and then we press the colon or semicolon key and type `what` and enter, then the console shows the information about that component as,

![alt text](<pics/Screenshot from 2025-05-23 20-35-56.png>)

It shows that the horizontal pin is attached to the metal3. Also, just after the horizontal pins, the decap cells are arranged.

#### 2. Library building and placement
In this section, we move on to the next stage after the floorplan, that is, the placement of the cells. Before delving into the cell placement, we discuss some crucial concepts related to placement.

##### Netlist binding and initial place design
**Bind netlist with physical cells**:  
Let us consider a netlist of gates. The representation of logic gates differs from one gate to another, such as a _NOT_ gate is represented as triangularly shaped. In reality, the logic gates have got physical dimensions with finite width and height. Similarly, the flip-flops are also like square boxes. Now, for every component of the netlist, we assign a particular shape with particular dimensions such that they have a finite width and height and a proper shape.

![alt text](<pics/Screenshot from 2025-05-23 20-55-35.png>)

Now we remove all the wires and place all the logic gates, flipflops, and blocks on a shelf in the same way as the books are placed in a library, similarly, the collection of the cells is called a Library.

Precisely, similar to a book library where we find all kinds of books, in the cell library, we find all kinds of gates, flip-flops, etc. In addition to it, the library also has the timing information, such as the time delay of the gates. A library can be divided into two sublibraries,
- One library consists of shape and size, and
- Another library might consist only of the delay information.

A library has various flavours of each and every cell, like a same cell can have different sizes in different shelves. The bigger the size of a cell, the lower the resistance path, so it will work faster and will have less delay. We can pick up from these what we want based on the timing condition and available space on the floorplan.

![alt text](<pics/Screenshot from 2025-05-23 21-06-41.png>)

**Placement**: The next step is to take those specific forms and sizes and put them on the floorplan after we have given each gate the appropriate size and shape. We have a specific netlist, a floorplan with input and output ports, and each component of this netlist has a specific size. Thus, we get the logic gates' physical view. The netlist must then be included in the floorplan. We must create the physical view gates on the floorplan using the connection data from the netlist.

![alt text](<pics/Screenshot from 2025-05-23 21-07-56.png>)

The pre-placed cells from the earlier slides are now visible in the layout. The placement will ensure that the pre-placed cell positions remain unaltered. Additionally, it will ensure that no cell is placed on top of the pre-placed cells. In order to maintain timing and minimise latency, we must arrange the physical view of the netlist on the floorplan so that logical connectivity is maintained and that specific circuit interacts with its input and output ports. The remaining components from the netlist will be arranged on the floorplan here initially. Every piece has been positioned so that it is close to both its input and output pins.

However, FF1 of Stage 4 and Din4 are still far away from the others. By adjusting the positioning, we can fix this problem.

![alt text](<Screenshot from 2025-05-23 21-12-31.png>)

##### Optimize placement using estimated wire-length and capacitance
**Optimize Plecement**: Here, we handle the distance issue in optimal location. Let's use FF1 to Din2 as an example. A wire must go from Din2 to FF1 (yellow), however we will attempt to estimate the capacitances before arranging the design or wiring. In the event that we look at the capacitance from Din2 to yellow FF1, it is significant due to the length of the wire; consequently, the result will likewise be enormous. Due to the large distance, it will be challenging for FF1 to receive the signal in its original form if we transmit it from Din2. In order to maintain the integrity of the signal, we can use various interim measures. This successfully drives the input from Din2 to the FF1. These transitional actions are referred to as Repeaters, which are essentially buffers that recondition the original signal to create a new signal which is just a replication of the original signal and transmits it forward. This process is repeated until we reach the cell where we wish to send the input, ensuring signal integrity. Therefore, the signal integrity issue can be solved by employing repeaters, however as more repeaters are utilised, more space will be used up in the specific design.

##### Final placement optimization
For example, for the logical sequence of blue FF1 to FF2, the distance between logic gate 2 and FF2 is significant and the requirement of a repeater becomes inevitable, as shown below

![alt text](<pics/Screenshot from 2025-05-23 21-20-35.png>)

Similarly, for the green logical sequence from Din4 -> FF1 -> gate-1 -> gate-2 -> FF2 -> Dout4; we need to place two buffers; 

![alt text](<pics/Screenshot from 2025-05-23 21-22-38.png>)

Each IC design process must go through a number of phases:
- Logic Synthesis is the first stage to complete. For example, if we have functionality that is programmed as an RTL, we must first turn it into legal hardware. This process is known as Logic Synthesis. The arrangement of gates that will replicate the original functionality that has been defined using an RTL is the output of the logic synthesis.
- Floorplaning is the next stage of logic synthesis, where we import the logic synthesis output and determine the die and core sizes.
- Following floorplaning, the next stage is placement, where we take the specific logic cell and arrange it on the chip so that the initial timing is optimal.
- The clock tree synthesis (CTS) stage comes next, where we make sure that the clock reaches every signal simultaneously and that each one has an equal rise and fall.
- Routing is the next stage; it must follow a certain flow that is determined by the flip-flop's characterisation.
- The final phase, static timing analysis (STA), is when we attempt to determine the circuit's maximum attainable frequency, hold time, and setup time. 'GATES or Cells' is a common element all phases share.

##### Congestion-aware placement using RePlAce
At the moment, we are constrained by congestion rather than timing. As a result, we are reducing congestion.

There are two phases to the placement process. Global and detailed. Legalisation does not occur during global placement, but it will occur following detailed placement. The first global placement occurs when we run the placement. The primary goal of global placement is to shorten wires.

We first perform the placement ny invoking `run_placement` as shown;

![alt text](<pics/Screenshot from 2025-05-23 21-45-14.png>)
![alt text](<pics/Screenshot from 2025-05-23 21-45-45.png>)

To visualise the layout after placement, we again invoke the magic command as  
`magic -T <path_to_openlane_dir>/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def`

![alt text](<pics/Screenshot from 2025-05-23 21-50-24.png>)

By zooming in using the key `z`, we see the placed cells during the placement stage;

![alt text](<pics/Screenshot from 2025-05-23 21-52-47.png>)

#### 3. Cell design and characterization flow
##### Inputs for cell design flow
Gates, flip-flops and buffers are referred to as "Standard Cells" in Cell Design Flow.  'Library' is the section in which these standard cells are located. Additionally, there are several other cells in the collection that are varied in size but have the same functionality.

![alt text](<pics/Screenshot from 2025-05-13 15-09-48.png>)

A cell design flow is devided into three parts:

![alt text](<pics/Screenshot from 2025-05-13 15-12-29.png>)

1. **Inputs**:  PDKs, DRC and LVS rules, SPICE models, libraries, and user-defined specifications are all necessary _inputs_ for cell design. Design rules and actual values are included in the tech file for DRC and LVS rules. Code can be created from rules. The threshold voltage equation is explained by the SPICE model.

![alt text](<pics/Screenshot from 2025-05-13 15-15-08.png>)
![alt text](<pics/Screenshot from 2025-05-13 15-16-29.png>)

##### Circuit design steps
2. **Design steps**: As shown in the figure below, the design steps further contain three steps, that is, circuit design, layout design and characterization. Within the circuit design, there are two steps,
- the first step is to implement the function itself, and
- the second step is to model the PMOS and NMOS transistor in such a fashion in order to meet the libraray.

![alt text](<pics/Screenshot from 2025-05-13 20-42-17.png>)

##### Layout design steps
The first stage in layout design is to build the function using a combination of PMOS and NMOS transistors. The second step is to extract the PMOS and NMOS network graphs from the implemented design. The next step after obtaining the network graphs is to determine Euler's route. In essence, Euler's path is the one that is only tracked once.

![alt text](<pics/Screenshot from 2025-05-13 20-44-20.png>)

The next step is to draw a stick diagram based on the Euler's path. This stick diagram is derived out of the circuit diagram.

![alt text](<pics/Screenshot from 2025-05-13 20-44-39_1.png>)

In order to obtain the correct rule we previously discussed, the next step is to transform this stick diagram into a standard layout. Cell width, cell length, and all other details, such as drain current and pin placements, will be available once we get the precise configuration.

![alt text](<pics/Screenshot from 2025-05-13 20-48-19_1.png>)

The next and last stage is to separate the parasitics of that particular layout and characterise it in terms of time. This would be the final phase. In other words, the result of the layout design will be GDSll before that happens. After the extracted spice netlist has been obtained, we will proceed to characterise it. Information on time, noise, and power may be obtained with the use of characterisation.

3. **Ouputs**: The output what one gets from the circuit design is CDL (circuit description language) file, GDSII, LEF, extracted spice netlist (.cir).

![alt text](<pics/Screenshot from 2025-05-13 20-49-28.png>)

##### Typical Characterization flow
In the typical characterization flow, the various steps are as follows:
1. Read in the model,
2. Read the extracted spice netlist,
3. Define or recognize the behaviour of the buffer,
4. Read the subcircuits of the inverter,
5. Need to attach the necessary power supplies,
6. Need to apply the stimulus,
7. Need to provide the necessary output capacitance,
8. Need to provide necessary simulation command. For example, if we are doing transient analysis so we need to give `.tran` command and if we are doing DC simulation then we give `.dc` command.

![alt text](<pics/Screenshot from 2025-05-13 20-57-40.png>)

#### 4. General time characterization parameters
##### Timing threshold definitions
In the figure below, the various timing threshold parameters are tabulated. The first four terms having pattern `slew_*_thr` are the slew thresholds. Mathematically, the slew is defined as the time-derivative of the voltage (dV/dt) and having geometric interpretaion as the slope of the $V-t$ graph. In both rising (red) and falling (blue) transitions, the slew is measured between two points (low and high points). These low and high points in both the transtions are reffered to as the threshold points.

![alt text](<pics/Screenshot from 2025-05-13 21-06-27.png>)

The lower points are generally at 20%-30% of the maximum value, while the higher points are 20%-30% lesser of the maximum value.

The lower four parameters having pattern `in_*_thr` and `out_*_thr` are the timinng thresholds for input and output transtions respectively. Theses thresholds are almost 50% of the maximum value.

##### Propagation delay and transition time
The propagation delay is defined as the time difference between output threshold and the input threshold.

![alt text](<pics/Screenshot from 2025-05-14 13-19-09.png>)

The transition time is defined as the difference between slew thresholds;

![alt text](<pics/Screenshot from 2025-05-14 13-24-26.png>)
![alt text](<pics/Screenshot from 2025-05-14 13-24-44.png>)

The figure below shows a typical measurement for various timing threshold parameters.

![alt text](<pics/Screenshot from 2025-05-14 13-26-19.png>)

### III. Design library cell using MAGIC layout and ngspice characterization
#### 1. Labs for CMOS inverter and ngspice simulations
##### IO placer revision


##### SPICE deck creation for CMOS inverter
In the previous sections, we read the voltage transfer characteristics of the CMOS. Here, we do the SPICE simulations and derive those characteristics on the real-time MOSFETs.

With $250-300\,nm$ MOSFETs, we will be doing some SPICE simulations;

**Step-1**: Create a SPICE deck (which is nothing but a connectivity information):
- Connectivity information about a netlist  
- Inputs that has to be provided to the simulation  
- Got the tap points at which we will take the outputs etc.

![spice-deck](<pics/Screenshot from 2025-05-15 12-57-19.png>)

We need to define the connectivity information of substrate PIN as well. The significance of the substrate PIN lies in its ability to tune the threshold voltage of PMOS ans CMOS. HOw do we give the connectivoty information
- Source of PMOS is connected to $V_{\text{dd}}$ abd source of NMOS is connected to $V_{\text{ss}}$.
- The output load capacitor ($C_{\text{load}}$) is obtained from the details of the input capacitances and ther other circuit details.

**Step-2**: Component values:
- The size of the PMOS is represented as $w_{c}/l_{c}$, where $w_{c}$ is the channel width and $l_{c}$ is the channel length. For example, the values in the example used are
$w_{c} = 0.375\,\mu m$ and $l_{c} = 0.25\,\mu m$.
- Similarly, the size of NMOS is written as $w_{c}/l_{c}$.
- The load capacitance ($C_{\text{load}}$) and we assumed a value of $10\,fF$, where $f := \text{femto} = 10^{-15}$ and $F$ stands for its SI unit which is Farad.
- The input gate voltage $V_{\text{in}}$, where we assumed a value of $2.5\,V$ and generally, it is kept as a multiple of channel length ($l_{c}$). For example, for $l_{c} = 0.25\,\mu m$, the input voltage $V_{\text{in}} = 2.5\,V$, which is 10 times of the channel length.
- The next is the Drain volatge ($V_{\text{dd}}$) and we assume it to be also equal to $V_{\text{in}}$, that is, $2.5\,V$.

**Step-3** Identifying the nodes:
The nodes are the points between which there is a component. For example, in the figure below, the drain supply $V_{\text{dd}}$ is connected bwteen the two nodes $v_{dd}$ and o (just above the ground $V_{\text{ss}}$).

We name these nodes as shown in the figure.

##### SPICE simulation labs for CMOS inverter

##### Switching threshold voltage ($V_m$)
In the SPICE simulation (see figure below) for a device where the PMOS channel width $w_{c}$ is $2.5$ times the NMOS channel width, we observed that the waveform passes through the center (near the center) of the graph in contrast to the case where the graph is shifted to left when channel widths were equal $w^{\text{PMOS}}_{c} = w^{\text{NMOS}}_{c}$. Now, It is important to undertand why such a shift is caused due to equal channel widths?

![waveform side-by-side](<pics/Screenshot from 2025-05-15 13-36-08.png>)

In the figure above, we see the two waveforms kept side-by-side, and the observations are the following
- the waveform shape is same which implies that the CMOS inverter device is very robust. Therefore, CMOS logic are widely used for logic designing.
- The parameters that define the _robustness of the CMOS_;
    - Switching threshold ($V_{\text{m}}$): The point at which the device switches. Particularly, it is the point where $V_{\text{in}} = V_{\text{out}}$.
    - Plot a straignt-line $V_{\text{in}} = V_{\text{out}}$ and identify the intersection point of the straight line and the waveform.
    - In the area around the intersection point, the PMOS and NMOS both are in saturation region. It means that both are turned on and then there is a high likelihood of leakage current.
    - Switching thresholds are $V_{m} \sim 0.8 \,V$ and $V_{m} \sim 1.2\,V$.
    - There are the boundary conditions at which $V_{m}$ arrive; $V_{\text{GS}} = V_{\text{DS}}$ and $I_{\text{DSP}} = -I_{\text{DSN}} $.

![alt text](<pics/Screenshot from 2025-05-15 13-40-43.png>)

##### Static and dynamic simulation of CMOS inverter

Dynamic simulation of a CMOS inverter: In this subsection, we attempt to addres a question viz. What is the value of the rise and the fall delay of a CMOS inverter and how does it depend on the threshold voltage ($V_{m}$)?

In the case of a dynamic simulation, the netlist will have more or same things exceot the input voltage, which is now a pulse. Hence, the connectivity inforamtion of the input voltage is encoded as `Vin in 0 0 pulse 0 2.5 0 10ps 10ps 1ns 2ns`, which reads that the input voltage is a pulse which is connected between two nodes in and 0. This pulse has a minimum of $0\,V$, a maximum of $2.5\,V$, it starts at 0, the rise-time of the pulse is $10\,ps$, the fall-time of the pulse is also $10\,ps$, it is having half-cycle period of $1\,ns$ and a full-cycle period of $2\,ns$.

![alt text](<pics/Screenshot from 2025-05-15 15-06-58_1.png>)

The analysis in this case is the _transient_ analysis and to encode this information, we use the command `.trans 10ps 4ns`, where .trans stands for the transient.

To calculate the dealy, it is basically 50-50%. It is basically the difference between a point at which the input crosses $50\%$ of its maximum value to a point at which the output crossed $50\%$ of its maximum value.
- If we do calculate this difference, when the output is rising and the input is falling, it becomes the _rise_ delay, and
- If we calculate it, when the output is falling and the input is rising, it becomes the _fall_ delay. 

![alt text](<pics/Screenshot from 2025-05-15 15-21-25_1.png>)

##### Lab steps to git clone `vstcelldesign`
Within the openLANE directory, clone the github repository by

`git clone https://github.com/nickson-jose/vsdstdcelldesign.git`

#### 2. Inception of layout and CMOS fabrication process
In this section, we discuss the 16-mask fabrication of a Complementary Metal-Oxide-Semiconductor (CMOS).

##### Create active regions
Within the openLANE flow, the verilog files (.v files) with design constrainsts are processed to a final GDSII layout. Now, How can an experimantalist use this file? Essentially, this is the GDSII file with which the fabrication is performed.

Initially as a first-step, we first choose a substrate on which the GDSII layout is fabricated. There are various kind of substrate but the most common is p-type sillicon substrate because it has high resistivity and required doping level.

What are the active regions? -> These are the places where we see the PMOS and NMOS.

How do we create active regions for transistors on this substrate? -> On the p-type substrate, we create small pockets which are referred to as active regions and the PMOS and NMOS transistors are created in these pockets. These pockets will be connected and connections to these pockets will be brought up in higher metal layers.

The experimental fabrication of these pockets/active regions invlove an experimental optical technique called _photolithography_.

To create these pockets, a $\text{SiO}_2$ oxide-layer of almost $\sim 40\,nm$ thickness is deposited on the p-substrate. Then, a silicon nitride $\text{Si}_3\text{N}_4$ (silicon nitride)layer of thickness $\sim 80\,nm$ is deposited over the oxide layer. And, then a photoresist film of $\sim 1\,\mu m$ is placed over the silicon nitride layer.

_Mask_: Any kind of layout in custom design is converted to a mask (protection-layer).

#### 3. Sky130 tech file labs
##### Lab steps to create final SPICE deck using sky130 tech
We use the magic file (`.mag` file) by cloning the [github repo](https://github.com/nickson-jose/vsdstdcelldesign) to visualise the layout of a CMOS inverter. Now, we use the command line console window with the magic tool to extract a file with extension `<fname>.ext` by invoking the command `ext`. Then, we create a SPICE file with extension `<fname>.spice` by invoking the commands
`ext2spice cthresh 0 rthresh 0`, this gives all the parasitic capacitances information as well. Then finally, `ext2pice` as shown in figure below,

![alt text](<pics/Screenshot from 2025-05-24 16-52-59.png>)

Within the directory, we get the `.spice` file. Let us open the SPICE file and look for the various information present in the file;

![alt text](<pics/Screenshot from 2025-05-24 16-59-23.png>)

As explained before, this contains the connectivity information for PMOS (`M1001`) and NMOS (`M1000`) and a conversion scale.
- To obtain the conversion scale, we see the smallest grid size using magic tool.
- We also need to include the lib files for NMOS and PMOS for ngspice simulator to run the DC and transient analysis.
- We also need to add the various power supplies. For eaxmple, `VDD VPWR 0 3.3V` implies that the power suppy VDD is connceted between VPWR and 0 nodes and it has the value $3.3\,V$. In addition to it, the pulse information is written as `Va A VGND PULSE(0V 3.3V 0 0.1ns 0.1ns 2ns 4ns)`;
- We also need to insert a line for the transient analysis; `.tran 1n 20n`. And finally, we add a subsection (to run the analysis)
```
.control
run
.endc
```

![alt text](<pics/Screenshot from 2025-05-24 17-21-27.png>)

##### Lab steps to characterize inverter using sky130 model files
Now, we run the spice simulation using the detailed `.spice` by invoking the command  
`ngspice sky130_inv.spice` and the output contains the various information like temperature at which the analysis is done and the number of data points,

![alt text](<pics/Screenshot from 2025-05-24 17-26-22.png>)

Then, to plot the output vs input, we need to invole a command `plot y vs time a` and we get

![alt text](<pics/Screenshot from 2025-05-24 17-29-40.png>)

We see that there are some spikes at the falling edges and to reduce them, we incresase the value of C3 which is connected between Y and VGND from $0.279f\text{F}$ to $2f\text{F}$; then the plot becomes

![alt text](<pics/Screenshot from 2025-05-24 17-42-17.png>)

Now, we characterize the transient plot by obtaining four parameters as discussed in previous the section, which are
- **Rise transition time**: The time taken by the output waveform to transit from 20% to 80% of its maximum value ($V_{\text{dd}}$).

![alt text](<pics/Screenshot from 2025-05-24 17-55-14.png>)

Therefore, the rise transition time is $ 2.24047\, ns - 2.17993\,ns \approx 0.06054\,ns$.

- **Fall transition time**: The time taken by the output waveform to fall from 80% to 20% of its maximum value ($V_{\text{dd}}$).

![alt text](<pics/Screenshot from 2025-05-24 18-01-52.png>)

Therefore, the fall transition time is $ 4.09336\, ns - 4.05011\,ns \approx 0.04325\,ns$.

- **Propagation delay**: The time difference between 50% of the output has arisen and 50% of the input has fallen;

![alt text](<pics/Screenshot from 2025-05-24 18-10-54.png>)

Therefore, the propagtion delay is $ 2.20726\, ns - 2.15000\,ns \approx 0.05726\,ns$.

- **Cell fall delay**: The time difference between the points when output has fallen to its 50 % and the input has arisen to its 50%;

![alt text](<pics/Screenshot from 2025-05-24 18-14-08.png>)

Therefore, the propagtion delay is $ 4.07544\, ns - 4.05003\,ns \approx 0.02541\,ns$.


##### Lab introduction to magic-tool options and DRC rules
##### Lab introduction to sky130 pdk's and steps to download labs
##### Lab introduction to magic and steps to load sky130 tech-rules
##### Lab exercise to fix poly.9 error in sky130 tech-file

### IV. Pre-layout timing analysis and importance of good clock tree
In this topic, we will learn about the pre-layout timing analysis using various open-source tools and learn about the good clock tree.
#### 1. Timing modelling using delay tables
##### Lab steps to convert grid info to track info
Till now, we have done design setup, floor plan and placement, and how to extract spice file out of a given .mag file and did the characterization. So while doing the characterization, we deviated from the `openLANE` flow and were doing the simulations in `ngspice` simulator and therefore a natural question is to ask where does this all get connected?

`OpenLANE` is actually a placement and routing (PnR) tool and for placement of any cell we do not require the entire .mag file information. So if we open a .mag file:

> magic -T <path to .tech file> <path to .mag file>

All the informations such as the power, ground, port and the logic part is contained in the .mag file and we do need all of these informations for placement of the cell. The only information which we will be requiring is the PR boundary (inner box), the power and the ground ports, the i/o ports. In fact, this is what is contained by the .lef file and the .lef file protects the IP or Macro.

Therefore, we will first extract the .lef file from the .mag file and then we will try to see if the extracted .lef file could be plugged into the picorv32a design flow.
Till now, we were working with the std cell library that came along with the openLANE and were doing the flow steps of picorv32a.
So How the cell which we designed can be plugged into the openLANE flow of picorv32a?

For PnR, we need to follow certain guidlines such as
- the i/o ports must lie on the intersection of the vertical and the horizontal tracks
- the width of the standrad cell should be an odd multiple of the track horizontal pitch
- the height of the standard cell should be an odd multiple of the track vertical picth

Measuring the accuracy of the designed layout so that it can be used with the PnR:
> % cd <openlane_dir>/pdks/sky130A/libs.tech/openlane/sky130_fd_sc_hd/  
% less tracks.info

For example, the line `li1 X 0.23 0.46` tells that for the layer li1, the hozrizontal track (X) attain an x-origin of 0.23 and having a pitch of 0.46. The pitch means the horizontal tracks are placed at the spacing of 0.46.

The tracks are used during the routing stage where the routes can go over the tracks which themselves are the metal traces. In the automated PnR flow, we need to specify where we want the routes to go. This specification is provided by the tracks.

Now, to verify that if the first guideline that forces the i/o ports to lie on the intersection of horizontal and vertical tracks, is being followed, we first press the key `g` within the magic tool in order to acivate the grid. Since the i/o ports lie in the layer li1, we need to use its tracks information from the tracks.info file. Within the magic command-line console window
> help grid (# for seeking the syntax help for grid)  
grid 0.46um 0.34um 0.23um 0.17um

We have converged the grid definition according to the tracks information. Routing of the li-layer can truly happens along the generated larger grid. Now, if we see by zooming in the i/o ports lie on the insersection points of the horizontal and vertical tracks.

##### Lab steps to convert magic layout to std cell LEF

Now, we move on for verifying the second guideline in previous subsection-[IV-1](#lab-steps-to-convert-grid-info-to-track-info) that tells that the width of the std cell should be an odd mutiple of the x-track pitch. For this, we look for the number of boxes that we generated previously within the PR boundary and (we see that there are three boxes enclosed within PR boudary and hence the cell-width  = 3 $\times x_{\text{pitch}}$).

Similarly, for the third guideline, we will look for the height of the std cell and cound the number of grid boxes within the PR boundary.

Now, we select a layer and converted it as the port. Setting a layer as a port decalares pin of the macro.
For the ports definition and how do we do that within the magic tool, please look the documentation at [vsdstdcelldesign](https://github.com/nickson-jose/vsdstdcelldesign).

Now, we extract the .lef file from the .mag file by invoking the steps
- give the cell a custom name
    > save sky130_vsdinv.mag (# verify once after saving it if the file is there or not)
- Now obtaining the .lef file
    > lef write

Open up the .lef file and verify the various changes made to the original .mag file such as the conversion of layers into the ports. Those converted layers actually provide the PIN informations in the generated .lef file and this is crucial for the PnR tool.

Now the .lef file is ready and the next step is to plug this into the picorv32a flow. Before doing that, move the generated files to `src` directory (which contains all the design files into one location).

##### Introduction to timing libs and steps to include new cell in synthesis
*Target*: We want to plug our designed custom cell into the picorv32a openlane flow.

Copy the generated .lef file in the previous section to the `.../openlane/designs/picorv32a/src` directory. Now the first step in the openlane flow is the synthesis and we have to ensure that we need to have a library that has the cell definition for synthesis. In the directory, we open the lib file
> less .../vsdstdcelldesign/libs/sky130_fd_sc_hd_typical.lib

that contains the created cell (sky130_vsdinv) definition. The directory contains the three .lib files slow (ss), fast (ff) and typical (tt) and that are defined for different process corners with different temperature values, different voltage values. We would be requiring the slow, fast and typical libraries for STA analysis.

Now copy all the libs files into the src directory
> cp .../vsdstdcelldesign/libs/sky130_fd_sc_hd_* .../openlane/designs/picorv32a/src

Now, we need to modify the `config.tcl` file
> nano .../picorv32a/config.tcl

Need to provide the reference for the .libs and the .lef file (it is already done) but for any sort of change or more details visit [vsdstdcelldesign](https://github.com/nickson-jose/vsdstdcelldesign).

Invoke the openlane tool by `./flow.tcl - interactive` and run the following steps:
> prep -design picorv32a -tag <dir_name> -overwrite

(The option `-tag` is used if we want to get the resulting files into an existing directory by overwriting the previous contents rather than creating a new one)

Now we need to include our .lef file by doing (Ref. [vsdstdcelldesign](https://github.com/nickson-jose/vsdstdcelldesign))
> set lefs [glob $::env(DESIGN_DIR)/src/*.lef]  
add_lefs -src $lefs

Now we run the synthesis and see whether this maps our custom cell into this picorv32a flow. This mapping will be taken from the `LIBS_SYNTH` inside the `config.tcl` file
> run_synthesis

This returns the TNS (total negative slack) and the wns (worst negative slack) there is a possibility that slack get violated if it happens we will see in later parts How do we can fix it.

##### Introduction to delay tables
We start with the two gates (AND and OR) and saw the feature of the AND gate that will allow the propagation of the clock to the complete clock tree.

![power aware cts1](<pics/d4/sk1-L4/Screenshot from 2025-05-18 16-11-10.png>)

We had this 1 pin (enable pin) of AND gate which was connected to logic 1 and whenever the enable pin was connected to logic 1 then only the clock will propagate through the gate to the rest of the circuit else it wont be. If the enable pin is at logic 0 the clock will be blocked at the input itself and it will not propagte through the gate to the rest of the circuit. Similalry for the OR gate which can also be used for such a purpose. Whenever the enable pin of the OR gate is at the logic 0 the clock propgates through gate to the rest of the circuit otherwise clock will be blocked at the input.

The advantage of this type of cicuit that we will be getting is that the rest of the circuit for that amount of time, there will be no switching and short circuit power that will be consumed by the clock tree during that period of time. Therefore, we are saving a lot of power and switching and the short circuit power.
This is referred to as clock gating technique. We are gating the clock to propagate to the rest of the circuit.

_How do we use this in a clock tree?_
We may think of blindly swapping the buffers with the gates and use the clock gating technique. But is this right? or what can go wrong?

<img src="pics/d4/sk1-L4/Screenshot from 2025-05-18 16-28-34_cropped.png" alt="Power Aware CTS2" width=50%>

Before delving into this, we do some time analysis of the clock tree we have

![alt text](<pics/d4/sk1-L4/Screenshot from 2025-05-18 17-16-54.png>)

There are some observations shown in the figure itself.
1. The capacitance or the load at the output node of each and every buffers in a clock tree is not same, it is varying.
2. Due to varying output capacitances of each and every buffer, the input transition is also varying and so for all the buffers present in the above clock tree, the input transition is also not constant and its also varying. It varyies in a range say $(10\,ps -- 100\,ps)$.
Therefore, we have varying input transition at the inpts of the buffers and the varying ouput load at the output of the buffer.

Therefore we will have variety of delays and therefore engineers introduced the concept of **delay tables**.

_Delay table for CBUF`1`_

A buffer of size-1 means it will have the PMOS and NMOS of size-1 and similarly, a buffer of size-2 will have PMOS and NMOS of size-2. Basically the size of PMOS or NMOS in nothing but $w/l$-ratio. A bigger size PMOS, we are actually recuing the resistance that changes the $\tau = R\,C$-delay. Therefore, we have individual/exclusive delay tables for each of the gates with each and every size.k

For example, if the input transition is varying and the output buffer capcitance is varying from 10fF-110fF.

filling up the delay table...

Eventually this delay table became the timing model of that particular buffer of size-1. The buffer will have a PMOS and NMOS and it has fixed size V_t and has a delay table like this.
For buffer of size-2 there is a separate delay table that is also created.
The delay tables became the timing models of the buffers of given sizes. 

##### Delay table usage
Delay tables are the representation of the delays for the different cells in a clock tree. So similar delay tables we will be having for differnt kind of delay tables for various gates such as AND, OR, XOR etc. All the gates will have individual delay table. Here we see the delay table of the buffers of the clock tree circuit of the previous subsection.

![alt text](<pics/d4/sk1-L4/Screenshot from 2025-05-18 17-30-11.png>)

How to read delay table? The particular buffer is taken out of the circuit and is charaterized for a range of input slews and a range of output load. For examaple, if the input slew is 60~ps and the ouput load is 70fF for a buffer of size-1, the delya table contains a value $x16$.

![alt text](<pics/d4/sk1-L4/Screenshot from 2025-05-18 17-41-34.png>)

The values that are not avaialbale in the delay table that are extrapolated from the given data. Essentially, we find some equations from the given data (delay table) for finding out missing numbers.

The objective is to find the delay for the top buffer of size-2 (see figure below) when the given input slew is 40~ps and the delay of the buff-1 is $x9'$ and the finding this for all the buffers in the circuit, we will be able to obtain the _latency_ (clock latency) at the output terminals;

![alt text](<pics/d4/sk1-L4/Screenshot from 2025-05-18 17-46-40.png>)

So now we proceed for calculating the delay of the buffer of size-2 and then calcuating the latency from the clock port to clock end points. The node-A is connected at the input to both size-2 buffers and therefore it is common to both the size-2 buffers and let us assume that a input slew at the input to both the size-2 buffers is of $60\,ps$, where this transition itself came fron the input transition to size-1 buffer and its output load capacitance and therefore it is also a $f(\text{input transition, output load})$. Actually, Similar to delay table of cells, we have the transition table for buffers also but this is what we are not looking into this course.

![alt text](<pics/d4/sk1-L5-6comb/Screenshot from 2025-05-18 23-05-25.png>)

The two size-2 buffers are identical with each other, connected to the same input node A and driving the same loads at nodes B and C of $50\,fF$, and therefore the delay value of $y15$ is valid for both the buffers. Now we calculate the latency from the left-most input node (input to size-1 buffer) and the clock end terminals (connected to the four flip-flops) as
$$ \text{latency} = x9' + y15 $$
for all the four end terminals connected to the flip flops. Here, we assumed that the wires delay is null.

![alt text](<pics/d4/sk1-L5-6comb/Screenshot from 2025-05-18 23-11-33.png>)

Now, since the latency of all the four terminals connected to the flip flops is same therefore the skew between any two pair of clock end terminals is 0.

In this direction, we consider an example, where both the size-2 buffers get the same input transition, the top size-2 buffer is continue to drive a load of $50\,fF$ but the size-2 buffer at the bottom is driving a load of $90\,fF$. In this case, the delay of size-2 buffer on the top remains $y15$ but the delay of size-2 buffer at the bottom is $y17$ and therefore the latency at the terminals of top size-2 buffer is $ x9' + y15 $ while for the size-2 buffer at the bottom is $x9' + y17$. Therfore, this leads to a non-zero total skew between the terminals connected to flip-flops. Therefore, it is important that at every level, each node is driving the same load.

In an other example, we consider two different-type buffers at the level-2 (top is size-2 buffer and bottom is a size-1 buffer), then for the same input transition of $60\,ps$ and if they both are driving the same load of $50\,fF$, then the delay of top size-2 buffer will be $y15$, while the delay of bottom size-1 buffer is $x15$ and since these numbers are different, it will again generate a non-zero skew at the clock end points. Therfore, it is also important to have identical buffe at the same level.

Therefore, these things need to be taken care of at very early stages of building the clock tree. If a small non-zero skew with this clock tree propagates down the complete chain of say millions of clock tree, Then it will be result into a large skew number.

![alt text](<pics/d4/sk1-L5-6comb/Screenshot from 2025-05-18 23-29-06.png>)

_Power aware CTS_

Let's say that the two bottom flops are not active for a certain period of time or in other words, they are active under certian conditions. So this section of the chip has got some special functionality and will trun on under certain conditions. In this case, we need not propagate the clock to this complete clock tree and we can actually stop the clock at the clock end terminals connected to the flops so that is how we achieve power aware clock tree synthesis.

##### Lab steps to configure synthesis settings to fix slack and include vsdinv

![alt text](<pics/d4/sk1-L7/Screenshot from 2025-05-18 23-36-41.png>)

To remove the slack error in the inclusion of sky130_vsdinv custom cell in the openlane design flow of picorv32a, we need to modify some of the parameters. The setails of such parameters can be obtained from the readme file in the `.../openlane/configuration/README.md` directory.

We read the timing report after the `run_synthesis` process to see where the delay is actually building up?

He is trying to make a balance between the chip_area by modifying the syntehsis strategy for the chip area by modifiying the environment parameter `SYNTH_STRATEGY` as
> echo $::env(SYNTH_STRATEGY)

the command `echo` is used for checking the used value within the flow process. Now we set the value by

> set ::env(SYNTH_STRATEGY) 1

He is also checking a parameter `SYNTH_BUFFERING` that put the buffer in between and controls the wire delays, whether it is on or off. He is also checking for a parameter `SYNTH_SIZING` that is related to the cell sizing it is basically the up and down sizing of the buffer based on the synth strategy.

One more parameter `SYNTH_DRIVING_CELL`, a driving cell that drives the input ports. If the input port has lot of fan-out then it needs more driving cells to drive the input.

![alt text](<pics/d4/sk1-L7/Screenshot from 2025-05-18 23-59-27.png>)
![alt text](<pics/d4/sk1-L7/Screenshot from 2025-05-19 00-08-57.png>)

With the reduced slack time, he further performed two things
> run_floorplan  
run_placement

In the placement run, the iterations happens happend for converging the overfloe (OVFL) and it will decrease when moving towards convergence. After this verify if our custom cell is included in the placement.

![alt text](<pics/d4/sk1-L7/Screenshot from 2025-05-19 00-14-29.png>)
![alt text](<pics/d4/sk1-L7/Screenshot from 2025-05-19 00-15-50.png>)

The overlap of the cells -> abettment -> to ensure that the power and ground rails are shared between the cells.

#### 2. Timing analysis using ideal clocks using openSTA
##### Setup timing analysis and introduction to flip-flop setup time
Timing analysis (with ideal clocks)

_Ideal clock_: The clock tree is not yet built in the clock path.

Single clock setup analysis means there is one clock that triggers both the launch flop and the capture flop.
There are scenarios where both the flops are triggered by different clocks. So we have
- a launch flop
- a capture flop
- a combinational logic between the flops
- and a clock network (without clock tree -> therefore ideal) of time period $T = 1\,ns$.

![alt text](<pics/d4/sk2-L1/Screenshot from 2025-05-19 00-49-19.png>)

We consider a clock signal of $T=1\,ns$, the zeroth edge is sent to the launch flop and and the other edge to the capture flop. So we have to do the setup time analysis with this simple circuit.

Let us assume that the combinational logic has a got a delay of $\Theta$, then the setup timing analysis (STA) says that for this particular circuit to work, we must have $\Theta < T$, that is the combinational logic delay from launch flop to all the way to capture flop must be lesser than the clock time period. If this condition is not hold, then the clock edge at $t=T$ needs to be shifted more right and then the clock period increases which further leads to a decrease in frequency ($\nu = 1/T$). This can not be allowed because the system is designed to work for a clock period of $1\,ns$.

_Introducing the more practical scenarios_

The first practical scenario is the flop itself. In principle, the flops got several MOSFETs, logic gates, resistances and capacitances inside it but an upper level structure of a flop consists of two mux. To understand this, we open-up the flop to undertsand its working.

![alt text](<pics/d4/sk2-L1/Screenshot from 2025-05-19 00-53-42.png>)

From the timing graph, when the clk is at 0, whatever is present at the $D$-input reaches from the point $D$ to the point $Q_M$ thorugh Mux1. So, there is a delay associated with this particular transit of information from $D$ to $Q_M$ and this amount of delay is equal to the delay of Mux1 (which is also built on PMOS, NMOS, resistances and capacitances etc.). In this case when the clock is equal to 0, the output is fed back to the input of the Mux2 and keeps rotating back to input.
Now, when clock switches from 0 to 1, then the output of the Mux1 is fed back to its input and keeps rotating. But the data that is present at $Q_M$ moves from $Q_M$ to $Q$. So, there is a second delay associated with this particular transit of information and equal to the Mux2 delay.

Now, the Mux1 delay or the Mux2 delay when the clock is at logic 0 or 1 that actually affects or restricts the combinational delay requirement. Now, if the clock edge at $T$ reaches the capture flop, the capture flop knows that it needs some finite amount of time for whatever input that comes at D to settle it properly to at its center. So as a result of that that amount of time ($S$) has to be subtracted from the complete clock period $T$.

![alt text](<pics/d4/sk2-L1/Screenshot from 2025-05-19 01-19-53.png>)

Now if the combinational logic delay $\Theta$ is less than $T-S$ where $S$ is the flop setup time, the capture flop has got sufficient time for taking whatever at the input $D$ to the center of the flop.

##### Introduction to clock jitter and uncertainty
In this subsection, we will continue with the setup timing analysis and add more practical scenarios that affect the timing analysis from the previous subsection.

The next practical scenario that we are going to consider is something that is referred to as _jitter_. So what happens is that the clock is beeen created by the a PLL (phase locked loop) and it is expected to send the clock signals at $0,T,2T,...$, but the clock source might have got some its own inbuilt variations and due to this it might provide the clock edge at before or after viz. $T-\delta, T, T+\delta$, where the $\delta $ is the variation due to the inherent variations in the PLL or the clock source. This variation is referred to as jitter.

![alt text](<pics/d4/sk2-L2/Screenshot from 2025-05-19 09-44-12.png>)

This jitter is usually very small but can be reason for the circuit failure. And therefore the combinational delay requirement becomes even more stringent because of this kind of variations in the clock signal. Those variations are captured on a term called as jitter (temporary variation of the clock period). This also has to be modelled in to the rquirement equation. The jitter value for both the launch flop and capture flop can be different.

The parameter that model the jitter is _setup uncertainty_ (SU) and therefore the combinational delay requirement equation modifies to
$ \Theta < (T - S - SU)$ as shown in the figure below;

![alt text](<pics/d4/sk2-L2/Screenshot from 2025-05-19 09-54-31.png>)

With the above discussion on setup timing requirement, we identify the timing path with single clock of the design (see figure below)

![alt text](<pics/d4/sk2-L2/Screenshot from 2025-05-19 09-57-36.png>)
![alt text](<pics/d4/sk2-L2/Screenshot from 2025-05-19 09-58-35.png>)

The combinational logic delay of the cells placed within the dashed circles are obtained by simply adding the delays of flops, gates and wires. If this satisfy the combinational delay criterion, we can be asssure that the placement (or the optimized placement that we've done) is good for the cells driven by the single clocks.  

In the next section, we take up on the challenge that come when we do similar kind of anaylsis with multiple clocks.

##### Lab steps to configure openSTA for post-synth timing analysis
In the labs of the previous sections till now, to reduce the total negative slack significantly we have modified some underlying parameters in the openLANE flow of picorv32a that we did with our custom cell sky130_vsdinv. Here we continue with the flow.

Whenever there is a timing violation, we carry out the analysis in a separate tool. We are doing the timing analysis in the openSTA.

Within the openlane directory, he created a file `pre_sta.conf` (see fig below)
- it read the libs files
- it reads a custom verilog file (there is one verilog file because synthesis is where the mapping happened and the logic got maps to gates)
- we also din floorplan and placment but no new netlist is actually created.

![alt text](<pics/d4/sk2-L3/Screenshot from 2025-05-19 10-52-39.png>)

In doing the CTS, it add buffers to the netlist and therefore we will see a new verilog file.

We are linking the design `picorv32a`. We are reading the `my_base.sdc` inside the source of the design of picorv32a [forked from the original `base.sdc` that comes with openlane and is located in the `openlane/scripts` directory.]. In my_base.sdc, we modified the `SYNTH_DRIVING_CELL` to the `sky130_fd_sc_hd__inv_8`. He also changed the CELL_PIN to Y and to know the `SYNTH_CAP_LOAD`, he got the details from the typical.lib file.

![alt text](<pics/d4/sk2-L3/Screenshot from 2025-05-19 11-03-34.png>)

Within the `pre_sta.conf` file, he used that `my_base.sdc` file.

![alt text](<pics/d4/sk2-L3/Screenshot from 2025-05-19 11-26-19.png>)

Now, to do the STA, He issued a command
> sta pre_sta.conf

It reports tha tns and wns both (read the `pre_sta.conf` file).

##### Lab steps to optimize synthesis to reduce setup violations
Till now, the analysis is done without CTS and assuming ideal clocks (zero skew) and therefore the hold analysis does not hold any significance.

In the setup analysis, where the delays are high we can do something there. Recall, the delay of a cell $f(\text{input slew, output load})$ an we will play with them for reducing the delay. He optimize the `fan_off` value.

To see the default values taken in the flow process, again open the `.../openlane/configuration/README.md` file and look for `SYNTH_MAX_FANOUT` and them setting it to 4 in the openlane flow and run_synthesis again.

In the meantime of run_synthesis, he chacked out a particular row with a high fan_out value by invoking
> report_net -connections _14635_

He found out that a size-1 buffer is driving four input pins and therefore a large delay.

The slack is now reduced further after setting the fan_out variable to -1.87. and with this our verilog file also got updated and running the `sta pre_sta.conf` reports this new slack value.

![alt text](<pics/d4/sk2-L4/Screenshot from 2025-05-19 11-49-21.png>)

Next, he pointed out how the input tansition is getting bad as passing thriugh chain of buffers which leads to a high slack vlaue. So for fixing this

![alt text](<pics/d4/sk2-L4/Screenshot from 2025-05-19 11-54-08.png>)

and the slack value decreases further.

##### Lab steps to do basic timing ECO
Here, continuing the previous lab steps, he replaced one more standard cell for further reducing the slack value. This is one of the method called upsizing the buffers to reduce the slack value.

To see the upsizing of the buffers, he does (see figure)


Please remember out target is to make the slack $>= 0$.

#### 3. Clock tree synthesis tritonCTS and signal integrity
##### Clock tree routing and buffering using H-Tree algorithm
In this subsection, we look at he clock tree synthesis section of the physical design flow.

In a typcial design as shown in figure, the clock signal goes to FF1 FF2 (orange) and FF1 and FF2 (Green) and blue. The purpose is to make this clock reach to the respective flops. So How do we do it?

![alt text](<pics/d4/sk3-L1/Screenshot from 2025-05-19 12-13-02.png>)

We first try to blindly connect clock to flops without any rule(see figure)

![alt text](<pics/d4/sk3-L1/Screenshot from 2025-05-19 12-14-39.png>)

the yellow wires conatins the resistances and capacitances. Now let's see the problem in it.
- $t_1$ is the time required to clock to reach flop1
- $t_2$ is the time required to clock to reach flop2

![alt text](<pics/d4/sk3-L1/Screenshot from 2025-05-19 12-19-18.png>)

We define then $\text{skew} = t_{2} - t_{1}$ and it is demanded that the skew should be ~$0\,ps$. To achieve 0 skew, the above blind connetion is a bad tree.

![alt text](<pics/d4/sk3-L1/Screenshot from 2025-05-19 12-18-04.png>)

Therefore, we need something more smarter and therefore we introduce the concept of _H-tree_.

H-tree basically takes the clock route and then it calcuates the distance between its terminal to all the end terminals. It try to come at the mid-point of the tree and build the tree from there.

![alt text](<pics/d4/sk3-L1/Screenshot from 2025-05-19 12-25-35.png>)

With this mid-point strategy, it is made sure that the clock recahes the flops approximately at the same time. Therefore teh skew value will be close to zero.

The next thing is the CTS (Buffering).

When we see the above clock tree, the physical wire got the resistance and capcuatnce and the signal travles along the physical path and tries to charge the capcialciantes in th epath and thereofr ethere will be a signal integrity problem.
This problem is got ridden by adding _repeaters_ in the network as we see prviosuly in the floorplan and placement section.

![alt text](<pics/d4/sk3-L1/Screenshot from 2025-05-19 12-33-34.png>)

We expect the same output waveform what was feded to the input but that does not haooens due to the resistenace and cpciatance of he physical wires (RC network of the in the circuit).

The repeaters helps in reproducing the signal from one point to another. The repeaters that we used for the data path and repeaters (or buffers) that is used for the clock differ in only one aspect. The clock repeaters will have equal rise and fall time. [clock buffer vs regular buffer]

![alt text](<pics/d4/sk3-L1/Screenshot from 2025-05-19 12-39-32.png>)

In the next section, we are going to do the setup timing analyis with the real clocks.

##### Crosstalk and clock net shielding
We continue the discussion of setup timing analysis with real clocks. Before that, we first understand the _clock net shielding_.

From the clock tree that we build in the previous subsection, we ensured that there is no skew (latency difference) between the clock port to the blue FF2 (assumed capture flop) clock pin minus the clock port to the yellow FF1 (assumed launch flop) clock pin.

Clock nets are considered to be the critical nets in our design becuase we have build the clock tree with zero skew. There can be an accedenetal skew if there is phenomena of crosstalk happens along the lines that deteriorate our tree desgin.

Brief discussion on the clock net shielding:
We take all the clock nets and shield them. For example, the shielding of yellow clock net is shown in the figure. Through shielding, we protect the clock net from the outside world or in other words, we are encapsulating this clock net to keep it protected from the outside world.

![alt text](<pics/d4/sk3-L2/Screenshot from 2025-05-19 13-03-47.png>)

Actually, if the coupling capacitance between the neighbouring wires and the clock net is huge, then there might be some coupling happening between the wire and the clock net and as a result of that there are couple of problems
- one is _glitch_: Whenever there is a switching activity happening at the agressor (A), because of the strong coupling capacitor ($C_M$) with the clock net (without shielding) will directly impact the clock net sitting below. And as a result of it, there is a glitch in the signal. Due to the switching activity of agressor, there is a dip in the voltage. So if the circuit is connected to a memory and the memory was initiated with some numbers, accidentally the glitch may result into a logic high (invered dip due to an inverter) at the reset (RST) pin and the complete memory might get reset. Therefore, the glitch is a very important factor while doing the clock tree building or in the physical design flow. Therefore shielding is what will protect the clock net.

![alt text](<pics/d4/sk3-L2/Screenshot from 2025-05-19 13-18-19.png>)

what if the vicitim and the agressor both are swithcing, then what can go wrong?
- and other is _delta delay_: Considering a clock tree with $L_1$ and $L_2$ being the latency from clock port to the flop terminals as shown in figure, we assumed that we build this tree in a fashion that  $L_1 = L_2$. Now because of crosstalk, there is some amount of delta delay, then there will be a non-zero skew and if there are millions of buffers present thi skew will grow exponentially or linearly. In order to protect from crosstalk we put a shield around the clock nets.

![alt text](<pics/d4/sk3-L2/Screenshot from 2025-05-19 13-27-39.png>)

It is not possible to shield all of the nets (clock nets and data nets) present in the design and we only shield the critical nets. There are other techniques as well to protect the clock nets from crosstalks (not discussed here).

##### Lab steps to run CTS using TritonCTS
In the last lab steps, we reduced the slack significantly by modifying many things, here we discuss the run CTS.

He is also replacing twokk more buffers (but that happned with a result in increasing area). Replacing a buffer of size-1 to size-8, actually that was driving 4 fanouts.

![alt text](<pics/d4/sk3-L3/Screenshot from 2025-05-19 13-39-54.png>)
![alt text](<pics/d4/sk3-L3/Screenshot from 2025-05-19 13-42-53.png>)
![alt text](<pics/d4/sk3-L3/Screenshot from 2025-05-19 13-44-03.png>)

Now, we take this netlist and we want to use the update netlist in the openlane flow.
This is done by
> help write_verilog  

Replacing the existing verilog file in `.../openlane/designs/picorv32a/runs/.../results/synthesis/` with our updated one

![alt text](<pics/d4/sk3-L3/Screenshot from 2025-05-19 13-49-18.png>)

Verify in the netlist file that if it contains our replaced buffers.

Now, in openlane flow
> run_floorplan  
run_placement

Whar we see is that the core area has increased.

Now running the CTS,
> run_cts

QoR -> Quality of Result for CTS, the lower the value of QoR the degraded the results.

The CTS step adds the clock buffers and therefore modify the netlist. Within the `.../picorv32a/runs/.../results/synthesis/` we will find a new verilog file with `picorv32a_synthesis_cts.v` whihc the previous verilog file plus the clock buffers.

##### Lab steps to verify CTS runs
Actually what happens when we execute any of the command `run_*` in the openlane flow. Let us undertand it from `run_cts` command. So this is where in tcl, there is a concept of `proc`s. So we look in the script directory of openlane,

![alt text](<pics/d4/sk3-L4/Screenshot from 2025-05-19 14-05-12.png>)

Within any of the `*.tcl` file, the `proc`s are defined (which are similar to functions or methods in the language of other programming langauges) so when we execute a command `*.run`, it execute a proc (like calling a function without arguments).

![alt text](<pics/d4/sk3-L4/Screenshot from 2025-05-19 14-09-44.png>)

Note that the pre-floorplan is controlled by openlane, [floorplanning, placement, CTS, optimiziation, Global routing] is controlled by the openroad and post global routing is again controlled by the openlane flow.

Open up the openroad scipts and there are many .tcl files and seeing the varibales from there by using `echo`:

![alt text](<pics/d4/sk3-L4/Screenshot from 2025-05-19 14-18-05.png>)

One of the limitation of the tritonCTS is that it can not create an optimized CTS for multi columns but can do for single (work in progress).
So when we ran the `run_cts` it was done for typical columns and therefore the values are for typical columns.

#### 4. Timing analysis with real clocks using openSTA
##### Setup timing analysis using real clocks
In this section, we build the clock tree and do the timing analysis using real-clocks. The circuitory with the real-clocks looks something like below (see figure)

![alt text](<pics/d4/sk4-l1/Screenshot from 2025-05-20 15-28-16.png>)

The clock edge will reach to launch and capture flops through a set of real buffers. Therefore, the equation for cominational delay requirement changes, particularly the clock delays have also be taken into consideration.

From figure, we see that the clock edge of the signal generated by the PLL of the clock is supposed to reach the flop at $0\,ns$ but due to the inplace buffers, the clock edge will not reach at $0$ but it will reach at 0 + buff_1_delay + buff_2_delay. Similarly, the clock edge is supposed to reach at the capture flop terminal at $t=T$ but it will reach after $T$ + buff_1_delay+ buff_3_delay + buff_4_delay.

![alt text](<pics/d4/sk4-l1/Screenshot from 2025-05-20 15-37-16.png>)

Now, lets us assume that the total delay of buff_1 and BUff_2 is $\Delta_1$ and the toal delay of signal to the capture flop is $\Delta_2$, and we also include from the previous secions the setup time (S) and the uncertianty time (SU); then the combinational delay requirement becomes

![alt text](<pics/d4/sk4-l1/Screenshot from 2025-05-20 15-47-26.png>)

The quantity $|\Delta_1 - \Delta_2|$ is called the clock _skew_. Any circuitary of the chip that follows this delay requirement equation is ready to use at $\nu = 1/T$, where $T$ is the clock period.

Anything, which is violating the above equation (we actually have term for this called _slack_) is not ready to be used and needs to be corrected.

_Introduction to slack_

In the above delay requirement requirement inequality the left hand side $\Theta + \Delta_1 $ is referred to as _data arrival time_ and the right hand side $(T + \Delta_2) - S - SU$ is refered to as _data required time_. Now, the data arrival time should be less than the data required time (causality)
> slack $:=$ Data required time - Data arrival time $>= 0$.

So if it comes out be positive (desired case) it is referred to as positive slack and if negative, it is referred to as negative slack.

![alt text](<pics/d4/sk4-l1/Screenshot from 2025-05-20 16-01-51.png>)

If we get the negative slack, it is referred to as _slack violation_.

Now, we discuss the _Hold Timing Analysis_:

It is also a timing analysis based upon the launch and capture flops but things are slightly different here.

We will discuss the hold timing analysis for the real-clocks only. In contrast to the case of the setup timing analysis where we sent the clock edges at $t=0$ to launch flop and $t=T$ edge to the capture flop, in this case, we send the clock edge at $t=0$ to the launch flop to launch the data and the same clock edge at $t=0$ to the capture flop to capture the data and also for hold timing analysis. The hold condition is

![alt text](<pics/d4/sk4-l1/Screenshot from 2025-05-20 20-03-27.png>)

Now, we under what is the _hold time (H)_?
Within the flop, we saw earlier that there were two muxes Mux1 and Mux2, the hold time (H) is actually the time delay of Mux2.

When the clock is at logic-1, the data is sent to the ouput $Q$ from the center point $Q_M$, and the time required to send the data from $Q_M$ to $Q$ is the time-delay of Mux2. So the capture flop knows this,and do not expect any input data during this time peiriod of sending the data from $Q_M$ to $Q$. The capture flow hence sends a message to the launch flop that send your data after my hold time and therefore hold your data till I send my existing data outside of the capture flop.

Now, If we add the clock network delays the hold timing equations incorporate as shown in figure below. 

![alt text](<pics/d4/sk4-l1/Screenshot from 2025-05-20 20-16-30.png>)

##### Hold timing analysis using real clocks
Here, we continue the hold timing analysis.

In the hold timing analysis, the clock jitter will not play much role because the same clock edge is being given to both the launch and the capture flops. Whatever uncertainty is at the clock edge at $t=0$, it will be sent to both the flops. But to be more realistic we add a term referred to as _hold undertainty_ (HU), then the hold timing condition becomes:

![alt text](<pics/d4/sk4-l2/Screenshot from 2025-05-20 20-24-08.png>)

Similar to STA, the left-hand-side of the above equation is _data arrival time_ and the rightt-hand-side is the _data required time_.

![alt text](<pics/d4/sk4-l2/Screenshot from 2025-05-20 20-29-19.png>)

Here also we define the slack value but it is now given by

> (HOLD) SLACK = Data arrival time - Data required time

Again, if it comes out to be negative, it is said that slack is violated.

So where is the $\Delta_1$ and $\Delta_2$ part from the existing design which we are looking at?

![alt text](<pics/d4/sk4-l2/Screenshot from 2025-05-20 20-33-43.png>)

The top orange flops and logic gates are driven by the clock-1. Therefore the $\Delta_1$ that captures the time delay of reaching the clock edge to the FF1 is 

![alt text](<pics/d4/sk4-l2/Screenshot from 2025-05-20 20-37-13.png>)

Similarly, we can calculate the delay $\Delta_2$;

![alt text](<pics/d4/sk4-l2/Screenshot from 2025-05-20 20-41-04.png>)

##### Lab steps to analyze timing with real clocks using openSTA
Openroad also integrates the openSTA tool and was an independent project before its integration with openlane.

Within the openlane flow, invoke the openroad tool by command (because it is integrated with openflow)
> openroad 

Our objective is to do an analysis of the clock tree (with entire circuit). OpenSTA is integrated into the openroad (and we will launch it inside the openroad) -> Why?
The varaibles of the openlane can be used.

We will first create a db file and to do that we do (within the invoked openroad tool)
> read_lef .../picorv32a/runs/.../tmp/merged.lef

Now we will read the .def file from the directory `.../picorv32a/runs/.../results/cts/`  where we have the picorv32a.cts.def file. We use this file (which is created post CTS) as
> read_def .../picorv32a/runs/.../results/cts/pciorv32a.cts.def

(Remeber lef file do not change but the def file got change whenever additional components or cells
have been added in the design and then we wiil be requiring a new db file.) Now we will be creating the db file by invoking
> write_db pico_cts.db

We can give any custom name to the db file like <custom name>.db and tt is created within the main directory of openlane. And now we read the db file
> read_db pico_cts.db

Now we do `read_verilog` and we read the verilog file that got generated after the CTS (because the clock members now have been inserted),
> read_verilog .../picorv32a/runs/.../results/synthesis/picorv32a.synthesis_cts.v

![alt text](<pics/d4/sk4-L4/Screenshot from 2025-05-19 20-54-19.png>)

Then, we will read the liberty files by invoking
> read_liberty -max $::env(LIB_MAX)

(Because of the advatage of using the environment variables, we invoked the openSTA tool within the openlane)
and also the min file
> read_liberty -min $::env(LIB_MIN)

Now the custom sdc file (kept in )
> read_sdc .../picorv32a/src/my_base.sdc

We did this min-max analysis in openROAD within openLANE and this is actually incorrect (Beacuse this resulted into a higher slack values) because we build the clock tree for the typical corner and the libraries that we included in the above analysisa are the MIN and MAX libraries for MIN and MAX corners. In other words, we build the clock tree for typical corner but analyzing it for min and max columns. SO what so we do in this case?

We first exit from the openROAD now, and we will include only the typical library for typical analysis. We again invoke the `openroad` and perform a typical analysis by including typical library only.

![alt text](<pics/d4/sk4-L4/Screenshot from 2025-05-19 21-54-19.png>)

-> tritonCTS is built to optimize only one corner

##### Lab steps to execute openSTA with right timing libraries and CTS assignment
A summary of the above commands

![alt text](<pics/d4/sk4-L4/Screenshot from 2025-05-19 21-25-16.png>)

SO here we did the timing analysis using the tritonCTS in the openLANE flow by exiting from the openROAD as described in previous subsection.

Now, we provide an answer to an question that What is the effect on the slack value if the buffer list is modified?
Buffer list: Within the `.../openlane/scripts/run_cts.tcl` there is an environment value `CTS_CLK_BUFFER_LIST` that provides different size buffers starting from size-1 to size-8. The CTS will place the first buffer from this list to meet the required Q-value.

If we remove the first buffer from this list via
> set ::env(CTS_CLK_BUFFER_LIST) [lreplace $::env(CTS_CLK_BUFFER_LIST) 0 0]

It will have three buffers of sizes-(2,4,8) and it will put the first buffer of size-2 from this list by comprosing the area of the chip.

Now we ran the CTS again by `run_cts`,

![alt text](<pics/d4/sk4-L4/Screenshot from 2025-05-19 21-56-44.png>)


##### Lab steps to observe impact of bigger CTS buffers on setup and hold timing
Here, we are continuing from the previous section:

![alt text](<pics/d4/sk4-L5/Screenshot from 2025-05-19 22-12-38.png>)

What are we observing in the given figure that the tritionCTS process is stalled with a number of warnings. There is one modification that we forgot to include in the second CTS run and the flow was incorrect. So we first kill the current openroad process;

we check for the current def file by
> echo $::env(CURRENT_DEF)

which results into picorv32a.cts.def which is incorrect (because of some updation we used the old one). Why?
Now, we restore first the placement .def file.

![alt text](<pics/d4/sk4-L5/Screenshot from 2025-05-19 22-33-39.png>)
![alt text](<pics/d4/sk4-L5/Screenshot from 2025-05-19 22-37-42.png>)
![alt text](<pics/d4/sk4-L5/Screenshot from 2025-05-19 22-42-34.png>)

Replacing the size-1 buffers with bigger size buffers led to a larger area but the good slack (setup and hold) values.

Now we will check the clock skew which must be maximum 10% of the clock period.
> report_clock_skew -hold

![alt text](<pics/d4/sk4-L5/Screenshot from 2025-05-19 22-47-33.png>)

### V. Final steps for RTL2GDS using tritonRoute and openLANE
#### 1. Routing and design rule check (DRC)
##### Introduction to Maze routing and LeeA-As algorithm
In this section, we discuss the next stage of the physical design flow which _routing and DRC stage_.

Routing means we need to route two points. For example, the input Din1 has to be connected to the FF1 by a physical wire to maintain a physical connectivity between them. But from algoithm viewpoint, Din1 is considered as source and the FF pin as the target and the algorithm has to find the best possible path to connect between these two points. Therefore routing means connecting two points with best possible path.

There are various algorithm for routing, but here we discuss one such Maze algorithm (Lee's algorithm - Lee 1961).

In the figure below, say, if the blue gate-1 is to be connected with blue gate-2 then blue gate-1 acts as the source and blue gate-2 acts as the target and the routing algorithm will find the sohurtest possible path (with minimum zig-zagslike most of them should be L-shaped) between these two points to route them.

![alt text](<pics/d5/sk1-l1/Screenshot from 2025-05-19 23-01-32.png>)

There are various methods to connect point-1 to point-2, but we are looking for best possible route between 1 and 2. From engineering viewpoint, its a route (a physical wire that connects 1 and 2) but from algorithm viewpoint (it is two points which are need to be connected and there are some search algorithm which will search for various paths between them.) The algorithm do not care if it is a buffer or a gate etc. to some extent.


In the above figure, we understand the routing of point-1 and point-2 and remember there can not be any routing that happend above the block placed. Therefore for an algorithm this block acts as an obstruction and is avoided by its corner coordinates and the algorithm understand it can not put any route throrugh this obstrcution.

What algorithm does?
- It creates a routing grid at the back with some standard dimensions. (the blue grid in the figure below.)
- Then the algorithm creates two points one is source (S) and the other is target (T).

![alt text](<pics/d5/sk1-l1/Screenshot from 2025-05-20 00-36-59.png>)

So what algorithm does to find an optimal path between S and T?

It labels the grid boxes adjacent to S as 1, then the boxes adjacento 1 as 2 and so on. The grid boxes once labeled are not labeled again. Also, the grid boxed diagonally attached are not considered adjacent.

##### Lee's algorithm conclusion
We continue the previous part here for the Lee's algorithm. So we continue labeling the adjacent grid boxes as higher integers.

![alt text](<pics/d5/sk1-l2/Screenshot from 2025-05-20 00-43-25.png>)

The algorithm lables the boxes until the target is reached. Now the algorihtm will choose an optimal path using these labels. In figures below, the two paths are shown by considering the grid labels as [1-2-3-4-5-6-7-8-9]

![alt text](<pics/d5/sk1-l2/Screenshot from 2025-05-20 00-46-21.png>)
![alt text](<pics/d5/sk1-l2/Screenshot from 2025-05-20 00-46-37.png>)

So which path the algorithm will choose?
Since the first path has got too many zig-zags and the other path has got only single bend (L-shaped). Any route with single bend is mostly preferred in case of a routing algorithm if it exists. In general, the algorithm prefers a routing path with minimum number of bends.

This maze routing algorithm is used for global routing. And there are some limitations to this Maze algorithms such as
- For miliions of routes, the labelling and then choosing an optimal path takes a very long time and memory.
There are algorithms such as line-search, stainer-tree which solves this memory runtime problems.

Considering one more example:

![alt text](<pics/d5/sk1-l2/Screenshot from 2025-05-20 00-56-15.png>)

After performing routing a typical design looks like:

![alt text](<pics/d5/sk1-l2/Screenshot from 2025-05-20 00-57-11.png>)

SO while we do routing its not just about connecting two points, we also need to follow some rules which is the topic of next subsection.

##### Design rule check (DRC)
We discuss here some of the rules that we need to follow for proper routing.

There are these routes (green wires) that connect a buffer to flip flop and a flop to a logic gate-1. These wire are created using optical phtolithography and the first rule says imposes the minimum width of this wire. The shape of wire after lithography is not exatly rectangle but there will be some wavy pattern of the wire after lithography. The optical wavelngth of the wire says that the wire width should be this much.

![alt text](<pics/d5/sk1-l3/Screenshot from 2025-05-20 01-04-40.png>)

The second rule is about the _minimum pitch_ between the wires: Within the optical lithography they first come up with the mutiple lines with varying pithces of closest to the largest and then the lines with optimal value are faithfully printed.

![alt text](<pics/d5/sk1-l3/Screenshot from 2025-05-20 01-09-20.png>)

The third rule is about the _minimum wire spacing_: The spacing between the wires must be minimum to a value given in the DRC rules.

![alt text](<pics/d5/sk1-l3/Screenshot from 2025-05-20 01-12-32.png>)

So there are tens of thousands of the rules that the router needs to take into account. Now, let us look at some another section, where a signal short (DRC violation) is shown because the wires are short with each other within the same metal layer.

![alt text](<pics/d5/sk1-l3/Screenshot from 2025-05-20 01-16-56.png>)

So How do we solve this problem of signal short?
We take any of the two layer and put it on another metal layer.

![alt text](<pics/d5/sk1-l3/Screenshot from 2025-05-20 01-20-21.png>)

The upper metal layers are kept wider than the lower metal layers for signal integrity. By introduing more metal layers solves the problem of signal short but some new DRC rules come into the picture,

![alt text](<pics/d5/sk1-l3/Screenshot from 2025-05-20 01-26-07.png>)
![alt text](<pics/d5/sk1-l3/Screenshot from 2025-05-20 01-27-40.png>)

In lithography, to print the boxes with minimum via width, the experimentalist comes up with many box size and then the lithography tells that a box size with this much minimum width can be printed on silicon using optical light.

Note: The DRC rules has more to do with the lithography printing techniques to laid down the layout on the silicon wafer.

The next steps is called as the **_parasitic extraction_**:

![alt text](<pics/d5/sk1-l3/Screenshot from 2025-05-20 01-33-38.png>)

The physical wires got the finite resistance and capacitance and these values are need to be extracted and used for further process. Standard parasitic extraction format (SPEF) is an IEEE format that is used to extract these parasitic values.


#### 2. Power distribution network (PDN) and routing
##### Lab steps to build power distribution network (PDN)
In this lab, we will be peroforming the last section of the PnR flow which is the routing. Till now we have done CTS and post CTS analysis.

Invoked the openlane tool and we want to know upto what stage we have completed the flow for a given run data inside the runs directoty. So what we can do is to echo a variable like `CURRENT_DEF` to know if we have done upto the CTS stage like
> echo $::env(CURRENT_DEF)

If the .def file exist it means we have completed upto the CTS stage.

Now the next step is to perform the routing, but before rotuing we first do the power distribution network (PDN). To generate the PDN, we do
> gen_pdn

so the above proc is used to generate the power distribution network. This will read the `.lef` and the `*_cts.def` files and will generate the grid and the straps for the power and the ground. The generated rails provide the power and ground for the standard cells. These rails are placed along the standard cells rows.

![alt text](<pics/d5/sk2-l1/Screenshot from 2025-05-20 01-49-22.png>)

Details about the standard cell rows, height of the custom cell...

![alt text](<pics/d5/sk2-l1/Screenshot from 2025-05-20 01-55-26.png>)

Next we generate the _straps_ for power thorughout the chip: So thing is that we have a picorv32 chip and it has to get the power and it gets power from $ V_{\text{dd}} $ and the ground straps and from these straps it goes to standard cells and rails.

##### Lab steps from power straps to std cell power

![alt text](<pics/d5/sk2-l2/Screenshot from 2025-05-20 10-57-49.png>)

The yellow boxes at the boundaries and the corners are the i/o and the corner pads respectively. There is one corner pad in each column and the green box in the interior is the picorv32. The red pads are the power pads and the blue pads are the ground pads.

The power pads provide the power to the outer ring (red rectangle) and the vertical strap in the middle ensures the supply to the chip.

Now, the power is supplied to the standard cells. The top horizontal standard cell row/ rail is supplied the power by the vertical straps and this is done via the horizontal red strap.

And similalrly, the power is supplied to any of the macros in the chip as shown in figure.

All these information of straps and the connections are contained in the generated report after doing the `gen_pdn`.

Now, we finally performs the routing step which we do in the next section amd note some important concepts related to global and detailed routing.

##### Basics of global and detailed routing and configure tritonRoute
Before _routing_, we might want to check the current def file that will be now something `*pdn.def`.
> echo $::env(CURRENT_DEF)

![alt text](<pics/d5/sk2-l3/Screenshot from 2025-05-20 11-39-32.png>)

The various env variables that are available for routing, 
![alt text](<pics/d5/sk2-l3/Screenshot from 2025-05-20 11-41-16.png>)

The tritonRoute is the engine that is used for routing and there are four (0-3) strategies for routing;
(tritonRoute14 is a more upgraded version of tritonRoute)
For 0, the routing will not be like optimized for zero DRC, but we gain something else like runtime and memorygain. anything more than 0 will take a longer runtime.

We will be using zero for this workshop (this even will takes around half an hour). We do check this variable if 0 or not;
> echo $::env(ROUTING_STRATEGY)

Then we perform the routing step by
> run_routing

![alt text](<pics/d5/sk2-l3/Screenshot from 2025-05-20 11-51-01.png>)

In any commercially available EDA tools, the routing solution space is very huge. In VLSI industries, they divide the entire routing into two steps
- Fast route: The routing region is divided into rectangle grid cells and it is presented as coarse 3d routing graph. The global route is done by fast route.
- Detail route: It is done by the tritonRoute. Fast route is followed by teh detail route. It should ensure that it needs to realize the vias in accordance with the global route solution. In other words, the outpout of the fast or global route is actually the route guide (the set of guides for each net). In detail routing, we find the best possible path (routing topology) for connectivity between the set of points.

![alt text](<pics/d5/sk2-l3/Screenshot from 2025-05-20 11-51-42.png>)

#### 3. TritonRoute features

#####  TritonRoute feature 1 - Honors pre-processed route guides
More on TritonRoute:

![alt text](<pics/d5/sk3-l1/Screenshot from 2025-05-20 12-11-05.png>)

![alt text](<pics/d5/sk3-l1/Screenshot from 2025-05-20 12-16-18.png>)

![alt text](<pics/d5/sk3-l1/Screenshot from 2025-05-20 12-17-05.png>)

##### TritonRoute Feature2 & 3 - Inter-guide connectivity and intra- & inter-layer routing

Inter-layer sequential panel routing:

![alt text](<pics/d5/sk3-l2/Screenshot from 2025-05-20 12-21-33.png>)

##### TritonRoute method to handle connectivity

![alt text](<pics/d5/sk3-l3/Screenshot from 2025-05-20 12-26-07.png>)

![alt text](<pics/d5/sk3-l3/Screenshot from 2025-05-20 12-28-05.png>)

Finding the most-optimal path between two APCs;

![alt text](<pics/d5/sk3-l3/Screenshot from 2025-05-20 12-33-37.png>)

MST -> Minimum spanning tree

Okay, so now the routing stage of the provious lab has been completed and we first analyze some files;

It shows some non-zero number for total violations beacuse we did routing by choosing strategy as 0. If we choose any number $>0$, we will have small to zero violations. For example, tritonRout14 will result into zero violations but will take much longer runtime and memory requirement.

We will have to correct the violations manually. So first we will find where this violations are happening? For this we read the file `.../picorv32a/runs/.../reports/routing/tritonRoute.drc` and that contains the details about the various violations. Like,

![alt text](<pics/d5/sk3-l4/Screenshot from 2025-05-20 12-45-14.png>)

Where is the resulted pre-processing routing guide which resulted from the global routing:

![alt text](<pics/d5/sk3-l4/Screenshot from 2025-05-20 12-47-35.png>)
We open this `fastroute.guide`, it contains all the informations in form of `llx lly urx ury _` for each net (eoi) .

The detail placement will ensure that the routing happens within the given routing guide.

Now, we do post-routing STA analysis;

- First goal is to extract the parasitic resistances and capacitances (In workshop openlane version the parasitic extractor has not been included with the openlane). Therefore, He is using a SPEF engine outside the `openlane`:

![alt text](<pics/d5/sk3-l4/Screenshot from 2025-05-20 13-19-46.png>)

It creates a `.spef` file in `.../picorv32a/runs/.../results/routing/` that contains the resistances and capacitances information.

After routing since the .def file has changed, we need to create a new .db file and we also got a new netlist post CTS and pre-route, we will be using these two files, `my_base.sdc`, and the generated .spef file for post-route STA analysis.

![alt text](<pics/d5/sk3-l4/Screenshot from 2025-05-20 13-26-03.png>)


_KUDOS_ fellas!  
Now you have a good idea for the open-source EDA tools, please go ahead and be an expert on the physical design flow. 

## Acknowledgements

Obviously this could'nt have been possible without this detailed and an exciting workshop and therefore I would like to thank @kunalghosh (Founder and CEO) of VLSI system design (VSD) and the entire team of VSD. For more details, please visit

VSD Website: [www.vlsisystemdesign.com](www.vlsisystemdesign.com)  
Github (Kunal Ghosh): [kunalg123](https://github.com/kunalg123)  
Github (Nickson Jose): [nickson-jose](https://github.com/nickson-jose)

</span>

