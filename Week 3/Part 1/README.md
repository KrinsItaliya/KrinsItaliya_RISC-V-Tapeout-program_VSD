# 📘 Week 3 Task - Post-Synthesis GLS & STA Fundamentals

🎯 Goal
Gain hands-on experience with Post-Synthesis Gate-Level Simulation (GLS) to confirm the synthesized circuit's behavior matches the original design, and explore the basics of Static Timing Analysis (STA) using tools like OpenSTA.

---
##
## 🧩 Step 1 – Perform synthesis of the BabySoC design. 

## 🧠 Yosys Synthesis Flow for VSDBabySoC

Here we do an module level synthesis beacuse it is not possible to work with gate level synthesis.

### --- Part 1: Read Libraries and Design ---

```bash
read_liberty -lib src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_liberty -lib src/lib/avsddac.lib
read_liberty -lib src/lib/avsdpll.lib
read_verilog src/module/vsdbabysoc.v
read_verilog -I src/include src/module/rvmyth.v
read_verilog -I src/include src/module/clk_gate.v
read_verilog src/module/avsddac_stub.v
read_verilog src/module/avsdpll_stub.v
```

### --- Part 2: Show the MODULE-LEVEL (RTL) design BEFORE synthesis for vsdbabysoc ---

```bash
show -format png -prefix vsdbabysoc_rtl vsdbabysoc
```

###  --- Part 3: Synthesize the Design (while preserving module boundaries) ---

```bash
synth -top vsdbabysoc
dfflibmap -liberty src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
opt
abc -liberty src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
### --- Part 4: Generate Gate-Level PNGs for Sub-modules (BEFORE flattening) ---

```bash
show -format png -prefix netlist_clk_gate clk_gate
show -format png -prefix netlist_avsdpll_stub avsdpll_stub
show -format png -prefix netlist_avsddac_stub avsddac_stub
```

### --- Part 5: Flatten the Design and Final Cleanup ---

```bash
flatten
setundef -zero
clean -purge
rename -enumerate
```
### --- Part 5: Write Final (Flattened) Netlist and Statistics ---

```bash
write_verilog -noattr reports/vsdbabysoc_netlist.v
stat -liberty src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
---

![vsdbabysoc_rtl](output_week3_part1/vsdbabysoc_rtl.png)
![netlist_clk_gate](output_week3_part1/netlist_clk_gate.png)
![output_synthesis](output_week3_part1/output_synthesis.png)
![printing_stestices_1](output_week3_part1/printing_stestices_1.png)
![printing_stestices_2](output_week3_part1/printing_stestices_2.png)
![printing_stestices_3](output_week3_part1/printing_stestices_3.png)

## 🧩 Part 2 – Post-Synthesis GLS

🔍 What Is GLS?
Gate-Level Simulation is used after synthesis to confirm that the behavior of the synthesized (gate-level) design still aligns with the original RTL description. It checks for logical correctness under synthesized conditions, optionally including timing.

### **Step 1: Compile the Testbench**
Run the following `iverilog` command to compile the testbench:
```bash
iverilog -o output/post_synth_sim/post_synth_sim.out   -DPOST_SYNTH_SIM -DFUNCTIONAL -DUNIT_DELAY=#1   -I src/include -I src/module src/module/testbench.v
```
---
### **Step 2: Navigate to the Post-Synthesis Simulation Output Directory**
```bash
cd output/post_synth_sim/
```
---
### **Step 3: Run the Simulation**

```bash
vvp post_synth_sim.out
```
### **Step 4: View the Waveforms in GTKWave**

```bash
gtkwave post_synth_sim.vcd
```
---

![post_synth_uut_gtkwave](output_week3_part1/post_synth_uut_gtkwave.png)
![post_synth_dac_gtkwave](output_week3_part1/post_synth_dac_gtkwave.png)
![post_synth_pll_gtkwave](output_week3_part1/post_synth_pll_gtkwave.png)

# 🔍 Functional vs Gate-Level Simulation in Digital Design

## 📘 Introduction

In digital design verification, two key types of simulations are used to validate a circuit’s correctness:

- **Functional Simulation**
- **Gate-Level Simulation (GLS)**

Though both serve to verify the design, they do so at different abstraction levels. This section explores their roles, differences, and why ensuring their outputs match is essential for a successful hardware design flow.

---

## 📊 Simulation Comparison Overview

| Feature                    | Functional Simulation                     | Gate-Level Simulation (GLS)                   |
|---------------------------|-------------------------------------------|-----------------------------------------------|
| 🧠 Abstraction Level       | RTL (Register Transfer Level)             | Gate-Level Netlist                            |
| 🛠️ Representation         | High-level behavioral code (e.g., Verilog) | Synthesized logic gates (AND, OR, DFFs, etc.) |
| ⏱️ Timing Information      | No (ideal logic)                          | Optional (can include unit/real delays)       |
| ⚡ Speed                  | Fast                                       | Slower (due to complexity)                    |
| 🎯 Purpose                | Validate logical behavior                 | Confirm synthesis didn’t change functionality |

---

## 🧩 Core Concepts

### 🔧 1. Functional Simulation

Functional simulation is performed **before synthesis** using the original RTL design.

#### ✅ Key Characteristics:

- Simulates logic behavior without physical timing.
- Used to check **logical correctness** of the RTL.
- Acts as the **golden reference** for later stages.
- Fast and efficient — ideal for early verification.

---

### 🔄 2. Logic Synthesis

Synthesis is the process of converting high-level RTL code into a **gate-level netlist** using tools like:

- **Yosys**
- **Synopsys Design Compiler**

#### 🧱 What Happens During Synthesis?

- RTL operations are mapped to actual hardware primitives:
  - Logic gates (AND, OR, XOR)
  - Flip-flops
  - Multiplexers
- The result is a netlist — a **structural representation** of the circuit ready for GLS and physical design.

---

### 🧪 3. Gate-Level Simulation (GLS)

GLS simulates the **synthesized netlist** to verify that synthesis preserved the RTL's behavior.

#### 🛠️ Features of GLS:

- Can include **unit delays** or **actual timing data**.
- Validates correct translation from RTL to gate-level.
- Used to catch issues like:
  - Glitches
  - Race conditions
  - Timing mismatches

#### 🧠 Why GLS Is Important:

It answers the question:  
**"Did the circuit still work as intended after synthesis?"**

---

## ✅ Why Matching Outputs Matters

If both simulations (functional & GLS) yield identical outputs, it confirms:

- ✔️ The synthesis tool maintained functional equivalence.
- ✔️ RTL logic was mapped accurately to hardware.
- ✔️ Timing assumptions (if included) did not affect functionality.

If they **don’t** match, possible causes include:

- ❌ Bugs in the RTL code only visible under gate-level timing.
- ❌ Incorrect synthesis constraints (e.g., clocks, resets).
- ❌ Tool configuration errors or library mismatches.

---

## 🧠 Conclusion

Understanding and running both **functional simulation** and **GLS** is crucial in the digital design workflow. Together, they ensure that:

- Your RTL design is functionally correct.
- The synthesized hardware behaves as expected.
- You're one step closer to a reliable silicon implementation.

> 🎯 **Best Practice:** Always verify your design both before and after synthesis to catch functional or synthesis-related issues early.

---

## 📚 Next Steps

Ready to go deeper? Explore:
- 🕓 Static Timing Analysis (STA)
- 🛠️ RTL linting and CDC checks
- 🏗️ Physical design (PD) and Place & Route (P&R)
