# VSD_Physical_Design_Workshop

Workshop covers full RTL to GDSII flow using OpenLANE tool by efabless (open source) and Sky130 PDK by Google and Skywater (open source).
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
	<li>
      <a href="#day-3-spice-simulation">Day 3 SPICE Simulation</a>
      <ul>
        <li><a href="#features-of-openlane-flow">Features of OpenLane Flow</a></li>
		<li><a href="#CMOS Process">CMOS Process</a></li>
		<li><a href="#spice-deck">SPICE Deck</a></li>
		<li><a href="#extract-spice-file-from-layout">Extract SPICE File from Layout</a></li>
		<li><a href="#example-of-spice-simulation">Example of SPICE Simulation</a></li>
      </ul>
    </li>
	<li>
      <a href="#day-4-pnr-and-timing-analysis">Day 4 PnR and Timing Analysis</a>
      <ul>
        <li><a href="#pnr-and-lef-files">PnR and LEF Files</a></li>
		<li><a href="#cell-lef-file-extraction">Cell LEF File Extraction</a></li>
		<li><a href="#example-to-check-pnr-tool-reqiremnts">Example to Check PnR Tool Requirements</a></li>
		<li><a href="#plug-custom-cell-into-existing-design">Plug Custom Cell into Existing Design</a></li>
		<li><a href="#fix-slack-violations-(pre-synthesis)">Fix Slack Violations (Pre-Synthesis)</a></li>
		<li><a href="#fix-slack-violations-(post-synthesis)">Fix Slack Violations (Post-Synthesis)</a></li>
		<li><a href="#cts-timing-analysis">CTS Timing Analysis</a></li>
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
	
Please refer to link to understand complete ASIC flow,
	https://www.einfochips.com/blog/asic-design-flow-in-vlsi-engineering-services-a-quick-guide/

<!-- Google-Skywater's PDK and OpenLANE -->
## Google-Skywater's PDK and OpenLANE

### Google-Skywater's PDK

Google and Skywater Technology Foundry have partnered to release first ever manufacturable open source 130nm process design kit(pdk). PDK is a collection of files that includes process design rules, behavioral models, analog designs, digital designs, support IPs and extracted data. PDK is in an interface between VLSI engineers and foundry. This particular PDK uses "SKY130" (130 nm) process node which supports 1 level of local interconnect and 5 levels of metals, and is capable of having inductors, has high sheet rho poly resistors, optional MiM capacitors and also includes SONOS shrunken cell.

Please refer to video link,
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

Zoom in on the floorplan and it can be noticed that the standard cells that are yet to be placed are accumulated at the bottom.

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

In the end check the legality results and ensure that all steps are okay.

![](/snapshots_lab_session/Day2/D2_lab_run_placcement2.JPG)

Similar to floorplan, results of placement stage can be view in magic tool. Here is the command structure for it,
	
	magic -T `path_to_tech_file` lef red `path_to_merged_lef_file` def read `path_to_placement_def_file` &

![](/snapshots_lab_session/Day2/D2_lab_invoke_magic_placement.JPG)

Here is the example of placement results in magic tool,

![](/snapshots_lab_session/Day2/D2_lab_placement_view.JPG)

### Cell Design Flow

Cell design flow consists of three steps,

  1. Input
  - process design kit (PDK)
  - DRC and LVS rules, for example, foundry specifies lambda-based rules in tech files. (lambda = L/2 , L -> minimum feature size)
  - SPICE models
  - Library and user-defined specifications
  
  2. Design steps
  - Implement cell function
  - Model MOS transistors such that they adhere to library specificationss
  - Layout design
    - Derive PMOS and NMOS network graph
	- Obtain Eueler's path
	- Get stick diagram
	- Convert stick diagram into layout but ensure that it does not violates library rules.
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
  3. Define/ recognise buffer behaviour
  4. Read subcircuits in the design
  5. Connect necessary power supplies
  6. Apply stimulus
  7. Add necessary output load capacitance
  8. Simulation command
  9. Feed the results to GUNA (tool)
  10. Output of GUNA is in terms of timing, noise,power.lib functions

<!-- Day 3 SPICE Simulation -->
## Day 3 SPICE Simulation

### Features of OpenLane Flow

In OpenLane flow, if needed, user is able to change certain flow parameters on the fly. The details of parameters which can be varied during the flow are listed in READ_ME.md file in the configuration folder. 

Command to check existing value of a parameter,

	echo $::env(`parameter_name`)
	
Command to set a value of a parameter,

	set ::env(`parameter_name`)
	
### CMOS Process

