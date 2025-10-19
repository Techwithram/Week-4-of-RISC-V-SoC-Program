# 🧩 **Day 4 — CMOS Noise Margin & Robustness Evaluation**

## 🎯 **Objective — Evaluating Static Behavior and Noise Immunity of CMOS Inverters**

The goal of **Day 4** is to evaluate how robust a **CMOS inverter** is when subjected to noise and input disturbances.
Using **Sky130 SPICE simulations**, we’ll analyze the **Voltage Transfer Characteristic (VTC)** to extract **Noise Margins (NML & NMH)** and understand how transistor sizing affects circuit reliability.

This session focuses on:

1. 🧩 **Static Robustness (Noise Margin Analysis):**
   Study how **output voltage stability** varies with noisy or fluctuating input signals using the inverter’s **VTC curve**.

2. ⚖️ **Width Variation & Robustness:**
   Investigate how **PMOS/NMOS sizing ratio (W/L)** changes influence the inverter’s ability to reject input noise and maintain logic integrity.

By the end of this session, you will:

* 🧠 Understand how **noise tolerance** ensures digital logic reliability in CMOS circuits.
* 📉 Learn to extract **VOH**, **VOL**, **VIL**, and **VIH** from SPICE-generated VTC plots.
* 🔬 Quantify **NML** and **NMH** using practical SPICE methods.
* ⚙️ Explore how **PMOS width variation** affects robustness and stability.
* 💡 Build the foundation for **Noise-Aware STA (Static Timing Analysis)** and **robust digital design**.

---

## ⚙️ **SPICE Deck — CMOS Inverter Noise Margin Simulation**

```spice
*Model Description
.param temp=27

*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

*Netlist Description
XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=1 l=0.15
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

## 📉 **Voltage Transfer Characteristic (VTC) and Noise Margins**

The **Voltage Transfer Characteristic (VTC)** shows the relationship between input voltage (**V<sub>in</sub>**) and output voltage (**V<sub>out</sub>**).
Noise margins are derived from this curve to quantify **how much unwanted voltage fluctuation** a logic signal can tolerate before malfunctioning.

![1](./images/01.png)

---

### 🧮 **Defining Noise Margin Parameters**

| Parameter | Symbol               | Description                             | Typical Value |
| --------- | -------------------- | --------------------------------------- | ------------- |
| **VOH**   | Output High Voltage  | Maximum output logic ‘1’                | ≈ VDD (1.8 V) |
| **VOL**   | Output Low Voltage   | Minimum output logic ‘0’                | ≈ 0 V         |
| **VIL**   | Input Low Threshold  | Vin where slope = −1 (left)             | ~0.4 V        |
| **VIH**   | Input High Threshold | Vin where slope = −1 (right)            | ~1.2 V        |
| **NMH**   | VOH − VIH            | Noise Margin High — logic ‘1’ tolerance | ≈ 0.6 V       |
| **NML**   | VIL − VOL            | Noise Margin Low — logic ‘0’ tolerance  | ≈ 0.4 V       |

At these critical points, the slope of the VTC curve equals **−1**, marking the transition boundaries between stable logic states and the undefined region.

---

### ⚖️ **Noise Margin Equations**

Noise margins define the safe voltage intervals that prevent logic error propagation:

$[
NM_H = VO_H - VI_H
]$

$[
NM_L = VI_L - VO_L
]$

✅ **Interpretation:**

* **High Noise Margin (NMH):**
  Tells how much noise a **logic ‘1’** signal can tolerate before being misread as a ‘0’.

* **Low Noise Margin (NML):**
  Tells how much noise a **logic ‘0’** signal can tolerate before being misread as a ‘1’.

Designers always aim for **large NMH/NML values** — improving circuit robustness.

---

## ⚙️ **Running the Simulation**

To generate the noise margin plot:

```bash
ngspice day4_inv_noisemargin_wp1_wn036.spice
plot out vs in
```

The output VTC curve appears like this:

![2](./images/02.png)

---

### 🔍 **Extracting Noise Margins from SPICE Plot**

In **ngspice interactive mode**:

1️⃣ Click on the left slope of VTC — terminal shows:

```
x0 = VIL, y0 = VOH
```

2️⃣ Click on the right slope — terminal shows:

```
x1 = VIH, y1 = VOL
```

Then manually compute:

```
NMH = VOH − VIH = y0 − x1
NML = VIL − VOL = x0 − y1
```

✅ These values quantify the **static noise tolerance** of your CMOS inverter.

---

## ⚙️ **Width Variation and Robustness Study**

Now, modify the **PMOS width (Wp)** while keeping **Wn = 0.36 µm** constant.

| Case  | PMOS Width (µm) | (Wp/Wn) Ratio | Observation                              |
| ----- | --------------- | ------------- | ---------------------------------------- |
| **1** | 0.84            | 2.33          | Balanced — symmetrical VTC               |
| **2** | 1.00            | 2.77          | Improved NMH, slightly reduced NML       |
| **3** | 1.20            | 3.33          | Stronger pull-up, VOH saturation earlier |
| **4** | 1.50            | 4.16          | Wider transition, degraded gain region   |

As Wp increases, **VTC shifts left**, improving **logic ‘1’ stability** but slightly weakening **logic ‘0’ immunity**.
Designers tune this ratio for **balanced noise margins** and **stable switching points**.

![3](./images/03.png)

---

## 🧩 **Practical Insights**

| Observation                        | Inference                                   |
| ---------------------------------- | ------------------------------------------- |
| Increasing PMOS width              | Enhances high-level noise tolerance (NMH ↑) |
| Increasing NMOS width              | Enhances low-level noise tolerance (NML ↑)  |
| Balanced sizing                    | Maximizes total noise margin                |
| Narrow transition slope            | Improves noise immunity                     |
| Sharp transition (−dVout/dVin ≫ 1) | Ensures strong logic separation             |

Noise margin optimization is critical in **low-power, high-speed designs**, where supply noise and coupling can distort signals.

---

## 🧠 **Summary Table — Day 4 Key Concepts**

| **Concept**                 | **Key Takeaway**                                                                |
| --------------------------- | ------------------------------------------------------------------------------- |
| **Noise Margin Definition** | Quantifies how much noise a CMOS circuit can tolerate without logic failure.    |
| **VOH, VOL, VIL, VIH**      | Extracted from VTC where slope = −1 — determine noise margin boundaries.        |
| **NMH & NML**               | Represent upper and lower noise tolerances for logic ‘1’ and ‘0’.               |
| **VTC-Based Evaluation**    | Allows visual understanding of robustness through input-output characteristics. |
| **Width Variation Effect**  | PMOS/NMOS sizing impacts margin symmetry and circuit reliability.               |
| **SPICE Simulation**        | Enables direct measurement of realistic noise immunity using Sky130 models.     |

---

### 🔗 **Next Step**

➡️ Proceed to **[Day 5 — CMOS Power, Delay & Performance Analysis](../Day5_Power_Supply_Variation/readme.md)**

Here, you’ll extend your understanding of inverter dynamics to **energy and timing metrics** — exploring **propagation delay**, **dynamic power**, and **leakage characteristics** under varying load and frequency conditions.

---
