# VSD_Physical_Design_Workshop
Workshop covers full RTL to GDSII flow using OpenLANE tool by efabless (open source) and Sky130 PDKs by Google and Skywater (open source).
<!-- PROJECT LOGO -->
<br />
<p align="center">

  ![](/snapshots_lab_session/Advanced-Physical-Design-using-OpenLANE_Sky130_1.JPG)

  <h3 align="center">Advanced Physical Design</h3>
</p>
<!-- TABLE OF CONTENTS -->
<details open="open">
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#abstract">Abstract</a>
    </li>
    <li>
      <a href="#rtl-to-gdsii-introduction">RTL to GDSII Flow</a>
    </li>
	<li>
      <a href="#google-skywater's-pdk-and-openlane">Google-Skywater's PDK and OpenLANE</a>
	  <ul>
        <li><a href="#google-skywater's-pdk">Google-Skywater's PDK</a></li>
        <li><a href="#openlane">OpenLANE</a></li>
		<li><a href="#instructions-to-install-openlane">Instructions to install OpenLANE</a></li>
      </ul>
    </li>
	<li>
      <a href="#day-1-how-to-run-openlane-flow">Day 1 How to Run OpenLane Flow</a>
      <ul>
        <li><a href="#interactive-openlane-flow">Interactive OpenLane Flow</a></li>
        <li><a href="#design-setup-stage">Design Setup Stage</a></li>
        <li><a href="#synthesis">Synthesis</a></li>
      </ul>
    </li>
		<li>
      <a href="#day-2-floorplan-and-placement">Day 2 Floorplan and Placement</a>
      <ul>
        <li><a href="#steps-in-floorplanning">Steps in Flooplanning</a></li>
		<li><a href="#floorplan">Floorplan</a></li>
		<li><a href="#steps-in-placement">Steps in Placement</a></li>
		<li><a href="#placement">Placement</a></li>
		<li><a href="#cell-design-flow">Cell Design Flow</a></li>
		<li><a href="#cell-characterization-flow">Cell Characterization Flow</a></li>
      </ul>
    </li>
	<li><a href="#references">References</a></li>
  </ol>
</details>

<!-- Abstract -->
## Abstract

Physical design is a process in VLSI in which a structured netlist from front end RTL design team is transformed into high-quality placement and routing by back-end design team to convert into geometrical design information of all physical layers which is used to tape-out chips.

ASIC design flow brings challenges and surprises at every step of the design cycle. The steps involved in IC physical design process and the tools to overcome the challenges faced in this process, from a RTL netlist to final tape-out, are introduced during a workshop. A hands-on in the physical design and the characterization of a standard cell built on Google-Skywater's open source 130nm process design kit with the help of Openlane flow, a fully-automated RTL to GDSII flow is one of the highlights of the workshop.

<!-- RTL to GDSII Flow -->
## RTL to GDSII Flow

RTL to GDSII is a process to convert behavioural logic written in high level languages to physical layout. RTL (register transfer level) defines a digital circuit design as a sequence of steps of data flow from one register to other register and logical operations that are performed on this data. Hardware Descriptive languages such as VHDL, Verilog are used to create high-level of representation of a circuit or also known as a netlist from a design at the RTL level.

Whereas GDSII (Graphical Data Stream Information Interchange) file is a final output of IC design cycle. It is a standard database file format that foundries use to exchange information related to physical layout. It is a binary file format that contains information like geometric shapes of layers, text labels.

Here are the basic steps invoved in the process to realize functional ASIC,

  1. Chip Specification: 
  Architecture, functionalities and specifications of the ASIC are defined at this stage.
	
  2. Design Entry / Functional Verification: 
  The functional and logical behaviour of the circuit is confirmed by a simulation of the RTL code.
	
  3. RTL block synthesis: 
  The RTL code is converted into gate-level netlist using a logical synthesis tool that meets required time contraints.
	
  4. Chip Partitioning: 
  ASIC design is partiioned into functional block and analysed the feasibility of reusing IPs from previous projects or using third party IPs.
	
  5. Design For Test (DFT) Insertion: 
  To figure out faults in the chip at early development stages high quality test techniques are introduced.
	
  6. Floor Planning (blueprint of a chip): 
  Physical implementation starts at this stage. Objective of the floorplan is to plan silicon area and robsut power distribution network to power whole chip.
	
  7. Placement: 
  It involves placement of standard cells on the rows formed in floorplan.
	
  8. Clock Tree Synthesis (CTS): 
  Create a clock distribution network to deliver clock to all sequential elements in required time, area and with low power consumption.
	
  9. Routing: 
  Find optimised ways to interconnect metal layers with valid patterns, here specifications defined in PDK such as minimum width, via spacing are used to inteconnect metal layers.
	
  10. Sign Off: 
  Routed layers undergo physical verification known as signoff checks to avoid any errors just before tapeout.
	
