# 📘 Week 2 Task – BabySoC Fundamentals & Functional Modelling

## Objective
To build a solid understanding of SoC fundamentals and practice functional modelling of the BabySoC using simulation tools such as **Icarus Verilog** and **GTKWave**.

## PART 2 Labs on - (Hands-on Functional Modelling)

## 📂 Project Structure

```txt
VSDBabySoC/
├── src/
│   ├── include/      # Header files (*.vh)
│   ├── module/       # Verilog + TLV modules
│   │   ├── vsdbabysoc.v   # Top-level module
│   │   ├── rvmyth.v       # CPU
│   │   ├── avsdpll.v      # PLL
│   │   ├── avsddac.v      # DAC
│   │   └── testbench.v    # Testbench
└── output/           # Simulation outputs
```

---

## 🛠️ Setup

### 📥 Cloning the Project

```bash
cd ~/VLSI
git clone https://github.com/manili/VSDBabySoC.git
cd VSDBabySoC/
```
---

## 🔧 TLV → Verilog Conversion

Since **RVMYTH** is written in **TL-Verilog (.tlv)**, we need to convert it to Verilog before simulating.

```bash
# Install tools
sudo apt update
sudo apt install python3-venv python3-pip

# Create virtual env
python3 -m venv sp_env
source sp_env/bin/activate

# Install SandPiper-SaaS
pip install pyyaml click sandpiper-saas

# Convert TLV → Verilog
sandpiper-saas -i ./src/module/*.tlv -o rvmyth.v --bestsv --noline -p verilog --outdir ./src/module/
```

✅ Now you’ll have `rvmyth.v` alongside your other Verilog files.

---

You need to add  two extra files for the synthesis which is ```avsddac_stub.v```and ```avsdpll_stub.v``` this is uploaded on this week2 part-2 (testbench and design file folder).

---

## 🧪 Simulation Steps

# 🔍 Pre-Synthesis Simulation Waveforms (GTKWave)

This repository contains the simulation results of a System-on-Chip (SoC) design using GTKWave for three main modules: Core, DAC, and PLL. These results were captured from a pre-synthesis simulation using the pre_synth_slm.vcd waveform file.

### pre-synthesis simulation

```bash
mkdir -p output/pre_synth_sim

iverilog -o output/pre_synth_sim/pre_synth_sim.out \
  -DPRE_SYNTH_SIM \
  -I src/include -I src/module \
  src/module/testbench.v

cd output/pre_synth_sim
./pre_synth_sim.out
```
### for see the gtk wave

```bash
gtkwave pre_synth_sim.vcd
```



### 1. CPU Core and Memory Interface

## Waveform Analysis
---

<img width="1639" height="1037" alt="pre_synthesis_core_gtkwave" src="https://github.com/user-attachments/assets/899d024d-444e-4586-b62e-bb1a4169c961" />

---
## 🔧 1. Core Waveform Analysis

Hierarchy: vsdbabysoc_tb -> u_dut -> core

Timestamp: 4790 ns

Analysis:
The waveform verifies the correct functioning of the CPU core logic.

OUT[9:0] = 210 confirms the core is generating expected output.

CPU_dmem_addr, CPU_dmem_rd_data, and CPU_dmem_rd_en are active, indicating memory read operations are taking place.

clk and rd_valid signals are toggling appropriately, synchronized with memory accesses.

This validates the instruction execution and memory interface in the processor pipeline, ensuring correct behavior before synthesis.

✅ Purpose: Verifies correct instruction flow and memory interactions in the RISC-V-based core before synthesis.

---

### 2. Digital-to-Analog Converter (DAC) Verification

## Waveform Analysis
---
<img width="1639" height="1037" alt="pre_synthesis_DAC" src="https://github.com/user-attachments/assets/663d5d00-5214-45f7-a603-1e3abe1d3fb6" />

---
## 🎛️ 2. DAC Waveform Analysis

Hierarchy: vsdbabysoc_tb -> u_dut -> dac

Timestamp: 84460 ns

Analysis:
This waveform confirms that the Digital-to-Analog Converter (DAC) module is working as intended.

D[9:0] and Dext[10:0] carry the digital data to be converted.

OUT shows a smoothly varying analog-equivalent waveform, indicating proper digital-to-analog conversion.

VREFH = 1 and VREFL = 0 establish the reference voltage range.

EN signal is high, enabling DAC output generation.

This validates the DAC's ability to translate digital signals into analog values — a key component for mixed-signal SoC applications.

✅ Purpose: Demonstrates the DAC output waveform generation from digital inputs and confirms accurate analog conversion simulation.

---


### 3. Phase-Locked Loop (PLL) Verification

## Waveform Analysis

<img width="1639" height="1037" alt="pre_synthesis_pll_gtkwave" src="https://github.com/user-attachments/assets/1b9fe4b3-0649-4daa-ae55-4c8d05e0ff40" />

## ⏱️ 3. PLL Waveform Analysis

Hierarchy: vsdbabysoc_tb -> u_dut -> pll

