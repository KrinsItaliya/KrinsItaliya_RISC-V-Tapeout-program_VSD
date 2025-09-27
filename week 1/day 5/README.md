# 📘 Day 5 — Control Structures in Synthesis & Loop-Based Hardware Generation

This document summarizes the core concepts explored on **Day 5**, focusing on how Verilog constructs like `if`, `case`, and loops influence the synthesized hardware. Understanding these coding patterns is critical for writing efficient and predictable RTL.

---

## ⚖️ Understanding `if` vs `case` in Synthesis

Conditional statements (`if`, `else`, `case`) are commonly used in RTL, but they translate to different hardware forms depending on how they're written.

### 🔹 `if-else` Chains
- When written as a chain (`if`, `else if`, `else`), this structure is interpreted as a **priority-based decision block**.
- The hardware generated reflects a **priority encoder**, where conditions are evaluated one after another in the given order.
- This can increase delay, especially when the condition list is long.

### 🔹 `case` Statements
- A `case` statement is typically synthesized into a **parallel decision logic**, often implemented as a **multiplexer tree**.
- Each option in the `case` is treated equally, making it more timing-friendly than a priority chain.
  
### ⚠️ Missing Assignments Lead to Latches
- If your `if` or `case` block does **not** assign a value to a variable in **every path**, synthesis tools will insert a **latch** to "hold" the previous value.
  
#### Why Latches Are Problematic:
- Latches are level-sensitive and not synchronized to a clock edge.
- They are harder to verify, can cause unintended behavior, and create difficulties in timing closure.
  
✅ **Best Practice:** Always assign a value to every output in all branches or include a `default` assignment to avoid latch inference.

---

## 🔁 Loop Constructs: `for loop` vs `generate for`

Loops are useful in hardware description, but it's essential to distinguish their usage in behavioral vs structural code.

### 🔄 Behavioral `for` Loops (Inside `always`)
- These are evaluated during synthesis and the tool **unrolls** them to create equivalent logic.
- It’s not actual looping hardware, but just a **repetition of logic**.
- Commonly used in creating shift registers, counters, or logic trees.

📌 Think of it as: **One machine performing many steps in order.**

---

### 🧱 Structural `generate for`
- Used outside procedural blocks to **instantiate multiple hardware elements**.
- The synthesis tool creates multiple hardware blocks based on the loop iterations.
- Great for building scalable designs like multi-bit registers, ALUs, or buses.

📌 Think of it as: **Multiple machines doing the same task in parallel.**

---

## 🔬 Lab Activities

### 🧪 Incomplete `if` and `case`
- You’ll experiment with conditional structures where not all paths assign a value to the output.
- These cases will help you observe **unexpected latch inference** during synthesis using tools like Yosys.
- You'll then fix the design by ensuring **full output coverage** across all conditions.

---

## ✅ Summary Table

| Construct Type   | Hardware Generated         | Usage Scope               | Key Risk            |
|------------------|-----------------------------|----------------------------|----------------------|
| `if-else` Chain  | Priority Logic (Encoder)    | Combinational Logic        | Can increase delay   |
| `case` Statement | Parallel Logic (MUX Tree)   | Combinational Logic        | Risk of latch        |
| `for` Loop       | Unrolled Combinational      | Inside `always` blocks     | Cannot instantiate   |
| `generate for`   | Repeated Modules/Registers  | Outside `always` blocks    | Synthesis-only usage |

---

> 💡 Tip: Cleaner, more complete RTL = more predictable, efficient hardware. Avoid incomplete branches and understand how synthesis interprets control structures.

## Lab on incomplete if
### steps of incomplete_if
1. open gtkwave
   ```bash
   iverilog incomp_if.v tb_incomp_if.v 
   ./a.out
   gtkwave tb_incomp_if.vcd
   ```
2. output of gtkwave
<img width="1638" height="1036" alt="incomp_if_waveform" src="https://github.com/user-attachments/assets/f382cd6d-6eb2-4f92-a45b-912997be8ffb" />


3. open yosys
   ```bash
   yosys
   ```
4. synthesis of incomplete_if
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog incomp_if.v
   synth -top incomp_if
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show
   ```
5. output of synthesis
<img width="1638" height="1036" alt="incomp_if_netlist" src="https://github.com/user-attachments/assets/0d0ff257-6977-40a5-9564-d31f69385c11" />


### steps of incomplete_if2
1. open gtkwave
   ```bash
   iverilog incomp_if2.v tb_incomp_if2.v 
   ./a.out
   gtkwave tb_incomp_if2.vcd
   ```
2. output of gtkwave
<img width="1638" height="1036" alt="incomp_if2_waveform" src="https://github.com/user-attachments/assets/43cfa593-ee0c-4369-9ba1-b064ac7ba20a" />


3. open yosys
   ```bash
   yosys
   ```
4. synthesis of incomplete_if2
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog incomp_if2.v
   synth -top incomp_if2
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show
   ```
