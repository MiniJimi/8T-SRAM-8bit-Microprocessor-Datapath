# 8T-SRAM-8bit-Microprocessor-Datapath
Advanced VLSI - Design and Characterization of a CMOS 8-bit Microprocessor Data Path

Project Summary
This project involves the schematic and layout design of an 8-bit microprocessor data path, including an ALU, a barrel shifter, and a register file using CMOS circuitry and Cadence VLSI design tools.

Data Path Overview
The data path is the core element of a microprocessor that can perform many of functions, including arithmetic and logic evaluation, data movement, and storage of results in a memory address specified by the instruction. In this design project, teams must design a complete 8-bit data path consisting of a register file, an ALU (arithmetic logic unit) and a shifter, as shown in Figure 1. The data path will be capable of multiple instructions determined by several control signals that comprise the operational code or “opcode”.
The ALU can perform both arithmetic and bit-wise logical operations. The ALU will have two 8-bit data inputs and several control signals that specify the ALU function to be executed in a given clock cycle. The ALU input data is loaded into the latch at the rising edge of the ALU clock (clk2), and the output is generated after a delay due propagation through the combinational logic of the ALU and the shifter. The central component of an ALU is an adder, and for this project a Manchester carry look-ahead adder will be used.
A shifter provides data manipulation capabilities. In this design project, a logarithmic barrel shifter will be used.
The register file is a block of memory that provides the data inputs to and stores outputs from the ALU. The register file at read/write address is specified by control bits included in the opcode. In this project, the register file and ALU/shifter will be synchronized by two non-overlapping clock phases that determine when data is latched into the register file (clk1) and into the ALU (clk2).