A manufacturing process to get final CMOS involves 16 masks. Steps to manufacture a CMOS are,
  
  1. Select a substrate
  - substrate doping level should be less that well doping level
	
  2. Create active region for transistor
  
  3. N-well and P-well formation
  - includes drive-in diffussion and twin tub formation process
	
  4. Formation of Gate
  - form P implant inside P-well
  - form N implant inside N-well
  - deposit polysilicon layer
  - dope polysilicon layer with N impurities to have a low sheet resistance
  - remove polysilicon layer in the areas expect gate region
	
  5. Lightly doped drain formation
  - N impurities to form N- implant inside P-well (N- -> indicates light doping)
  - P impurities to form P- implant inside N-well (P- -> indicates light doping)
	
  6. Source and Drain formation
  - form N+ implant in P-well to get source and drain
  - form P+ implant in N-well to get source and drain

  7. Form contacts and local interconnects
  
  8. Higher level metal interconnect formation
  
	Note: As process moves from bottom to top, the thickness of metal interconnects increases.
	
### SPICE Deck

SPICE deck is a set of commands that includes,
  - Connectivity information
  - Input and output node information
  - VDD and VSS tap information
  - Commands to analyse a circuit
  - Library information

Once the SPICE deck file (.spice extension) is ready, here is the command format to run simulation process,
	
	nspice `name_of_the_spice_deck_file`
	
If an example of a CMOS Inverter is considered then CMOS robustness can be defined with SPICE simulation by determinig parameters such as
  - Switching Threshold (Vm)
  - Rise time and Fall time

### Extract SPICE File from Layout

A custom inverter cell is taken as an example and will be used throughout the flow exercises. Here is an example of how to extract SPICE file from custom inverter cell layout in magic tool. Command to open layout file (.mag file extension) in magic,
	
	magic -T `path_to_tech_file` 'path_to_mag_file` &
	
![](/snapshots_lab_session/Day3/D3_lab_magic_inv_view.JPG)

Inverter cell view in magic tool,

![](/snapshots_lab_session/Day3/D3_lab_magic_inv_view2.JPG)

  - Go to tkcon terminal and enter below commands without quotes
  - enter command, 'extract all' (gives .ext file in the same directory)
  - enter command, 'ext2spice cthresh 0 rthesh 0' (this is to extract parasitics)
  - enter command, 'ext2spice' (should give .spice file in the same directory)

![](/snapshots_lab_session/Day3/D3_lab_tkcon_spice_extract1.JPG)

![](/snapshots_lab_session/Day3/D3_lab_tkcon_spice_extract3.JPG)

![](/snapshots_lab_session/Day3/D3_lab_tkcon_spice_extract4.JPG)

### Example of SPICE Simulation

Here is the example of transient response to characterize the Inverter cell in above-mentioned steps,

Modify the extracted SPICE file so that SPICE deck has,
  - right scaling value
  - scaling value should be according to grid size of the layout
    - grid size of the layout can be checked in the tkcon window
  - MOS model names should be as mentioned in the library
  - specify power supply and ground in the design
  - specify pulse input signal
  - include a command to perform transient analysis

![](/snapshots_lab_session/Day3/D3_lab_inv_spice_deck_example.JPG)

Run a spice file with ngspice tool,

![](/snapshots_lab_session/Day3/D3_lab_ngspice_inv_run.JPG)

Plot transient response out of SPICE simulation using this command format,
	
	plot `output_node` vs time `input_node`

![](/snapshots_lab_session/Day3/D3_lab_inv_transient_response.JPG)

Calculate rise time, fall time and propagation delay from transient response,

Output Rise Time: 0.04218 nsec

![](/snapshots_lab_session/Day3/D3_lab_inv_out_rise_time.JPG)

Output Fall Time: 0.02754 nsec

![](/snapshots_lab_session/Day3/D3_lab_inv_out_rise_time.JPG)

Input Rise Time: 0.05997 nsec

![](/snapshots_lab_session/Day3/D3_lab_inv_in_rise_time.JPG)

Input Fall Time: 0.05988 nsec

![](/snapshots_lab_session/Day3/D3_lab_inv_in_fall_time.JPG)

Propagation delay when output is rising: 0.0335 nsec

![](/snapshots_lab_session/Day3/D3_lab_inv_propagation_out_rise.JPG)

Propagation dealay when output is falling: 0.00412 nsec

![](/snapshots_lab_session/Day3/D3_lab_inv_propagation_out_fall.JPG)

<!-- Day 4 PnR and Timing Analysis -->
## Day 4 PnR and Timing Analysis

### PnR and LEF Files

Place and Route (PnR) is an automated but an iterative process. It does not need any circuit information other than input, output, PRboundary, power rail and ground rail. Tracks are used in the route process and some of the basic guidelines to follow in route process are,
  - Input and output port should lie on the intersection of vertical and horizontal tracks on the inteconnect layer
  - Width of the standard cell should be in odd multiples of horizontal track pitch
  - Height of the standard cell should be in odd multiples of vertical track pitch
  
