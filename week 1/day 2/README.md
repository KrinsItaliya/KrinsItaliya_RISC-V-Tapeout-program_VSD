# 📘 Day 2 — Timing, Synthesis Methodologies & Flip-Flop Design

This document covers the **Day 2** material, focusing on the role of timing libraries, comparing different synthesis strategies, and exploring efficient coding styles for flip-flops.

---

## 🕓 Introduction to Timing Libraries (`.lib`)

### 📝 Theory
A **timing library file (`.lib`)** is a critical component in the digital design flow. It is a text file provided by the semiconductor foundry that contains detailed performance characteristics of all the standard cells available in a specific Process Design Kit (PDK).

**Key Characteristics in a `.lib` file:**

- **Timing Information:** This includes propagation delays for gates, setup and hold times for flip-flops, and other timing arcs. The data is often presented in lookup tables based on input transition times and output load capacitance.
- **Power Consumption:** It defines the leakage power and dynamic power consumed by each cell under various conditions.
- **Physical Area:** The physical footprint of each standard cell.
- **PVT Corners:** Libraries are provided for different **Process, Voltage, and Temperature (PVT)** corners (e.g., fast, slow, typical) to ensure the design works reliably across all expected operating conditions.

The synthesis tool (like Yosys) and the Static Timing Analysis tool (like OpenSTA) heavily rely on these `.lib` files to translate RTL into an optimized gate-level netlist and to verify its performance against timing constraints.
---
## Lab on timeing libraries
1. To open netlist
   ```bash
   cd lib/
   vim sky130_fd_sc_hd__tt_025C_1v80.lib
   ```

## 🏗️ Hierarchical vs. Flat Synthesis

### 📝 Theory
Synthesis is the process of converting high-level RTL code into a gate-level netlist. The two primary methodologies for this process are flat and hierarchical synthesis.

#### Flat Synthesis
In **flat synthesis**, the tool removes all levels of hierarchy in the design and synthesizes the entire project as a single, massive module.
- **Analogy:** Building a car by dumping all its individual nuts, bolts, and panels into one giant pile and assembling it from scratch.
- **Pros:** Allows the synthesis tool to perform optimizations across the entire design without being limited by module boundaries. This can often lead to the best possible timing and area results (**Quality of Results - QoR**).
- **Cons:** Very slow and consumes a huge amount of memory, making it impractical for large, complex designs like a modern SoC.

#### Hierarchical Synthesis
In **hierarchical synthesis**, the design is synthesized module by module, preserving the original hierarchy defined in the RTL code.
- **Analogy:** Building a car by first assembling the engine, then the chassis, and then the interior, and finally putting these pre-built sub-assemblies together.
- **Pros:** Significantly faster and uses less memory. Scalable, allowing large teams to work on different blocks of the design simultaneously.
- **Cons:** Since optimization is contained within each module, the final result might be slightly less optimal compared to flat synthesis.

For almost all modern, complex designs, **hierarchical synthesis** is the standard industry practice.

---

## Lab on Falt and hierarchical Synthesis

### Lab steps
Here we use an multiple modules in this file we run flat and hierarchical synthesis both

### hirachical synthesis
1. open yosys
   ```bash
   yosys
   ```
2. synthesis code for hierarchical
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   read_verilog multiple_modules.v
   synth -top multiple_modules 
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show multiple_modules
   ```
3. Output of the hierarchical synthesis
<img width="3272" height="1994" alt="image" src="https://github.com/user-attachments/assets/0ead0a44-d54f-4424-95bd-a12c68cc3bc3" />


### flatten synthesis
1. open yosys
   ```bash
   yosys
   ```
2. synthesis code for flatten
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   read_verilog multiple_modules.v
   synth -top multiple_modules 
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   flatten
   show 
   ```
3. Output of the hierarchical synthesis
<img width="1636" height="997" alt="dff_multiple modules_flatten" src="https://github.com/user-attachments/assets/3bbc5aaf-7db4-4a8b-98d0-f2f879378c27" />


4. synthesis the submodule 1 and submodule 2 seprate
   ```bash
   synth -top sub_module1
   synth -top sub_module2
   ```

5. output of the submodule
<img width="1636" height="997" alt="sub_module1_netlist" src="https://github.com/user-attachments/assets/9746e77d-629f-4e88-b523-dbc01fe1310d" />


## \U0001f4a1 Various Flop Coding Styles and Optimization

### \U0001f4dd Theory
Flip-flops are the fundamental storage elements in synchronous digital circuits. The way you write the Verilog code for a flip-flop directly influences the hardware that the synthesis tool creates.

# \U0001f4d8 Synchronous vs. Asynchronous Inputs: Key Definitions

This document provides a quick reference for the fundamental concepts of synchronous and asynchronous inputs in the context of flip-flops and sequential logic.

