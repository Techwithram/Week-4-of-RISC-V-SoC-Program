# ⚡ Week 4 — CMOS Circuit Design using Sky130 PDK

## 🧩 RISC-V Reference SoC Tapeout Program

Welcome to **Week 4** of the **RISC-V SoC Tapeout Program**, where we move from digital logic abstraction to the **transistor-level building blocks** that make up every logic gate. 🌱

This week, you’ll simulate and analyze **CMOS devices** using **Ngspice** with the **Sky130 PDK**, exploring how **transistor physics** directly impacts **timing, power, and reliability** in silicon. Each day progressively links analog device behavior to digital circuit performance. ⚙️💡

---

## 📚 Week 4 — Task Overview

| Day       | Focus Area                                                      | Folder Link                            | Description                                                                                                                                                      |
| --------- | --------------------------------------------------------------- | -------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Day 1** | 🔍 **NMOS Drain Current (Id) vs Drain-to-Source Voltage (Vds)** | [Day1](https://github.com/Techwithram/Week-4-of-RISC-V-SoC-Program/blob/main/Day%201)          | Simulate **Id–Vds** curves for NMOS under multiple **Vgs** values, identify **linear** and **saturation** regions, and understand **channel-length modulation**. |
| **Day 2** | ⚡ **Velocity Saturation & CMOS Inverter VTC**                   | [Day2](./Day2_CMOS_VTC)                | Explore **Id–Vgs** behavior, estimate **threshold voltage (Vth)**, and build a **CMOS inverter** to obtain its **Voltage Transfer Characteristic (VTC)**.        |
| **Day 3** | ⏱️ **Switching Threshold & Dynamic Simulation**                 | [Day3](./Day3_CMOS_Switching_Dynamics) | Apply a pulse input to the inverter, perform **transient analysis**, and extract **switching threshold**, **rise/fall delays**, and **propagation delay**.       |
| **Day 4** | 🧮 **Noise Margin & Robustness Evaluation**                     | [Day4](./Day4_Noise_Margin_Robustness) | From the VTC, calculate **VOH**, **VOL**, **VIH**, **VIL**, and determine **Noise Margins (NMH, NML)** to assess logic-level stability.                          |
| **Day 5** | ⚙️ **Power Supply & Device Variation Analysis**                 | [Day5](./Day5_Power_Supply_Variation)  | Examine how **VDD scaling** and **device W/L ratio variations** affect the inverter’s **switching point**, **transition slope**, and **robustness**.             |

---

## 🌟 Key Learnings

By completing Week 4, you will:

* 🧠 Understand **NMOS and PMOS I-V characteristics** using the Sky130 process.
* ⚙️ Construct and simulate a **CMOS inverter** from transistor models.
* 📈 Evaluate **VTC**, **switching thresholds**, and **dynamic behavior**.
* 🔋 Explore the impact of **VDD and device variations** on performance and noise margin.
* 🔍 Bridge **analog transistor physics** to **digital timing closure** principles used in later stages.

---

## 📂 Repository Structure

```
Week4_CMOS_Circuit_Design_sky130/
│
├── Day1_NMOS_Id_vs_Vds/
├── Day2_CMOS_VTC/
├── Day3_CMOS_Switching_Dynamics/
├── Day4_Noise_Margin_Robustness/
├── Day5_Power_Supply_Variation/
└── README.md
```

Each day folder includes:

* 🧾 Ngspice netlist (`.cir` or `.spice`)
* 📊 Matplotlib or Ngspice plots (`.png`)
* 🧩 Result analysis and discussion

---

## 🔗 Week 4 Main Report

You can view the detailed documentation and plots here:
➡️ [**Week 4 – CMOS Circuit Design using Sky130 PDK**](https://github.com/Nideshkanna/week4_CMOS_Circuit_Design_sky130/blob/main/README.md)

---

## 🏁 End of Week 4

This marks the **completion of Week 4**, the final week of the Analog/Device simulation phase in our tapeout journey.

Next, in 👉 [**Week 5 – STA and Timing Closure (Sign-off Analysis)**](https://github.com/Nideshkanna/week5_STA_TimingClosure_sky130), you’ll apply the transistor-level insights from this week to timing verification and sign-off of the RISC-V SoC design. 🕒✨

---