LEF files contain information necessary for PnR process. There are two types of LEF files, one is 'tech lef', which includes layer information, DRC rules, via information and the other is 'cell lef' which contains abstract information of standard cells. As LEF files does not contain any logic design information, they protect design IP. 

### Example to Check PnR Tool Requirements

Here is an example to check with the help of guidelines if PnR tool requirements are satisfied by above-mentioned custom inverter cell.
  
  - To verify if ports lie on the interconnect layer, track information and layout grid should be converged
  
  The offset value (distance from the origin to the routing track) and the pitch value (centre-to-centre distance between the routing tracks) along x and y direction are defined in the track information file. Typical data format in track info file is,
	
	Layer Direction Offset Pitch
  
  ![](/snapshots_lab_session/Day4/D4_lab_track_info_file.JPG)
  
  command to converge grid and track information,
	
	grid [Xspacing[Yspacing[Xorigin Yorigin]]]
	
  ![](/snapshots_lab_session/Day4/D4_lab_li1_intersection_check.JPG)
  
  In the above snapshot it can observed that the ports lie on the intersection of horizontal and vertical tracks.
  
  - To verify if width is in odd multiples of x direction pitch value, count the number of blocks within PRboundary marked in white lines in below image.
  
  ![](/snapshots_lab_session/Day4/D4_lab_width_check.JPG)  
  
  - In the same way, guideline for the cell height can also be verified.
  
### Cell LEF File Extraction

Open a custom inverter cell design in magic tool, command format to extract lef is,
	
	lef write `desired_file_name`
	
	Note: If file name is not specified then magic uses same file name as that of the .mag file.
	
![](/snapshots_lab_session/Day4/D4_lab_lef_file_extract.JPG)

![](/snapshots_lab_session/Day4/D4_lab_lef_file_extract1.JPG)

### Plug Custom Cell into Existing Design

In order to include custom cell design into an existing design, 'config.tcl' of the existing design should be modified to include,
  - libraries that contain custom cell information
  - extracted lef file of the custom cell
  
Here is an example of modifies 'config.tcl' file,

![](/snapshots_lab_session/Day4/D4_lab_config_file_modifications.JPG)

	Note: If tagged folder is used for the openlane flow then make sure that overwrite argument is passed in design setup stage. Overwrite argument will overwrite existing data in the runs folder with latest configuaration form config.tcl file.
	
Additional commands before the synthesis run,

![](/snapshots_lab_session/Day4/D4_lab_commands_before_run_synthesis.JPG)

Check synthesis logs to verify that the custom cell is added to the design.

![](/snapshots_lab_session/Day4/D4_lab_custom_cell_addition_to_synthesis.JPG)

Although cell is added to the design negative slack values are reported after the successful synthesis run.

![](/snapshots_lab_session/Day4/D4_lab_slack_values_after_synthesis.JPG)

tns => Total Negative Slack (addition of all negative slacks)
wns => Worst Negative Slack

### Fix Slack Violations (Pre-Synthesis)

	Note:
	
	Difference between data arrival time and data required time is called as slack.
	In the design, slack values should either be positive or zero.
	

Details of parameters related to synthesis configuration are listed in READ_ME.md file in '/configurations' folder. The value of these parameters could be changed and its effect could be analysed on the design. For example, strategy for synthesis like time focused or area focused could be set with SYNTH_STRATEGY parameter whereas enabling SYNTH_BUFFERING adds buffers along the path to reduce delay. 

	Note:

	In IC design, designers should always strike a right balance of time, area and power requirements. For example, addition of buffers do reduce time delay but that is at the cost of area on the chip.
	
- Set synthesis configuration
- Run synthesis process again

Slack values were improved by changing the synthesis configuration,

![](/snapshots_lab_session/Day4/D4_lab_slack_reduction_step1.JPG)
![](/snapshots_lab_session/Day4/D4_lab_slack_reduction_step1_note.JPG)

- run floorplan process
- run placement process

Verify that custom cell is included in the PnR flow using magic tool.

### Fix Slack Violations (Post-Synthesis)

For post-synthesis analysis, OpenSTA tool can be used. OpenSTA is an external tool and runs outside the OpenLane flow which means STA (Static Timing Analysis) tool does not have access to configuration parameters in OpenLane flow. Therefore, in order to run STA with OpenSTA,

  - Create a .sdc file which includes OpenLane flow parameters and necessary additional parameters (Defaults settings used by OpenLane can be found in ../openlane/scripts/base.sdc)
  - Create a .conf file to configure analysis with OpenSTA
  - Refer / add .sdc file from first step into .conf file
  - In addition to that, .conf file should have,
    - path of netlist file 
    - link to design
    - path of libraries	

Example of a .conf file,

![](/snapshots_lab_session/Day4/D4_lab_presta_conf_modifications.JPG)

to run STA use command format,

	sta `config_file_name`
	