---

## \u23f0 Synchronous Inputs

A **synchronous** input can only affect a flip-flop's state on the active edge of a clock signal. This means the input's effect is synchronized with the system clock, ensuring predictable and stable behavior.

---

## Lab on synchronous dff
### Lab steps
1. Open gtkwave
   ```bash
   iverilog dff_syncres.v tb_dff_syncres.v 
   ./a.out
   gtkwave tb_dff_syncres.vcd
   ```
2. output gtkwave
  <img width="1636" height="1029" alt="dff_syncres_waveform" src="https://github.com/user-attachments/assets/768c53b9-3379-46dc-89ae-ceb17da2d8a9" />

3. open yosys
   ```bash
   yosys
   ```
4. for synthesis
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   read_verilog dff_syncres.v
   synth -top dff_syncres
   dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show
    ```
5. synthesis output
<img width="1636" height="1029" alt="dff_syncres_netlist" src="https://github.com/user-attachments/assets/10443a7d-3c7b-4a11-837e-b7d0aef26ed5" />


   
## \u26a1 Asynchronous Inputs

An **asynchronous** input affects a flip-flop's state immediately, without waiting for a clock edge. These inputs override the clock and synchronous data.

-   **Asynchronous Set:** A specific asynchronous input that immediately forces a flip-flop's output to a logic '1' state, regardless of the clock.

---
## Lab on asynchronous_set dff
### Lab steps
1. Open gtkwave
   ```bash
   iverilog dff_async_set.v tb_dff_async_set.v
   ./a.out
   gtkwave tb_dff_async_set.vcd
   ```
2. output gtkwave
<img width="1636" height="1029" alt="dff_async_set_waveform" src="https://github.com/user-attachments/assets/3992360c-56de-4312-b101-919ba12aaafb" />


3. open yosys
   ```bash
   yosys
   ```
4. for synthesis
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   read_verilog dff_async_set.v
   synth -top dff_async_set
   dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show
    ```
5. synthesis output
 <img width="1636" height="1029" alt="dff_async_ set_netlist" src="https://github.com/user-attachments/assets/49eff8eb-d1c5-415f-85ec-b81113a9032e" />



-   **Asynchronous Reset (or Clear):** The counterpart to set, this input immediately forces the flip-flop's output to a logic '0'.

---
## Lab on asynchronous dff
### Lab steps
1. Open gtkwave
   ```bash
   iverilog dff_asyncres.v tb_dff_asyncres.v
   ./a.out
   gtkwave tb_dff_asyncres.vcd
   ```
2. output gtkwave
<img width="1636" height="1029" alt="dff_asyncres_waveform" src="https://github.com/user-attachments/assets/b2e2ffb9-64ad-4d8d-b98d-26f58eff5867" />

3. open yosys
   ```bash
   yosys
   ```
4. for synthesis
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   read_verilog dff_asyncres.v
   synth -top dff_asyncres
   dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show
    ```
5. synthesis output
<img width="1636" height="1029" alt="dff_asyncres_netlist" src="https://github.com/user-attachments/assets/9cb8abff-f96b-494c-8998-f9554879e25f" />

#### Optimization
Synthesis tools are highly optimized to recognize standard, clean coding styles. Writing unconventional or complex logic for simple elements like flip-flops can confuse the tool, leading to inefficient hardware. Using a **clock enable** is a common and efficient optimization technique to reduce power by preventing the flop from toggling unnecessarily on every clock cycle.

## Lab on optimaization mul_2
### Lab steps
1. synthesis of mul_2
   ```bash
   yosys
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog mult_2.v
   synth -top mul2
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show
   ```
2. output synthesis
<img width="1636" height="1029" alt="mul2_netlist" src="https://github.com/user-attachments/assets/d003e7bc-e9db-45cf-912d-0903a04b280d" />

4. open netlist
   ```bash
   write_verilog -noattr mult2_net.v
   !gvim mul2
   ```
5. output of the netlist
<img width="969" height="626" alt="mul_2_netlist" src="https://github.com/user-attachments/assets/4ecdeda2-dc4a-4dd3-8c6b-e27699562e64" />

## Lab on optimaization mul_8
### Lab steps
1. synthesis of mul_8
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog mult_8.v
   synth -top mult8
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show
   ```
2. output synthesis
<img width="1638" height="1036" alt="mult8_netlist" src="https://github.com/user-attachments/assets/fc37c153-2c51-4c3a-9afc-03cbb91342f8" />

4. open netlist
   ```bash
   write_verilog -noattr mult8_net.v
   !gvim\u00a0mult8
   ```
5. output of the netlist
<img width="971" height="633" alt="mul_8_netlist" src="https://github.com/user-attachments/assets/24cb14ad-ae83-49ce-b614-fe14b48377ca" />




