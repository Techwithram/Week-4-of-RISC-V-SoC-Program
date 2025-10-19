# ⚙️ **Day 1 — Basics of NMOS Drain Current (Id) vs Drain-to-Source Voltage (Vds)**

### *NgspiceSky130 — CMOS Circuit Design and SPICE Simulation Journey*

---

## 📘 **Introduction to Circuit Design and SPICE Simulations**

In this session, we begin our **hands-on journey** into **analog device-level circuit design** using **NGSPICE** with the **Sky130 open-source PDK**.
We start by analyzing the **NMOS transistor**, the fundamental building block of digital and analog ICs, and explore how its **drain current (Id)** varies with **drain-to-source voltage (Vds)** for different **gate voltages (Vgs)**.

---

## 🔍 **Why Do We Need SPICE Simulations?**

SPICE (**Simulation Program with Integrated Circuit Emphasis**) is the heart of all modern circuit design and verification tools.
It helps engineers simulate circuit behavior **before fabrication**, saving time, cost, and debugging effort.

### 🎯 **Key Reasons for Using SPICE Simulations**

1. ✅ **Functional Verification** — Validate the circuit operation under different bias and input conditions.
2. ⚡ **Performance Evaluation** — Check timing, gain, and speed before physical implementation.
3. 🔋 **Power Analysis** — Measure static and dynamic power consumption.
4. 🔧 **Design Optimization** — Tune transistor sizes, bias voltages, and other parameters for efficiency.
5. 🧠 **Learning Insight** — Visualize the relationship between electrical quantities like voltage, current, and threshold behavior.

---

## 🧩 **Introduction to the Basic Element — NMOS Transistor**

An **NMOS transistor** is a **voltage-controlled device** where current flows between **Drain (D)** and **Source (S)** terminals, controlled by the **Gate (G)** voltage.

### 📘 **Structure Overview**

* **Gate (G):** Controls the conductivity of the channel.
* **Source (S):** Where carriers (electrons) enter.
* **Drain (D):** Where carriers leave.
* **Body/Substrate (B):** Usually tied to the lowest potential (GND for NMOS).

![01](./images/01.png)

---

## ⚡ **Strong Inversion and Threshold Voltage**

When **V<sub>GS</sub> (Gate-Source Voltage)** exceeds a certain value, called the **Threshold Voltage $(V<sub>th</sub>)$**, the transistor forms a conductive channel between the source and drain.

### ✨ **Key Points**

* Below **V<sub>th</sub>**, transistor is **OFF** (no current flows).
* Above **V<sub>th</sub>**, electrons form a strong inversion layer — the transistor turns **ON**.
* The region where this transition occurs is crucial for switching circuits.

![02](./images/02.png)

---

## 🧲 **Threshold Voltage with Positive Substrate Potential**

When the **substrate (body)** is at a **higher potential than the source**, a **body effect** occurs.
This increases the **threshold voltage**, making it harder to turn ON the NMOS.

### 🧮 **Body Effect Equation**

$[
V_T = V_{T0} + \gamma \left( \sqrt{2\Phi_F + V_{SB}} - \sqrt{2\Phi_F} \right)
]$
Where:

* $(V_{T0})$: Zero-bias threshold voltage
* $(V_{SB})$: Source-to-body voltage
* $(\gamma)$: Body effect coefficient
* $(\Phi_F)$: Fermi potential

![03](./images/03.png)

---

## 🔬 **NMOS Resistive and Saturation Regions**

Now, let’s explore how NMOS behaves in two key operating regions: **Resistive (Linear)** and **Saturation (Active)**.

---

### 🧮 **Resistive Region (Linear Operation)**

Occurs when:
$[
V_{DS} < (V_{GS} - V_T)
]$

Here, the channel is **uniformly formed** along its length, and the transistor acts as a **voltage-controlled resistor**.

**Drain current equation:**
$[
I_D = \mu_n C_{ox} \frac{W}{L} \left[ (V_{GS} - V_T)V_{DS} - \frac{V_{DS}^2}{2} \right]
]$

* Current increases linearly with **V<sub>DS</sub>**.
* Suitable for **analog amplifiers and switches**.

![04](./images/04.png)

---

### 💨 **Drift Current Theory**

In the linear region:

* Current flow is mainly **drift current** caused by the **electric field** across the channel.
* The induced charge density in the channel at a point *x* is:
  $[
  Q_i(x) = -C_{ox}\left[(V_{GS} - V(x)) - V_T\right]
  ]$
* Drain current:
  $[
  I_D = \mu_n C_{ox} \frac{W}{L} (V_{GS} - V_T - \frac{V_{DS}}{2})V_{DS}
  ]$


![05](./images/05.png)

---

### ⚙️ **Pinch-Off Condition and Saturation Region**

When:
$[
V_{DS} \ge (V_{GS} - V_T)
]$

* The channel near the **drain** end depletes — this point is called the **pinch-off point**.
* Beyond this point, **current saturates** and becomes nearly constant.

**Saturation region drain current:**
$[
I_D = \frac{1}{2}\mu_n C_{ox}\frac{W}{L}(V_{GS} - V_T)^2
]$

![06](./images/06.JPG)

---

### 🧾 **SPICE Observation for Resistive & Saturation Operation**

In **Ngspice**, you can simulate the NMOS transistor behavior by sweeping **V<sub>DS</sub>** for different **V<sub>GS</sub>** values and plotting **I<sub>D</sub>**.

* At low **V<sub>DS</sub>**, Id increases linearly → resistive region.
* At higher **V<sub>DS</sub>**, Id flattens → saturation region.

![07](./images/07.png)

---

