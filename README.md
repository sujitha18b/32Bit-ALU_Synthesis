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
ALU.v:
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

ALU_run.tcl:
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

input_constraints:
```
create_clock -name clk -period 10 [get_ports clk]
set_input_delay 2.0 -clock clk [get_ports {a b op}]
set_output_delay 2.0 -clock clk [get_ports {result carry_out zero_flag}]
set_driving_cell -lib_cell INVX1 [get_ports {a b op}]
set_load 0.1 [get_ports {result carry_out zero_flag}]
```
#### Synthesis RTL Schematic :
![Screenshot 2025-05-22 124904](https://github.com/user-attachments/assets/992fcf37-8117-4961-ba75-979cce521345)

#### Area report:
![Screenshot 2025-05-22 124924](https://github.com/user-attachments/assets/75c79a2f-5568-4e16-aeec-04df00ed7bcd)

#### Power Report:
![Screenshot 2025-05-22 124936](https://github.com/user-attachments/assets/8507d637-1ed4-4dbb-980f-70ce42c4b83f)

#### Result: 

The generic netlist of 32 bit ALU  has been created, and area, power reports have been tabulated and generated using Genus.
