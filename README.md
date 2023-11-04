# pes_sipo

## Serial in Parallel out Shift Register

The shift register, which allows serial input (one bit after the other through a single data line) and produces a parallel output is known as the Serial-In Parallel-Out shift register. The logic circuit given below shows a serial-in-parallel-out shift register. The circuit consists of four D flip-flops which are connected. The clear (CLR) signal is connected in addition to the clock signal to all 4 flip flops in order to RESET them. The output of the first flip-flop is connected to the input of the next flip flop and so on. All these flip-flops are synchronous with each other since the same clock signal is applied to each flip-flop. 
![](https://github.com/Akshay1000101/pes_sipo/blob/main/glsscreenshots/sipoimage.png?raw=true)
The above circuit is an example of a shift right register, taking the serial data input from the left side of the flip-flop and producing a parallel output. They are used in communication lines where demultiplexing of a data line into several parallel lines is required because the main use of the SIPO register is to convert serial data into parallel data.

the code is
![](https://github.com/Akshay1000101/pes_sipo/blob/main/glsscreenshots/code%20new%20correct.PNG?raw=true)
First read the design and testbench using the command

iverilog pes_sipo.v pes_sipo_tb.v

./a.out

then open the waveform using gtkwave
gtkwave sipo.vcd
![](https://github.com/Akshay1000101/pes_sipo/blob/main/glsscreenshots/waveform.PNG?raw=true)
Then synthesize the design and generate the netlist file
read_liberty -lib sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog pes_sipo.v
synth -top pes_sipo
abc -liberty sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr pes_sipo_net.v
![](https://github.com/Akshay1000101/pes_sipo/blob/main/glsscreenshots/yosys%201.PNG?raw=true)
![](https://github.com/Akshay1000101/pes_sipo/blob/main/glsscreenshots/yosys%202.PNG?raw=true)
![](https://github.com/Akshay1000101/pes_sipo/blob/main/glsscreenshots/yosys%203.PNG?raw=true)

### Gate Level simulation
iverilog primitives.v sky130_fd_sc_hd.v pes_sipo_net.v pes_sipo_tb.v
gtkwave pes_sipo.vcd
![](https://github.com/Akshay1000101/pes_sipo/blob/main/glsscreenshots/gate%20level%20simulation.PNG?raw=true)

The Synthesis and Simulation Waveforms are matching

OpenLANE is a comprehensive digital ASIC design flow consisting of multiple stages. By default, all these flow steps are executed sequentially. Each stage can contain various sub-stages. OpenLANE can also be run interactively. The typical OpenLANE flow includes the following stages:

1. Synthesis:
   - Yosys: Performs RTL synthesis using GTech mapping.
   - abc: Performs technology mapping to standard cells described in the Process Design Kit (PDK), allowing customization of synthesis techniques for desired results.
   - OpenSTA: Conducts static timing analysis on the generated netlist to produce timing reports.
   - Fault: Introduces scan-chain insertion for post-fabrication testing, supporting Automatic Test Pattern Generation (ATPG) and test patterns compaction.

2. Floorplan and Power Distribution Network (PDN):
   - Init_fp: Defines the core area for the macro, as well as rows (used for placement) and tracks (used for routing).
   - Ioplacer: Places the macro's input and output ports.
   - PDN: Generates the power distribution network.
   - Tapcell: Inserts welltap and decap cells in the floorplan.

3. Placement:
   - Placement is carried out in two steps: global placement and detailed placement.
   - RePLace: Performs global placement, which initially places the designs across the chip, sometimes resulting in standard cell overlaps.
   - Resizer: Applies optional optimizations to the design.
   - OpenPhySyn: Executes timing optimizations on the design.
   - OpenDP: Performs detailed placement to legalize the globally placed components.

4. Clock Tree Synthesis (CTS):
   - TritonCTS: Synthesizes the clock distribution network.

5. Routing:
   - FastRoute: Conducts global routing to generate a guide file for the detailed router.
   - TritonRoute: Performs detailed routing based on the global routing guides.
   - SPEF-Extractor: Extracts Standard Parasitic Exchange Format (SPEF) data, including parasitic information.

6. GDSII Generation:
   - Magic: Exports the final GDSII layout file from the routed DEF file.

7. Checks:
   - Magic: Performs Design Rule Check (DRC) and Antenna Checks.
   - Netgen: Performs Layout versus Schematic (LVS) Checks.

This flow provides a systematic and automated approach to the ASIC design process, ensuring that designs meet specified criteria, pass various checks, and are ready for fabrication.

## Physical design
The physical design step is a critical phase that involves converting the logical design of an integrated circuit into a physical representation that can be manufactured. This step typically includes several key processes and employs various EDA (Electronic Design Automation) tools. Firstly, floor planning is performed to allocate space for different functional blocks, ensuring efficient use of silicon real estate. Next, placement tools are used to position standard cells and other components on the chip's layout. Following placement, the routing phase uses tools to create the intricate network of metal traces that connect different components. This stage must adhere to design rules and constraints to avoid signal integrity issues. Finally, sign-off checks, such as Design Rule Checking (DRC) and Layout Versus Schematic (LVS) verification, are conducted to ensure that the physical design complies with manufacturing and electrical design specifications. EDA tools like Cadence, Synopsys, and Mentor Graphics play a significant role in automating and streamlining these processes, ultimately leading to the creation of a manufacturable VLSI chip.
![](https://github.com/Akshay1000101/pes_sipo/blob/main/screenshots2/2.png?raw=true)
![](https://github.com/Akshay1000101/pes_sipo/blob/main/screenshots2/1.png?raw=true)


## Reports
### Area and Statistics


![](https://github.com/Akshay1000101/pes_sipo/blob/main/screenshots2/3.png?raw=true)

## Floorplan and Placement
The floorplan and placement steps are fundamental stages in the chip design process. These steps involve using specialized electronic design automation (EDA) tools to define the physical layout and organization of the components on a semiconductor chip. The floorplan step primarily focuses on allocating and positioning major functional blocks or modules within the chip's layout. Designers need to consider factors such as the chip's size, power distribution, and the proximity of components for efficient communication.

Once the floorplan is established, the placement step comes into play. Placement tools are employed to determine the precise locations of individual cells, standard cells, and macros within the chip area defined in the floorplan. The objective is to optimize chip performance, minimize interconnect delays, and ensure that there is efficient use of space while adhering to design constraints and rules. These tools use algorithms and optimization techniques to generate an initial placement, which designers can then refine iteratively to meet specific design goals. Together, the floorplan and placement stages lay the foundation for the successful implementation of a VLSI chip, ensuring that it meets performance, power, and area requirements.

![](https://github.com/Akshay1000101/pes_sipo/blob/main/screenshots2/4.png?raw=true)
```
magic -T /home/akshay/.volare/sky130A/libs.tech/magic/sky130A.tech lef ../../tmp/merged.nom.lef def pes_sipo.def
```

```
magic -T /home/akshay/.volare/sky130A/libs.tech/magic/sky130A.tech lef ../../tmp/merged.nom.lef def pes_sipo.def
```
## CTS 
The given design does not have any clock defined
![](https://github.com/Akshay1000101/pes_sipo/blob/main/screenshots2/5.png?raw=true)

## Routing
Routing involves connecting the various components and interconnections on a chip. This step is essential to create a physical layout that matches the logical design. VLSI routing is typically performed using specialized Electronic Design Automation (EDA) tools. The process begins with a pre-routed netlist, which contains information about the logical connections between components. The EDA tools then determine the most optimal paths for interconnections, taking into account factors such as signal integrity, power consumption, and area constraints. The tools also need to consider design rules, such as minimum spacing and width of wires, to ensure manufacturability. During routing, the tools may use algorithms like maze routing, detailed routing, and global routing to navigate the chip's layout, ensuring that all connections are made efficiently and without any physical conflicts. Proper routing is crucial for the overall functionality, performance, and manufacturability of the integrated circuit.
![](https://github.com/Akshay1000101/pes_sipo/blob/main/screenshots2/6.png?raw=true)
```
magic -T /home/akshay/.volare/sky130A/libs.tech/magic/sky130A.tech lef ../../tmp/merged.nom.lef def pes_sipo.def
```
## Magic Antenna check
![](https://github.com/Akshay1000101/pes_sipo/blob/main/screenshots2/7.png?raw=true)
files are in the folder screenshots2