# ⚙️ SPICE Simulation Setup — NMOS Id vs Vds (Day 1)

## 💻 Introduction to SPICE

**SPICE (Simulation Program with Integrated Circuit Emphasis)** is the industry-standard tool for simulating analog and mixed-signal circuits before fabrication.
In this session, we’ll explore how to perform a **DC sweep simulation** for an **NMOS transistor** using **Ngspice** and the **Sky130 PDK** models.

You’ll visualize the **Id–Vds characteristics** across multiple **Vgs levels**, validating the fundamental MOSFET behavior — from **linear** to **saturation** regions.

---

## ⚙️ Setting Up SPICE with Sky130

To use **SPICE** with **Sky130** technology, first clone the open-source repository containing pre-built model files and example circuits.

```bash
git clone https://github.com/kunalg123/sky130CircuitDesignWorkshop.git
```

### 📁 Important Files in the Repository

| File Path                                                                | Description                                        |
| ------------------------------------------------------------------------ | -------------------------------------------------- |
| `/sky130_fd_pr/cells/nfet_01v8/sky130_fd_pr__nfet_01v8__tt.pm3.spice`    | SPICE model for the NFET (typical process corner). |
| `/sky130_fd_pr/cells/nfet_01v8/sky130_fd_pr__nfet_01v8__tt.corner.spice` | Corner model for NFET (used for PVT variations).   |
| `/sky130_fd_pr/models/sky130.lib.pm3.spice`                              | Main library containing all Sky130 process models. |

ℹ️ **Note:** All example `.spice` files for daily experiments are located inside the `design/` directory.

```bash
nidesh@nexus-73:~/Soc/4/sky130CircuitDesignWorkshop$ ls
design  README.md
nidesh@nexus-73:~/Soc/4/sky130CircuitDesignWorkshop$ cd design/
nidesh@nexus-73:~/Soc/4/sky130CircuitDesignWorkshop/design$ ls
day1_nfet_idvds_L2_W5.spice      day4_inv_noisemargin_wp1_wn036.spice
day2_nfet_idvds_L015_W039.spice  day5_inv_devicevariation_wp7_wn042.spice
day2_nfet_idvgs_L015_W039.spice  day5_inv_supplyvariation_Wp1_Wn036.spice
day3_inv_tran_Wp084_Wn036.spice  sky130_fd_pr
day3_inv_vtc_Wp084_Wn036.spice

```

---

## 🧱 Understanding a SPICE Netlist

The SPICE netlist defines the **circuit**, **model**, and **simulation commands**.
Below is an example setup for **NMOS Id–Vds simulation**:

```spice
*** Model Description ***
.param temp=27

*** Including sky130 library files ***
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

*** Netlist Description ***
XM1 vdd n1 0 0 sky130_fd_pr__nfet_01v8 w=5 l=2
R1 n1 in 55
Vdd vdd 0 1.8
Vin in 0 1.8

*** Simulation Commands ***
.op
.dc Vdd 0 1.8 0.1 Vin 0 1.8 0.2

.control
run
display
setplot dc1
.endc

.end
```

![08](./images/08.png)

---

## ✍️ Circuit Syntax Explained

| Element                     | Description                                  |
| --------------------------- | -------------------------------------------- |
| **XM1**                     | MOSFET instance name                         |
| **vdd n1 0 0**              | Node connections — Drain, Gate, Source, Body |
| **sky130_fd_pr__nfet_01v8** | Sky130 NMOS model name                       |
| **w=5 l=2**                 | Transistor dimensions (in micrometers)       |
| **Vdd, Vin**                | DC sources for biasing                       |

🧠 *Example:*

```
M1 D G S B sky130_fd_pr__nfet_01v8 W=2u L=0.15u
```

---

## 🧪 Including Sky130 Model Libraries

Include the model files in your netlist to enable accurate transistor behavior:

```spice
.include "sky130_fd_pr/models/sky130.lib.spice"
```

![09](./images/09.png)

---

## 🚀 Running Your First SPICE Simulation

Run the simulation in the terminal:

```bash
ngspice day1_nfet_idvds_L2_W5.spice
```

Inside the Ngspice prompt, plot the drain current:

```bash
plot -vdd#branch
```

This command plots the **drain current (Id)** vs **drain-to-source voltage (Vds)** for multiple gate voltages (Vgs).

![10](./images/10.png)

---

## 📊 Observations — Id vs Vds

In the plotted waveforms, observe:

* 📈 **Linear Region:** Current increases linearly with Vds at small voltages.
* 🌀 **Saturation Region:** Id flattens out as Vds increases (channel pinch-off).
* ⚙️ **Effect of Vgs:** Higher Vgs increases Id due to stronger channel inversion.

---

## 🧠 Summary

| Concept                  | Key Takeaway                                                |
| ------------------------ | ----------------------------------------------------------- |
| **SPICE Simulation**     | Essential for pre-fabrication circuit verification          |
| **NMOS Device Behavior** | Controlled by gate voltage (Vgs)                            |
| **Resistive Region**     | Linear Id–Vds behavior                                      |
| **Saturation Region**    | Id saturates ∝ (Vgs − Vth)²                                 |
| **Body Effect**          | Raises Vth when substrate potential increases               |
| **Ngspice**              | Open-source simulator for device and circuit-level analysis |

---

## 🔗 Next Step

➡️ Proceed to **[Day 2 — Velocity Saturation and CMOS Inverter VTC](../Day2_CMOS_VTC/readme.md)**
Here, you’ll explore **Id–Vgs characteristics**, extract **Vth**, and analyze the **Voltage Transfer Characteristic (VTC)** of a CMOS inverter.

---
