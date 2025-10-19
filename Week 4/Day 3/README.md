# ⚡[Day-3] CMOS Switching Threshold and Dynamic Simulations

---

## 1. Voltage Transfer Characteristics – SPICE Simulations

### 1.1 SPICE Deck Creation for CMOS Inverter

A SPICE deck is a text-based file that describes the CMOS inverter circuit. It defines the connections, components, and simulation commands required for running Ngspice.  
The CMOS inverter includes one NMOS and one PMOS transistor connected in a complementary configuration.  
The input voltage is swept from 0 to the supply voltage to obtain the DC transfer characteristic curve.  
This deck also specifies the power supply, transistor dimensions, and the model library from the Sky130 PDK.

The purpose of creating this deck is to simulate the voltage transfer characteristic (VTC) and study the static behavior of the inverter.

---

### 1.2 SPICE Simulation for CMOS Inverter

When the SPICE simulation is executed, it generates a plot of output voltage versus input voltage, known as the Voltage Transfer Characteristic.  
This curve shows how the inverter transitions from logic high to logic low as the input changes.  

At low input voltage, the NMOS is off and PMOS is on, so the output is high.  
At high input voltage, the NMOS turns on and PMOS turns off, pulling the output low.  
The middle region is the transition region, where both transistors conduct simultaneously.  

From this simulation, the switching threshold, gain, and noise margins can be observed and analyzed.

---

### 1.3 Labs – Sky130 SPICE Simulation for CMOS Inverter

### simulation for inverter transistor chateristices
### step-1:
```bash
ngspice day3_inv_tran_Wp084_Wn036.spice
plot out vs time in
```
<img width="1606" height="1270" alt="image" src="https://github.com/user-attachments/assets/37ae84ed-2816-4101-9927-40e6b1f034be" />
<img width="1624" height="1028" alt="image" src="https://github.com/user-attachments/assets/cf9ef67a-5d28-42d2-bb58-bfabf62b7c05" />

### simulation for inverter vtc chateristices
### step-1:
```bash
ngspice day3_inv_tran_Wp084_Wn036.spice
plot out vs in
```
<img width="1624" height="1254" alt="image" src="https://github.com/user-attachments/assets/2523ea60-f08b-4689-b193-205e8d40de43" />
<img width="1718" height="962" alt="image" src="https://github.com/user-attachments/assets/33bef033-541f-4cc6-80a4-0c63a63df86e" />

## 2. Static Behavior Evaluation – CMOS Inverter Robustness and Switching Threshold

### 2.1 Switching Threshold, Vm

The switching threshold is the input voltage at which the inverter output equals the input voltage.  
At this point, both the NMOS and PMOS transistors conduct the same amount of current, balancing the circuit.  
It determines how the inverter switches between logic states and plays a major role in defining noise immunity and stability.  

A well-designed inverter has its switching threshold close to the mid-point of the supply voltage, ensuring symmetrical switching characteristics.

---
<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/e7796a24-dde8-47ac-a880-bd1c942daa1d" />

### 2.2 Effect of Device Sizing on Switching Threshold

The switching threshold depends on the relative strengths of the NMOS and PMOS transistors.  
If both transistors are of equal size, the threshold might shift away from the center due to mobility differences.  
Since electrons move faster than holes, the NMOS tends to be stronger than the PMOS.  
To balance this, the PMOS transistor is usually made wider to equalize the drive current.  

Proper sizing results in a symmetrical transfer curve and equal noise margins for both logic levels.

---
<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/d5eafe34-e4e7-41e7-aa2d-6b8ce3e72f95" />


### 2.3 Determining Transistor Sizing for Desired Threshold

By adjusting the width and length of the PMOS and NMOS devices, the inverter’s switching threshold can be positioned as desired.  
If the PMOS is made stronger, the threshold voltage shifts upward, and if the NMOS is stronger, the threshold shifts downward.  
This control helps in optimizing circuit behavior for speed, power, or robustness depending on the design target.  

This aspect is very important for digital design where uniform switching points ensure predictable logic operation across multiple gates.

---
<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/9a0c6006-61bc-4e4b-bbde-3e7f40905759" />
<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/30d159f5-f57e-4391-8e02-9f3322f19bb5" />

### 2.4 Static and Dynamic Simulation of CMOS Inverter

The static simulation involves sweeping the input voltage slowly to observe the steady-state response, producing the VTC.  
It helps analyze static parameters such as noise margins, gain, and switching voltage.  

In contrast, the dynamic or transient simulation applies a time-varying input signal, such as a pulse, to observe how fast the output responds.  
From this, important timing parameters like rise time, fall time, and propagation delay are extracted.  
These values show how quickly the inverter can switch states, which directly affects the overall speed of digital circuits.

---

### 2.5 Simulation with Increased PMOS Width

Increasing the PMOS width improves the pull-up strength of the inverter, helping the output reach the high voltage faster.  
This modification results in a more balanced and symmetric transfer curve and reduces the rise time during dynamic operation.  
However, it also slightly increases the load capacitance, leading to a small increase in power consumption.  

In practice, designers tune the PMOS width carefully to achieve the desired trade-off between speed, symmetry, and power efficiency.

---

### 2.6 Applications of CMOS Inverter in Clock Network and STA

CMOS inverters are the most fundamental elements in digital design.  
They are used to build logic gates, buffers, and complex circuits like flip-flops and memory cells.  
In clock networks, inverters are employed to shape, buffer, and distribute the clock signal efficiently while maintaining proper timing.  

During static timing analysis (STA), inverter characteristics such as delay, slew rate, and transition time are used to calculate timing paths across the circuit.  
Hence, understanding the inverter’s static and dynamic behavior is essential for reliable digital system design.

---

## Summary

| Concept | Description |
|----------|-------------|
| SPICE Deck | Defines inverter circuit and simulation setup |
| VTC | Output vs Input curve showing switching behavior |
| Switching Threshold | Point where inverter output equals input |
| Device Sizing | Adjusts threshold and symmetry of VTC |
| Static Simulation | Finds steady-state characteristics |
| Dynamic Simulation | Measures delay, rise, and fall times |
| PMOS Width Increase | Improves pull-up strength and symmetry |
| Applications | Used in clock buffers, logic gates, and timing paths |

---