5. output of synthesis
<img width="1638" height="1036" alt="incomp_if2_netlist" src="https://github.com/user-attachments/assets/e0e6ec7c-f983-4240-9613-04002d6b9bbc" />



* **Labs on "Incomplete overlapping `case`":** These labs will explore how priority or overlapping case conditions are synthesized, often resulting in priority logic similar to `if-else` chains.

## Lab on incomplete case
### Steps of incomp_case

1. open gtk wave
   ```bash
   iverilog incomp_case.v tb_incomp_case.v 
   ./a.out
   gtkwave tb_incomp_case.vcd
   ```
2. output of gtk wave
<img width="1638" height="1036" alt="incomp_csae_waveform" src="https://github.com/user-attachments/assets/f92a4453-8a98-4042-a822-f9759e5c06fc" />


3. open yosys
   ```bash
   yosys
   ```
4. synthesis of incomp_case
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog incomp_case.v
   synth -top incomp_case
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show
   ```
5. output of synthesis
<img width="1638" height="1036" alt="incomp_case_netlist" src="https://github.com/user-attachments/assets/51021c9b-2a1f-41e3-9e74-da1ea7ad5d7a" />


### Steps of comp_case

1. open gtk wave
   ```bash
   iverilog comp_case.v tb_comp_case.v 
   ./a.out
   gtkwave tb_comp_case.vcd
   ```
2. output of gtk wave
<img width="1638" height="1036" alt="comp_case_waveform" src="https://github.com/user-attachments/assets/ed0042e0-75d1-46df-acc8-5bb9786d6f26" />


3. open yosys
   ```bash
   yosys
   ```
4. synthesis of comp_case
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog comp_case.v
   synth -top comp_case
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show
   ```
5. output of synthesis
<img width="1638" height="1036" alt="comp_case_netlist" src="https://github.com/user-attachments/assets/0050fa76-f5d2-4999-a24f-72b13c99cae0" />


### Steps of partial_case_assign

1. open gtk wave
   ```bash
   iverilog partial_case_assign.v tb_partial_case_assign.v 
   ./a.out
   gtkwave tb_partial_case_assign.vcd
   ```
2. output of gtk wave
<img width="643" height="407" alt="partial_case_assign_Waveorm" src="https://github.com/user-attachments/assets/7311bf11-0a28-48fd-9182-39b2963bbe33" />


3. open yosys
   ```bash
   yosys
   ```
4. synthesis of partial_case_assign
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog partial_case_assign.v
   synth -top partial_case_assign
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib\u00a0
   show
   ```
5. output of synthesis
<img width="1638" height="1036" alt="partial_case_assign_netlist" src="https://github.com/user-attachments/assets/0ad046b4-6bd9-42fb-9d3d-5afd081afa8f" />


### Steps of bad_case

1. open gtk wave
   ```bash
   iverilog bad_case.v tb_bad_case.v
   ./a.out
   gtkwave tb_bad_case.vcd
   ```
2. output of gtk wave
<img width="1638" height="1036" alt="bad_case_waveform" src="https://github.com/user-attachments/assets/0c067a0f-de7b-4854-a369-e2dae48baf73" />


3. open yosys
   ```bash
   yosys
   ```
4. synthesis of bad_case
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog bad_case.v
   synth -top bad_case
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   write_verilog -noattr blocking_cavnet_net.v
   show
   ```
5. output of synthesis
<img width="1638" height="1036" alt="bad_case_netlist" src="https://github.com/user-attachments/assets/95283ef5-929f-49d7-b58d-67ac41335c6e" />


6. gls for bad_case
   ```bash
   iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v bad_case_net.v tb_bad_case.v
   ./a.out
   gtkwave\u00a0tb_bad_case.vcd
   ```

7. output of gls
<img width="1638" height="1036" alt="bad_case_gls_waveform" src="https://github.com/user-attachments/assets/33ba35f5-de13-41bf-9029-1e4e226217e8" />


* **Labs on "`for loop`" and "`for generate`":** You will implement a function (like a multi-bit shifter or adder) using both constructs. By comparing the synthesized schematics, you will see the clear structural difference between a single unrolled combinational block (`for loop`) and multiple parallel instances (`for generate`).

## Labs on for and for genrater
### Steps of mux_generate

1. open gtk wave
   ```bash
   iverilog mux_generate.v tb_mux_generate.v
   ./a.out
   gtkwave tb_mux_generate.vcd
   ```
2. output of gtk wave
<img width="1638" height="1036" alt="mux_generate_waveform" src="https://github.com/user-attachments/assets/8e083f91-9a2b-4153-b129-17211b612d4a" />


3. open yosys
   ```bash
   yosys
   ```
