<span style="color: #ff2400 ; font-family: 'LM Sans 10'; font-weight: 'bold'">

# NASSCOM-VSD Digital VLSI SoC Design and Planning Workshop

</span>

This repository provides the documentation that is prepared while doing the workshop theory lectures and the labs. If you like this, please fork this repo and do your desired modifications. _Enjoy the reading!_

## Table of contents

<span style="color: #000000; font-family: 'Nimbus Sans'">

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
