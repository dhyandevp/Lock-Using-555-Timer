# 🔐 4-Key Combination Lock Using 555 Timer

[![Hardware Verified](https://img.shields.io/badge/Hardware-Breadboard%20Verified-success?style=for-the-badge&logo=espressif&logoColor=white)](./assets/images/breadboard_top_view.jpg)
[![IC](https://img.shields.io/badge/IC-NE555%20%2F%20HLF555-blue?style=for-the-badge&logo=circuitverse&logoColor=white)](./docs/SCHEMATIC.md)
[![Logic](https://img.shields.io/badge/Logic-Pure%20Hardware%20(No%20MCU)-orange?style=for-the-badge)](./docs/SCHEMATIC.md)
[![Supply Voltage](https://img.shields.io/badge/Power-5V%20DC-red?style=for-the-badge)](./docs/BOM.md)
[![Pulse Width](https://img.shields.io/badge/Pulse%20Width-~5.2%20Seconds-purple?style=for-the-badge)](./docs/CALCULATIONS.md)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)](./LICENSE)

A pure hardware, non-programmable electronic combination lock built on a solderless breadboard using an **NE555 / HLF555 timer IC in monostable multivibrator mode**, powered by a regulated **5V DC supply**.

No microcontrollers, no firmware, and no code—just pure analog and combinational hardware logic!

---

## 📑 Table of Contents

- [✨ Key Features](#-key-features)
- [🎬 Live Demonstration](#-live-demonstration)
- [📐 Circuit Schematic](#-circuit-schematic)
- [⚙️ How It Operates](#️-how-it-operates)
- [📌 Component & Pin Assignment](#-component--pin-assignment)
- [⚡ Critical Construction Takeaways](#-critical-construction-takeaways)
- [🧮 Timing Calculations](#-timing-calculations)
- [📋 Bill of Materials (BOM)](#-bill-of-materials-bom)
- [🔌 Actuator Extension: Driving a Solenoid / Relay](#-actuator-extension-driving-a-solenoid--relay)
- [🌐 Interactive Simulation](#-interactive-simulation)
- [📂 Repository Structure](#-repository-structure)
- [📄 License](#-license)

---

## ✨ Key Features

- **Pure Hardware Logic:** Operates entirely through passive components, switches, and a standard 555 timer. Immune to firmware crashes, buffer overflows, and software bugs.
- **Simultaneous 4-Key Combination ($K_1–K_4$):** Requires pressing all four correct pushbuttons simultaneously to complete the ground path and pull Pin 2 (TRIGGER) below $\frac{1}{3} V_{CC}$.
- **Timed Lock Release:** Once triggered, Pin 3 (OUTPUT) drives an indicator LED (or relay/solenoid) HIGH for **approximately 5.2 seconds** before automatically re-locking:
  $$T \approx 1.1 \times R_3 \times C_1 = 1.1 \times 47\text{ k}\Omega \times 100\ \mu\text{F} \approx 5.17\text{ s}$$
- **Instant Reset / Trap Keys ($W_1–W_4$):** Four decoy tactile switches wired in parallel directly to Pin 4 (RESET). Pressing any wrong key pulls Pin 4 to 0V, immediately shutting off the output and aborting the timing cycle.
- **Tamper & Brute-Force Defense:** Holding any wrong key overrides the trigger unconditionally. An attacker attempting to press all 8 buttons at once will pull the active-low RESET pin low, permanently disabling the lock while pressed.
- **Power Rail & Control Voltage Decoupling:** Incorporates $C_2$ ($10\text{ nF}$) on Pin 5 and $C_3$ ($100\text{ nF}$) across the power rails to eliminate false triggering from output switching transients.

---

## 🎬 Live Demonstration

### 1. Valid 4-Key Entry & Timed Unlock (~5.2s)
Pressing switches **$K_1, K_2, K_3, K_4$** simultaneously triggers the monostable timer, turning the output LED **ON** for $\approx 5.2$ seconds:

![Valid 4-Key Entry Demo](./assets/media/demo_unlock.gif)

### 2. Trap Key Press & Instant Reset
Pressing any decoy key (**$W_1, W_2, W_3, \text{or } W_4$**) pulls Pin 4 (RESET) low, instantly extinguishing the LED and re-locking:

![Trap Key Instant Reset Demo](./assets/media/demo_reset_trap.gif)

> 📹 *A high-definition MP4 video of the complete circuit operation is available at [`assets/media/lock_demo_720p.mp4`](./assets/media/lock_demo_720p.mp4).*

---

## 📸 Physical Breadboard Implementation

| Top-Down View | Perspective Layout |
| :---: | :---: |
| ![Top View](./assets/images/breadboard_top_view.jpg) | ![Angled View](./assets/images/breadboard_angled_view.jpg) |
| *DIP-8 555 IC with taped pin tags, series & parallel keys* | *Breadboard rails, capacitors, and USB 5V power leads* |

---

## 📐 Circuit Schematic

![Circuit Schematic](./docs/schematics/circuit_schematic.svg)

### Hardware Logic Diagram

![Hardware Logic Flow](./docs/schematics/logic_diagram.svg)

---

## ⚙️ How It Operates

### 1. Standby State (Locked)
- Pin 2 (TRIGGER) is pulled up to $+5\text{V}$ through $R_1$ ($10\text{ k}\Omega$).
- Pin 4 (RESET) is pulled up to $+5\text{V}$ through $R_2$ ($10\text{ k}\Omega$).
- The internal RS flip-flop inside the 555 timer is in the RESET state.
- Pin 3 (OUTPUT) remains **LOW ($0\text{V}$)**; the indicator LED is off.
- The internal discharge transistor (Pin 7) is saturated ON, clamping timing capacitor $C_1$ to Ground ($0\text{V}$).

### 2. Valid Combination Entry ($K_1 \land K_2 \land K_3 \land K_4$)
- The four valid switches are wired in **strict series** between Pin 2 and Ground.
- Pressing all four buttons at the exact same moment completes the circuit to Ground, pulling Pin 2 from $+5\text{V}$ down to $0\text{V}$.
- Because $0\text{V} < \frac{1}{3} V_{CC}$ ($1.67\text{V}$), the internal trigger comparator fires, setting the internal RS latch.
- Pin 3 switches **HIGH ($\approx 3.7\text{V} - 4.2\text{V}$)**, forward-biasing the LED through $R_4$ ($330\ \Omega$).
- Pin 7 turns OFF (high impedance), allowing $C_1$ ($100\ \mu\text{F}$) to charge exponentially from $+5\text{V}$ through $R_3$ ($47\text{ k}\Omega$).

### 3. Automatic Timed Re-Lock
- As $C_1$ charges according to $V_C(t) = V_{CC}(1 - e^{-t / R_3 C_1})$, the voltage on Pin 6 (THRESHOLD) ramps up.
- When $V_C(t)$ reaches $\frac{2}{3} V_{CC}$ ($3.33\text{V}$), the internal threshold comparator fires and resets the flip-flop.
- Pin 3 falls back to **LOW ($0\text{V}$)**, turning off the LED.
- Pin 7 turns ON, rapidly discharging $C_1$ to $0\text{V}$, returning the system to Standby.

### 4. Trap / Decoy Key Override ($W_1 \lor W_2 \lor W_3 \lor W_4$)
- Four "wrong" keys are wired in **parallel** between Pin 4 (RESET) and Ground.
- Pin 4 is active-low ($< 0.7\text{V}$).
- Pressing **any single trap key** immediately forces Pin 4 to $0\text{V}$, which bypasses the comparators and directly clears the internal latch.
- The output immediately cuts to $0\text{V}$, turning off the LED.
- If an intruder attempts to press all keys simultaneously, the trap keys assert Pin 4 low, completely overriding Pin 2 and preventing the lock from triggering.

---

## 📌 Component & Pin Assignment

| IC Pin / Function | Breadboard Connection | Role / Technical Notes |
| :---: | :--- | :--- |
| **Pin 1 (GND)** | Blue Rail (GND) | IC ground reference ($0\text{V}$). |
| **Pin 2 (TRIGGER)** | $10\text{ k}\Omega$ pull-up ($R_1$) to $+5\text{V}$; $K_1 \rightarrow K_2 \rightarrow K_3 \rightarrow K_4$ in series to GND | Trigger input; requires all four switches closed simultaneously to pull below $1.67\text{V}$. |
| **Pin 3 (OUTPUT)** | $330\ \Omega$ ($R_4$) $\rightarrow$ LED Anode (+); LED Cathode (-) to GND | Lock indicator output ($\approx 5.15\text{ mA}$ drive current). |
| **Pin 4 (RESET)** | $10\text{ k}\Omega$ pull-up ($R_2$) to $+5\text{V}$; $W_1, W_2, W_3, W_4$ in parallel to GND | Active-low master reset; immediately cuts off output when any $W$ key is pressed. |
| **Pin 5 (CONTROL)** | $10\text{ nF}$ ceramic capacitor ($C_2$, code `103`) to GND | Noise decoupling for internal voltage divider reference ($\frac{2}{3} V_{CC}$). |
| **Pin 6 (THRESHOLD) & Pin 7 (DISCHARGE)** | Tied together; connected to node of $47\text{ k}\Omega$ ($R_3$) to $+5\text{V}$ and $100\ \mu\text{F}$ ($C_1$) to GND | Timing node defining pulse duration ($T = 1.1 R_3 C_1$). |
| **Pin 8 (VCC)** | Red Rail ($+5\text{V}$) | IC power supply input ($+5\text{V}$ DC). |
| **Decoupling ($C_3$)** | $100\text{ nF}$ ceramic capacitor (code `104`) across $+5\text{V}$ and GND | High-frequency power supply rail stabilization. |

---

## ⚡ Critical Construction Takeaways

### 1. Schematic vs. Physical Package Layout
Functional schematics rearrange pins logically for readability, but the physical IC is a standard **DIP-8 package**:
- Pins 1 to 4 run down the left side (Notch at top: 1-GND, 2-TRIG, 3-OUT, 4-RESET).
- Pins 5 to 8 run up the right side (5-CTRL, 6-THRESH, 7-DISCH, 8-VCC).
- Always verify pin numbering against the physical notch before powering on.

![DIP-8 Pinout & Switch Bridging Rule](./docs/schematics/555_pinout.svg)

### 2. Tactile Switch Internal Bridging (The Diagonal Wiring Rule)
- 4-pin tactile pushbuttons have internally shorted pairs across opposite sides.
- Wiring across internally shorted terminals causes a permanent short-circuit to Ground.
- **Best Practice:** Connect wires **diagonally** across opposing corner pins (e.g., top-left to bottom-right). This guarantees an open circuit until the button is physically depressed.

### 3. True Series Continuity
- In the combination code chain ($K_1–K_4$), **only the final switch ($K_4$) must touch the Ground rail**.
- Intermediate terminals must connect strictly in series ($K_1 \rightarrow K_2 \rightarrow K_3 \rightarrow K_4$). If any intermediate switch wire touches Ground, partial codes will trigger the lock.

---

## 🧮 Timing Calculations

The monostable pulse width is governed by:

$$T = R_3 \cdot C_1 \cdot \ln(3) \approx 1.1 \times R_3 \times C_1$$

Substituting the circuit values:
- $R_3 = 47\text{ k}\Omega = 47{,}000\ \Omega$
- $C_1 = 100\ \mu\text{F} = 100 \times 10^{-6}\text{ F}$

$$T \approx 1.1 \times (4.7 \times 10^4\ \Omega) \times (1.0 \times 10^{-4}\text{ F}) \approx \mathbf{5.17\text{ seconds}}$$

*For tolerance variations ($\pm 20\%$ capacitor variance) and custom timing resistor lookup tables, see [`docs/CALCULATIONS.md`](./docs/CALCULATIONS.md).*

---

## 📋 Bill of Materials (BOM)

| Designator | Component | Value / Rating | Package | Qty |
| :--- | :--- | :--- | :--- | :---: |
| **U1** | NE555 / HLF555 Timer IC | Standard Bipolar / CMOS | DIP-8 | 1 |
| **R1, R2** | Carbon/Metal Film Resistors | $10\text{ k}\Omega \pm 5\%$, 1/4W | Axial | 2 |
| **R3** | Metal Film Resistor | $47\text{ k}\Omega \pm 5\%$, 1/4W | Axial | 1 |
| **R4** | Carbon Film Resistor | $330\ \Omega \pm 5\%$, 1/4W | Axial | 1 |
| **C1** | Aluminum Electrolytic Capacitor | $100\ \mu\text{F}$, 16V–25V | Radial | 1 |
| **C2** | Ceramic Disc Capacitor | $10\text{ nF}$ (code `103`) | Radial | 1 |
| **C3** | Ceramic Disc Capacitor | $100\text{ nF}$ (code `104`) | Radial | 1 |
| **K1–K4** | Tactile Pushbutton Switches | Momentary SPST, 6x6mm | DIP-4 | 4 |
| **W1–W4** | Tactile Pushbutton Switches | Momentary SPST, 6x6mm | DIP-4 | 4 |
| **LED1** | Light Emitting Diode | 5mm Blue/Green ($V_F \approx 2.0\text{V}$) | Radial | 1 |
| **BB1** | Solderless Breadboard | 830 Tie Points with rails | Solderless | 1 |
| **PWR** | DC Power Source | 5V DC Regulated | USB / Bench | 1 |

*For complete sourcing details and alternatives, see [`docs/BOM.md`](./docs/BOM.md).*

---

## 🔌 Actuator Extension: Driving a Solenoid / Relay

To actuate a physical 12V or 5V electric door strike, solenoid lock, or high-power load, connect an NPN transistor driver to Pin 3:

```
                  +12V (or +5V External Lock Power)
                       │
                       ├───[ Solenoid / Relay Coil ]───┐
                       │                               │
                      [▲] 1N4007 Flyback Diode        │
                      [│] (Cathode to +V)             │
                       │                               │
                       └───────────────────────────────┤
                                                       │  Collector
Pin 3 (OUT) ───[ 1 kΩ ]─── Base (2N2222 / TIP120) ────┤
                                                       │  Emitter
                                                      GND (Common Ground)
```

> [!IMPORTANT]
> Always install a **1N4007 flyback diode** antiparallel across inductive coils (solenoids, relays) to protect the switching transistor from high-voltage back-EMF spikes when de-energizing.

---

## 🌐 Interactive Simulation

Test and modify this circuit right inside your browser:
1. Open [falstad.com/circuit/circuitjs.html](https://www.falstad.com/circuit/circuitjs.html).
2. Click **File → Import From Text...**
3. Paste the contents of [`simulation/falstad_circuit.txt`](./simulation/falstad_circuit.txt).
4. Interact with the virtual switches and observe real-time current flows, capacitor charging curves, and state transitions!

---

## 📂 Repository Structure

```
.
├── README.md                          # Flagship project documentation & overview
├── LICENSE                            # MIT License
├── .gitignore                         # Git exclusion rules
├── docs/
│   ├── SCHEMATIC.md                   # In-depth circuit theory & internal 555 architecture
│   ├── BREADBOARD_GUIDE.md            # Step-by-step breadboard assembly walkthrough
│   ├── TROUBLESHOOTING.md             # Fault-isolation tree & DC test-point table
│   ├── BOM.md                         # Detailed bill of materials & sourcing notes
│   ├── CALCULATIONS.md                # Mathematical formulas & timing lookup tables
│   └── schematics/
│       ├── circuit_schematic.svg      # Complete vector circuit schematic
│       ├── 555_pinout.svg             # DIP-8 physical pinout & switch bridging guide
│       └── logic_diagram.svg          # Combinational logic flow & state transitions
├── assets/
│   ├── images/
│   │   ├── breadboard_top_view.jpg    # High-resolution top breadboard photo
│   │   └── breadboard_angled_view.jpg # High-resolution perspective breadboard photo
│   └── media/
│       ├── demo_unlock.gif            # Animated demonstration of valid 4-key unlock
│       ├── demo_reset_trap.gif        # Animated demonstration of instant trap reset
│       ├── lock_demo_720p.mp4         # Web-optimized 720p full demonstration video
│       └── VID20261023223546.mp4      # Original raw recording of hardware test
└── simulation/
    ├── README.md                      # Guide to browser and SPICE simulations
    └── falstad_circuit.txt            # Importable Falstad circuit simulator text file
```

---

## 📄 License

This project is open-source hardware licensed under the [MIT License](./LICENSE). Feel free to build, modify, and integrate into your own projects!
