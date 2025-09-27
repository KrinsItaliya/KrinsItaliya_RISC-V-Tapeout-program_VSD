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

 
## Sequential Logic Optimizations

This involves optimizing circuits that contain memory elements like flip-flops, where the output depends on both current inputs and past states.

* **Retiming:** Moves registers across combinational logic blocks to better balance timing paths. This helps to shorten the critical path delay, allowing the design to run at a higher clock frequency without changing the circuit's overall function.
* **State Minimization:** For Finite State Machines (FSMs), this technique identifies and merges equivalent states. Fewer states require fewer flip-flops to implement, which directly reduces the circuit's area.
* **Clock Gating:** A major power-saving technique where the clock signal to a group of flip-flops is temporarily turned off when they are not changing state. This eliminates the dynamic power consumed during unnecessary switching. 

---
## Lab on sequantial optimaization

### Lab on dff_const1
1. open gtkwave
   ```bash
   iverilog dff_const1.v tb_dff_const1.v
   ./a.out
   gtkwave tb_dff_const1.vcd
   ```
2. output of gtkwave
<img width="1638" height="1036" alt="dff_const1_waveform" src="https://github.com/user-attachments/assets/020cf726-2d40-40d7-819a-51cb92928a61" />


3. open yosys
   ```bash
   yosys
   ```
4. synthesis of dff_const1
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog dff_const1.v
   synth -top dff_const1
   dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show 
   ```
5. output of synthesis
<img width="1638" height="1036" alt="dff_const1_netlist" src="https://github.com/user-attachments/assets/c088dfa7-bd0c-4745-9c81-4f9b01a49d19" />


### Lab on dff_const2
1. open gtkwave
   ```bash
   iverilog dff_const2.v tb_dff_const2.v
   ./a.out
   gtkwave tb_dff_const2.vcd
   ```
2. output of gtkwave
<img width="1638" height="1036" alt="dff_const2_waveform" src="https://github.com/user-attachments/assets/7df62355-740f-492c-b55c-c8fcec98449a" />


3. open yosys
   ```bash
   yosys
   ```
4. synthesis of dff_const2
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog dff_const2.v
   synth -top dff_const2
   dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show 
   ```
5. output of synthesis
<img width="1638" height="1036" alt="dff_const2_netlist" src="https://github.com/user-attachments/assets/02cf8075-bc1c-4160-a79e-8509bd741ef4" />


### Lab on dff_const3
1. open gtkwave
   ```bash
   iverilog dff_const3.v tb_dff_const3.v
   ./a.out
   gtkwave tb_dff_const3.vcd
   ```
2. output of gtkwave
<img width="1638" height="1036" alt="dff_const3_waveforn" src="https://github.com/user-attachments/assets/55ce3f20-ef3f-47a8-ba06-8b46c37ba58e" />


3. open yosys
   ```bash
   yosys
   ```
4. synthesis of dff_const3
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog dff_const3.v
   synth -top dff_const3
   dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show 
   ```
5. output of synthesis
<img width="1638" height="1036" alt="dff_const3_netlist" src="https://github.com/user-attachments/assets/63f38315-ea16-44ab-bcff-79039ad8cbf5" />


### Lab on dff_const4
1. open gtkwave
   ```bash
   iverilog dff_const4.v tb_dff_const3.v
   ./a.out
   gtkwave tb_dff_const3.vcd
   ```
2. output of gtkwave
<img width="1638" height="1036" alt="dff_const4_waveform" src="https://github.com/user-attachments/assets/6cd38f9b-f52b-46fb-9cff-0c7434bb4715" />


3. open yosys
   ```bash
   yosys
   ```
4. synthesis of dff_const4
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog dff_const4.v
   synth -top dff_const4
   dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show 
   ```
5. output of synthesis
<img width="1638" height="1036" alt="dff_cost4_netlist" src="https://github.com/user-attachments/assets/d4e8b419-80e7-4a99-a1a7-2211944410f6" />


### Lab on dff_const5
1. open gtkwave
   ```bash
   iverilog dff_const5.v tb_dff_const5.v
   ./a.out
   gtkwave tb_dff_const5.vcd
   ```
2. output of gtkwave
<img width="1638" height="1036" alt="dff_const5_waveform" src="https://github.com/user-attachments/assets/797ecffe-bb88-45a7-b904-af1ccb0cee08" />


3. open yosys
   ```bash
   yosys
   ```
4. synthesis of dff_const5
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog dff_const5.v
   synth -top dff_const5
   dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show 
   ```
5. output of synthesis
<img width="1638" height="1036" alt="dff_const5_netlist" src="https://github.com/user-attachments/assets/8c53cc9d-49a7-4710-8a98-33fa07dd9b15" />


## \U0001f5d1\ufe0f Sequential Optimizations for Unused Outputs

This is a powerful technique for reducing waste in a design.

* **Logic Trimming (or Dead Code Elimination):** If the output of a flip-flop or a block of logic is not connected to anything else in the design (i.e., it has no fan-out), it serves no purpose. The synthesis tool will identify and completely remove these unused registers and any combinational logic that solely drives them, leading to significant savings in both area and power.

---
## Lab on sequantial logic unused case

### lab on couter_opt
1. open yosys
   ```bash
   yosys
   ```
2. synthesis counter_opt
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   read_verilog counter_opt.v
   synth -top counter_opt
   dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show
   ```
3. output of synthesis
<img width="1638" height="1036" alt="counter_opt_netlist" src="https://github.com/user-attachments/assets/f4382142-c6b5-465c-9e77-92546a5df710" />

### copy counter_opt file to counter_opt2 file
1. copy and open the file
   ```bash
   cp counter_opt.v counter_opt2.v
   gvim counter_opt2.v
   ```
### after open gvim counter_opt2.v change the line assign q = count[0]; to assign q = (count[2:0] == 3'b100); and save
### run the counter_opt2 file
1. open yosys
   ```bash
   yosys
   ```
2. synthesis counter_opt2
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   read_verilog counter_opt2.v
   synth -top counter_opt
   dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib\u00a0
   show
   ```
3. output of synthesis
<img width="1638" height="1036" alt="counter_opt_synthesis_netlist" src="https://github.com/user-attachments/assets/12f48418-f1a0-4263-8696-dbd9eccfc830" />
