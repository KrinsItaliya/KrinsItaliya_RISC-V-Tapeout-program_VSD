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