4. synthesis of mux_generate
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog mux_generate.v
   synth -top mux_generate
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   write_verilog -noattr mux_generate_net.v
   show
   ```
5. output of synthesis
<img width="1638" height="1036" alt="mux_generate_netlist" src="https://github.com/user-attachments/assets/28e6c021-cb53-4c04-886f-06e481d87a07" />


6. gls for mux_generate
   ```bash
   iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v mux_generate_net.v tb_mux_generate.v
   ./a.out
   gtkwave tb_mux_generate.vcd
   ```

7. output of gls
<img width="1638" height="1036" alt="mux_generate_gls_waveform" src="https://github.com/user-attachments/assets/61a26fa2-3535-4e4e-bb23-4f75fdb784ee" />


### Steps of demux_case

1. open gtk wave
   ```bash
   iverilog demux_case.v tb_demux_case.v
   ./a.out
   gtkwave tb_demux_case.vcd
   ```
2. output of gtk wave
<img width="1638" height="1036" alt="demux_case_waveform" src="https://github.com/user-attachments/assets/686991a3-584d-4e6d-bb94-c00a60376966" />


3. open yosys
   ```bash
   yosys
   ```
4. synthesis of demux_case
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog demux_case.v
   synth -top demux_case
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   write_verilog -noattr demux_case.v
   show
   ```
5. output of synthesis
<img width="1638" height="1036" alt="demux_case_netlist" src="https://github.com/user-attachments/assets/0d5a8729-e668-424b-b921-de0799344abc" />


6. gls for demux_case
   ```bash
   iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v demux_case_net.v tb_demux_net.v
   ./a.out
   gtkwave tb_demux_case.vcd
   ```

7. output of gls
![demux_case_gls_waveform](https://github.com/user-attachments/assets/1da54ec2-82f3-4bd2-86c1-2719c3351708)




### Steps of demux_generate

1. open gtk wave
   ```bash
   iverilog demux_generate.v tb_demux_generate.v
   ./a.out
   gtkwave tb_demux_generate.vcd
   ```
2. output of gtk wave
<img width="1638" height="1036" alt="demux_generate_waveform" src="https://github.com/user-attachments/assets/f74485e3-cfc7-497b-8fd7-842b3fe324e3" />


3. open yosys
   ```bash
   yosys
   ```
4. synthesis of demux_generate
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog demux_generate.v
   synth -top demux_generate
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   write_verilog -noattr demux_generate_net.v
   show
   ```
5. output of synthesis
<img width="1638" height="1036" alt="demux_generate_netlist" src="https://github.com/user-attachments/assets/d4be550d-5a02-4010-85af-e39cd90b42c2" />


6. gls for demux_generate
   ```bash
   iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v demux_generate_net.v tb_demux_generate.v
   ./a.out
   gtkwave tb_demux_generate.vcd
   ```

7. output of gls
<img width="1638" height="1036" alt="demux_generate_waveform_gls" src="https://github.com/user-attachments/assets/f0269729-03ce-4a7e-aacc-dc8b1e85c97a" />

### Steps of ripple_carry_adder

1. open gtk wave
   ```bash
   iverilog fa.v rca.v tb_rca.v
   ./a.out
   gtkwave tb_rca.vcd
   ```
2. output of gtk wave
<img width="1638" height="1036" alt="rca_waveform" src="https://github.com/user-attachments/assets/94bc2465-fa7b-46c3-9c7a-861d8decf4ec" />


3. open yosys
   ```bash
   yosys
   ```
4. synthesis of ripple_carry_adder
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog fa.v rca.v
   synth -top fa rca
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   write_verilog -noattr rca_net.v
   select top rca
   show
   ```
5. output of synthesis
<img width="1638" height="1036" alt="rca_netlist" src="https://github.com/user-attachments/assets/3df51c1e-e9fa-462e-9096-7ca0f293f1c3" />


6. gls for ripple_carry_adder
   ```bash
   iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v rca_net.v tb_rca.v
   ./a.out
   gtkwave\u00a0tb_rca.vcd
   ```

7. output of gls
<img width="1638" height="1036" alt="rca_waveform_gls" src="https://github.com/user-attachments/assets/a869962d-4b94-477e-b3df-dc4344521605" />


# 📌 Key Takeaways from Day 5

A concise recap of the fundamental RTL-to-gate synthesis behaviors discussed:

- **Conditional Logic: `if-else` vs. `case`**
  - The `if-else-if` structure results in **priority-based logic**, where each condition is checked sequentially, which can lengthen critical paths.
  - On the other hand, `case` statements typically yield **parallel decision logic** (like multiplexers), which is often more efficient in hardware.

- **Unintended Latches**
  - If not all possible paths assign a value to an output in a conditional block (`if` or `case`), synthesis tools insert **latches** to preserve signal state. These are generally discouraged due to potential timing issues and verification complexity.

- **Behavioral Loops (`for`)**
  - A `for` loop used within a procedural block is **unrolled** during synthesis, producing a repeated block of logic rather than actual looping hardware. It's mainly suited for combinational designs.

- **Structural Generation (`generate for`)**
  - The `generate for` construct is used for **replicating hardware elements**, such as instantiating modules or registers multiple times. It's evaluated at elaboration time, not during simulation or synthesis.

---


