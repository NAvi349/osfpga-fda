
## Day 1

### FPGA Introduction

#### Flow

 We shall be looking at a 4-bit counter example and the RISC-V RVMyth Processor through two flows:
- Vivado
- Skywater OpenSource FPGGA (SOFA)

#### FPGA and FPGA Architecture

Programmable Logic Devices.
- The hardware can be customized after the manufacturing by programming the device
- PLA - Produces Minterms, Variable AND Gates, Input to OR Gates is fixed. But sequential elements cannot be programmed using this
- CPLD - Complex Programmable Logic Devices
- FPGA - Field Programmable Gate Arrays

FPGA consists of Lookup Tables, Flip Flops and CLBs (Configurable Logic Blocks)
#### ASIC Vs FPGAs

| ASIC                          | FPGA           |
| ---------------------         |:-------------: |
| Not programmable              | Reprogrammable |
| Final Stage implementation    | Useful in prototyping a design|  
| Huge time required to design  | Less Time Comparatively      |
| RTL to Layout                 | RTL to Bitstream generation |

#### Applications
- FPGAs are good for implementing algorithms which are able to run parallely.
- Hardware acceleration implementation of **Neural Networks**.
- Embedded Systems
- Machine Learning

#### FPGA Architecture

- Can be used to implement any combinatorial and sequential design.
![image](https://user-images.githubusercontent.com/66086031/171312512-8e650388-62ec-40c5-9e9d-f9fd99baa14f.png)

(Diagram taken from Course Slide for reference)

An FPGA Architecture consists of

- **Configurable Logic Blocks** 
	* It contains an N-input Lookup Tables, Carry chain, Multiplexer and a Flip Flop. 

- **Programmable Interconnects**
	* The wires which connect the CLBs.
	
- **Programmable I/O**

#### Bitstream

- User generates a bitstream for their design specification, and uploads it into the board.
- Bitstream describes what CLBs that have to be connected and their function.
- It also defines the connection between the CLBs and I/Os.

#### Lookup table

- The Lookup table consists of all the possible minterms of a N-input function.
- Basically a function can be expressed as Sum of Products form or Sum of Minterms form.
- If we want to implement a logic function, the corresponding minterms of the function are selected by using the Mux.
- The output of the Mux feeds into the Flip Flop.
- A N - input Mux can be used to implement any N - input Function or some N + 1 input function with an additional NOT gate.

![image](https://user-images.githubusercontent.com/66086031/171421974-95cb4404-285a-4fcf-87a7-1e1e86fa5db9.png)

#### FPGA Programming

- HDL - Verilog
- HLS - C/C++/Python

#### FPGA Design Methodology

```mermaid
graph TD;
	A[Architectural Description] --> B[RTL Design and testbench]
	B --> C[Behavioral Simulation]
```

![image](https://user-images.githubusercontent.com/66086031/171422258-df96f7d1-5377-49af-ae48-7eba5c7922ce.png)

#### Synthesizable and Non - Synthesizable

**The following are non-synthesizable:**
- User defined delays
- Inital block
- nmos, pmos primitives
- Dynamic Memory allocation
- Infinite loops

#### Basys 3 FPGA Board

![image](https://user-images.githubusercontent.com/66086031/171423298-d47564ea-52dd-4e3a-b77a-1f218a44c157.png)
(Image taken from slide for reference)

#### Tools used

- Xilinx Vivado 2019.2 - https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/vivado-design-tools/archive.html

#### Different ways of programming

- Local Programming using a board 
- Remote Programming - A FPGA connected to a remote server: I/O through Virtual I/O.

### Counter Example in Vivado

- We use a frequency synthesizer to generate a slow clock to observe the outputs

<details>
	<summary> Code </summary>
		
module counter(clk,reset,count);
input clk,reset;
output reg [3:0] count = 4'b0000;
reg [25:0] count_reg;
reg clk_div = 1'b0;

always @ (posedge clk)
begin
if (reset)
    begin
        clk_div <= 1'b0;
        count_reg <= 26'd0;
     end
else
    begin
        count_reg <= count_reg + 1;
        if (count_reg == 26'h3ffffff) // for synthesis
    //   if (count_reg == 26'd12) // for simulation
        begin
            clk_div <= ~ clk_div;
            count_reg <= 26'd0;
        end
    end
 end
  
	 always @ (posedge clk_div) begin
		 if (reset)
		 begin
		    count <= 4'b0000;
		 end
		 else
		 begin
		    count <= count + 1'b1;
		 end
	 end
endmodule			    
	
</details>
			    
			    
### Vivado-counter example

i. Type ```vivado``` in the terminal.
	
ii. Create a new project.

iii. Do the following settings
![image](https://user-images.githubusercontent.com/66086031/171430681-824be24d-7f03-4c07-82f8-d7fe3da126c9.png)
	
iv. Click finish.

v. Add source files



	

			    

### Virtual Input/Output Counter


## Acknowledgements

- Dr. Xifan Tang, OpenFPGA and Chief Engineer RapidSilicon



