# 🧩 **Day 3 — CMOS Switching Threshold & Dynamic Simulations**

## 🎯 **Objective — Understanding CMOS Inverter Behavior through Static & Dynamic SPICE Simulations**

The goal of **Day 3** is to perform an in-depth exploration of the **CMOS inverter**, the fundamental building block of digital integrated circuits.
Using **Sky130 SPICE simulations**, we analyze both **static** and **dynamic** characteristics to understand how the inverter behaves under different operating conditions.

This day focuses on two complementary aspects:

1. 🧩 **Static Behavior (Voltage Transfer Characteristics – VTC):**
   Study how the **output voltage (Vout)** varies with **input voltage (Vin)** and determine the **switching threshold (Vm)** — the point where both NMOS and PMOS transistors conduct equally.
   This helps us visualize the inverter’s **logic-level boundaries**, **gain region**, and **noise margins**.

2. ⚡ **Dynamic Behavior (Transient Analysis):**
   Examine how the inverter responds to **time-varying inputs** using transient simulations.
   Measure **propagation delays**, **rise/fall times**, and the effects of **load capacitance (C<sub>L</sub>)** on switching speed and power performance.

By the end of this session, you will:

* 🧠 Grasp how **transistor sizing (W/L ratios)** directly impacts switching thresholds and signal symmetry.
* 📈 Learn how **SPICE simulations** translate theoretical equations into practical voltage and timing plots.
* ⚙️ Build a foundation for **Static Timing Analysis (STA)** and **clock network design**, both of which rely heavily on CMOS inverter characteristics.
* 💡 Appreciate how **device physics**, **sizing trade-offs**, and **load conditions** collectively define real-world digital circuit performance.

---

## ⚙️ **SPICE Deck 1 — CMOS Inverter VTC Simulation**

```spice
*Model Description
.param temp=27

*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

*Netlist Description
XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=0.84 l=0.15
XM2 out in 0   0   sky130_fd_pr__nfet_01v8 w=0.36 l=0.15

Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in  0 1.8V

*Simulation commands
.op
.dc Vin 0 1.8 0.01

.control
run
setplot dc1
display
.endc

.end
```

---

## 📉 **Voltage Transfer Characteristics (VTC)**

The **VTC curve** shows how the output voltage (**V<sub>out</sub>**) changes with input voltage (**V<sub>in</sub>**).
It highlights the inverter’s **switching point** and **noise margins**.

### 🧮 **Key Regions**

| Input Region          | Transistor States | Output Voltage | Description         |            |     |
| --------------------- | ----------------- | -------------- | ------------------- | ---------- | --- |
| **Low Vin** (< Vth)   | NMOS OFF, PMOS ON | ≈ VDD          | Logic ‘1’ output    |            |     |
| **Transition** (≈ Vm) | Both partially ON | Drops rapidly  | Region of high gain | dVout/dVin | > 1 |
| **High Vin** (> Vth)  | NMOS ON, PMOS OFF | ≈ 0 V          | Logic ‘0’ output    |            |     |

![1](./images/01.png)

---

## ⚖️ **Switching Threshold (Vm)**

The **switching threshold** is defined where:
$[
V_{in} = V_{out}
]$

At this point, both transistors conduct equally, and the inverter is most sensitive to input changes.

| Parameter           | Symbol         | Typical Value @ 1.8 V VDD |
| ------------------- | -------------- | ------------------------- |
| Switching Threshold | V<sub>m</sub>  | ≈ 0.88 V                  |
| Supply Voltage      | V<sub>DD</sub> | 1.8 V                     |

---

## 🧠 **Analytical Relations**

The switching threshold depends on the transistor width ratio ((W/L)_p / (W/L)_n).

| Case        | ((W/L)_p / (W/L)_n) | Effect on Vm   | VTC Behavior         |
| ----------- | ------------------- | -------------- | -------------------- |
| Balanced    | ≈ 2                 | Vm ≈ VDD/2     | Symmetrical VTC      |
| PMOS narrow | < 2                 | Vm shifts up   | Strong ‘0’, weak ‘1’ |
| PMOS wide   | > 2                 | Vm shifts down | Strong ‘1’, weak ‘0’ |


🧮 Expression of Vm in terms of transistor sizes:
$[
V_m \approx \frac{V_{DD} + |V_{tp}| - \sqrt{\frac{\beta_n}{\beta_p}}(V_{DD} - V_{tn})}{1 + \sqrt{\frac{\beta_n}{\beta_p}}}
]$

where $(\beta = \mu C_{ox}(W/L))$

---

## ⚙️ **SPICE Deck 2 — Transient Response Simulation**

```spice
*Model Description
.param temp=27

*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

*Netlist Description
XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=0.84 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.36 l=0.15

Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in 0 PULSE(0V 1.8V 0 0.1ns 0.1ns 2ns 4ns)

*Simulation commands
.tran 1n 10n

.control
run
.endc

.end
```

---

## ⚡ **Transient Analysis**

This simulation examines the **dynamic response** of the inverter to a square-wave input.

### 📊 **Waveform Description**

* **Input (Vin):** PULSE from 0 V to 1.8 V with 2 ns high time and 4 ns period.
* **Output (Vout):** Delayed inversion of Vin, with finite rise and fall times.

![2](./images/02.png)

---

## 📈 **Key Dynamic Parameters**

| Parameter             | Symbol                | Description                                   |
| --------------------- | --------------------- | --------------------------------------------- |
| **Propagation Delay** | t<sub>pd</sub>        | Time between 50% of Vin and Vout transition   |
| **Rise Time**         | t<sub>r</sub>         | Time Vout rises from 10% to 90% of VDD        |
| **Fall Time**         | t<sub>f</sub>         | Time Vout falls from 90% to 10% of VDD        |
| **Load Capacitance**  | C<sub>L</sub> = 50 fF | Affects switching speed and power dissipation |

---

## 🧩 **Practical Insights**

| Observation                            | Inference                                    |
| -------------------------------------- | -------------------------------------------- |
| Increasing PMOS width                  | Improves rise time but increases capacitance |
| Increasing NMOS width                  | Speeds fall time but affects balance         |
| Balanced β<sub>p</sub> ≈ β<sub>n</sub> | Ensures symmetrical switching behavior       |
| Dynamic simulation                     | Reveals realistic delays not seen in VTC     |

---

## 🧠 **Summary Table — Day 3 Key Concepts**

| **Concept**                      | **Key Takeaway**                                                           |
| -------------------------------- | -------------------------------------------------------------------------- |
| **CMOS Inverter Switching**      | Transition between logic states occurs near the switching threshold (Vm).  |
| **Static Characteristics (VTC)** | Defines logic behavior, switching region, and voltage gain.                |
| **Dynamic Response**             | Captures rise/fall times, propagation delay, and waveform transitions.     |
| **Transistor Sizing Impact**     | Larger W/L improves drive strength but affects power and delay trade-offs. |
| **SPICE Transient Simulation**   | Enables visualization of real-time inverter operation and delay metrics.   |

---

### 🔗 **Next Step**

➡️ Proceed to **[Day 4 — CMOS Noise Margin & Robustness](../Day4_Noise_Margin_Robustness/readme.md)**

Here, you’ll **quantify the inverter’s reliability** by calculating **Noise Margins (NMH & NML)** from the VTC curve, analyze **logic level stability**, and explore how **device sizing** and **supply variations** impact **noise immunity** and **circuit robustness**.

---
