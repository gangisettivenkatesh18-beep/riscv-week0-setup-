# Day 1: Introduction to Verilog

## Objectives
- Learn the basics of Verilog HDL.
- Understand the structure of a simple Verilog module.
- Write and simulate a basic RTL design.

## Topics Covered
1. Introduction to Verilog
2. Structure of a Verilog Program
3. Writing a simple combinational circuit
4. Simulation using Icarus Verilog & GTKWave

## Example: 2-to-1 Multiplexer

### Code (mux.v)
Verilog


module mux (
    input wire a,
    input wire b,
    input wire sel,
    output wire y
);
    assign y = sel ? b : a;
endmodule
---
TEST BENCH
---
    `timescale 1ns/1ps
    module counter_tb;

    reg clk, reset;
    wire [3:0] count;

    // Instantiate counter
    counter uut (
        .clk(clk),
        .reset(reset),
        .count(count)
    );

    // Clock generation
    always #5 clk = ~clk;

    initial begin
        $dumpfile("counter.vcd");
        $dumpvars(0, counter_tb);

        // Initialize
        clk = 0; reset = 1;
        #10 reset = 0;

        // Run simulation
        #100 $finish;
    end
    endmodule
    
SIMULATION STEPS
---
Compile and run with Icarus Verilog
Bash


iverilog -o counter.out counter.v counter_tb.v
vvp counter.out
View waveform in GTKWave
gtkwave counter.vcd
<a href="Day_1/mux_yosys.png" target="_blank">
  <img src="https://res.cloudinary.com/dg6kvs6ij/image/upload/v1758956499/Screenshot_2025-09-25_174832_foafup.png
" alt="MUX schematic" width="600" >
</a>

SYNTHESIS  LAB YOSYS
---
### Synthesis with Yosys

1. Install yosys if not already installed:
     
Bash


 sudo apt-get install yosys
2.Run yosys and read the Verilog design:
y
osys
yosys> read_verilog mux.v
yosys> synth -top mux
yosys> show
3.The synth command performs synthesis, and show generates a logic schematic for the module.
By default, it opens a schematic in a GUI (using Graphviz).

4.You can also write the synthesized netlist into a file:
yosys> write_verilog mux_netlist.v

OUTPUT FORM
---

<a href="Day_1/mux_yosys.png" target="_blank">
  <img src="https://res.cloudinary.com/dg6kvs6ij/image/upload/v1758956500/Screenshot_2025-09-25_184136_mrotyu.png
" alt="MUX schematic" width="600" >
</a>

---

6.EXPECTED OUTPUT

1.Yosys will display a synthesis report in the terminal.

2.A schematic window showing the 2-to-1 multiplexer with inputs a, b, sel and output y.

3.Netlist (mux_netlist.v) is generated with only primitive gates.

---
7.SUMMARY OF DAY 1

1.Learned Verilog basics.

2.Designed and simulated a 2-to-1 MUX.

3.Synthesized the design with Yosys.