Please refer to link below to understand complete ASIC flow,
	https://www.einfochips.com/blog/asic-design-flow-in-vlsi-engineering-services-a-quick-guide/

<!-- Google-Skywater's PDK and OpenLANE -->
## Google-Skywater's PDK and OpenLANE

### Google-Skywater's PDK

Google and Skywater Technology Foundry have partnered to release first ever manufacturable open source 130nm process design kit(pdk). PDK is a collection of files that includes process design rules, behavioral models, analog designs, digital designs, support IPs and extracted data. PDK is in an interface between VLSI engineers and foundry. This particular PDK uses "SKY130" (130 nm) process node which supports 1 level of local interconnect and 5 levels of metals, and is capable of having inductors, has high sheet rho poly resistors, optional MiM capacitors and also includes SONOS shrunken cell.

Please refer to video link below,
	https://www.youtube.com/watch?v=EczW2IWdnOM&feature=youtu.be

### OpenLANE

OpenLANE is a ASIC design flow which is built on open source EDA tools. It is aimed to produce a clean GDSII with no human intervation to give a true tape-out experience to VLSI engineers. OpenLane flow is tuned to work with Skywater 130nm open PDK. 

Here is a diagram that shows fully automated RTL to GDSII flow in OpenLane and the open ource EDA tools used in each design step,

![](/snapshots_lab_session/OpenLANE_flow.JPG)

Please refer to this video for detailed description on OpenLane flow,
	https://www.youtube.com/watch?v=Vhyv0eq_mLU

### Instructions to install OpenLANE

As this is an open source flow, it is available on github. Please check the links below for more information and installation instructions.

	https://github.com/efabless/openlane
	
	https://github.com/nickson-jose/openlane_build_script 

<!-- Day 1 How to Run OpenLane Flow -->
## Day 1 How to Run OpenLane Flow

### Interactive OpenLane Flow

OpenLANE flow consists of several stages. It can be used in either interactive mode or autonomous mode. Command to use openlane flow in interactive mode,

	./flow.tcl -interactive
	
to load software dependencies of the OpenLane, use command,

	package require openlane 0.9

![](/snapshots_lab_session/Day1/D1_lab_invoke_openlane.JPG)

### Design Setup Stage

Now, to initiate design setup stage which creates directory structure for the runs, use command,

	prep -design `name_of_the_design_folder`

![](/snapshots_lab_session/Day1/D1_lab_design_setup_stage1.JPG)

Design setup stage uses configuration file that contains parameters to prepare a run, a path to this configuration file is also shown here.
	
### Synthesis

Synthesis step internally performs RTL synthesis (tool -> yosys), performs technology mapping (tool -> abc) and performs static timing analysis on the generated netlist to produce timing reports (tool -> OpenSTA). Use command,

	run_synthesis

![](/snapshots_lab_session/Day1/D1_lab_synthesis0.JPG)

Example of printed statistics,

![](/snapshots_lab_session/Day1/D1_lab_flop_ratio.JPG)

![](/snapshots_lab_session/Day1/D1_lab_synthesis1.JPG)

Properties like flop ratios, buffer ratios and timing information can be evaluated using the statistic printed during synthesis run.

<!-- Day 2 Floorplan and Placement -->
## Day 2 Floorplan and Placement

### Steps in Flooplanning

Typical steps involved in floorplanning are,
  
  1. Calculate Utilization factor and Aspect Ratio of the Core:
  Utilization factor is a ratio of area occupied by the netlist to the total area of the core. Utilization factor of 50% - 75% ensures enough room to optimise during routing stage. Whereas aspect ratio is a ratio of height of the core to width of the core. It defines the shape of the core, for example, aspect ratio value 1 means core is in square shape.
  
  2. Define location of preplaced cells: 
  Memory, comparator, MUXs are examples of the preplaced cells. These blocks or cells are usually designed once and then used at multiple instances in the design. The location of such IPs or blocks is defined and placed on the chip before automated placement and route stage, therefore, named as preplaced cells.
  
  3. Place decoupling capacitors: 
  Long wire paths could have voltage drop along the line and severely affect power supplied to the preplaced cells. Large decoupling capacitors are placed as close as possible to the preplaced cells to decouple them from the power supply and avoid problems of the voltage drop along the line.
  
  4. Power planning: 
  At times, power drawn or sinked at the source could lead to variations in supplied voltage, either voltage drop or ground bounce effect. These variations could drive design operation to fault state or indeterinate state. To avoid this problem, well distributed power network with many power straps is planned.
  A parallel structure of metal straps is ussed to network VDD and VSS on the chip. Parallel structure ensures low resistance and also addresses elctro migration problem.
  
  5. Pin placement: 
  It is necessary to carefully plan pin locations because optimal pin placement could result into low power consumpption and improved timing delays.
  
  6. Logical cell placement blockage

