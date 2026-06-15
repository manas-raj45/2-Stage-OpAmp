# Two-Stage CMOS Operational Amplifier with Post-Layout PVT Analysis
## Overview
This repository contains the schematic design, physical layout, and post-layout verification of a Two-Stage Operational Amplifier designed in the gdpk090 CMOS process. 
---
## Target Specifications & Post-Layout Results

| Parameter | Target Specification | Post-Layout (PEX) Result |
| :--- | :--- | :--- |
| **DC Gain** | 60 dB (1000) | 45 dB (53 dB Pre-Layout) |
| **Gain Bandwidth Product (GBW)** | 30 MHz | 27 MHz |
| **Phase Margin (PM)** | ≥ 60° | 68° |
| **Common-Mode Rejection Ratio (CMRR)** | - | 66 dB |
| **Slew Rate** | 20 V/µs | 17.42 V/µs |
| **Max Common-Mode Input (ICMR+)** | 1.6 V | 1.66 V |
| **Min Common-Mode Input (ICMR-)** | 0.8 V | 0.838 V |
| **Calculated Headroom** | - | 0.59 V |
| **Output Swing** | - | 1.2 V |
| **Load Capacitance (C_L)** | 2 pF | 2 pF |
| **Power Dissipation** | ≤ 300 µW | 309 µW |

---
## 1. Schematic Design and Biasing
The core architecture is a two-stage operational amplifier featuring a differential input pair with a current mirror load, followed by a common-source second stage.
* **Pre-Layout Performance:** The ideal schematic successfully achieved a **DC Gain of 53 dB** before layout parasitics were introduced.
* **The Problem (Instability):** Uncompensated two-stage op-amps have two high-impedance nodes that create two low-frequency poles, leading to poor phase margin, ringing, and oscillation.
* **The Solution (Miller Compensation):** A capacitor was added between the first and second stages. Using pole-splitting, this pushes the dominant pole lower and the non-dominant pole past the unity-gain frequency, achieving a solid **68° Phase Margin**.
* **Biasing & Voltage Swing:** Transistor dimensions and bias currents were sized to explicitly maintain a **Calculated Headroom of 0.59 V**. This guarantees all MOSFETs remain in saturation while allowing a clean **Output Swing of 1.2 V**.
<br>
<img width="1181" height="638" alt="schematic_region of op" src="https://github.com/user-attachments/assets/6707be0a-ee93-40d5-b1f8-df6c8d1b0950" />
<img width="1176" height="635" alt="opamp_schematic" src="https://github.com/user-attachments/assets/ade7ef6d-3273-49b2-87ca-5255a26cccc1" />
<img width="996" height="789" alt="gain and phase" src="https://github.com/user-attachments/assets/4608aeeb-78ed-402a-a92f-de440fd605c1" />
<br>

## 2. Physical Layout & Parasitic Extraction (PEX)
* **Post-Layout Gain Drop:** DC Gain dropped from the pre-layout **53 dB** down to **45 dB** after PEX.
* **Reason for Variation:** Physical layout extraction introduces real-world routing parasitics, specifically overlapping metal layer capacitances and trace resistances. This parasitic loading lowers the effective output impedance of the internal nodes, inherently reducing the overall gain and bandwidth.
<br>
<img width="1666" height="486" alt="drc" src="https://github.com/user-attachments/assets/267b52aa-505a-4208-9b00-9f7a01fdb473" />
<img width="1646" height="570" alt="lvs" src="https://github.com/user-attachments/assets/4b3129a0-fda1-488b-a069-d65989c85313" />
<img width="1657" height="496" alt="pex" src="https://github.com/user-attachments/assets/2db37272-1a64-47e4-af32-526953c4bf20" />
<br>

## 3. PVT Corner & Sensitivity Analysis
A 9-corner PVT sweep was conducted on the extracted view to verify stability across manufacturing and environmental stresses.
* **Temperatures:** -40°C, 42.5°C, 125°C
* **Supply Voltages:** 1.75 V, 1.8 V (Nominal), 1.95 V
* **PVT Variations Explained:** The spread in the gain and phase plots is expected behavior. Temperature swings alter carrier mobility, supply variations shift available overdrive voltage, and process corners (Fast-Fast/Slow-Slow) shift MOSFET threshold voltages. These factors combined alter the DC operating point, but the amplifier remains fully functional and stable across all tested boundaries.
* **Bandwidth Temperature Dependence:** As temperature rises, carrier mobility degrades, leading to a reduction in transconductance. This directly impacts the bandwidth, demonstrating a clear inverse relationship:
  * **-40°C:** 80 KHz
  * **42.5°C:** 62 KHz
  * **125°C:** 51 KHz
<br>
<img width="989" height="665" alt="pvt gain" src="https://github.com/user-attachments/assets/e1eca729-9b1b-4d70-9959-3864463df8b0" />
<img width="761" height="596" alt="pvt phase deg" src="https://github.com/user-attachments/assets/6a428c23-8367-4153-b799-43bd180dd9b0" />