![](/snapshots_lab_session/Day4/D4_lab_invoke_sta.JPG)

	Note:
	
	At this point, slack values should be same as the values obtained at the end of pre-synthesis timing analysis process because same netlist file is used here.
	
Statistic displayed on the screen shows that delay is high for the huge fanout nets (fanout value of 6 or more). Also, higher delay value is observed when input slew is high or output capacitance is high.

Exit OpenSTA tool.

  - How to Change fanout of Nets
  
  In the OpenLane, synthesis process gives an option to set maximum fanout. For parameter details, check READ_ME.md in '/configurations' folder.
  
  - Go back to OpenLane flow
  - Change SYNTH_MAX_FANOUT parameter value, for example, here it is set to 4.
  - Run sysnthesis step again because the only netlist will be updated with changes of the fanout parameter.
  - On successful run, exit flow and go back to OpenSTA tool.

Re-run .conf file through STA tool because netlist is updated with new fanout condition.

![](/snapshots_lab_session/Day4/D4_lab_high_fan_out_net_example.JPG)

High input slew can be observed on the buffers in the design, for example, sky130_fd_sc_hd__buf_1. 

  - How to Replace Cell in STA
  
OpenSTA allows user to check connections of particular net, and to replace particular cell in the design and observe its effects.

![](/snapshots_lab_session/Day4/D4_lab_net_conn_in_sta.JPG)

Upsizing buffer is one of the option to improve timings in the design but also remember that high strength buffers occupy more area on the chip. 

![](/snapshots_lab_session/Day4/D4_lab_replace_cell_in_sta.JPG)

Results after buffer cell is replaced with higher strengh buffer from the library,

![](/snapshots_lab_session/Day4/D4_lab_results_after_replace_cell.JPG)

These steps can be repeated multiple times to get improved slack values but keep in mind the downfalls as well.

  - Netlist from OpenSTA
  
Replacing cell changes the netlist of the design. OpenSTA allows to write this netlist into a file. In OpenSTA tool use command format,

	write_verilog `path_to_synthesis_file_in_flow`
	
![](/snapshots_lab_session/Day4/D4_lab_improved_slack_netlist_to_openlane_flow.JPG)

One can easily verify if netlist is overwritten,

  - open netlist file, ' less netlist_file_name'
  - search for the replacement cell name in the file
  - if it is there, netlist is successfully written.
  
Now, coming back to OpenLane flow,

  - run floorplan
  - run placement
    - one may notice number of iterations to converge the design are more and core has increased because bigger bufferes are used.
	
### CTS Timing Analysis

  - Run CTS Process

Parameters of the CTS (Clock Tree Synthesis) process can be checked in the READ_ME.md file at '.. /configurations' directory. QoR stands for quality of result, lower the value of QoR degraded are the process results but on other hand higher QoR puts more run overhead. Use command,

	run_cts
	
In CTS process, clock buffers are added to the design and that results into new netlist. One should see additional netlist file in '../results/synthesis' folder.
This new netlist contains information from previous netlist plus netlist information of clock buffers. And the new .def file created at this step can be checked using magic tool.

  - Timing Analysis in OpenLane Flow

OpenSTA tool is integrated into OpenRoad application therefore one can do timing analysis using OpenSTA within OpenLane flow. Open openlane flow and to invoke to openroad, use command,
	
	openroad

A database needs to be created to run analysis in openraod. Databsse contains information from .lef and .def generated from the cts run. As .lef contains technology related information, it does not change. Whereas .def file changes only when new component or cell is added to the design. Only in such scenarios database has to be recreated.

	read_lef `path_to_merged_lef_file`
	
	read_def `path_to_cts_def_file`
	
	write_db `custom_db_name`
	
Once, database is created, here are the setup steps for timing analysis,

  - read_db `db_file_name`
  - read_verilog `netlist_file_after_cts_run`
  - read_liberty `path_to_typical_cell_library`
  - link design
  - read_sdc `path_to_custom_sdc_file`
  - enter analysis command
  
![](/snapshots_lab_session/Day4/D4_lab_openroa_with_typical_lib1.JPG)

![](/snapshots_lab_session/Day4/D4_lab_cal_set_delay_in_openroad.JPG)

The positive slack values can be seen the displayed statistics.


<!-- References --> 
## References
	1. ASIC flow: https://www.einfochips.com/blog/asic-design-flow-in-vlsi-engineering-services-a-quick-guide/
	
	2. Skywater's PDK: https://www.youtube.com/watch?v=EczW2IWdnOM&feature=youtu.be
	
	3. OpenLane flow: https://www.youtube.com/watch?v=Vhyv0eq_mLU
	
	4. OpenLane Installation: https://github.com/efabless/openlane
	
	5. OpenLane Installation: https://github.com/nickson-jose/openlane_build_script
	