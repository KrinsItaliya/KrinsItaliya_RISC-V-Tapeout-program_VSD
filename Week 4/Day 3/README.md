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
![netlist_spice_inv_trns](output_week4/netlist_spice_inv_trns.png)
![ngspice_inv_trns](output_week4/ngspice_inv_trns.png)

### simulation for inverter vtc chateristices
### step-1:
```bash
ngspice day3_inv_tran_Wp084_Wn036.spice
plot out vs in
```
![netlist_spice_inv_vtc](output_week4/netlist_spice_inv_vtc.png)
![ngspice_inv_vtc](output_week4/ngspice_inv_vtc.png)

## 2. Static Behavior Evaluation – CMOS Inverter Robustness and Switching Threshold

### 2.1 Switching Threshold, Vm

The switching threshold is the input voltage at which the inverter output equals the input voltage.  
At this point, both the NMOS and PMOS transistors conduct the same amount of current, balancing the circuit.  
It determines how the inverter switches between logic states and plays a major role in defining noise immunity and stability.  

A well-designed inverter has its switching threshold close to the mid-point of the supply voltage, ensuring symmetrical switching characteristics.

---
![switch_threshold](output_week4/switch_threshold.jpeg)

### 2.2 Effect of Device Sizing on Switching Threshold

The switching threshold depends on the relative strengths of the NMOS and PMOS transistors.  
If both transistors are of equal size, the threshold might shift away from the center due to mobility differences.  
Since electrons move faster than holes, the NMOS tends to be stronger than the PMOS.  
To balance this, the PMOS transistor is usually made wider to equalize the drive current.  

Proper sizing results in a symmetrical transfer curve and equal noise margins for both logic levels.

---
![switch_threshold_comparision](output_week4/switch_threshold_comparision.jpeg)

### 2.3 Determining Transistor Sizing for Desired Threshold

By adjusting the width and length of the PMOS and NMOS devices, the inverter’s switching threshold can be positioned as desired.  
If the PMOS is made stronger, the threshold voltage shifts upward, and if the NMOS is stronger, the threshold shifts downward.  
This control helps in optimizing circuit behavior for speed, power, or robustness depending on the design target.  

This aspect is very important for digital design where uniform switching points ensure predictable logic operation across multiple gates.

---
![switch_threshold_equations](output_week4/switch_threshold_equations_1.jpeg)
![switch_threshold_equations_2](output_week4/switch_threshold_equations_2.jpeg)

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
