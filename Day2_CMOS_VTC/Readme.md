# ⚙️ **Day 2 — Velocity Saturation and Basics of CMOS Inverter VTC**

### *NgspiceSky130 — CMOS Circuit Design and SPICE Simulation Journey*

---

## 📘 **Introduction to Velocity Saturation and CMOS Inverter Behavior**

In **Day 2**, we dive deeper into transistor-level behavior and explore **velocity saturation** effects in **short-channel NMOS devices** using the **Sky130 PDK**.
We also design and simulate the **CMOS inverter**, the most fundamental logic gate in digital ICs, and plot its **Voltage Transfer Characteristic (VTC)**.

This marks our first step into **digital circuit-level design** — linking **device physics** to **logic switching performance**.

---

## ⚡ **Understanding Velocity Saturation in MOSFETs**

In short-channel technologies (like **130 nm**), the carrier velocity no longer increases linearly with the electric field due to **scattering effects**.
Instead, it **saturates** at a maximum value — this is known as **velocity saturation**.

### ✨ **Key Insights**

* At **low fields**, drift velocity increases linearly with the electric field.
* At **high fields**, it approaches a **constant saturation velocity (v<sub>sat</sub>)**.
* As a result, **drain current (I<sub>D</sub>)** increases more slowly in saturation.
* This affects transistor **speed**, **delay**, and **output drive strength**.

---

## 🧮 **Drain Current with Velocity Saturation**

Modified drain current equation in the **saturation region**:

$[
I_D = \frac{W}{L} \mu_n C_{ox} (V_{GS} - V_T) V_{DSsat}
]$

where


$[
V_{DSsat} = \frac{E_{sat} L}{1 + \frac{E_{sat} L}{V_{GS} - V_T}}
]$


and $E_{sat}$ is the critical electric field at which velocity saturation begins.

📘 *Observation:* As channel length **L** decreases, **velocity saturation dominates**, leading to **reduced current gain** and **lower transconductance (g<sub>m</sub>)**.

---

## 🔬 **SPICE Simulation — NMOS Id vs Vgs**

We perform a **DC sweep simulation** on an NMOS transistor, varying **Vgs** and measuring **Id** to extract the **Threshold Voltage (V<sub>th</sub>)** and observe **velocity saturation**.

### 🧾 **Sample SPICE Netlist (Id–Vgs Sweep)**

```spice
*Model Description
.param temp=27


*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt


*Netlist Description

XM1 Vdd n1 0 0 sky130_fd_pr__nfet_01v8 w=0.39 l=0.15

R1 n1 in 55

Vdd vdd 0 1.8V
Vin in 0 1.8V

*simulation commands

.op
.dc Vin 0 1.8 0.1 

.control

run
display
setplot dc1
.endc

.end

```

### 💻 **Run Simulation**

```bash
ngspice day2_nfet_idvgs_L015_W039.spice
```

Plot **I<sub>D</sub> vs V<sub>GS</sub>** and note the **threshold voltage (V<sub>th</sub>)** at the onset of conduction.

![01](./images/01.png)

---

## 🧠 **Threshold Voltage Extraction**

You can extract **V<sub>th</sub>** using the **linear extrapolation method**:

1. Plot **√I<sub>D</sub>** vs **V<sub>GS</sub>**.
2. Extrapolate the straight-line portion of the curve to intersect the V<sub>GS</sub> axis.
3. The intercept point gives **V<sub>th</sub>**.

🧾 Example:

| Parameter                | Value    |
| ------------------------ | -------- |
| Extracted V<sub>th</sub> | ≈ 0.45 V |

---

## ⚙️ **SPICE Simulation — NMOS Id vs Vds**

To understand the **output characteristics** of the NMOS transistor, we perform a **DC sweep** varying **V<sub>DS</sub>** from 0 V to 1.8 V at multiple **V<sub>GS</sub>** bias levels.
This helps visualize the **linear region**, **saturation region**, and the impact of **velocity saturation**.

### 🧾 **Sample SPICE Netlist (Id–Vds Sweep)**

```spice
*Model Description
.param temp=27

*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

*Netlist Description
XM1 Vdd n1 0 0 sky130_fd_pr__nfet_01v8 w=0.39 l=0.15
R1 n1 in 55

Vdd vdd 0 1.8V
Vin in 0 1.8V

*Simulation Commands
.op
.dc Vdd 0 1.8 0.1 Vin 0 1.8 0.2

.control
run
display
setplot dc1
.endc

.end
```

### 💻 **Run Simulation**

```bash
ngspice day2_nfet_idvds_L015_W039.spice
```

This simulation performs a **nested DC sweep**:

* The **outer sweep** varies **V<sub>GS</sub>** (Vin) from 0 V to 1.8 V in 0.2 V steps.
* The **inner sweep** varies **V<sub>DS</sub>** (Vdd) from 0 V to 1.8 V in 0.1 V steps.

You can plot **I<sub>D</sub> vs V<sub>DS</sub>** for each **V<sub>GS</sub>** to observe how the transistor transitions from **ohmic** to **saturation** regions.

### 📈 **Expected Observation**

* For **small V<sub>DS</sub>**, the device operates in the **linear region** (Id increases linearly).
* As **V<sub>DS</sub>** increases, **Id** saturates — marking the **saturation region**.
* The **higher the V<sub>GS</sub>**, the larger the **drain current** and the earlier the device enters saturation.

