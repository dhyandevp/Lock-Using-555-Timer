# Breadboard Construction & Assembly Guide

This guide walks through assembling the **4-Key Combination Lock Using 555 Timer** step-by-step on a standard 830-point solderless breadboard.

---

## 📸 Completed Breadboard Reference

![Breadboard Top View](../assets/images/breadboard_top_view.jpg)
*Figure 1: Top-down view of verified breadboard layout showing IC placement, series combination keys, and parallel trap keys.*

![Breadboard Perspective View](../assets/images/breadboard_angled_view.jpg)
*Figure 2: Perspective view illustrating breadboard rails, tactile pushbutton orientations, and timing capacitor.*

---

## 🗺️ Architectural Breadboard Wiring Map

![Breadboard Wiring Map](./schematics/breadboard_wiring_map.svg)

*(Vector map available at [`docs/schematics/breadboard_wiring_map.svg`](./schematics/breadboard_wiring_map.svg))*

---

## 📍 Row-by-Row Breadboard Coordinate Table (MB-102 Standard)

Follow this exact connection map row-by-row on any standard 830-point solderless breadboard:

| Row / Location | From Socket | To Socket | Component / Jumper Wire | Functional Description |
| :--- | :--- | :--- | :--- | :--- |
| **Power Distribution** | Top Red Rail (+) Left | Top Red Rail (+) Right | Red Jumper Wire | Bridges split power rail across row 30 break |
| **Power Distribution** | Top Blue Rail (-) Left | Top Blue Rail (-) Right | Blue Jumper Wire | Bridges split ground rail across row 30 break |
| **Power Distribution** | Top Red Rail (+) | Bottom Red Rail (+) | Red Jumper Wire | Distributes +5V to both sides of breadboard |
| **Power Distribution** | Top Blue Rail (-) | Bottom Blue Rail (-) | Blue Jumper Wire | Distributes common GND to both sides |
| **Rail Decoupling** | Top Red Rail (+) | Top Blue Rail (-) | $C_3$ ($100\text{ nF}$ Ceramic, code `104`) | High-frequency rail decoupling across supply |
| **Row 10 (IC Ground)** | Row 10, Col E | — | NE555 IC Pin 1 | IC Ground terminal |
| **Row 10** | Row 10, Col D | Top Blue Rail (-) | Black/Blue Jumper Wire | Connects Pin 1 to Ground |
| **Row 10 (IC Power)** | Row 10, Col F | — | NE555 IC Pin 8 | IC +5V ($V_{CC}$) supply terminal |
| **Row 10** | Row 10, Col G | Bottom Red Rail (+) | Red Jumper Wire | Connects Pin 8 to +5V |
| **Row 11 (Trigger)** | Row 11, Col E | — | NE555 IC Pin 2 | Trigger sensing input |
| **Row 11** | Top Red Rail (+) | Row 11, Col D | $R_1$ ($10\text{ k}\Omega$, Brown-Black-Orange) | Pull-up resistor maintaining Pin 2 at +5V |
| **Row 11** | Row 11, Col C | Row 24, Col C | Yellow Jumper Wire | Routes Trigger node to Switch $K_1$ input |
| **Row 11 (Discharge)**| Row 11, Col F | — | NE555 IC Pin 7 | Internal discharge transistor collector |
| **Row 12 (Threshold)**| Row 12, Col F | — | NE555 IC Pin 6 | Upper comparator threshold input |
| **Row 11–12 (Tie)** | Row 11, Col G | Row 12, Col G | Short Jumper Wire | Ties Pin 6 and Pin 7 together |
| **Row 12** | Bottom Red Rail (+) | Row 12, Col H | $R_3$ ($47\text{ k}\Omega$, Yellow-Violet-Orange) | Timing resistor charging $C_1$ ($T \approx 1.1 R_3 C_1$) |
| **Row 12** | Row 12, Col I | Bottom Blue Rail (-) | $C_1$ ($100\ \mu\text{F}$ Electrolytic) | Timing capacitor (Long lead to Row 12, stripe to GND) |
| **Row 12 (Output)** | Row 12, Col E | — | NE555 IC Pin 3 | Push-pull totem pole output |
| **Row 12** | Row 12, Col D | Row 14, Col D | $R_4$ ($330\ \Omega$, Orange-Orange-Brown) | Current-limiting resistor for indicator LED |
| **Row 14** | Row 14, Col C | Top Blue Rail (-) | Indicator LED (Cyan/Blue) | Anode (long leg) at Row 14, Cathode (flat rim) to GND |
| **Row 13 (Control)** | Row 13, Col F | — | NE555 IC Pin 5 | Internal $\frac{2}{3} V_{CC}$ ladder tap |
| **Row 13** | Row 13, Col H | Bottom Blue Rail (-) | $C_2$ ($10\text{ nF}$ Ceramic, code `103`) | Decoupling capacitor filtering Pin 5 to GND |
| **Row 13 (Reset)** | Row 13, Col E | — | NE555 IC Pin 4 | Active-low Master Reset input |
| **Row 13** | Top Red Rail (+) | Row 13, Col D | $R_2$ ($10\text{ k}\Omega$, Brown-Black-Orange) | Pull-up resistor maintaining Pin 4 at +5V |
| **Row 13** | Row 13, Col C | Row 42, Col F | Orange/White Jumper Wire | Routes Reset line to Trap Key Bus ($W_1–W_4$) |
| **Row 24–26 (Key 1)** | Row 24, Col C & E | Row 26, Col C & E | Tactile Switch $K_1$ (SPST) | Combination Key 1 (Wired diagonally: 24C to 26E) |
| **Row 26–28** | Row 26, Col E | Row 28, Col C | Jumper Wire | Series link from $K_1$ output to $K_2$ input |
| **Row 28–30 (Key 2)** | Row 28, Col C & E | Row 30, Col C & E | Tactile Switch $K_2$ (SPST) | Combination Key 2 (Wired diagonally: 28C to 30E) |
| **Row 30–32** | Row 30, Col E | Row 32, Col C | Jumper Wire | Series link from $K_2$ output to $K_3$ input |
| **Row 32–34 (Key 3)** | Row 32, Col C & E | Row 34, Col C & E | Tactile Switch $K_3$ (SPST) | Combination Key 3 (Wired diagonally: 32C to 34E) |
| **Row 34–36** | Row 34, Col E | Row 36, Col C | Jumper Wire | Series link from $K_3$ output to $K_4$ input |
| **Row 36–38 (Key 4)** | Row 36, Col C & E | Row 38, Col C & E | Tactile Switch $K_4$ (SPST) | Combination Key 4 (Wired diagonally: 36C to 38E) |
| **Row 38** | Row 38, Col E | Top Blue Rail (-) | Blue/Black Jumper Wire | Final Ground return (**Only $K_4$ connects to GND!**) |
| **Row 42–44 (Trap 1)**| Row 42, Col F & H | Row 44, Col F & H | Tactile Switch $W_1$ (SPST) | Trap Key 1 (Wired diagonally: 42F to 44H) |
| **Row 44** | Row 44, Col H | Bottom Blue Rail (-) | Blue Jumper Wire | Sinks $W_1$ to Ground when pressed |
| **Row 42–46 (Bus)** | Row 42, Col F | Row 46, Col F | Jumper Wire | Extends Pin 4 Reset Bus to Trap Key $W_2$ |
| **Row 46–48 (Trap 2)**| Row 46, Col F & H | Row 48, Col F & H | Tactile Switch $W_2$ (SPST) | Trap Key 2 (Wired diagonally: 46F to 48H) |
| **Row 48** | Row 48, Col H | Bottom Blue Rail (-) | Blue Jumper Wire | Sinks $W_2$ to Ground when pressed |
| **Row 46–50 (Bus)** | Row 46, Col F | Row 50, Col F | Jumper Wire | Extends Pin 4 Reset Bus to Trap Key $W_3$ |
| **Row 50–52 (Trap 3)**| Row 50, Col F & H | Row 52, Col F & H | Tactile Switch $W_3$ (SPST) | Trap Key 3 (Wired diagonally: 50F to 52H) |
| **Row 52** | Row 52, Col H | Bottom Blue Rail (-) | Blue Jumper Wire | Sinks $W_3$ to Ground when pressed |
| **Row 50–54 (Bus)** | Row 50, Col F | Row 54, Col F | Jumper Wire | Extends Pin 4 Reset Bus to Trap Key $W_4$ |
| **Row 54–56 (Trap 4)**| Row 54, Col F & H | Row 56, Col F & H | Tactile Switch $W_4$ (SPST) | Trap Key 4 (Wired diagonally: 54F to 56H) |
| **Row 56** | Row 56, Col H | Bottom Blue Rail (-) | Blue Jumper Wire | Sinks $W_4$ to Ground when pressed |

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
