# Bill of Materials (BOM)

Complete list of all electronic components, hardware, and tools required to build the **4-Key Combination Lock Using 555 Timer**.

---

## 📋 Component List

| Item | Designator | Component Description | Value / Specification | Package | Qty | Function & Purpose | Recommended Part # / Equivalents |
| :---: | :--- | :--- | :--- | :--- | :---: | :--- | :--- |
| **1** | **U1** | 555 Timer IC | General Purpose Bipolar or CMOS Timer | DIP-8 | 1 | Monostable pulse generation & logic control | NE555P, LM555CN, HLF555, TLC555 (CMOS), ICM7555 |
| **2** | **R1, R2** | Carbon Film / Metal Film Resistor | $10\text{ k}\Omega \pm 5\%$, $0.25\text{ W}$ | Axial Through-Hole | 2 | Pull-up resistors for Pin 2 (TRIG) and Pin 4 (RESET) | Standard 1/4W resistor (Brown-Black-Orange-Gold) |
| **3** | **R3** | Metal Film Resistor | $47\text{ k}\Omega \pm 1\% \text{ or } 5\%$, $0.25\text{ W}$ | Axial Through-Hole | 1 | Monostable timing resistor ($T \approx 1.1 R_3 C_1$) | Standard 1/4W resistor (Yellow-Violet-Orange-Gold) |
| **4** | **R4** | Carbon Film Resistor | $330\ \Omega \pm 5\%$, $0.25\text{ W}$ | Axial Through-Hole | 1 | Current-limiting resistor for indicator LED (~5 mA) | Standard 1/4W resistor (Orange-Orange-Brown-Gold) |
| **5** | **C1** | Aluminum Electrolytic Capacitor | $100\ \mu\text{F} \pm 20\%$, $16\text{V} - 25\text{V}$ | Radial Through-Hole | 1 | Timing capacitor defining $\approx 5.2\text{ s}$ pulse width | Nichicon / Panasonic / generic radial electrolytic |
| **6** | **C2** | Multilayer Ceramic Capacitor (MLCC) | $10\text{ nF}$ ($0.01\ \mu\text{F}$), marked `103` | Radial Disc / MLCC | 1 | Control voltage decoupling (Pin 5 to GND) | 50V ceramic disc capacitor |
| **7** | **C3** | Multilayer Ceramic Capacitor (MLCC) | $100\text{ nF}$ ($0.1\ \mu\text{F}$), marked `104` | Radial Disc / MLCC | 1 | Power supply rail decoupling across $+5\text{V}$ and GND | 50V ceramic disc capacitor |
| **8** | **K1–K4** | Tactile Pushbutton Switches | Momentary SPST, $12\text{V} / 50\text{ mA}$ | $6\times 6\text{ mm}$ DIP-4 | 4 | 4-Key Series Combination input | Omron B3F-1000, standard 6x6 tactile switch |
| **9** | **W1–W4** | Tactile Pushbutton Switches | Momentary SPST, $12\text{V} / 50\text{ mA}$ | $6\times 6\text{ mm}$ DIP-4 | 4 | 4-Key Parallel Reset / Trap decoy input | Omron B3F-1000, standard 6x6 tactile switch |
| **10** | **LED1** | Light Emitting Diode | $5\text{ mm}$ Diffused / Water Clear (Cyan/Blue/Green) | Radial $5\text{ mm}$ | 1 | Output lock state indicator | Standard $5\text{ mm}$ T-1 3/4 LED ($V_F \approx 2.0\text{V} - 3.0\text{V}$) |
| **11** | **BB1** | Solderless Breadboard | 830 Tie Points with dual power rails | Solderless | 1 | Prototyping chassis | MB-102 standard 830-point breadboard |
| **12** | **JW1** | Solderless Jumper Wires | 22 AWG solid core or flexible male-to-male | Various lengths | ~25 | Interconnection wires | Standard breadboard jumper wire kit |
| **13** | **PWR** | 5V DC Power Supply | $5\text{V} \pm 5\%$, $\ge 500\text{ mA}$ | USB-A breakout or bench supply | 1 | System power supply | USB cable stripped or breadboard power module |

---

## 🧰 Required Tools & Equipment

- **Digital Multimeter (DMM):** For verifying continuity, measuring resistor values, and checking test-point voltages.
- **Wire Stripper / Needle-Nose Pliers:** For bending component leads neatly into breadboard sockets.
- **5V DC Regulated Source:** USB 5V adapter, breadboard power module, or bench power supply.