Timestamp: 18420 ns

Analysis:
The waveforms confirm the Phase-Locked Loop (PLL) is functioning correctly.

REF (reference clock) is toggling with a constant period.

VCO_IN shows voltage variations that control the VCO frequency.

Calculated period = 35.416 ns and refpd = 283.33 ns demonstrate the output clock frequency generation.

CLK output is active and stable, confirming proper frequency synthesis.

This test validates the clock generation subsystem, ensuring that the SoC receives a stable and controlled clock signal.

✅ Purpose: Confirms that PLL frequency synthesis and phase locking mechanisms are operational during simulation.

---

## 🧠 Yosys Synthesis Flow for VSDBabySoC

Here we do an module level synthesis beacuse it is not possible to work with gate level synthesis.

### --> Part 1: Read Libraries and Design ---

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

### --> Part 2: Show the MODULE-LEVEL (RTL) design BEFORE synthesis for vsdbabysoc ---

```bash
show -format png -prefix vsdbabysoc_rtl vsdbabysoc
```

###  --> Part 3: Synthesize the Design (while preserving module boundaries) ---

```bash
synth -top vsdbabysoc
dfflibmap -liberty src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
opt
abc -liberty src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
### --> Part 4: Generate Gate-Level PNGs for Sub-modules (BEFORE flattening) ---

```bash
show -format png -prefix netlist_clk_gate clk_gate
show -format png -prefix netlist_avsdpll_stub avsdpll_stub
show -format png -prefix netlist_avsddac_stub avsddac_stub
```

### --> Part 5: Flatten the Design and Final Cleanup ---

```bash
flatten
setundef -zero
clean -purge
rename -enumerate
```
### --> Part 5: Write Final (Flattened) Netlist and Statistics ---

```bash
write_verilog -noattr reports/vsdbabysoc_netlist.v
stat -liberty src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
---

## Output photo :

<img width="1639" height="1037" alt="vsdbabysoc_rtl" src="https://github.com/user-attachments/assets/85546cc8-2554-48ce-956c-004f17c20847" />
![netlist_clk_gate](https://github.com/user-attachments/assets/2eae7612-3da7-441c-b3bd-c04ce3a4e278)
<img width="1464" height="934" alt="output_synthesis" src="https://github.com/user-attachments/assets/d3ef15e3-54d4-4f64-ba03-71ee22a1a87e" />
<img width="1592" height="753" alt="printing_statistics_1" src="https://github.com/user-attachments/assets/5848ab63-468f-4462-9204-b077df2b5748" />
<img width="1598" height="966" alt="printing_statistics_2" src="https://github.com/user-attachments/assets/2a5d0c06-afe8-437a-a8e8-206554a3cde1" />
<img width="1464" height="932" alt="printing_statistics_3" src="https://github.com/user-attachments/assets/f8b8b984-8188-4daf-8ba1-ddaee3b16587" />
<img width="1464" height="932" alt="printing_statistics_4" src="https://github.com/user-attachments/assets/9663a96b-6c2b-4fea-b32d-4b0469cbd77c" />

##  Post-Synthesis Verification (GLS)

To ensure the synthesis process did not alter the design's logic, we perform a **Gate-Level Simulation (GLS)**. This involves simulating the generated netlist with the same testbench used for the original RTL.

### **Command**: The `iverilog` command for GLS is more complex, as it requires the Verilog models for the Sky130 standard cells.

```bash
iverilog -DFUNCTIONAL -DUNIT_DELAY=#1 -o simulation/post_synth_sim.out src/gls_model/primitives.v src/gls_model/sky130_fd_sc_hd.v reports/vsdbabysoc_netlist.v src/module/avsdpll.v src/module/avsddac.v src/module/testbench.v
 ```

### **Execution**:

 ```bash
 cd simulation
 ./post_synth_sim.out
 gtkwave dump.vcd
 ```

### Output GTKWave

<img width="1639" height="1037" alt="post_synthesis_vsdbabysoc_gtkwave" src="https://github.com/user-attachments/assets/d142161e-5d2c-4456-b5f8-ffa58fe8a4e6" />


<img width="1639" height="1037" alt="post_synthesis_uut_gtkwave" src="https://github.com/user-attachments/assets/1d7bd4ae-a603-4ca9-9dd2-26df888bb00c" />


## 🔑 Key Takeaways

🧠 Learned SoC basics by simulating and analyzing core modules: CPU, DAC, and PLL.

🔄 Converted TL-Verilog to Verilog using SandPiper for CPU simulation.

📊 Verified functionality using GTKWave — ensured correct data flow, analog output, and clock generation.

⚙️ Synthesized design with Yosys using Sky130 libraries and custom DAC/PLL models.

🧩 Visualized modules pre- and post-synthesis, understanding how RTL maps to gate-level.

✅ Performed Gate-Level Simulation (GLS) to confirm logic correctness after synthesis.

🚀 From RTL to gate-level — built, simulated, synthesized, and verified a complete BabySoC!
