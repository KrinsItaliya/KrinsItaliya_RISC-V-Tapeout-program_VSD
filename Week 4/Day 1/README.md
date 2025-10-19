
# 🧠 [DAY-1] Basics of NMOS Drain Current (Id) vs Drain-to-Source Voltage (Vds)

---

## 1. Introduction to Circuit Design and SPICE Simulations

### 1.1 Why do we need SPICE simulations?

SPICE (Simulation Program with Integrated Circuit Emphasis) is a powerful tool used to simulate and analyze electronic circuits before they are physically fabricated.  
It allows designers to predict how a circuit will behave under different voltage, current, and temperature conditions.  

SPICE helps engineers understand nonlinear device characteristics such as those found in transistors and diodes, and it greatly reduces the need for physical prototyping — saving both time and cost.  
Its models are built using mathematical equations derived from semiconductor device physics, giving accurate and realistic circuit predictions.

---

### 1.2 Basic Element in Circuit Design – NMOS

The NMOS (n-channel MOSFET) is a voltage-controlled semiconductor device used widely for switching and amplification.  
It has four terminals — **Gate (G)**, **Source (S)**, **Drain (D)**, and **Body (B)**.

When the gate-to-source voltage (Vgs) becomes greater than the threshold voltage (Vth), an inversion layer forms on the surface of the semiconductor substrate.  
This creates a conductive channel that allows electrons to move from the drain to the source terminal.

**Key parameters:**
- Vgs: Voltage between gate and source  
- Vds: Voltage between drain and source  
- Vth: Threshold voltage that determines when the transistor turns ON

In NMOS devices, the current flow is due to electrons (majority carriers), and the strength of conduction is controlled by the applied gate voltage.

<img width="1280" height="800" alt="image" src="https://github.com/user-attachments/assets/1ee29181-1856-4fc5-926f-07aebe9aa471" />
<img width="1280" height="800" alt="image" src="https://github.com/user-attachments/assets/179899d9-05d0-48f9-86ff-62d2638cc51f" />
---

### 1.3 Strong Inversion and Threshold Voltage

When the gate voltage exceeds the threshold voltage (Vgs > Vth), the MOSFET enters **strong inversion**.  
In this state, a dense electron channel forms under the gate oxide, and current flows easily between drain and source.

If the gate voltage is below Vth, the MOSFET operates in the **weak inversion** or **subthreshold region**, where only a small leakage current flows.  

Thus, Vth marks the transition between OFF and ON states of the transistor.

<img width="1280" height="800" alt="image" src="https://github.com/user-attachments/assets/1ffe3d8a-a2f2-4a5d-ac92-52d7379f4739" />
<img width="1280" height="800" alt="image" src="https://github.com/user-attachments/assets/2e37b23d-5077-4334-81c6-adf0cb9f826f" />
---

### 1.4 Threshold Voltage with Positive Substrate Potential

The threshold voltage of a MOSFET is not constant — it depends on the **body bias** or **source-to-body voltage (Vsb)**.  
When a positive substrate potential is applied (for NMOS), it increases the depletion region under the gate, requiring a higher gate voltage to invert the channel.

This is called the **body effect**. A higher body bias makes the transistor harder to turn ON since more gate voltage is required to form the conducting channel.


---

## 2. NMOS Resistive Region and Saturation Region of Operation

### 2.1 Resistive Region (Linear Region)

In the resistive or linear region, the MOSFET behaves like a **voltage-controlled resistor**.  
This occurs when the drain-to-source voltage (Vds) is smaller than the overdrive voltage (Vgs - Vth).  

In this region, the current through the device increases approximately linearly with Vds.  
The conductivity of the channel depends on how strongly the gate voltage enhances the channel — a higher Vgs results in a lower channel resistance and higher drain current.

---

### 2.2 Drift Current Theory

The drain current in a MOSFET mainly results from the **drift of electrons** under the influence of the electric field between drain and source.  
The movement of these electrons due to the applied Vds forms the drift current.

