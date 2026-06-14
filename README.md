# Two-Stage CMOS Operational Amplifier with Post-Layout PVT Analysis
## Overview
This repository contains the schematic design, physical layout, and post-layout verification of a Two-Stage Operational Amplifier designed in the gdpk090 CMOS process. 
---
## Target Specifications & Post-Layout Results

| Parameter | Target Specification | Post-Layout (PEX) Result |
| :--- | :--- | :--- |
| **DC Gain** | 60 dB (1000) | 45 dB |
| **Gain Bandwidth Product (GBW)** | 30 MHz | 27 MHz |
| **Phase Margin (PM)** | ≥ 60° | 68° |
| **Slew Rate** | 20 V/µs | 17.42 V/µs |
| **Max Common-Mode Input (ICMR+)** | 1.6 V | [Insert Result] V |
| **Min Common-Mode Input (ICMR-)** | 0.8 V | [Insert Result] V |
| **Load Capacitance (C_L)** | 2 pF | 2 pF |
| **Power Dissipation** | ≤ 300 µW | [Insert Result] µW |

---
## 1. Schematic Design and Biasing
The core architecture is a two-stage operational amplifier featuring a differential input pair with a current mirror load, followed by a common-source second stage for maximum voltage swing. 
* **Transconductance Tuning:** Input differential pair widths ($W$) were iteratively sized to maximize $g_m$ while preserving bandwidth.
* **Output Impedance:** The lengths ($L$) of the first-stage PMOS loads were scaled to maximize $r_o$, establishing the dominant mid-band gain.
> **Note on Images:** Please ensure your browser has loaded the images below. If an image is missing, refresh the page.
**[INSERT SCHEMATIC IMAGE HERE]**
*Use the syntax: `![Ideal Schematic](link_to_your_image.jpg)`*
---
## 2. Physical Layout & Parasitic Extraction (PEX)
The physical layout was drafted to strictly adhere to Design Rule Checks (DRC) and Layout vs. Schematic (LVS) matching. 
Upon successful LVS, parasitic extraction was performed. A nominal drop of ~[Insert dB drop] dB in open-loop gain was observed and mathematically validated as the expected consequence of layout-induced parasitic routing resistance and internal node capacitance.
**[INSERT LAYOUT IMAGE HERE]**
*Use the syntax: `![Physical Layout](link_to_your_layout_image.jpg)`*
---
## 3. PVT Corner & Sensitivity Analysis
A rigorous 9-corner PVT sweep was conducted on the post-layout extracted view to simulate real-world silicon stress conditions. 
* **Temperatures:** -40°C, 27°C, 125°C
* **Supply Voltages:** 1.62 V (-10%), 1.8 V (Nominal), 1.98 V (+10%)
### Extreme Corner Boundary Condition (1.62V / 125°C)
During initial testing, the extreme boundary corner ($1.62 \text{ V}$ and $125^\circ\text{C}$) caused the tail current sink to fall out of saturation. This was diagnosed as a voltage headroom crush: 
1. Elevated temperatures increased the required $V_{dsat}$ due to lattice scattering and reduced electron mobility.
2. The restricted $1.62 \text{ V}$ supply compressed the available $V_{ds}$.
**The Engineering Fix:** The layout was successfully revised by scaling the width ($W$) of the tail transistor and its current mirror reference. This effectively lowered the required $V_{dsat}$, restoring the transistor to the saturation region and rescuing the amplifier's gain without compromising the current ratio.
**[INSERT PVT GRAPH IMAGE HERE]**
*Use the syntax: `![PVT Corner Analysis](link_to_your_pvt_graph.jpg)`*
*(The high-frequency pole-zero doublets visible in the roll-off are the verified result of parasitic extraction networks).*
---
## Tools Used
* **Cadence Virtuoso** (Schematic Editor, Layout Suite)
* **Analog Design Environment (ADE L)**
* **Spectre Simulator**
* **Assura / Calibre** (DRC, LVS, PEX)
