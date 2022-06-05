# Table of Contents

- [Table of Contents](#table-of-contents)
  - [Day 1](#day-1)
    - [FPGA Introduction](#fpga-introduction)
      - [Flow](#flow)
      - [FPGA and FPGA Architecture](#fpga-and-fpga-architecture)
      - [ASIC Vs FPGAs](#asic-vs-fpgas)
      - [Applications](#applications)
      - [FPGA Architecture](#fpga-architecture)
      - [Bitstream](#bitstream)
      - [Lookup table](#lookup-table)
      - [FPGA Programming](#fpga-programming)
      - [FPGA Design Methodology](#fpga-design-methodology)
      - [Synthesizable and Non - Synthesizable](#synthesizable-and-non---synthesizable)
      - [Basys 3 FPGA Board](#basys-3-fpga-board)
      - [Tools used](#tools-used)
      - [Different ways of programming](#different-ways-of-programming)
    - [Counter Example in Vivado](#counter-example-in-vivado)
    - [Vivado-counter example](#vivado-counter-example)
      - [Static Timing Analysis](#static-timing-analysis)
        - [Setup Time (Max. Delay) Constraints](#setup-time-max-delay-constraints)
        - [Hold Time (Min. Delay) Constraints](#hold-time-min-delay-constraints)
        - [Setup Slack](#setup-slack)
        - [Hold Slack](#hold-slack)
      - [Synthesis](#synthesis)
      - [Implementation](#implementation)
        - [Resource Utilization](#resource-utilization)
      - [Bit Stream](#bit-stream)
      - [Programming](#programming)
      - [Timing Analysis](#timing-analysis)
      - [Power](#power)
      - [Area](#area)
      - [Vivado Summary](#vivado-summary)
    - [Virtual Input/Output Counter](#virtual-inputoutput-counter)
    - [VIO Code](#vio-code)
    - [Elaboration and Pin Assignment](#elaboration-and-pin-assignment)
    - [Bitstream Generation](#bitstream-generation)
  - [Day 2](#day-2)
    - [Intro to OpenFPGA](#intro-to-openfpga)
    - [Custom FPGAs](#custom-fpgas)
    - [Verilog to Routing (VTR)](#verilog-to-routing-vtr)
    - [VTR FLow](#vtr-flow)
    - [VPR Flow on a Pre-Synthesized Design](#vpr-flow-on-a-pre-synthesized-design)
      - [.blif file](#blif-file)
      - [Lab on VPR on a Pre-Synthesized Design](#lab-on-vpr-on-a-pre-synthesized-design)
      - [Output of the VPR Step](#output-of-the-vpr-step)
      - [Timing Reports](#timing-reports)
        - [VPR Setup Slack](#vpr-setup-slack)
        - [VPR Hold Slack](#vpr-hold-slack)
      - [Constraints](#constraints)
    - [VTR Flow](#vtr-flow-1)
      - [VTR Synthesis and Simulation](#vtr-synthesis-and-simulation)
      - [VTR Timing Analysis](#vtr-timing-analysis)
      - [Area Analysis](#area-analysis)
      - [Power Analysis](#power-analysis)
    - [Comparison of Basys3 and VTR Flow](#comparison-of-basys3-and-vtr-flow)
      - [Post Implementation Timing](#post-implementation-timing)
      - [Area Comparison](#area-comparison)
      - [Power Comparison](#power-comparison)
  - [Day 3 RISC - V core Programming on Vivado](#day-3-risc---v-core-programming-on-vivado)
    - [RTL-to-Synthesis](#rtl-to-synthesis)
      - [Behavioural Simulation](#behavioural-simulation)
      - [Elaboration](#elaboration)
    - [Synthesis-to-bitstream](#synthesis-to-bitstream)
      - [Core Resource Utilization](#core-resource-utilization)
      - [Schematic](#schematic)
      - [Implementation](#implementation-1)
      - [Bitstream-Generation for RISC - V core](#bitstream-generation-for-risc---v-core)
  - [Day 4 - SOFA FPGA Fabric IP](#day-4---sofa-fpga-fabric-ip)
    - [Intro to SOFA](#intro-to-sofa)
      - [SOFA counter Statistics](#sofa-counter-statistics)
    - [SOFA counter Timing Analysis](#sofa-counter-timing-analysis)
      - [SOFA counter Timing Report](#sofa-counter-timing-report)
    - [SOFA counter Post Implementation](#sofa-counter-post-implementation)
    - [SOFA Power Analysis](#sofa-power-analysis)
  - [Day 5](#day-5)
  - [Acknowledgements](#acknowledgements)

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

| ASIC                         |              FPGA              |
| ---------------------------- | :----------------------------: |
| Not programmable             |         Reprogrammable         |
| Final Stage implementation   | Useful in prototyping a design |
| Huge time required to design |    Less Time Comparatively     |
| RTL to Layout                |  RTL to Bitstream generation   |

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

  - It contains an N-input Lookup Tables, Carry chain, Multiplexer and a Flip Flop.

- **Programmable Interconnects**

  - The wires which connect the CLBs.

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

- Xilinx Vivado 2019.2 - <https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/vivado-design-tools/archive.html>

#### Different ways of programming

- Local Programming using a board
- Remote Programming - A FPGA connected to a remote server: I/O through Virtual I/O.

### Counter Example in Vivado

- We use a frequency synthesizer to generate a slow clock to observe the outputs

<details>
 <summary> Code </summary>

```verilog
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
        ///count <= 4'b0;
     end
else
    begin
        count_reg <= count_reg + 1;
    /// if (count_reg == 26'h3ffffff) // for synthesis
        if (count_reg == 26'd12) // for simulation
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
```

</details>

### Vivado-counter example

i. Type `vivado` in the terminal.

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

viii. Do the I/O pin assignment as follows and save as `constraints.xdc` file.

![image](https://user-images.githubusercontent.com/66086031/171456676-0166c665-f6ec-439f-b8ea-9f144d7cb4e1.png)
![image](https://user-images.githubusercontent.com/66086031/171457762-be279c03-ed55-4ecb-8c16-7d7a5545ebed.png)

#### Static Timing Analysis

##### Setup Time (Max. Delay) Constraints

![image](https://user-images.githubusercontent.com/66086031/166118620-dd932eef-2e14-4689-a24d-d1e1e2098bcb.png)

- The data should be available **Tsetup** before the **capturing clock edge** comes.
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

ii. We can also view the detailed path report for each path. It specifies the starting and endpoint.

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
**VIO Outputs:**Reset

![image](https://user-images.githubusercontent.com/66086031/171530603-6fbabc7c-6099-4305-a947-f58444616006.png)

i. Click on IP Catalog in the Project Manager.

ii. Search VIO

![image](https://user-images.githubusercontent.com/66086031/171529540-1367c9ae-83a8-45e6-8c89-1572c78eca7f.png)

iii. Configure as shown.

![image](https://user-images.githubusercontent.com/66086031/171529946-f8629fa0-c100-42f0-a454-3d56d6aa5992.png)

![image](https://user-images.githubusercontent.com/66086031/171529996-b6b9eb55-66ef-4e0c-ac1e-8020f91dc9ea.png)

![image](https://user-images.githubusercontent.com/66086031/171530060-78c084b3-7c9b-4e7d-9832-4ab3b6362f49.png)

iv. Click generate

v. Go to IP Sources and click on Instantion Template and copy these lines from the `.veo` file.

![image](https://user-images.githubusercontent.com/66086031/171530413-62af72a2-f647-454d-b0b4-c5e3fbccf012.png)

vi. Use this to instantiate the VIO in the `.v` file

![image](https://user-images.githubusercontent.com/66086031/171530512-b9e79bc6-61a9-4883-9acd-4b3398872145.png)

### Elaboration and Pin Assignment

![image](https://user-images.githubusercontent.com/66086031/171531855-d43518f9-699e-4ffc-b1c4-ca22c95c60b6.png)

### Bitstream Generation

![image](https://user-images.githubusercontent.com/66086031/171533795-017d2854-ae13-4b49-8fc2-07f33e5ad4b3.png)

## Day 2

### Intro to OpenFPGA

- It is an open source framework to quickly generated a custom FPGA fabric specific to our design
- It is automated
- Reduces development time
- Generally it takes 24 hours to produce a production ready layouts

### Custom FPGAs

![image](https://user-images.githubusercontent.com/66086031/171633283-a0b723c1-a5e2-4192-9128-8b70aadff69c.png)

- For certain applications custom-made FPGAs provide huge performance gain.
- Custom FPGA architectures are expensive to produce
- OpenFPGA allows us to customize our own FPGA fabrix using a set of templates
- Generate verilog netlists based on an XML file - VPR ( Versatile Place and Route )
- Automatically generates Verilog testbenches

Visit this repo to install OpenFPGA: <https://github.com/lnis-uofu/OpenFPGA>

### Verilog to Routing (VTR)

- XML - based architecture description language to describe the custom FPGA architecture
- **Download Link:** <https://github.com/verilog-to-routing/vtr-verilog-to-routing>
- **Documentation:** <https://docs.verilogtorouting.org>

- Basically it maps our RTL to a placed and routed FPGA

![image](https://user-images.githubusercontent.com/66086031/171636949-5d0da10d-f612-44c5-8fc1-5135ab87efcf.png)

### VTR FLow

![image](https://user-images.githubusercontent.com/66086031/171637784-22745a03-dd8f-4078-b0fa-06035be33286.png)

i. We shall first run VPR on a Pre-synthesized circuit.
ii. Then, we shall run entire VPR Flow from RTL

### VPR Flow on a Pre-Synthesized Design

i. **Packing** - combines the technology mapped netlist into (CLBs) Complex Logic Blocks.
ii. **Placement** - postition of the CLBs - it produces a .place file.
iii. **Routing** - interconnection between blocks
iv. **Analysis** - Analyzes the implementation

Input: Blif file, Earch

This is the general structure of a Earch.xml file. It describes the FPGA Architecture.

```xml
<!--
-->
<architecture>
  <models>
    <model name = "">
    </model>
  </models>
  <tiles>
    <tile name = "">
    </tile>
  </tiles>
  <layout> <!-- Grid Layout, aspect ratio -->
  </layout>
  <device> <!-- Transistor definitions -->
  </device>

  <switchlist>
  </switchlist>

  <segmentlist>
  </segmentlist>

  <directlist>
  </directlist>

</architecture>
```

#### .blif file

```blif
.names - LUTS
```

#### Lab on VPR on a Pre-Synthesized Design

i. Create a working directory.

```console
mkdir -p vtr_work/quickstart/vpr_tseng
cd ./vtr_work/quickstart/vpr_tseng
```

ii. Now pass the FPGA architecture and the technology mapped netlist of the design.

```console
$VTR_ROOT/vpr/vpr \
$VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
$VTR_ROOT/vtr_flow/benchmarks/blif/tseng.blif \
--route_chan_width 100
--disp on
```

![image](https://user-images.githubusercontent.com/66086031/171648951-9778a11d-6e5c-4c0b-93fd-28b58d83eb7a.png)

![image](https://user-images.githubusercontent.com/66086031/171650479-561f9b5c-7711-47b0-9706-2fa692af1680.png)

![image](https://user-images.githubusercontent.com/66086031/171650888-21d931f6-809c-46cd-9091-186ab3d90f2d.png)

![image](https://user-images.githubusercontent.com/66086031/171651073-a890cfff-692d-4fb8-aa4f-332aa7aced28.png)

iii. Additional parameter can be applied here

```console
$VTR_ROOT/vpr/vpr \
$VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
$VTR_ROOT/vtr_flow/benchmarks/blif/tseng.blif \
--route_chan_width 100
--analysis
--disp on
```

#### Output of the VPR Step

![image](https://user-images.githubusercontent.com/66086031/171880403-785999f0-d4f8-4c5a-8a09-86de68b51e81.png)

- .net file: post packed circuit, circuit in terms of CLBs.
- .place file: how the cells are placed
- .route file: interconnects
- .log file:

#### Timing Reports

##### VPR Setup Slack

![image](https://user-images.githubusercontent.com/66086031/171881471-b6d87fc5-c0e4-4eff-94b0-7f3cc24ee1f4.png)

- Here, the setup slack is violated as no clock constraints are specified.

##### VPR Hold Slack

![image](https://user-images.githubusercontent.com/66086031/171881668-1a117227-631b-4a1d-8275-37b6bcb18082.png)

#### Constraints

i. Create a `tseng.sdc` file

```sdc
create_clock -period 10 -name pclk
set_input_delay -clock pclk -max 0 [get_ports {*}]
set_output_delay -clock pclk -max 0 [get_ports {*}]
```

ii. Run include sdc file as part of the command

```console
$VTR_ROOT/vtr_flow/arch/timing/EArch.xml $VTR_ROOT/vtr_flow/benchmarks/blif/tseng.blif --route_chan_width 100 --disp on --sdc_file tseng.sdc
```

- Now we can see that the setup slack is met.

![image](https://user-images.githubusercontent.com/66086031/171883334-6cdab73b-d178-4df3-88e3-071e0a7791f2.png)

### VTR Flow

#### VTR Synthesis and Simulation

i. Run the automated VTR Flow command.

```console
$VTR_ROOT/vtr_flow/scripts/run_vtr_flow.py \
      /home/knavin2002/Desktop/openfpga/Day\ 2/vtr_work/counter.v \
      $VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
      -temp_dir . --route_chan_width 100

```

![image](https://user-images.githubusercontent.com/66086031/171900694-f28af658-1fc6-4039-b4fa-fc02df185784.png)

ii. We can run vpr (flow analysis) on the pre-vpr file.blif file.

```console
$VTR_ROOT/vpr/vpr $VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
                  counter --circuit_file counter.pre-vpr.blif \
                  --route_chan_width 100 \
                  --analysis --disp on
```

![image](https://user-images.githubusercontent.com/66086031/171903176-c2ecb949-061f-4605-b1d8-85d64550d49a.png)

iii. Now run the entire VPR Flow

```console
$VTR_ROOT/vpr/vpr $VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
                  counter --circuit_file counter.pre-vpr.blif \
                  --route_chan_width 100 --disp on

```

![image](https://user-images.githubusercontent.com/66086031/171903834-96a94a23-6b08-495e-9fce-77ca92b2e020.png)

iv. Generation of Post implementation netlist

```console
$VTR_ROOT/vpr/vpr $VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
                  counter.pre-vpr.blif \
                  --gen_post_synthesis_netlist on

```

![image](https://user-images.githubusercontent.com/66086031/171905696-a27cac38-e1e1-4a07-9a73-eae0707ace8c.png)
![image](https://user-images.githubusercontent.com/66086031/171907528-cdafb746-a444-4c49-96a2-53027ff9ba76.png)

- To run the post implementation netlist, we need primitves file

v. Simulation in Xilinx Vivaldo. Include post_synthesis file, testbench and primitives file

![image](https://user-images.githubusercontent.com/66086031/171908726-a64f6a64-9934-4507-b0f1-f2c6b1cdced2.png)

- Include the path to the .sdf file correctly in the testbench.

![image](https://user-images.githubusercontent.com/66086031/171911723-af793e64-25d4-4b9f-b051-8734ba217e71.png)

![image](https://user-images.githubusercontent.com/66086031/171911596-7b329a31-01c7-4231-9409-3918527af000.png)

#### VTR Timing Analysis

- We have to pass the `.blif` to the VPR Flow otherwise the tool will have its own constraints.

i. Create a `constraints.sdc` file. Create constraints as follows. Also change the name of the clock in the pre-vpr.blif as `up_counter_clk`.

```sdc
create_clock -period 10 up_counter_clk
set_input_delay -clock up_counter_clk -max 0 [get_ports {*}]
set_output_delay -clock up_counter_clk -max 0 [get_ports {*}]
```

ii. Now run VPR flow with te timing constraints.

```console
  $VTR_ROOT/vpr/vpr \
  $VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
  counter.pre-vpr.blif --route_chan_width 100 \
  --sdc_file /home/knavin2002/Desktop/openfpga/Day\ 2/tseng.sdc
```

![image](https://user-images.githubusercontent.com/66086031/171972373-a9691b36-9484-4202-84f1-89af574d8ae2.png)

iii. We can check whether the setup and hold time constraints are met.

![image](https://user-images.githubusercontent.com/66086031/171972473-302a5451-adbb-40a2-8338-dfc8346845ea.png)

iv. Now change the clock period to 5ns in `.sdc` file.

![image](https://user-images.githubusercontent.com/66086031/171972520-4ca625c5-9668-405a-91bb-6c56aa89aabc.png)

v. Check the setup and hold constraints.

![image](https://user-images.githubusercontent.com/66086031/171972921-b27f7109-9422-43a1-b729-645f865c632e.png)

![image](https://user-images.githubusercontent.com/66086031/171973147-065037c9-6d2f-4d3f-992a-3c6fb827f33b.png)

- They are still met.

#### Area Analysis

i. Open `vpr_stdout.log` file.

![image](https://user-images.githubusercontent.com/66086031/171973545-691c24c5-16c6-4a39-966e-174fb26eadbf.png)

![image](https://user-images.githubusercontent.com/66086031/171973735-4488fa7a-e908-47c5-ac09-cf8f428fdde0.png)

![image](https://user-images.githubusercontent.com/66086031/171973845-100f38ec-a402-41b6-b3a1-d322fbe71eee.png)

#### Power Analysis

i. Include the `-power` and the `cmos_tech` xml file the vtr flow command.

```console
  $VTR_ROOT/vtr_flow/scripts/run_vtr_flow.py \
  /home/knavin2002/Desktop/openfpga/Day\ 2/vtr_work/counter.v \
  $VTR_ROOT/vtr_flow/arch/timing/EArch.xml -power -cmos_tech \
  $VTR_ROOT/vtr_flow/tech/PTM_45nm/45nm.xml \
  -temp_dir . --route_chan_width 100
```

![image](https://user-images.githubusercontent.com/66086031/171974532-08650fb6-c77f-4b1b-bc3b-6e8f5e1d0d33.png)

![image](https://user-images.githubusercontent.com/66086031/171974575-3da79a6f-4fc5-4819-b874-9b795d561b9a.png)

### Comparison of Basys3 and VTR Flow

- Now we shall consolidating all the results from the Vivado flow and the VTR Flow of the counter design.

#### Post Implementation Timing

![image](https://user-images.githubusercontent.com/66086031/171983435-7c5bf0e0-d847-4eed-ba6f-8acc05c28653.png)

- `.sdc` file
- Clock period `10ns` or `100 MHz`

| Parameter                        | Basys3 | VTR Earch |
| :------------------------------- | :----: | :-------: |
| **Technology**                   |        |           |
| **Worst Negative Slack - Setup** |        |           |
| **Worst Negative Slack - Hold**  |        |           |

- Min. Slack

| Parameter                        | Basys3 | VTR Earch (ns) |
| :------------------------------- | :----: | :------------: |
| **Minimum Time Period**          |        |     $1.8$      |
| **Worst Negative Slack - Setup** |        |     $0.34$     |
| **Worst Negative Slack - Hold**  |        |    $0.293$     |

#### Area Comparison

![image](https://user-images.githubusercontent.com/66086031/171983481-1f9c9f95-bd65-48a9-b924-b53d66dcc5dd.png)

#### Power Comparison

![image](https://user-images.githubusercontent.com/66086031/171983532-22cb6a7d-72f5-4926-95a5-14a0229087b4.png)

## Day 3 RISC - V core Programming on Vivado

- Now we shall go through the Vivado flow for the RVMyth Processor
- RISC - V RVMyth: <https://github.com/NAvi349/riscv-myth-ws>
- Converting TL - Verilog to Verilog: <https://github.com/NAvi349/mixed-riscv-soc>
- We shall test a program that sums numbers from 1 to N.
- N is 9 for this example.
- $Sumof(1 to 9) = 45$

### RTL-to-Synthesis

#### Behavioural Simulation

i. Create a new project and add the core as a source and add a testbench as simulation source.

ii. Run Behavioural Simulation.

![image](https://user-images.githubusercontent.com/66086031/171986102-c10f384a-112b-4511-b457-ad81278c5264.png)

- We can see the output is 45 as expected.

#### Elaboration

i. Assign pin number as shown.

![image](https://user-images.githubusercontent.com/66086031/171987099-4661a8a0-65d4-41be-aa14-2345161f359e.png)

ii. We shall use ILA to observer the outputs. So remove the output ports and declare it as reg.

![image](https://user-images.githubusercontent.com/66086031/171987204-05d181d8-7ce4-4bfe-91f4-b77b22445158.png)

iii. Run Elaboration again

![image](https://user-images.githubusercontent.com/66086031/171987490-4f9efea2-7552-4313-99a4-f1618396d95c.png)

iv. From IP Catalog instantiate ILA and connect to the RISC - V core

![image](https://user-images.githubusercontent.com/66086031/171987650-cd0a65aa-e2c3-47a3-8f3d-9ed1de6f6975.png)

v. Run Synthesis and then add constraints
![image](https://user-images.githubusercontent.com/66086031/171987874-1772e9b2-6305-49d6-aa7e-934f366f8cb2.png)

vi. Run synthesis

![image](https://user-images.githubusercontent.com/66086031/171987948-23ad11fc-8ff0-4d84-91b6-f5b79f4de115.png)

### Synthesis-to-bitstream

#### Core Resource Utilization

![image](https://user-images.githubusercontent.com/66086031/171988029-ec6ce57f-287f-4c68-a4c2-bb5a4c66c9c7.png)

#### Schematic

![image](https://user-images.githubusercontent.com/66086031/171988057-57b6190f-9844-4654-801e-8cabbf2974ad.png)

#### Implementation

i. Run implementation.

![image](https://user-images.githubusercontent.com/66086031/171988247-aa477dfb-5425-42a7-94a3-9926d786c3d0.png)

ii. Report timing summary

![image](https://user-images.githubusercontent.com/66086031/171988266-031f75e3-337b-4240-b96e-470b785ef12e.png)

- All the user constraints are met.

#### Bitstream-Generation for RISC - V core

i. Run Bitstream generation.

![image](https://user-images.githubusercontent.com/66086031/171988364-a56f1871-4994-40a7-8f00-657bcb95d85c.png)

![image](https://user-images.githubusercontent.com/66086031/171988392-1ec53703-30a8-4213-bf66-99ee4a4d3423.png)

## Day 4 - SOFA FPGA Fabric IP

### Intro to SOFA

- **SOFA** - **S**kywate **O**pensource **F**PG**A**s
- Open-source FPGA IPs
- Skywater 130nm PDK and OpenFPGA Framework
- HD - High Density FPGAs - embedded FPGAs.
- **Documentation and installation:** <https://github.com/lnis-uofu/SOFA>

i. Clone the repository

```console
git clone https://github.com/lnis-uofu/SOFA.git
```

ii. Make `FPGA1212_QLSOFA_HD_PNR/` as the current directory.

iii. Copy the `counter.v` file to the benchmark folder.

![image](https://user-images.githubusercontent.com/66086031/172032759-1b03d407-2bad-4ae3-a1aa-e504cb29522f.png)

iii. Open `task_simulation.conf`

```console
  gedit FPGA1212_QLSOFA_HD_task/config/task_simulation.conf
```

![image](https://user-images.githubusercontent.com/66086031/172032612-005ca116-549a-4bd1-b245-2ff61a0a936f.png)

iv. Add the following lines under respective sections

```text
[BENCHMARKS]
bench0=${PATH:TASK_DIR}/BENCHMARK/counter_new/counter.v

[SYNTHESIS_PARAM]
bench0_top = up_counter
```

![image](https://user-images.githubusercontent.com/66086031/172032909-cf9edcda-4f7d-4c3c-aab5-4e5de632a1a5.png)

- `generate_testbench.openfpga` will call the vpr flow

![image](https://user-images.githubusercontent.com/66086031/172032941-f87a3f3f-f67f-441e-81f0-f80a7383470e.png)

- architecture is available under `arch` directory.

![image](https://user-images.githubusercontent.com/66086031/172032968-38c3c95b-db99-430a-9e67-3687b089b132.png)

v. Run the makefile

```console
make runOpenFPGA
```

![image](https://user-images.githubusercontent.com/66086031/172033156-8ca1ee9c-cfd5-4609-9a36-dc181d5313bf.png)

vi. View the generated files

```console
 cd FPGA1212_QLSOFA_HD_task/latest/vpr_arch/up_counter/MIN_ROUTE_CHAN_WIDTH/
```

![image](https://user-images.githubusercontent.com/66086031/172033197-1edcad9a-59e8-4476-8890-88c9ce843de8.png)

#### SOFA counter Statistics

![image](https://user-images.githubusercontent.com/66086031/172033231-973ce382-95bc-46ec-9248-9d277e581b87.png)

![image](https://user-images.githubusercontent.com/66086031/172033246-37bdd5cd-23e9-45b5-ae9e-e81368436eab.png)

### SOFA counter Timing Analysis

i. Create `counter.sdc` file in the `counter_new` folder in `BENCHMARK`.

```sdc
create_clock -period 20 clk
set_input_delay -clock clk -max 0 [get_ports {*}]
set_output_delay -clock clk -max 0 [get_ports {*}]
```

![image](https://user-images.githubusercontent.com/66086031/172033441-00f9b600-a562-4d4b-9885-316518766e0c.png)

ii. Open `generate_testbench.openfpga` and the `counter.sdc` file in the vpr arguments.

![image](https://user-images.githubusercontent.com/66086031/172033674-e3d381a5-36fd-4038-b377-e4498f391401.png)

iii. Run the makefile command

```console
make runOpenFPGA
```

![image](https://user-images.githubusercontent.com/66086031/172033701-095aad69-9f61-445c-bb04-e86cb4add9c9.png)

#### SOFA counter Timing Report

- Setup slack

![image](https://user-images.githubusercontent.com/66086031/172033776-681bc8de-cb77-485d-a593-9ab431b4683b.png)

- Hold Slack

![image](https://user-images.githubusercontent.com/66086031/172033802-ef8cc3f0-6981-405b-a632-a0f8e10ccea4.png)

### SOFA counter Post Implementation

i. Add this argument in the vpr flow in the `generate_testbench.openfpga` file.

```console
--gen_post_synthesis_netlist on
```

![image](https://user-images.githubusercontent.com/66086031/172035527-d91138a5-b998-4a65-9387-ea02e906491e.png)

ii. Run makefile again

![image](https://user-images.githubusercontent.com/66086031/172035669-0c8ac0f9-1e43-421e-bddc-af24ae3f834d.png)

- post implementation file

![image](https://user-images.githubusercontent.com/66086031/172035723-539b5947-e134-45b8-8a86-751de384693b.png)

iii. Run simulation through vivado.

![image](https://user-images.githubusercontent.com/66086031/172035990-c4288ca2-0244-48ab-b0e4-2bb2340a13f6.png)

### SOFA Power Analysis

i. Change this two lines in the `task_simulation.conf` file.

![image](https://user-images.githubusercontent.com/66086031/172036360-bedb6b63-6496-4a35-9403-2b378f826c4f.png)

ii. Add this new line the `generate_testbench.openfpga` file

```console
vpr ${VPR_ARCH_FILE} ${VPR_TESTBENCH_BLIF} --clock_modeling ideal \
  --device ${OPENFPGA_VPR_DEVICE_LAYOUT} --route_chan_width ${OPENFPGA_VPR_ROUTE_CHAN_WIDTH} \
  --absorb_buffer_luts off --power \
  --activity_file /home/knavin2002/Desktop/openfpga/Day-4/SOFA/FPGA1212_QLSOFA_HD_PNR/FPGA1212_QLSOFA_HD_task/latest/vpr_arch/up_counter/MIN_ROUTE_CHAN_WIDTH/up_counter_ace_out.act \
  --tech_properties /home/kunalg123/Desktop/vtr-verilog-to-routing/vtr_flow/tech/PTM_45nm/45nm.xml
```

![image](https://user-images.githubusercontent.com/66086031/172037076-08db7d4e-965f-4571-8bac-3905e5af7782.png)

iii. Add the options for power analysis in `vpr_arch.xml` file.

```xml
<!-- added for power analysis  -->
  <power>
      <local_interconnect C_wire="2.5e-10"/>
      <mux_transistor_size mux_transistor_size="3"/>
      <FF_size FF_size="4"/>
      <LUT_transistor_size LUT_transistor_size="4"/>
  </power>
  <clocks>
    <clock buffer_size="auto" C_wire="2.5e-10"/>
  </clocks>
```

![image](https://user-images.githubusercontent.com/66086031/172037227-0380d16b-4f98-4b01-842c-cb6f8dcaf683.png)

iv. Run the makefile command again.

![image](https://user-images.githubusercontent.com/66086031/172051095-ca5c3613-f964-4b23-ad79-6fc9ac3fad15.png)

v. See the output in `counter.power`.

![image](https://user-images.githubusercontent.com/66086031/172055323-9dfd4bf4-76d9-4223-a05b-ceb6254fcb0c.png)


## Day 5 SOFA RISC - V Core

- We shall fall the same steps for the RISC - V RVMyth Core


i. Git clone the SOFA repo as before.

ii. Make these changes in the corresponding files. Also refer to the files in the Day 5 folder. Comment lines 157 and 158, line 259 in the `vpr.xml`.

![image](https://user-images.githubusercontent.com/66086031/172039477-23d1c3f6-ff44-4135-8f95-cc033f403b51.png)

![image](https://user-images.githubusercontent.com/66086031/172055379-e21906be-c393-46e9-ac61-00f7dd3a7a37.png)

![image](https://user-images.githubusercontent.com/66086031/172055461-bdebf1ba-9f32-48b8-9928-c4259bf4f075.png)


iii. Run makefile command

![image](https://user-images.githubusercontent.com/66086031/172055509-a4b27623-4d82-42a8-a9f4-0cd97972e127.png)

iv. Open `openfpgashell.log`.

![image](https://user-images.githubusercontent.com/66086031/172056129-1b050377-e194-46ce-9353-526cfb4fc14a.png)

### RISC - V Resource Utilization

- Open `vpr_stdout.log` for the output statistics.
![image](https://user-images.githubusercontent.com/66086031/172056227-528620e4-b311-46d8-9ec4-7c98e109630d.png)

- Logic Elements
![image](https://user-images.githubusercontent.com/66086031/172056253-a34ca07d-5bdb-404a-849d-8d3cfe94aa88.png)

### SOFA RISC - V Timing Reports

i. Create `.sdc` file

```sdc
create_clock -period 200 clk
set_input_delay -clock clk -max 0 [get_ports {*}]
set_output_delay -clock clk -max 0 [get_ports {*}]
```

ii. Pass the .sdc file as argument in `generate_testbench.openfpga`

![image](https://user-images.githubusercontent.com/66086031/172056551-bb808a17-b372-4aaf-ae8d-14c820fd0213.png)

![image](https://user-images.githubusercontent.com/66086031/172056743-8dda3f7c-32a8-4df6-a759-585db3521a62.png)

iii. Run the makefile command.

- Setup slack

![image](https://user-images.githubusercontent.com/66086031/172056780-8ebc0d61-a503-406e-a2e4-68faf4f81bb8.png)

- Hold slack

![image](https://user-images.githubusercontent.com/66086031/172056822-2d414dce-54ab-4b8a-b0a5-8b23e7b3ced4.png)

###

## Acknowledgements

- Dr. Xifan Tang, OpenFPGA and Chief Engineer RapidSilicon
