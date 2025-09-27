# 📘 Day 3 — Combinational and Sequential Optimizations

This document provides an overview of **Day 3** topics, focusing on the various optimization techniques that synthesis tools apply to combinational and sequential logic to improve a design's Power, Performance, and Area (PPA).

---

## 🎯 Introduction to Optimizations

**Logic optimization** is the process where a synthesis tool automatically transforms a gate-level circuit into a smaller, faster, and more power-efficient version without changing its logical function. The primary goal is to meet the design constraints for **timing (performance)**, **power consumption**, and **chip area**.

---

## ➗ Combinational Logic Optimizations

This involves optimizing logic that does not contain memory elements, where the output is purely a function of the current inputs.

- **Constant Propagation:** Simplifies logic by replacing parts of a circuit with constant values. For example, an AND gate with one input tied to logic '0' will always output '0', so the entire gate can be replaced by a direct connection to ground.

- **Boolean Logic Simplification:** Uses rules of Boolean algebra to reduce the number of gates. For instance, the expression `(A AND B) OR (A AND NOT B)` can be simplified to just `A`, eliminating the need for two AND gates and an OR gate.

- **Common Sub-expression Elimination:** If an identical piece of logic is used in multiple places, the tool synthesizes it only once and shares the result. This reduces the total gate count and saves area.

---
## 🔄 Sequential Logic Optimizations

Sequential optimizations focus on logic that includes memory elements like flip-flops and latches. These optimizations improve timing, reduce power, and minimize area while preserving the correct sequence of operations.

- **Retiming:** Moves flip-flops across combinational logic gates without changing the circuit’s behavior. This helps balance the logic delay across pipeline stages, improving the overall clock frequency.

- **Clock Gating:** Adds control logic to selectively disable the clock signal to flip-flops when their state does not need to change, reducing dynamic power consumption.

- **Register Merging:** Combines multiple registers into one when possible, saving area and simplifying routing.

- **Flip-flop Sharing:** Identifies and merges flip-flops with equivalent behavior to reduce the number of storage elements.

- **Removal of Redundant Registers:** Detects flip-flops that do not affect the output or system state and removes them, optimizing area and power.

---
## Lab on Combinational optimaization

### Lab on opt_check
1. open yosys
   ```bash
   yosys
   ```
2. synthesis of opt_check
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   read_verilog opt_check.v
   synth -top opt_check
   opt_clean -purge
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show
   ```
3. output :
<img width="1638" height="1036" alt="opt_check_netlist" src="https://github.com/user-attachments/assets/2ca64c9f-9ee8-48be-85c0-df00eba8ec76" />


### Lab on opt_check2
1. open yosys
   ```bash
   yosys
   ```
2. synthesis of opt_check2
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   read_verilog opt_check2.v
   synth -top opt_check2
   opt_clean -purge
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show
   ```
3. output :
<img width="1638" height="1036" alt="opt_check2_netlist" src="https://github.com/user-attachments/assets/0b755f14-7bc4-4898-bef2-d9a5a1ccd8cc" />


### Lab on opt_check3
1. open yosys
   ```bash
   yosys
   ```
2. synthesis of opt_check3
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   read_verilog opt_check3.v
   synth -top opt_check3
   opt_clean -purge
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show
   ```
3. output :
<img width="1638" height="1036" alt="opt_check3_netlist" src="https://github.com/user-attachments/assets/100ee74f-18f2-4ba4-b31a-260e5cfc73a5" />


### Lab on opt_check4
1. open yosys
   ```bash
   yosys
   ```
2. synthesis of opt_check4
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   read_verilog opt_check4.v
   synth -top opt_check4
   opt_clean -purge
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show
   ```
3. output :
<img width="1638" height="1036" alt="opt_check4_netlist" src="https://github.com/user-attachments/assets/70d03bdd-fc7c-4888-8dfc-8ce715f60bad" />

### Lab on multiple_module_opt
1. open yosys
   ```bash
   yosys
   ```
2. synthesis of multiple_module_opt
   ```bash
   read_verilog multiple_module_opt.v
   synth -top multiple_module_opt
   flatten
   opt_clean -purge
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show
   ```
3. output :
<img width="1638" height="1036" alt="multiple_module_opt_netlist" src="https://github.com/user-attachments/assets/3b9b5c97-7527-46f3-8706-bc4cb8a11185" />


### Lab on multiple_module_opt2
1. open yosys
   ```bash
   yosys
   ```
2. synthesis of multiple_module_opt2
   ```bash
   read_verilog multiple_module_opt2.v
   synth -top multiple_module_opt2
   flatten
   opt_clean -purge
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show
   ```
3. output :
<img width="1638" height="1036" alt="multiple_module_opt2_netlist" src="https://github.com/user-attachments/assets/3dc19172-42ac-4171-9060-06f9c7a1473d" />


