# 🧪 [Day 5] CMOS Power Supply & Device Variation Robustness  
> **Sky130 + Ngspice Simulation**

This simulation explores how **supply voltage changes** and **process/device variations** affect CMOS inverter behavior. Ensuring robustness in the presence of these variations is essential for reliable, real-world VLSI design.

---

## ⚡ Power Supply Variation

Digital circuits require a stable **V<sub>DD</sub>**, but real systems often experience fluctuations due to:

- IR drop in the power grid
- High switching activity
- External noise/interference
- Voltage droop from shared resources

These variations are critical to evaluate during design.

---

### 🔧 Effects of Power Supply Variation

| Variation Type         | Impact                                                            |
|------------------------|-------------------------------------------------------------------|
| **Lower V<sub>DD</sub>**         | Slower switching, reduced noise margin, risk of logic failure         |
| **Higher V<sub>DD</sub>**        | Faster circuits, but increased dynamic power and potential overstress |
| **Fluctuating V<sub>DD</sub>**   | Unpredictable behavior, possible metastability                        |

---

### ✅ Why It Matters

Robust circuits must handle supply noise gracefully. Supply variation analysis helps ensure:

- Consistent output transitions
- Preserved noise margins
- Reliable operation across corners

---

## ⚙️ Simulation: Power Supply Variation

Using **Ngspice**, we simulate the inverter VTC under varying V<sub>DD</sub>.

### 🛠️ File: `day5_inv_supplyvariation_Wp1_Wn036.spice`

#### ▶️ Run:
```bash
ngspice day5_inv_supplyvariation_Wp1_Wn036.spice
plot out vs in
```
📈 Output: 

<img width="3260" height="1476" alt="image" src="https://github.com/user-attachments/assets/2cdf0a38-6634-4721-83ef-f39df23ee6cc" />
<img width="1796" height="1106" alt="image" src="https://github.com/user-attachments/assets/ad1a73cc-0716-4db7-abd5-56c06739d49f" />

## ⚖️ Advantages and Disadvantages of Low Supply Voltage

Using lower supply voltage can reduce power consumption but increases risk of logic failure.

| ✅ Advantages | ⚠️ Disadvantages |
|---------------|------------------|
| Reduces dynamic power | Reduces noise margin |
| Less heat generated | Slows down switching |
| Improves efficiency | Increases sensitivity to variations |

---

## 🏗️ Device Variation

Even when using the same fabrication process, transistors on a chip are not exactly identical. Small physical differences occur during manufacturing, causing **device variations**.

### 🔬 Sources of Device Variations

| Source | Description |
|--------|-------------|
| Etching variations | Inaccurate pattern transfer during lithography |
| Oxide thickness variations | Uneven gate oxide affects transistor behavior |
| Channel doping variations | Changes in impurity concentration affect switching |
| Line edge roughness | Irregular transistor dimensions |

These variations change transistor behavior from its design intent, affecting circuit performance.

---
<img width="2026" height="1006" alt="image" src="https://github.com/user-attachments/assets/d1a3725e-ba67-4460-b9d7-9c97226814de" />

### inverter chain analysis:
<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/7d00ea02-0408-4e85-85e0-212654ab6e1d" />
<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/1323c374-3106-4248-91c8-5b4638cb597b" />


## 🧪 Simulation of Device Variations

Smart SPICE simulations help observe inverter behavior under **process variations**. Random variations are applied to transistor dimensions and material properties to evaluate robustness. This ensures the design works even in worst-case silicon fabrication scenarios.

---
### simulation steps:
### step-1:
```bash
ngspice day5_inv_devicevariation_wp7_wn042.spice
plot out vs in
```

### output:
<img width="2130" height="1428" alt="image" src="https://github.com/user-attachments/assets/46bee441-8632-4517-b061-081edcf153e2" />

<img width="2028" height="1010" alt="image" src="https://github.com/user-attachments/assets/62cf93d5-2dfb-4da3-b252-065ec34be97e" />


## ✅ Design Robustness

Robust CMOS design must tolerate:
- Power supply changes
- Manufacturing variations
- Temperature changes
- Electrical noise

To improve robustness:
- Proper transistor sizing
- Using guard bands
- Corner simulations
- Power integrity design

---






