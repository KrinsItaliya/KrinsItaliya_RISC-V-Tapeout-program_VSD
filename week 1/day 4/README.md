# 📘 Day 4 — GLS, Blocking vs. Non-blocking, and Synthesis-Simulation Mismatch

This document covers the **Day 4** concepts, focusing on:

- The differences between blocking and non-blocking assignments in Verilog
- The risk of simulation-synthesis mismatches
- An introduction to Gate-Level Simulation (GLS) for design verification

---

## 🚦 Blocking (`=`) vs. Non-blocking (`<=`) Assignments

Understanding the proper use of these assignment types is **critical** for writing synthesizable and reliable RTL.

### 🔁 Blocking Assignments (`=`)
- Executed **sequentially**, one after the other (like steps in a recipe)
- The **next statement waits** until the current one finishes
- Commonly used in **combinational logic**

**Example:**
```verilog
always @(*) begin
  a = b;
  c = a;  // Uses the updated value of a
end
```

### Non-blocking Assignments (`<=`)
A **non-blocking assignment** (`<=`) is executed concurrently. The simulator evaluates all the right-hand side expressions first and then, at the end of the time step, assigns those evaluated values to the left-hand side variables.

* **Analogy:** A snapshot in time. All events are captured simultaneously and updated together.
* **Best Used For:** **Sequential logic** (`always @(posedge clk)`), as it accurately models how flip-flops in hardware all change state at the same time on a clock edge.

---

## Synthesis-Simulation Mismatch

A **synthesis-simulation mismatch** is a critical design bug where the pre-synthesis RTL simulation behaves differently from the post-synthesis gate-level simulation (and the final silicon).

The most common cause is the **improper use of blocking (`=`) assignments to model sequential logic**.

* **How it Happens:** In simulation, the blocking assignments create a sequential chain of events within a single clock cycle. However, the synthesis tool often interprets these as parallel hardware elements (like independent flip-flops) that should trigger simultaneously. This difference between the simulated sequential behavior and the synthesized parallel hardware is the source of the mismatch. 

---

## Lab on Synthesis simulation mismatch

### Lab steps for synthesis and gls on ternary_operator
1. open gtk wave
   ```bash
   iverilog ternary_operator_mux.v tb_ternary_operator_mux.v
   ./a.out
   gtkwave tb_ternary_operator_mux.vcd
   ```
2. output of gtk wave
<img width="1638" height="1036" alt="ternary_operator_mux_waveform" src="https://github.com/user-attachments/assets/15f982c1-8165-4c69-a59a-1c8058dce1b6" />


3. open yosys
   ```bash
   yosys
   ```
4. synthesis of ternary_operator
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog ternary_operator_mux.v
   synth -top ternary_operator_mux
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   write_verilog -noattr ternary_operator_mux_net.v
   show
   ```
5. output of synthesis
<img width="1638" height="1036" alt="ternary_operator_mux_netlist" src="https://github.com/user-attachments/assets/76d7bf5e-dc78-4a09-a3ad-8132360eae8f" />


6. gls simulation for ternary_operator
   ```bash
   iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v ternary_operator_mux_net.v tb_ternary_operator_mux.v
   ./a.out
   gtkwave tb_ternary_operator_mux.vcd
   ```
7. output of gls
<img width="1638" height="1036" alt="ternary operator_gls_waveform" src="https://github.com/user-attachments/assets/0f8aed44-5206-46ca-80a3-7790222fd1ac" />


### Lab steps for synthesis and gls on bad_mux
1. open gtk wave
   ```bash
   iverilog bad_mux.v tb_bad_mux.v
   ./a.out
   gtkwave tb_bad_mux.vcd
   ```
2. output of gtk wave
<img width="1638" height="1036" alt="bad_mux_waveform" src="https://github.com/user-attachments/assets/ea0607cc-4e81-416d-9621-b73fd81c6070" />


3. open yosys
   ```bash
   yosys
   ```
4. synthesis of bad_mux
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog bad_mux.v
   synth -top bad_mux
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   write_verilog -noattr bad_mux_net.v
   show
   ```
5. output of synthesis
<img width="1638" height="1036" alt="bad_mux_netlist" src="https://github.com/user-attachments/assets/0d48ae77-ff8f-4c21-89c9-10dffb753a7d" />


6. gls simulation for bad_mux
   ```bash
   iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v bad_mux_net.v tb_bad_mux.v
   ./a.out
   gtkwave tb_bad_mux.vcd
   ```
7. output of gls
<img width="1638" height="1036" alt="bad_mux_gls_waveform" src="https://github.com/user-attachments/assets/2b800eaa-f30e-4f05-9a53-a007db22088a" />


##  Labs Overview

### Labs on GLS and Synthesis-Simulation Mismatch
These labs will guide you through running a **Gate-Level Simulation (GLS)** on a synthesized design. The primary goal is to compare the GLS waveform against the initial RTL simulation waveform to confirm that the synthesized hardware is functionally correct and matches the design intent.

---
## Lab steps for synthesis and gls on blocking_caveat
1. open gtk wave
   ```bash
   iverilog blocking_caveat.v tb_blocking_caveat.v
   ./a.out
   gtkwave tb_blocking_caveat.vcd

   ```
2. output of gtk wave
<img width="1638" height="1036" alt="blocking_caveat_waveform" src="https://github.com/user-attachments/assets/74e625c5-ae32-483e-935e-4f57237a496b" />


3. open yosys
   ```bash
   yosys
   ```
4. synthesis of blocking_caveat
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog blocking_caveat.v
   synth -top blocking_caveat
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   write_verilog -noattr blocking_cavnet_net.v
   show
   ```
5. output of synthesis
<img width="1638" height="1036" alt="blocking _caveat_netlist" src="https://github.com/user-attachments/assets/af50f219-ad0d-4f3c-8cbb-34d0543a0799" />

6. gls simulation for blocking_caveat
   ```bash
   iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v blocking_cavnet_net.v tb_blocking_caveat.v
   ./a.out
   gtkwave tb_blocking_caveat.vcd
   ```
7. output of gls
<img width="1638" height="1036" alt="blocking_caveat_gls_waveform" src="https://github.com/user-attachments/assets/403f6342-cb0d-4b2e-8d84-6ee2545fa675" />

