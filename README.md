
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

![image](https://user-images.githubusercontent.com/66086031/171312512-8e650388-62ec-40c5-9e9d-f9fd99baa14f.png)

(Diagram taken from Course Slide for reference)

An FPGA Architecture consists of

- **Configurable Logic Blocks** 
	* It contains an N-input Lookup Tables, Multiplexer and a Flip Flop. 
	* The Lookup table consists of all the possible minterms of a N-input function.
	* Basically a function can be expressed as Sum of Products form or Sum of Minterms form.
	* If we want to implement a logic function, the corresponding minterms of the function are selected by using the Mux.
	* The output of the Mux feeds into the Flip Flop.
- **Configurable**







### Vivado-counter example



### Virtual Input/Output Counter


## Acknowledgements

- Dr. Xifan Tang, OpenFPGA and Chief Engineer RapidSilicon