![02](./images/02.png)

---

### 🧩 **Concept Insight — Velocity Saturation**

At **deep submicron lengths (L = 0.15 µm)**, the drain current deviates from ideal quadratic behavior due to **carrier velocity saturation**.
This physical effect limits **current drive** and influences **switching speed** — a key concept for **modern CMOS scaling**.

---

## ⚙️ **Building a CMOS Inverter**

A **CMOS Inverter** combines **PMOS** and **NMOS** transistors to form a logic NOT gate.
It outputs the **complement** of the input and serves as the basis for all digital logic families.

### 📘 **CMOS Inverter Circuit**

* **PMOS:** Source at VDD, drain connected to NMOS drain.
* **NMOS:** Source at GND.
* **Output:** Taken at the common drain node.
* **Input:** Shared gate terminal.

![03](./images/03.png)

---

Perfect! We can **replace the existing SPICE netlist section with a richer theory-backed explanation**, keeping it aligned with **Day 2 focus**: velocity saturation, short-channel effects, and basic CMOS VTC understanding. Here’s a **reworked section** you can use:

---

## 🧠 **CMOS Inverter — Voltage Transfer Characteristics (VTC) and Theory**

In Day 2, we focus on the **basic CMOS inverter behavior** and the effects of **velocity saturation** in short-channel devices using Sky130 SPICE simulations.

---

### ⚙️ **Key Operating Principles**

1. **MOSFET as a Switch**

   * NMOS: Conducts when ( V_{GS} > V_{th} )
   * PMOS: Conducts when ( V_{SG} > |V_{th}| )

2. **Drain Current Dependence**

   * **Long-channel devices:** Drain current ( I_D ) depends quadratically on ( V_{GS} - V_{th} )
   * **Short-channel devices:** High electric fields cause **velocity saturation**, limiting ( I_D )

3. **CMOS Inverter Action**

   * **Low Input ((V_{in} < V_{th}))**: NMOS OFF, PMOS ON → ( V_{out} \approx V_{DD} )
   * **Transition Region**: Both transistors partially ON → ( V_{out} ) falls rapidly
   * **High Input ((V_{in} > V_{th}))**: NMOS ON, PMOS OFF → ( V_{out} \approx 0 )

---

### 📊 **Switching Threshold (Vm)**

The **switching threshold voltage (V_m)** is the input voltage at which:

[
V_{in} = V_{out}
]

* At ( V_m ), **NMOS and PMOS currents are equal** in magnitude.
* Determines **logic transition point** and **noise margins**.
* For **balanced rise/fall times**, PMOS is typically sized **≈ 2× NMOS (W/L)**.

| Parameter           | Symbol         | Typical Value |
| ------------------- | -------------- | ------------- |
| Switching Threshold | V<sub>m</sub>  | ≈ 0.88 V      |
| Supply Voltage      | V<sub>DD</sub> | 1.8 V         |
| PMOS/NMOS ratio     | Wp/Wn          | ≈ 2           |

---

### 🧪 **Theory — Velocity Saturation**

* In **short-channel devices**, electrons cannot accelerate indefinitely.
* **Drain current saturates** at high ( V_{DS} ), limiting switching speed.
* Important for **low-power, high-speed CMOS design**.

| Effect                   | Impact on Inverter                              |
| ------------------------ | ----------------------------------------------- |
| Velocity Saturation      | Limits drain current, reduces gain              |
| Short Channel Effect     | Threshold voltage shift, increased leakage      |
| VTC Transition Sharpness | Affected by (W/L) ratio and mobility saturation |
| Switching Threshold Vm   | Determines inverter’s balanced operating point  |

---

### 🧩 **CMOS Inverter VTC Visualization**

* The **VTC curve** represents ( V_{out} ) vs ( V_{in} )
* Shows **transition region**, **logic high/low levels**, and **switching threshold**
* **Sharpness** of the transition indicates the inverter’s **gain and speed**

> 💡 Observations:
>
> * Symmetric switching occurs with ( (W/L)*{PMOS} \approx 2 \times (W/L)*{NMOS} )
> * Propagation delay and noise margin are **directly influenced** by device sizing and velocity saturation.

![04](./images/04.png)

---

### 🧠 **Summary Table — Day 2 Key Concepts**

| Concept                      | Key Takeaway                                                         |
| ---------------------------- | -------------------------------------------------------------------- |
| **Velocity Saturation**      | Limits electron velocity at high electric fields                     |
| **Short Channel Effect**     | Reduces drain current and transconductance                           |
| **CMOS Inverter VTC**        | Shows input-output behavior, transition region, and Vm               |
| **Switching Threshold (Vm)** | Determines logic transition point, affects noise margin              |
| **SPICE Simulation**         | Enables observation of short-channel and velocity saturation effects |

---
## 🔗 **Next Step**

➡️ Proceed to **[Day 3 — CMOS Switching Threshold and Dynamic Simulations](../Day3_CMOS_Switching_Dynamics/readme.md)**
Here, you’ll simulate **transient responses** of the inverter under **pulse inputs**, measure **rise/fall delays**, and relate them to **timing performance** in STA.

---
