
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
	- Set the testbench as top module while during Behavioural Simulation
	
vi. Run Behavioural Simulation
	
![image](https://user-images.githubusercontent.com/66086031/171450253-86659bb2-df54-46eb-b6f3-3fe1002653a6.png)
	- The output counter gets incremented at every posedge of the slow clock


vii. Now set the design as top unit (not the testbench)

vii. Run Elaboration
	
![image](https://user-images.githubusercontent.com/66086031/171467301-0a08fd2e-4885-4799-928c-e92d318f9d98.png)

	
viii. Do the I/O pin assignment as follows and save as ```constraints.xdc``` file.
	
![image](https://user-images.githubusercontent.com/66086031/171456676-0166c665-f6ec-439f-b8ea-9f144d7cb4e1.png)
![image](https://user-images.githubusercontent.com/66086031/171457762-be279c03-ed55-4ecb-8c16-7d7a5545ebed.png)

#### Static Timing Analysis
	
##### Setup Time (Max. Delay) Constraints

![image](https://user-images.githubusercontent.com/66086031/166118620-dd932eef-2e14-4689-a24d-d1e1e2098bcb.png)

- The data should be available **Tsetup**  before the **capturing clock edge** comes.
- We need **fast cells** to satisfy **Max Delay** constraints.
- Setup time constraints are always calculated with respect to the next clock edge.

##### Hold Time (Min. Delay) Constraints

![image](https://user-images.githubusercontent.com/66086031/166118808-82e6ff9a-34c3-4cbc-b40a-23cc1f2faa43.png)

- Data should be stable for **T-hold** after the clock edge to be captured properly.
- If the next data comes **earlier than T-hold**, the current data will not be captured properly.
- We need **slow cells** to satisfy **hold time** constraints

##### Setup Slack

- Setup slack = Required Arrival Time(Clock setup time) - Arrival Time (Data)
- Delay in clock helps us
- Delay in data is not feasible here

##### Hold Slack

- Hold Slack = Arrival Time(Data) - Required Arrival Time(hold time of FF)
- Here delay in data is helpful
- But delay in clock is not desirable.
	
#### Synthesis
	
i. Now run synthesis.
	
![image](https://user-images.githubusercontent.com/66086031/171467889-c8af77a8-afbb-4dc3-946d-716d4f58f411.png)
	
ii. Primary clock is selected.
![image](https://user-images.githubusercontent.com/66086031/171468421-019040e0-79d6-4cd5-9d03-bb461236fee5.png)

iii. No generated clock.
![image](https://user-images.githubusercontent.com/66086031/171468376-cb623b4e-9e5c-4e0f-9090-13b4ddf79c09.png)

iv Skip to finish.
	
![image](https://user-images.githubusercontent.com/66086031/171469728-0ce56443-9bd6-4b9e-87b9-9c2078ba1f04.png)

v. Synthesized netlist
![image](https://user-images.githubusercontent.com/66086031/171469881-59dc363f-2522-42f6-b6b9-eef91c4f4adb.png)

#### Implementation
	
![image](https://user-images.githubusercontent.com/66086031/171471188-822fe88a-b3af-4e42-ab06-2e84c8b0c89a.png)

##### Resource Utilization

![image](https://user-images.githubusercontent.com/66086031/171471394-19abdedc-9766-4c3d-abb8-03319d8219f1.png)

#### Bit Stream
![image](https://user-images.githubusercontent.com/66086031/171472464-638031c6-4830-413a-8d9a-3781c7008b27.png)

#### Programming
- Click program device to send the bitstream to the FPGA board.

#### Timing Analysis
	
This can be done after synthesis or after implementation.

i. Click on Report Timing Summary
	
![image](https://user-images.githubusercontent.com/66086031/171527437-dc59d715-5e3c-4ea8-8879-543664abaf95.png)

2. We can also view the detailed path report for each path. It specifies the starting and endpoint.
	
![image](https://user-images.githubusercontent.com/66086031/171527522-d6eaa30e-5d50-4e02-a348-37fff8b6d4a3.png)

- As the slack is positive, all the timing constraints are met.
	
#### Power 

![image](https://user-images.githubusercontent.com/66086031/171527762-e6a9544d-3e9f-4995-b79f-75275ae13d33.png)

#### Area

- Click on report Resource Utilization
	
![image](https://user-images.githubusercontent.com/66086031/171527868-01199dea-c006-4b8a-8c6f-285ad9361f12.png)
	
![image](https://user-images.githubusercontent.com/66086031/171527983-43badbf2-3ef7-4768-821e-4b1ed1a4e51e.png)

- Summary 
	
![image](https://user-images.githubusercontent.com/66086031/171528074-5dab274f-06c5-4906-b1a4-9065509d6140.png)

#### Vivado Summary
(insert tables of all the data)
	
	
### Virtual Input/Output Counter

- Using this method we can program a remote FPGA connected to a cloud.
- The VIO provides the inputs to the FPGA and probes the output.
- VIO output is the input to the FPGA
- VIO input is the output of the FPGA
	
![image](https://user-images.githubusercontent.com/66086031/171528367-5bcf1eff-2340-4aae-88b4-f46d1984bd06.png)

### VIO Code

- Make these changes. The reset and clock are come from VIO. So they are no longer input ports. The counter output will be probed by the VIO, so it is no longer an output port.

**VIO Inputs:** Slow Clock, Counter Output
**VIO Outputs: **Reset
	
![image](https://user-images.githubusercontent.com/66086031/171529048-206b9e03-dae7-4fd7-9b93-78d56cd2a1ea.png)
	
i. Click on IP Catalog in the Project Manager.
	
ii. Search VIO 
	
![image](https://user-images.githubusercontent.com/66086031/171529540-1367c9ae-83a8-45e6-8c89-1572c78eca7f.png)

iii. Configure as shown.
	
![image](https://user-images.githubusercontent.com/66086031/171529946-f8629fa0-c100-42f0-a454-3d56d6aa5992.png)

![image](https://user-images.githubusercontent.com/66086031/171529996-b6b9eb55-66ef-4e0c-ac1e-8020f91dc9ea.png)

![image](https://user-images.githubusercontent.com/66086031/171530060-78c084b3-7c9b-4e7d-9832-4ab3b6362f49.png)
	
iv. Click generate

v. Go to IP Sources and click on Instantion Template and copy these lines from the ```.veo``` file.
	
![image](https://user-images.githubusercontent.com/66086031/171530413-62af72a2-f647-454d-b0b4-c5e3fbccf012.png)

vi. Use this to instantiate the VIO in the ```.v``` file

![image](https://user-images.githubusercontent.com/66086031/171530512-b9e79bc6-61a9-4883-9acd-4b3398872145.png)


## Acknowledgements

- Dr. Xifan Tang, OpenFPGA and Chief Engineer RapidSilicon