### Floorplan
  
To run floorplan step in OpenLane flow, use command,

	run_floorplan
	
![](/snapshots_lab_session/Day2/D2_lab_run_floorplan.JPG)

The results of floorplan can be viewed in magic tool, here is command structure,
	
	magic -T `path_to_tech_file` lef red `path_to_merged_lef_file` def read `path_to_floorplan_def_file` &
	
	Note: Ampersand sign at the end of the command frees the command console.

![](/snapshots_lab_session/Day2/D2_lab_invoke_magic_floorplan.JPG)

Floorplan viewed in magic tool,

![](/snapshots_lab_session/Day2/D2_lab_floorplan_view.JPG)

Zoom in on the floorplan and it can be noticed that the standard cells that arre yet to be placed are accumulated at the bottom.

![](/snapshots_lab_session/Day2/D2_lab_magic_stdcell.JPG)
	
	Note: Tap cells seen here are used to prevent latch up in CMOS, it connects N-build to VDD and substrate to GND to prevent latch up.

### Steps in Placement

Typical steps involved in placement are,

  1. Bind netlist with physical cells: 
  Library contains information about physical cells such as delay information, timeing delays. Usually,library gives option to choose a standard cell among its variants with different shapes, sizes, operating voltage. 
  
	Note: Larger shape allows cell to have low resistance and that makes it faster, but then it requires more area. Also, bigger cells have more drive strength compared to their smaller variants.
	
  2. Placement: 
  Placement takes place at this step.
  
  3. Optimize placement: 
  In this step, wire length and capacitances are estimated and bsed on that repeater are inserted. Repeaters are basically buffers and used in the design to maintain signal integrity.
  
	Note: Repeaters used on clock lines are different from the repeater that are used in data paths.
	
### Placement

To run placement stage in OpenLane flow, use command,
	
	run_placement
	
![](/snapshots_lab_session/Day2/D2_lab_run_placcement.JPG)

![](/snapshots_lab_session/Day2/D2_lab_run_placcement1.JPG)

	Note: Keep an eye on the overflow flag in the statistics shown on the screen. As iterations increases the overflow flag value should drop near to zero because this ensures that design placemnt converges with time.

![](/snapshots_lab_session/Day2/D2_lab_run_placcement2.JPG)

In the end check the legality results and ensure that all steps are okay.

Similar to floorplan, results of placement stage can be view in magic tool. Here is the command structure for it,
	
	magic -T `path_to_tech_file` lef red `path_to_merged_lef_file` def read `path_to_placement_def_file` &

![](/snapshots_lab_session/Day2/D2_lab_invoke_magic_placement.JPG)

Here is the example of placement results in magic tool,

![](/snapshots_lab_session/Day2/D2_lab_placement_view.JPG)

### Cell Design Flow

Cell design flow consists of three steps,

  1. Input
  - process design kit (PDK)
  - DRC and LVS rules
	for example, foundry specifies lambda-based rules in tech files. (lambda = L/2 , L -> minimum feature size)
  - SPICE models
  - Library and user-defined specifications
  
  2. Design steps
  - Implement functional
  - Model MOS transistors such that they adhere to library specificationss
  - Layout design
    - derive PMOS and NMOS network graph
	- obtain Eueler's path
	- get stick diagram
	- convert stick diagram into layout but ensure that it does not violates library rules.
  - Characterization
    - Extract parasitics from layout and characterise in terms of time delay
	
  3. Output
  - CDL (circuit description lannguage)
  - GDSII, LEF, extracted SPICE netlist
  - Timing, noise, power.lib functions
  
### Cell Characterization Flow

Here are the typical steps involved in cell characterization,
  
  1. Read model files of MOS from foundry
  2. Read extracted SPICE netlist
  3. Define/ recofnise buffer behaviour
  4. Read subcircuits in the design
  5. Connect necessary power supplies
  6. Apply stimulus
  7. Add necessary output load capacitance
  8. Simulation command
  9. Feed the results to GUNA (tool)
  10. Output of GUNA is in terms of timing, noise,power.lib functions

<!-- References --> 
## References
	1. ASIC flow: https://www.einfochips.com/blog/asic-design-flow-in-vlsi-engineering-services-a-quick-guide/
	
	2. Skywater's PDK: https://www.youtube.com/watch?v=EczW2IWdnOM&feature=youtu.be
	
	3. OpenLane flow: https://www.youtube.com/watch?v=Vhyv0eq_mLU
	
	4. OpenLane Installation: https://github.com/efabless/openlane
	
	5. OpenLane Installation: https://github.com/nickson-jose/openlane_build_script
	