# Troubleshooting & Diagnostic Guide

This guide provides a systematic fault-isolation workflow, root-cause troubleshooting matrix, and direct DC voltage test-point tables for the **4-Key Combination Lock Using 555 Timer**.

---

## ⚡ Multimeter Voltage Test-Point Matrix

Set your digital multimeter (DMM) to **DC Volts (20V range)**. Clip the black meter probe to the **Blue Ground Rail (GND)**, and probe each IC pin with the red lead across the three primary circuit states:

| IC Pin | Function | Standby (Idle) | Active Pulse (Unlocked) | Reset / Trap Key Pressed | Diagnostic Meaning |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **Pin 1** | GND | `0.00 V` | `0.00 V` | `0.00 V` | Must be solid 0V. If higher, ground rail is floating or broken. |
| **Pin 2** | TRIG | `5.00 V` | `0.00 V` *(momentary)* | `5.00 V` | Held high by $R_1$. Drops to 0V only when $K_1-K_4$ are all pressed. |
| **Pin 3** | OUT | `0.00 V - 0.1 V` | `3.6 V - 4.2 V` | `0.00 V` | High during timed unlock. Drops immediately when reset or timed out. |
| **Pin 4** | RESET | `5.00 V` | `5.00 V` | `0.00 V` *(momentary)* | Held high by $R_2$. Drops to 0V whenever any $W_1-W_4$ key is pressed. |
| **Pin 5** | CTRL | `3.33 V` | `3.33 V` | `3.33 V` | Internally fixed at $\frac{2}{3} V_{CC}$. If not $\approx 3.3\text{V}$, 555 IC is damaged. |
| **Pin 6** | THRESH | `0.00 V` | `0.00 V → 3.33 V` *(ramp)* | `0.00 V` | Ramps up exponentially during pulse. When it reaches 3.33V, pulse ends. |
| **Pin 7** | DISCH | `0.05 V` | `Floating / High Z` | `0.05 V` | Tied to Pin 6. Clamped to GND in standby; open-collector during pulse. |
| **Pin 8** | VCC | `5.00 V` | `4.90 V - 5.00 V` | `5.00 V` | Regulated supply voltage. Should not sag below 4.5V during switching. |

---

## 🔍 Systematic Fault Tree

### Fault 1: LED Is Stuck ON Permanently
- **Likely Cause 1: Pin 2 (Trigger) is held low.**
  - *Check:* Measure voltage at Pin 2. If it is $0\text{V}$, one of the switches in the $K_1–K_4$ chain is shorted or accidentally bridged across horizontally shorted pins.
  - *Fix:* Rewire switches diagonally. Ensure $R_1$ ($10\text{ k}\Omega$) is securely connected between Pin 2 and $+5\text{V}$.
- **Likely Cause 2: Timing capacitor ($C_1$) is missing, backward, or leaky.**
  - *Check:* If $C_1$ is installed backward (reverse polarity), it will conduct leakage current and prevent Pin 6 from ever reaching $\frac{2}{3} V_{CC}$ ($3.33\text{V}$).
  - *Fix:* Ensure the positive terminal of $C_1$ is connected to Pin 6/7 and negative stripe goes to GND.
- **Likely Cause 3: Pin 6 (Threshold) is not connected to Pin 7 / $R_3$.**
  - *Check:* Verify continuity between Pin 6 and Pin 7. If Pin 6 is floating, it cannot detect when $C_1$ has charged.

---

### Fault 2: LED Never Turns ON When Pressing K1–K4
- **Likely Cause 1: One or more switches in the series chain have no contact.**
  - *Check:* Set DMM to continuity/resistance mode. Probe across the entire $K_1-K_4$ chain (from Pin 2 lead to GND). When all 4 buttons are pressed, resistance should read $< 1\ \Omega$. If open circuit (`OL`), test each switch individually to locate the loose jumper or broken switch.
- **Likely Cause 2: Pin 4 (Reset) is shorted to Ground.**
  - *Check:* Measure voltage at Pin 4. If it reads $0\text{V}$, the chip is permanently held in reset.
  - *Fix:* Check $W_1–W_4$ switches for horizontal pin bridging shorts. Ensure $R_2$ ($10\text{ k}\Omega$) is present and connected to $+5\text{V}$.
- **Likely Cause 3: LED installed with reverse polarity.**
  - *Check:* The longer lead of the LED must connect toward Pin 3 (via $R_4$); the shorter lead (flat edge) must connect to GND.

---

### Fault 3: Lock Triggers with Fewer Than 4 Keys Pressed (Partial Code Trigger)
- **Likely Cause: Intermediate switch grounded to breadboard rail.**
  - *Check:* Only the terminal of **$K_4$** should touch the Ground rail. If the wire from $K_1 \rightarrow K_2$, $K_2 \rightarrow K_3$, or $K_3 \rightarrow K_4$ accidentally touched the blue GND rail, pressing only 1, 2, or 3 buttons will pull Pin 2 to $0\text{V}$.
  - *Fix:* Follow the wire from each switch; ensure intermediate junctions only touch other switch terminals and do not bridge into the ground rail.

---

### Fault 4: Pressing Trap Key (W1–W4) Does Not Reset Lock
- **Likely Cause 1: Trap switches not wired to Pin 4.**
  - *Check:* Measure voltage at Pin 4 while pressing each $W$ key individually. The voltage must drop from $5.0\text{V}$ down to $0.0\text{V}$ on every button press.
- **Likely Cause 2: Pull-up resistor $R_2$ is too small.**
  - *Check:* If $R_2$ was mistakenly installed as $100\ \Omega$ instead of $10\text{ k}\Omega$, switch contact resistance might fail to pull Pin 4 below $0.7\text{V}$. Confirm $R_2 = 10\text{ k}\Omega$ (Brown-Black-Orange).

---

### Fault 5: False Triggering When Other Appliances Turn ON
- **Likely Cause: Missing power supply decoupling capacitors.**
  - *Explanation:* The NE555 has a bipolar output stage that momentarily draws a large current spike (up to $300\text{ mA}$ shoot-through) when switching states. Without decoupling, this causes a voltage sag that can re-trigger Pin 2.
  - *Fix:*
    1. Place a **$100\text{ nF}$ ceramic capacitor ($C_3$)** directly between Pin 8 and Pin 1.
    2. Ensure **$10\text{ nF}$ capacitor ($C_2$)** is connected between Pin 5 and GND.