---

### 2.3 Drain Current Model for Linear Region

In the linear region:

- When Vds is small, channel is uniformly inverted
- Id increases linearly with Vds
- As Vds increases, inversion charge near drain reduces, making curve slightly nonlinear

---

![nmos_analysis](https://github.com/user-attachments/assets/58fdcec6-5017-45b7-ab20-40bfdfad64b8)


### 2.4 SPICE Conclusion to Resistive Operation

SPICE simulations show that:

- Id increases linearly with small Vds
- Higher Vgs results in higher Id and lower channel resistance
- Confirms Vgs controls conductivity

---

### 2.5 Pinch-Off Region Condition

As Vds increases and reaches (Vgs - Vth), the channel near the drain pinches off.  
This is the start of **saturation region**, where increasing Vds further does not increase Id significantly.

---

<img width="1280" height="800" alt="image" src="https://github.com/user-attachments/assets/afff5dd4-fb89-4820-9a67-d0a31e164191" />
<img width="1280" height="800" alt="image" src="https://github.com/user-attachments/assets/b7a9795c-26fe-4d6d-82a5-be2567a54993" />


### 2.6 Drain Current Model for Saturation Region

In the saturation region:

- Channel is pinched off near the drain
- Id becomes nearly constant (controlled by Vgs)
- Slight increase in Id due to **channel length modulation** (λ)

---

## 3. Introduction to SPICE

### 3.1 Basic SPICE Setup

SPICE uses **netlists** to define circuits and perform simulations like:

- DC Sweep
- AC Analysis
- Transient Analysis

---

### 3.2 Circuit Description in SPICE Syntax

SPICE syntax includes:
- Devices: transistors, resistors, capacitors
- Voltage sources
- `.dc`, `.model`, `.include`, etc.

Sky130 PDK models are used for realistic simulation.

---

### 3.3 Technology Parameters (Sky130)

Includes:
- Electron mobility
- Oxide capacitance
- Width (W) and Length (L)
- Threshold voltage (Vth)

All values are taken from Sky130 Process Design Kit.

---

## 4. SPICE Simulation

<img width="1280" height="590" alt="image" src="https://github.com/user-attachments/assets/340af331-3dcf-461c-b767-3fccbce3233a" />

<img width="1280" height="590" alt="image" src="https://github.com/user-attachments/assets/e1a3ea1f-cff8-4912-bd3f-0be0f745fabe" />

<img width="1280" height="590" alt="image" src="https://github.com/user-attachments/assets/76b88557-aaa3-4ee1-82e9-b03f57c6ab10" />

### 📁 Clone GitHub Repository

```bash
git clone https://github.com/kunalg123/sky130CircuitDesignWorkshop.git
cd sky130CircuitDesignWorkshop/design
```

 ▶️ Run Simulation : 
```bash
ngspice day1_nfet_idvds_L2_W5.spice
plot -vdd#branch
```
🖼️ Output :

<img width="1176" height="701" alt="spice_netlist_day1" src="https://github.com/user-attachments/assets/b50ab95e-c6d8-44f9-b221-dc971572d05b" />

<img width="1176" height="1003" alt="output_week4:ngspice_w5_l2" src="https://github.com/user-attachments/assets/01285419-7140-46c1-8e38-bc26063958cf" />

<img width="2138" height="1378" alt="image" src="https://github.com/user-attachments/assets/bfa94aaf-fe5b-49da-a8dc-ff9d1484f42f" />


## Summary Table :

| Region                 | Condition         | Behavior                                |
| ---------------------- | ----------------- | --------------------------------------- |
| **Cutoff**             | Vgs < Vth         | Transistor OFF, no current flows        |
| **Linear (Resistive)** | Vds < (Vgs - Vth) | Acts like a voltage-controlled resistor |
| **Saturation**         | Vds ≥ (Vgs - Vth) | Id becomes nearly constant              |
