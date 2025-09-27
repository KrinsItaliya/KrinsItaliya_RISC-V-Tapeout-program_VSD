# 📘 Day 1 — Verilog RTL Design, Simulation & Synthesis with Open-Source Tools

This document presents **Day 1** of the RTL Design & Synthesis Workshop using **Verilog**, **Icarus Verilog**, **GTKWave**, **Yosys**, and the **Sky130 PDK**.

---

## 🧠 Topics Covered

- RTL (Register Transfer Level) design using Verilog  
- Functional simulation using Icarus Verilog  
- Waveform visualization via GTKWave  
- Logic synthesis with Yosys  
- Mapping to standard cells using Sky130 PDK  

---

## 📝 RTL Design Overview

**Verilog** is an HDL (Hardware Description Language) used to describe digital systems.  
**RTL** refers to the design abstraction where data flows between registers through combinational logic.

### 💡 Core Concepts

- RTL = Flip-Flops (Sequential Logic) + Combinational Logic  
- Simulation helps verify design correctness  
- Synthesis maps RTL to real hardware using logic gates  
- Tools used:
  - `iverilog` — Simulator  
  - `gtkwave` — Waveform Viewer  
  - `yosys` — Synthesis Tool  
  - `Sky130` — Open-source Standard Cell Library  

---

## 🧪 Lab: End-to-End Flow — Simulation + Synthesis

Follow the steps below to run simulation, view waveforms, synthesize the design, and generate the gate-level netlist using Sky130 cells.

### 🛠️ Step-by-Step Instructions

#### 🔁 1. Clone the Workshop Repository

```bash
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
cd sky130RTLDesignAndSynthesisWorkshop/verilog_files/
```
▶️ 2. Compile the RTL and Testbench
```bash
iverilog good_mux.v tb_good_mux.v -o sim.out
```
Compile and run:
   ```bash
   iverilog good_mux.v tb_good_mux.v
   ./a.out 
    gtkwave tb_good_mux.vcd
   ```
3. Output:
<img width="1641" height="1028" alt="good_mux_waveform" src="https://github.com/user-attachments/assets/fe41d0c2-2b04-46d2-908b-af5f6226a9b0" />



## Introduction to Yosys and Logic Synthesis
 **Theory**

Yosys is an open-source logic synthesis tool.  
It transforms RTL into a gate-level netlist and can map designs to a PDK\u2019s standard cells.

**Workflow:**
- Read RTL \u2192 `read_verilog your_design.v`  
- Synthesize Logic \u2192 `synth`  
- Map to Standard Cells \u2192 `abc -liberty your_lib.lib`  
- View Design (Optional) \u2192 `show`  
- Export Netlist \u2192 `write_verilog output.v`

----
## Lab using yosys

###  Lab Steps for synthesis
1. open yosys
   ```bash
   yosys
   ```
2. for synthesis run below comment in yosys
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
    read_verilog good_mux.v
    synth -top good_mux
    abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
    show
   ```
3.Output:
<img width="1636" height="997" alt="netlist" src="https://github.com/user-attachments/assets/17906170-f644-40a4-add5-b226057428ca" />


4. For netlist:
   ```bash
   write_verilog -noattr good_mux_netlist.v
   !vim good_mux_netlist.v
   ```
5. Output of Netlist:
![good_mux_netlist](https://github.com/user-attachments/assets/46ca3f2f-6cde-4d7f-b028-0d0a41701464)



---

##  Key Outcomes from Day 1

- Understood **RTL Design** concepts using Verilog.  
- Learned to simulate designs with **Icarus Verilog (iverilog)**.  
- Visualized waveforms using **GTKWave**.  
- Gained introduction to **Yosys** for logic synthesis.  
- Performed synthesis with **Sky130 PDK standard cells**.  
- Observed how RTL is transformed into a **gate-level netlist**.  
