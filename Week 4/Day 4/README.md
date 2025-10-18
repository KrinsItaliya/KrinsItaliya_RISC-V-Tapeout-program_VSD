
# 🔊 [Day-4] CMOS Noise Margin – SPICE Simulation using Sky130

This module explores the **Noise Margin** characteristics of a **CMOS inverter** using SPICE simulation with the Sky130 PDK. Noise Margin is critical to ensure reliable logic-level operation under real-world noise and variation conditions.

---

## 📌 What is Noise Margin?

In digital circuits, **Noise Margin** refers to the tolerance of logic signals against electrical interference. It defines how much noise a signal can withstand without being misinterpreted.

A robust digital design ensures:
- Logic `1` is not falsely read as `0`
- Logic `0` is not falsely read as `1`

This margin ensures the **integrity** of logic levels across multiple stages of logic gates.

---

## 🔧 CMOS Inverter and Noise Behavior

A **CMOS inverter** is made of:
- **PMOS** transistor pulling the output high when input is low
- **NMOS** transistor pulling the output low when input is high

Due to real-world effects (like power supply droop, temperature, or capacitive loading), the output voltage levels may degrade. This introduces potential for logic errors.

**Noise Margin** quantifies the safe operating region by comparing:
- Ideal logic levels (`VOH`, `VOL`)
- Actual switching threshold (`VM`)
- Acceptable noise levels (`NMH`, `NML`)

---

## 📊 Static Voltage Characteristics

Digital signals operate in voltage ranges:
- **Logical HIGH (1)**: e.g., 1.8V
- **Logical LOW (0)**: e.g., 0V

The CMOS inverter’s **Voltage Transfer Characteristic (VTC)** helps visualize:
- Transition from HIGH to LOW
- Regions of valid logic
- Margins for noise immunity

📷 *Ideal vs Practical Noise Margins:*

![Ideal vs Practical](output_week4/noise_margin_ideal_practical.jpeg)

📷 *Noise Margin Regions on VTC:*

![VTC Curve with NMH/NML](output_week4/noise_margin.jpeg)

---

## ✅ Why Noise Margin Matters

| Benefit | Description |
|---------|-------------|
| ✔️ Signal Reliability | Handles real-world noise gracefully |
| ✔️ Robustness | Tolerates temperature & manufacturing variations |
| ✔️ Design Confidence | Ensures logic levels are interpreted correctly |
| ✔️ Interconnect Safety | Maintains signal integrity over long wires |

---

## 🔄 Key Factors Influencing Noise Margin

| Parameter | Impact |
|-----------|--------|
| **V<sub>DD</sub> (Supply Voltage)** | Lower V<sub>DD</sub> reduces voltage swing, decreases margin |
| **Transistor Sizing (W/L)** | Impacts switching point and output drive strength |
| **PMOS/NMOS Ratio** | Alters threshold voltage (VM) and output swing |
| **Temperature** | High temp increases resistance, reduces noise immunity |
| **Process Variations** | Can shift V<sub>t</sub>, affect drive strength |
| **Load Capacitance** | Affects transition speed and signal fidelity |

---

## ⚖️ Impact of PMOS Width (W<sub>P</sub>)

Adjusting PMOS width affects:

- The **inverter’s switching threshold (V<sub>M</sub>)**
- The **rise time** (since PMOS pulls up)
- The **output HIGH level stability**

🔁 **Wider PMOS** → Stronger pull-up → Higher VM  
⚠️ But can also lead to:
- Increased power consumption
- Higher input capacitance

📷 *Comparison of noise margins with different PMOS sizes:*

![Comparison](output_week4/noise_margin_comparison.jpeg)

📷 *Summary Diagram:*

![Summary](output_week4/noise_margin_summary.jpeg)

---

## 🧪 Practical Simulation with Sky130 (Using ngspice)

### 🔍 Objective:
Simulate the **VTC of a CMOS inverter** and extract **Noise Margins** for a given sizing configuration.

### 🔧 File Used:
`day4_inv_noisemargin_wp1_wn036.spice`  
- PMOS width: 1µm  
- NMOS width: 0.36µm  
- L = 150nm (default for Sky130)

---

### ▶️ Simulation Steps:

#### Step 1: Run ngspice
```bash
ngspice day4_inv_noisemargin_wp1_wn036.spice
