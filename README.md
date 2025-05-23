<span style="color: #ff2400 ; font-weight: 'bold'">

# NASSCOM-VSD Digital VLSI SoC Design and Planning Workshop

</span>

This repository provides the documentation that is prepared while doing the workshop theory lectures and the labs. If you like this, please fork this repo and do your desired modifications. _Enjoy the reading!_

## Table of contents

<span style="color: #000000">

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
##### Introduction to RISC-V
##### From software applications to hardware

#### 2. SoC design and openLANE
##### Introduction to all components of open-source digital ASIC design
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