Data Path Design Specifications
The following specifications must be achieved by the data path you design.
• bit width: 8
• inputs: two 8-bit words (from the memory bus, A and B), 3 to 5 control bits for the ALU, 3 to 5 control bits for the shifter, 11 control/address bits for the register file, two non-overlapping clocks (clk1, clk2).
• outputs: 8-bit word (F), 1 carry-out
• functions: must implement at least the following functions:
ALU functions
o A NOR B
o A XOR B
o Increment A
o Decrement A
o A NAND B
o A + B
o A - B
o NOT A (invert)
If you implement only these functions, you must do so with only three ALU control signals. There is no explicit Cin bit; Cin must be generated from the ALU control bits.
Shifter
The shifter must implement at least the following functions:
o Pass (no shift)
o Shift_Left_1 (LSL1)
o Shift_Left_2 (LSL2)
o Shift_Right_1 (LSR1)
o Shift_Right_2 (LSR2)
(LSR
If you implement only these functions, you must do so with only three Shift control signals.
Expanded functions
Up to two function expansion bits are provided for each the shifter and the ALU to implement additional functions (a total of 4 possible additional expansion bits), provided that all of the expanded functions are utilized and characterized, including the timing data of the function on the extracted circuit. Please be cautious when choosing to implement additional functions, as this additional characterization at the end can be very time consuming.
Register file
The 8x8 SRAM register file must implement 2-port read_from_memory and 1-port write_to_memory functions. Each clock cycle will be either a read or a write cycle, so two cycles will be needed to complete a function.
• power supply: VDD = 3 volts, referenced to 0 volt ground.
• size specifications: no specific layout size is required, but optimization of physical size is a design priority and higher scores will be awarded to groups with smaller layouts. The timing specifications: signal timing requirements are defined below. The worst case propagation delay for all ALU+shifter functions measured from the extracted layout must be less than 50ns.

Data Path Layout
The 8-bit datapath can be implemented by constructing a 1-bit slice horizontally and stacking 8 one-bit slices vertically.  The control signals run vertically across each slice. You may implement some functional blocks as 8 bits, but make sure you organize it in 1-bit slices that can be pitch-matched to the other functional blocks.

Register File Description
The 8-bit register file should be implemented as an 8x8 SRAM. Since the SRAM is small, you do not have to implement a sense amplifier. The register file should have two 8-bit read (output) ports and one 8-bit write (input) port. Input to the register file can come either from the ALU output or from an external memory bus, as selected by a MUX at the input of the register file. The memory bus should be implemented as an 8-bit input word. An 8-bit latch should be included to store the register file input word, which, during a write cycle, will be saved to the SRAM block.
The register file uses an enable signal for the general task of enabling inputs to and outputs from the register file at the proper time. The register file must be carefully designed to properly implement the enable function. In the read cycle, register file output is passed to the ALU when enable is high; otherwise that output should be at high impedance. The output should also be at high impedance during the entire write cycle, when the R/W signal is high. In the write cycle, data is stored in the SRAM only when enable is high; otherwise the data from the register file latch should not be allowed to affect the SRAM. To accomplish this, the enable signal should act on the SRAM address decoder outputs. The enable function allows the data path to implement functions that do not act on the register file, such as branches or status register checks, although these functions are not implemented in this design project.
The register file address actually contains two independent 3-bit addresses. Address A should specify the address for data passed to ALU input A, and address B should specify ALU input B. Address A is also used to specify the address for storing data to the SRAM during a write cycle. In a realistic data path the timing should allow the value of address A to be changed before the write cycle


Clock/Timing Description
Two non-overlapping clock phases are required for the data path, as shown in Fig. 3. Data from the ALU/shifter or memory is loaded in the register file latch when clk1 is high and held there while clk1 is low. Data from the register file is loaded in the ALU latch when clk2 is high and held there when clk2 is low. Two cycles of the clocks are needed to complete a data path function; data is passed to and through the ALU/shifter in the “read” cycle, and ALU/shifter data is stored in the register file on the “write” cycle. Address and control signals for all data path functions must be synchronized to the clock signals.

Project Requirements
The following cell views for all circuit blocks must be created using Cadence tools and stored in your group project directory:
• schematic
• symbol
• layout
• extraction
The complete data path cell must pass the following tests and verifications:
• functional simulation of all data path functions
• DRC
• LVS
Input and output signals must be named as follows:
• Inputs
o md<0:7>, memory bus data
o rfs, reg. file mux selection
o rfen, reg. file enable
o rw, read/write
o ada<0:2>, register address A
o adb<0:2>, register address B
o f<0:9>, ALU and shifter controls, including optional expanded function controls
o clk1, clk2
• Outputs
o Y<0:7>
o Cout
The following characteristics must be measured and reported on the extracted layout:
• slowest logic function and propagation delay for that function (ALU only, from extracted layout)
• slowest arithmetic function and propagation delay from that function (ALU only, from extracted layout)
• slowest propagation delay of the data path (ALU + Shifter, from extracted layout)
Propagation delays are measured from clock edge to last output transition.
• physical area of the complete data path layout
• total number of transistors in the data path (available from the final LVS output)
Circuit Requirements:
• a Manchester carry generation carry look-ahead adder must be implemented
• a logarithmic barrel shifter must be implemented
• the register file memory must be implemented with a multi-port 8x8 SRAM
• minimum sized transistors can be used, but are not required
• when possible, existing primitive cells (INV, NAND, etc.) should be used to construct circuit blocks, and any new primitive cells must be designed at the transistor level
• all required functions must be implemented using only the specified control/select inputs
• you must employ hierarchical design where you instantiate lower level cells into higher level circuit block, both in schematic and layout design
• buffers should be designed and included wherever fan-out is larger than 5 gates, or wire lengths exceed 100 um
Layout Requirements:
• follow all previously established layout guidelines for cell size (pitch), internal routing, input/output pad placement, etc.
• all designed circuit schematics must be laid out, must pass DRC, and must pass LVS
all functional blocks should be designed using only metal1 and metal2, reserving metal3 for routing chip-level control and data signals in the top level of hierarchy.
• all cell I/O signals and power traces must be connected to single points in the final cell, i.e., you can not have multiple ports for VDD, Ground, or any I/O signal.
• attempt to consistently route metal1 and metal2 in perpendicular directions. you may need to twist cells around and stack them in seemingly odd ways to achieve your goals, but always make sure you have good power supply to all cells and that you have good access to run metal2 and metal 3 in a single direction in your final data path cell.
