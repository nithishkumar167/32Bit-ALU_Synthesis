# 32Bit-ALU_Synthesis

## Aim:

Synthesize 32 Bit ALU design using Constraints and analyse area and Power reports.

## Tool Required:

Functional Simulation: Incisive Simulator (ncvlog, ncelab, ncsim)

Synthesis: Genus

### Step 1: Getting Started

Synthesis requires three files as follows,

◦ Liberty Files (.lib)

◦ Verilog/VHDL Files (.v or .vhdl or .vhd)

### Step 2 : Performing Synthesis

The Liberty files are present in the library path,

• The Available technology nodes are 180nm ,90nm and 45nm.

• In the terminal, initialise the tools with the following commands if a new terminal is being
used.

◦ csh

◦ source /cadence/install/cshrc

• The tool used for Synthesis is “Genus”. Hence, type “genus -gui” to open the tool.

• Genus Script file with .tcl file Extension commands are executed one by one to synthesize the netlist.

## ALU.v:
```
module ALU (
    input  [3:0] A, B,
    input  [2:0] ALU_Sel,
    output reg [3:0] ALU_Out,
    output reg CarryOut
);
    always @(*) begin
        case (ALU_Sel)
            3'b000: {CarryOut, ALU_Out} = A + B;
            3'b001: {CarryOut, ALU_Out} = A - B;
            3'b010: ALU_Out = A & B;
            3'b011: ALU_Out = A | B;
            3'b100: ALU_Out = A ^ B;
            3'b101: ALU_Out = ~A;
            3'b110: ALU_Out = A << 1;
            3'b111: ALU_Out = A >> 1;
            default: ALU_Out = 4'b0000;
        endcase
    end
endmodule
```
## ALU_run.tcl:
```
read_libs /cadence/install/FOUNDRY-01/digital/90nm/dig/lib/slow.lib
read_hdl ALU.v
elaborate
read_sdc alu_design_constrains.sdc 
syn_generic
report_area
syn_map
report_area
syn_opt
report_area 
report_area > alu_area.txt
report_power > alu_power.txt
write_hdl > alu_netlist.v
gui_show
```
## input_constraints:
```
create_clock -name clk -period 10 [get_ports clk]
set_input_delay 2.0 -clock clk [get_ports {a b op}]
set_output_delay 2.0 -clock clk [get_ports {result carry_out zero_flag}]
set_driving_cell -lib_cell INVX1 [get_ports {a b op}]
set_load 0.1 [get_ports {result carry_out zero_flag}]
```

#### Synthesis RTL Schematic :

![Screenshot (78)](https://github.com/user-attachments/assets/e0fe8f62-8513-4d2c-b12d-72a1c3025308)

#### Area report:

![Screenshot (75)](https://github.com/user-attachments/assets/5f702150-2f0d-4d6b-b9a6-41e2d5d175e5)

#### Power Report:

![Screenshot (74)](https://github.com/user-attachments/assets/df763a58-00ec-4aa5-9773-afbd48caae12)

#### Result: 

The generic netlist of 32 bit ALU  has been created, and area, power reports have been tabulated and generated using Genus.
