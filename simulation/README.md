# Circuit Simulation Guide

This folder contains simulation files and instructions for testing the **4-Key Combination Lock Using 555 Timer** in software before or alongside your physical breadboard build.

---

## 🌐 1. Falstad Web Circuit Simulator

The circuit can be simulated in any modern web browser using the open-source [Falstad Circuit Simulator](https://www.falstad.com/circuit/circuitjs.html).

### How to Run the Simulation:
1. Open [falstad.com/circuit/circuitjs.html](https://www.falstad.com/circuit/circuitjs.html).
2. Go to the menu: **File → Import From Text...**
3. Open the [`falstad_circuit.txt`](./falstad_circuit.txt) file in this directory, copy its entire contents, paste it into the dialog box, and click **OK**.
4. The simulation will render immediately:
   - **K1–K4** are simulated as 4 series pushbuttons wired to the Trigger node.
   - **W1–W4** are simulated as 4 parallel pushbuttons wired to the Reset node.
   - Click on switches **K1, K2, K3, and K4** simultaneously to trigger the monostable pulse.
   - Observe the LED illuminate green/cyan and the timing capacitor ($C_1$) charge up to $\frac{2}{3} V_{CC}$ over ~5.2 seconds before shutting off.
   - Press any of **W1–W4** at any time to verify immediate shutoff and trap lockout.

---

## ⚡ 2. Circuit Parameters for SPICE / LTspice / Wokwi

If you are modeling this circuit in **LTspice**, **NGspice**, or **Wokwi**:

| Component | Parameter | SPICE Model / Value | Function |
| :--- | :--- | :--- | :--- |
| **U1** | NE555 / LM555 | Standard 555 Macro / Subcircuit | Monostable core |
| **V1** | DC Voltage Source | `5V` | System supply rail |
| **R1** | Pull-up Resistor | `10k` | Trigger bias to VCC |
| **R2** | Pull-up Resistor | `10k` | Reset bias to VCC |
| **R3** | Timing Resistor | `47k` | Timing charge resistance |
| **R4** | Limiting Resistor | `330` | LED current limiter |
| **C1** | Timing Capacitor | `100u` (electrolytic) | Defines $T = 1.1 \times R_3 \times C_1$ |
| **C2** | Control Decoupling | `10n` | Pin 5 noise filtering |
| **C3** | Rail Decoupling | `100n` | High-frequency rail bypass |
| **D1** | Indicator LED | Standard Diode ($V_F \approx 2.0\text{V}$) | Output indicator |

### Transient Analysis Settings:
- Stop time: `10s`
- Time step: `1ms`
- Initial conditions: `UIC` (Use Initial Conditions, $V(C_1) = 0\text{V}$)
