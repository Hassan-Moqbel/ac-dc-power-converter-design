# AC-to-DC Converter (Full-Wave Rectification)

![Power Electronics](https://img.shields.io/badge/Domain-Power_Electronics-FF6F00?style=for-the-badge)
![AC-to-DC Conversion](https://img.shields.io/badge/Topology-AC_to_DC_Conversion-009999?style=for-the-badge)
![Full-Wave Rectification](https://img.shields.io/badge/Circuit-Full_Wave_Bridge-4B0082?style=for-the-badge)
![Hardware Verified](https://img.shields.io/badge/Status-Hardware_Verified-28A745?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)

## Executive Overview
The foundational step in almost all modern electronic power supplies is the efficient conversion of alternating current (AC) grid power into stable direct current (DC). This project demonstrates the theoretical modeling, physical breadboard prototyping, and oscilloscope verification of an analog **AC-to-DC Converter**. By utilizing a step-down transformer, a solid-state full-wave bridge rectifier, and electrolytic passive capacitive filtering, the system successfully strips the negative half-cycle and smooths the pulsating transient into a usable DC rail.

> [!CAUTION]
> **High-Voltage Mains AC & Stored Charge Hazard**
> This physical prototype interfaces directly with lethal 220V/110V Mains AC. Safe execution requires strict adherence to primary-side isolation protocols, including physical barriers and in-line fuse protection before the transformer primary coil. Additionally, high-capacitance electrolytic filters can hold a dangerous DC charge long after the circuit is unplugged. Always use a high-wattage bleeder resistor to manually discharge capacitors before physically handling the prototype.

## System Highlights
- **Step-Down Mains Isolation**: Safely steps 220V grid AC down to a low-voltage AC potential while providing galvanic isolation.
- **4-Diode Full-Wave Bridge Topology**: Converts the full $360^\circ$ of the sine wave (both positive and negative half-cycles) into pulsating DC, doubling the efficiency and fundamental ripple frequency compared to half-wave rectifiers.
- **Bulk Electrolytic Reservoir Filtering**: Passive capacitive ripple suppression to flatten the pulsating waveforms into a continuous DC bus voltage suitable for analog/digital loads.
- **Oscilloscope Validated**: Hardware performance was verified via empirical bench-testing and direct waveform analysis.

## System Architecture Diagram

```mermaid
flowchart LR
    AC["Mains AC 220V/50Hz"] -->|Isolation| XFMR["Step-Down Transformer"]
    XFMR -->|Secondary AC Sine| BRIDGE["Full-Wave Rectifier \n4x 1N4007"]
    BRIDGE -->|100Hz Pulsating DC| FILTER["Electrolytic Smoothing Reservoir"]
    FILTER -->|Ripple DC Bus| LOAD["Resistive Load / R_L"]
```

## Theoretical & Mathematical Models

### 1. Peak Secondary Voltage & Diode Drops

Because two silicon diodes conduct during every half cycle in a bridge configuration, the theoretical peak DC voltage reaching the filter capacitor is:

$$
V_{\text{peak}} = \sqrt{2} V_{\text{rms}} - 2V_D
$$

*(where $V_D \approx 0.7\text{ V}$)*

### 2. Average Unfiltered DC Voltage

Without the smoothing capacitor, the average output voltage of the full-wave rectified sine wave evaluates to:

$$
V_{\text{dc,raw}} = \frac{2V_m}{\pi} \approx 0.636 V_m
$$

### 3. Capacitive Peak-to-Peak Ripple Voltage

When the capacitor is added, it discharges into the load between peaks. The peak-to-peak ripple voltage ($V_{r(p-p)}$) is derived as:

$$
V_{r(p-p)} = \frac{I_{\text{dc}}}{2 f C} = \frac{V_{\text{dc}}}{2 f R_L C}
$$

*(Note: For a mains frequency $f_{\text{grid}} = 50\text{ Hz}$, the full-wave ripple frequency is $2f = 100\text{ Hz}$).*

### 4. Filtered DC Output Approximation

The practical DC output voltage under load becomes:

$$
V_{\text{dc,filtered}} \approx V_{\text{peak}} - \frac{V_{r(p-p)}}{2}
$$

## Repository Layout Tree
```text
.
├── docs/                  # In-depth engineering PDF report
├── media/
│   ├── photos/            # Original hardware breadboard photos and oscilloscope traces
│   └── videos/            # Original MP4 demonstration and bench testing videos
└── README.md              # P01 Gold Standard Documentation
```

## Laboratory Testing & Waveform Analysis
The physical prototype was verified using an oscilloscope. The testing phases consisted of:
1. **Unfiltered Observation**: Probing the output of the diode bridge without the capacitor installed. The trace reveals a raw, pulsating $100\text{Hz}$ DC wave touching 0V every 10ms.
2. **Capacitive Filtering**: Upon inserting the bulk electrolytic capacitor in parallel with the load, the oscilloscope trace flattens, demonstrating the theoretical $V_{\text{dc,filtered}}$ with a small triangular ripple $V_{r(p-p)}$ riding on top as the capacitor discharges into the load resistor between AC cycles.

## Authentic Media Catalog
- **Engineering Report**: [`docs/Conversion from  (AC) to (DC)حسن  مقبل .pdf`](docs/)
- **Oscilloscope & Breadboard Media**: See `media/photos/` for original `[ORIGINAL HARDWARE & WAVEFORM ARTIFACTS]`.
- **Bench Test Videos**: Original video clips (up to 85MB) are preserved locally in `media/videos/` as `[ORIGINAL HARDWARE TEST VIDEOS]`.

## Engineering Audit & Design Tradeoffs
- **Full-Wave Bridge vs. Center-Tapped Transformer**: This design utilizes a 4-diode bridge rather than a 2-diode center-tapped transformer. The bridge topology requires a cheaper, simpler transformer with only two secondary wire leads and allows for a diode PIV rating of only $V_m$(instead of$2V_m$). The tradeoff is a higher forward voltage drop ($1.4\text{V}$vs$0.7\text{V}$), slightly reducing the final$V_{peak}$.
- **Passive Filtering vs. Active Regulation**: This project stops at passive capacitive filtering. While acceptable for basic DC motors or resistive heating elements, the output voltage will sag under heavy load variations and grid fluctuations. For sensitive electronics, an active Linear Regulator (e.g., LM317 or 7812) or SMPS topology must be cascaded after this filtering stage.

---

**Hassan Moqbel Morshed Ghaleb**
Mechatronics Engineer | Mechanical Design & CAD (SolidWorks & AutoCAD) | Preventive Maintenance & Electromechanical Systems | Industrial Automation, Control Systems, Robotics & Intelligent Machines | CAD/FEA, Embedded Systems, Python & C++
[GitHub](https://github.com/Hassan-Moqbel) · [Facebook](https://www.facebook.com/share/1BqxAgVjHi/) · [LinkedIn](https://www.linkedin.com/in/hassan-moqbel)

## License
This project is licensed under the [MIT License](LICENSE).
