# Breadboard Construction & Assembly Guide

This guide walks through assembling the **4-Key Combination Lock Using 555 Timer** step-by-step on a standard 830-point solderless breadboard.

---

## 📸 Completed Breadboard Reference

![Breadboard Top View](../assets/images/breadboard_top_view.jpg)
*Figure 1: Top-down view of verified breadboard layout showing IC placement, series combination keys, and parallel trap keys.*

![Breadboard Perspective View](../assets/images/breadboard_angled_view.jpg)
*Figure 2: Perspective view illustrating breadboard rails, tactile pushbutton orientations, and timing capacitor.*

---

## ⚠️ Critical Rule 1: Tactile Switch Bridging

Standard 4-pin tactile pushbuttons (`6x6 mm` DIP) can be confusing for beginners because they have 4 legs but only perform single-pole single-throw (SPST) switching.

![Tactile Switch Pinout](./schematics/555_pinout.svg)

### The Internal Short Danger:
- Pins on the same horizontal edge are **permanently shorted together internally** inside the plastic housing!
- If you wire across a permanently shorted pair, the switch will act as a solid wire, permanently pulling the pin to Ground or bridging two circuits even when the button is NOT pressed.

### Safe Diagonal Wiring Rule:
Always wire jumper leads to **diagonally opposing pins** (e.g., top-left pin $A_1$ and bottom-right pin $B_2$):
1. Test continuity with a digital multimeter in beep mode before pushing buttons into the breadboard.
2. Orient the switch so that the curved lead pairs span the breadboard center trough or separate row groups.
3. Connecting diagonally guarantees 0 continuity when unpressed, and a solid low-resistance contact ($< 0.1\ \Omega$) only when pressed.

---

## ⚠️ Critical Rule 2: True Series Continuity for Combination Keys (K1–K4)

For the combination entry keys ($K_1, K_2, K_3, K_4$):
- **Only the final switch ($K_4$) must touch the Ground (GND) rail.**
- Switch $K_1$ input connects to Pin 2 (TRIGGER) and $R_1$ pull-up.
- Switch $K_1$ output connects strictly to Switch $K_2$ input.
- Switch $K_2$ output connects strictly to Switch $K_3$ input.
- Switch $K_3$ output connects strictly to Switch $K_4$ input.
- Switch $K_4$ output connects to the common Blue Ground Rail.

> [!CAUTION]
> If any intermediate switch ($K_1, K_2,$ or $K_3$) accidentally connects to the GND rail, any partial combination will prematurely trigger the lock! Verify with a multimeter that Pin 2 only reads $0\text{V}$ when **all four keys** are pressed simultaneously.

---

## ⚠️ Critical Rule 3: Parallel Trap Key Wiring (W1–W4)

For the wrong / decoy trap keys ($W_1, W_2, W_3, W_4$):
- All 4 switches connect in **parallel** between Pin 4 (RESET) and Ground.
- Input terminal of each trap switch connects to the Pin 4 reset bus node.
- Output terminal of each trap switch connects to the Blue Ground Rail.
- Pressing **any single trap key** pulls Pin 4 to $0\text{V}$, instantly aborting the timer pulse.

---

## 🛠️ Step-by-Step Assembly Instructions

### Step 1: Power Rail Setup
1. Use an 830-point breadboard with top and bottom power distribution rails.
2. Bridge the top and bottom Red rails ($+5\text{V}$) using red jumper wires.
3. Bridge the top and bottom Blue rails ($\text{GND}$) using blue/black jumper wires.
4. Place a **$100\text{ nF}$ ceramic capacitor ($C_3$, code `104`)** directly across $+5\text{V}$ and $\text{GND}$ near the IC power pins to absorb switching noise.

### Step 2: 555 IC Placement
1. Straddle the NE555 timer IC across the central divider trough (rows 15–18 typical).
2. Ensure the semi-circular notch faces **LEFT** (or UP depending on breadboard orientation):
   - Pins 1 to 4 are on the lower half: Pin 1 (GND), Pin 2 (TRIG), Pin 3 (OUT), Pin 4 (RESET).
   - Pins 5 to 8 are on the upper half: Pin 5 (CTRL), Pin 6 (THRESH), Pin 7 (DISCH), Pin 8 (VCC).
3. Connect **Pin 1** directly to the Blue Rail ($\text{GND}$).
4. Connect **Pin 8** directly to the Red Rail ($+5\text{V}$).

### Step 3: Timing Network (Pins 6 & 7)
1. Insert a short jumper wire bridging **Pin 6 (Threshold)** and **Pin 7 (Discharge)** together.
2. Connect a **$47\text{ k}\Omega$ resistor ($R_3$, Yellow-Violet-Orange-Gold)** from the $+5\text{V}$ Red rail to the Pin 6/7 node.
3. Connect a **$100\ \mu\text{F}$ electrolytic capacitor ($C_1$)**:
   - Long lead (**Anode, $+$**) connects to the Pin 6/7 node.
   - Short lead with stripe (**Cathode, $-$**) connects to the Blue Rail ($\text{GND}$).

### Step 4: Control Voltage Bypass (Pin 5)
1. Insert a **$10\text{ nF}$ ceramic capacitor ($C_2$, code `103`)** from **Pin 5** to the Blue Rail ($\text{GND}$).
2. This stabilizes the internal comparator reference voltage against power rail fluctuations.

### Step 5: Output Indicator (Pin 3)
1. Connect a **$330\ \Omega$ resistor ($R_4$, Orange-Orange-Brown-Gold)** from **Pin 3** to an unused breadboard tie point.
2. Insert an LED:
   - Long lead (**Anode, $+$**) connects to the resistor.
   - Short lead with flat rim (**Cathode, $-$**) connects to the Blue Rail ($\text{GND}$).

### Step 6: Trigger Subsystem & Combination Keys (Pin 2)
1. Insert a **$10\text{ k}\Omega$ pull-up resistor ($R_1$, Brown-Black-Orange-Gold)** from the Red Rail ($+5\text{V}$) to **Pin 2**.
2. Mount tactile switches $K_1, K_2, K_3, K_4$ in a neat row.
3. Route a jumper wire from Pin 2 to switch $K_1$'s input pin.
4. Daisy-chain $K_1$ output $\rightarrow$ $K_2$ input $\rightarrow$ $K_2$ output $\rightarrow$ $K_3$ input $\rightarrow$ $K_3$ output $\rightarrow$ $K_4$ input.
5. Connect $K_4$ output to the Blue Rail ($\text{GND}$).

### Step 7: Trap Subsystem & Decoy Keys (Pin 4)
1. Insert a **$10\text{ k}\Omega$ pull-up resistor ($R_2$, Brown-Black-Orange-Gold)** from the Red Rail ($+5\text{V}$) to **Pin 4**.
2. Mount tactile switches $W_1, W_2, W_3, W_4$ in an adjacent row or interleaved amongst the combination keys.
3. Connect the input pins of all 4 switches to a common line tied to **Pin 4**.
4. Connect the output pins of all 4 switches to the Blue Rail ($\text{GND}$).

---

## 🎨 Recommended Jumper Wire Color Scheme

| Color | Recommended Signal / Rail |
| :--- | :--- |
| **Red** | Power Rail ($+5\text{V}$ DC) |
| **Blue / Black** | Ground Rail ($0\text{V}$ / GND) |
| **Yellow** | Series Combination Key Daisy-Chain |
| **Orange / White** | Trap Key (Pin 4 Reset) Bus |
| **Green** | Output Signal (Pin 3 to LED) |
| **Brown** | Timing Node (Pins 6 & 7 to RC) |
