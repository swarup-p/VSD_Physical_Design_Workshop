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
	<li><a href="#references">References</a></li>
  </ol>
</details>

<!-- Abstract -->
## Abstract

Physical design is a process in VLSI in which a structured netlist from front end RTL design team is transformed into high-quality placement and routing by back-end design team to convert into geometrical design informatiion of all physical layers which is used to tape-out chips. ASIC design flow brings challenges and surprises at every step of the design cycle. The steps involved in IC physical design process and the tools to overcome the challenges faced in this process, from a RTL netlist to final tape-out, are introduced during a workshop. A hands-on in the physical design and the characterization of a standard cell built on Google-Skywater's open source 130nm process design kit with the help of Openlane flow, a fully-automated RTL to GDSII flow is one of the highlights of the workshop.

<!-- RTL to GDSII Flow -->
## RTL to GDSII Flow
RTL to GDSII is a process to convert behavioural logic written in high level languages to physical layout. RTL (register transfer level) defines a digital circuit design as a sequence of steps of data flow from one register to other register and logical operations that are performed on this data. Hardware Descriptive languages such as VHDL, Verilog are used to create high-level of representation of a circuit or also known as a netlist from a design at the RTL level. Whereas GDSII (Graphical Data Stream Information Interchange) file is a final output of IC design cycle. It is a standard database file format that foundries use to exchange information related to physical layout. It is a binary file format that contains information like geometric shapes of layers, text labels.

Here are the basic steps invoved in the process to realize functional ASIC,

	1. Chip Specification: Architecture, functionalities and specifications of the ASIC are defined at this stage.
	
	2. Design Entry / Functional Verification: The functional and logical behaviour of the circuit is confirmed by a simulation of the RTL code.
	
	3. RTL block synthesis: The RTL code is converted into gate-level netlist using a logical synthesis tool that meets required time contraints.
	
	4. Chip Partitioning: ASIC design is partiioned into functional block and analysed the feasibility of reusing IPs from previous projects or using third party IPs.
	
	5. Design For Test (DFT) Insertion: To figure out faults in the chip at early development stages high quality test techniques are introduced.
	
	6. Floor Planning (blueprint of a chip): Physical implementation starts at this stage. Objective of the floorplan is to plan silicon area and robsut power distribution network to power whole chip.
	
	7. Placement: It involves placement of standard cells on the rows formed in floorplan.
	
	8. Clock Tree Synthesis (CTS): Create a clock distribution network to deliver clock to all sequential elements in required time, area and with low power consumption.
	
	9. Routing: Find optimised ways to interconnect metal layers with valid patterns, here specifications defined in PDK such as minimum width, via spacing are used to inteconnect metal layers.
	
	10. Sign Off: Routed layers undergo physical verification known as signoff checks to avoid any errors just before tapeout.
	
Please refer to: https://www.einfochips.com/blog/asic-design-flow-in-vlsi-engineering-services-a-quick-guide/

<!-- References --> 
## References
	1. https://www.einfochips.com/blog/asic-design-flow-in-vlsi-engineering-services-a-quick-guide/
