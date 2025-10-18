# 🧠 [Day-2] Velocity Saturation and Basics of CMOS Inverter Voltage Transfer Characteristics (VTC)

---

## 1. SPICE Simulation for Lower Nodes and Velocity Saturation Effect

### 1.1 Introduction

As CMOS technology scales down to smaller feature sizes (sub-micron and nanometer nodes), the behavior of MOSFETs deviates from the long-channel model.  
One of the most important non-ideal effects observed in short-channel devices is **velocity saturation**.  

In long-channel MOSFETs, carrier velocity increases linearly with the electric field.  
However, in short-channel devices, at high electric fields, carrier velocity no longer increases linearly — it **saturates** to a maximum limit known as **saturation velocity (vsat)**.  
This phenomenon significantly impacts current drive and device performance.

---

### 1.2 SPICE Simulation for Lower Nodes

When simulating advanced technology nodes (like 130 nm or smaller) using SPICE, the effects of mobility degradation and velocity saturation must be included.  

The **Sky130 PDK** models already account for these effects through advanced physical parameters in the BSIM (Berkeley Short-channel IGFET Model).  

SPICE simulations for lower nodes reveal that:
- Drain current saturates earlier compared to long-channel devices.
- The transition between linear and saturation regions is sharper.
- The transconductance (gm) decreases due to reduced carrier velocity.

This allows accurate modeling of real transistor behavior under high-field conditions.

---

### 1.3 Drain Current vs Gate Voltage for Long and Short Channel Devices

In long-channel MOSFETs, the drain current in saturation increases quadratically with gate voltage.  
However, in short-channel MOSFETs, due to velocity saturation, the relationship becomes nearly linear for high Vgs.

**Key differences:**
- **Long-channel:** Current ∝ (Vgs - Vth)²  
- **Short-channel:** Current ∝ (Vgs - Vth)  

This means that as device dimensions shrink, the drain current no longer grows as fast with increasing Vgs, resulting in reduced gain and transconductance.

SPICE simulations can plot Id–Vgs curves for both long and short channel devices to visualize this change in slope.

---

![velocity_saturation](output_week4/velocity_saturation_1.jpeg)
![velocity_saturation](output_week4/velocity_saturation_2.jpeg)

---

### 1.4 Velocity Saturation at Lower and Higher Electric Fields

In the **low electric field region**, carrier drift velocity is proportional to the electric field (v = μE).  
At **higher fields**, carriers experience scattering and their velocity approaches a limiting value — the **saturation velocity (vsat)**.

The field at which this transition occurs depends on:
- The semiconductor material (e.g., silicon, GaAs)
- Carrier mobility
- Channel length

For silicon, typical saturation velocity is around 10⁷ cm/s.  
Once the field exceeds the critical value (Ec), velocity saturation dominates and limits the drain current increase.

---

![velocity_saturation](output_week4/velocity_saturation_3.jpeg)

---

### 1.5 Velocity Saturation Drain Current Model

In presence of velocity saturation, the current expression is modified to include the effect of limited carrier velocity.  
Instead of quadratic dependence on gate voltage, the model assumes that velocity reaches vsat beyond a certain field, making Id nearly linear with (Vgs - Vth).

Key points of this model:
- It is more accurate for deep submicron devices.
- Current is limited by the maximum drift velocity.
- Channel-length dependence is reduced.
- Drain current increases less sharply with gate voltage.

SPICE simulations based on BSIM models automatically include this effect.

---

## 2. CMOS Voltage Transfer Characteristics (VTC)

---

### 2.1 Introduction

A **CMOS inverter** is the fundamental building block of all digital logic circuits.  
It consists of a **PMOS** and **NMOS** transistor connected in a complementary fashion.  
Understanding its **Voltage Transfer Characteristic (VTC)** helps analyze logic behavior, switching points, and noise margins.

---

### 2.2 MOSFET as a Switch

In digital circuits, a MOSFET operates mainly as a **voltage-controlled switch**:
- The NMOS conducts when its gate voltage exceeds the threshold voltage (logic 1).
- The PMOS conducts when its gate voltage is below its threshold voltage (logic 0).

Thus, one of the transistors is always ON, while the other is OFF, providing rail-to-rail output levels with low static power dissipation.

---

### 2.3 Standard MOS Voltage Current Parameters

The key parameters defining the inverter behavior include:
- **Vthn, Vthp:** Threshold voltages of NMOS and PMOS
- **Kn, Kp:** Process transconductance parameters (related to W/L ratios and mobility)
- **VDD:** Supply voltage
- **Vin, Vout:** Input and output voltages

These determine the inverter’s switching threshold and the shape of the VTC curve.

---

### 2.4 PMOS/NMOS Drain Current vs Drain Voltage

The PMOS and NMOS devices operate in complementary regions depending on the input voltage.  
For different Vin levels:
- NMOS current increases as Vin rises.
- PMOS current decreases as Vin rises.

At the switching point, both transistors conduct simultaneously — this defines the **transition region** of the inverter.

Plotting Id vs Vd for each device gives insight into how their currents balance to produce the inverter VTC.

---

### 2.5 Step 1 – Convert PMOS Gate-Source Voltage to Source-Voltage Relation

To analyze inverter behavior, it’s convenient to express the PMOS voltage relation in terms of the source voltage (since its source is connected to VDD).  
This helps compare the PMOS and NMOS currents under the same Vout condition.

When the input voltage Vin changes, the gate-source voltages of both devices change accordingly, affecting their conduction levels.

---

### 2.6 Step 2 & Step 3 – Convert PMOS and NMOS Drain-Source Voltage to Output Voltage

In the inverter configuration:
- The **output voltage (Vout)** is the drain voltage of both transistors.
- The NMOS source is connected to ground, and the PMOS source is connected to VDD.

By relating the drain current equations to Vout, we can analyze how current flows during switching.  
At equilibrium (static operation), the NMOS and PMOS currents are equal in magnitude, determining the output voltage for each input level.

---

### 2.7 Step 4 – Merge PMOS and NMOS Load Curves and Plot VTC

The **Voltage Transfer Characteristic (VTC)** is obtained by plotting Vout against Vin.  
This curve shows three distinct regions:

1. **NMOS Cutoff / PMOS ON:**  
   - Vin is low  
   - NMOS is OFF, PMOS pulls Vout to VDD (logic HIGH)

2. **Transition Region:**  
   - Vin is around the inverter threshold voltage  
   - Both transistors conduct partially  
   - The output voltage changes rapidly from HIGH to LOW

3. **PMOS Cutoff / NMOS ON:**  
   - Vin is high  
   - NMOS conducts fully, PMOS is OFF  
   - Vout is pulled to GND (logic LOW)
