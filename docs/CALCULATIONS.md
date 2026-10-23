# Circuit Calculations & Design Formulas

This document details the mathematical models, component tolerance bounds, timing customizer tables, and power budget calculations for the **4-Key Combination Lock Using 555 Timer**.

---

## ⏱️ Monostable Timing Equation

The output pulse width $T$ of an NE555 timer configured in monostable multivibrator mode is determined by the RC charging network on Pins 6 (THRESHOLD) and 7 (DISCHARGE):

$$T = R_3 \cdot C_1 \cdot \ln(3) \approx 1.09861 \times R_3 \times C_1 \approx 1.1 \times R_3 \times C_1$$

Where:
- $R_3$: Timing resistor in Ohms ($\Omega$)
- $C_1$: Timing capacitor in Farads ($\text{F}$)
- $T$: Output pulse duration in seconds ($\text{s}$)

---

## 📊 Nominal Value & Tolerance Analysis

In real-world breadboard circuits, passive components have manufacturing tolerances. Aluminum electrolytic capacitors typically carry a tolerance of $\pm 20\%$, while standard carbon-film resistors carry $\pm 5\%$ (or $\pm 1\%$ for metal-film).

### Nominal Parameters:
- $R_{3(\text{nom})} = 47\text{ k}\Omega = 4.7 \times 10^4\ \Omega$
- $C_{1(\text{nom})} = 100\ \mu\text{F} = 1.0 \times 10^{-4}\text{ F}$

$$T_{\text{nom}} = 1.098612 \times 47{,}000 \times (100 \times 10^{-6}) = \mathbf{5.163\text{ seconds}} \approx \mathbf{5.17\text{ s}}$$

### Extreme Bound Calculations:

| Parameter | Resistor $R_3$ ($\pm 5\%$) | Capacitor $C_1$ ($\pm 20\%$) | Calculated Duration $T$ | Variance |
| :--- | :--- | :--- | :--- | :--- |
| **Minimum ($T_{\text{min}}$)** | $44.65\text{ k}\Omega$ ($-5\%$) | $80.0\ \mu\text{F}$ ($-20\%$) | **$3.92\text{ seconds}$** | $-24.0\%$ |
| **Nominal ($T_{\text{nom}}$)** | $47.00\text{ k}\Omega$ | $100.0\ \mu\text{F}$ | **$5.16\text{ seconds}$** | $0.0\%$ |
| **Maximum ($T_{\text{max}}$)** | $49.35\text{ k}\Omega$ ($+5\%$) | $120.0\ \mu\text{F}$ ($+20\%$) | **$6.51\text{ seconds}$** | $+26.0\%$ |

> [!NOTE]
> Physical measurements on breadboard prototypes typically yield between **4.8s and 5.5s** due to electrolytic capacitor aging, dielectric absorption, and minor leakage currents.

---

## 🎛️ Timing Customization Lookup Table

To change how long the lock stays unlocked, adjust $R_3$ and $C_1$ using standard E12/E24 values:

| Desired Unlock Time | Recommended Resistor ($R_3$) | Recommended Capacitor ($C_1$) | Theoretical Duration ($T$) |
| :---: | :---: | :---: | :---: |
| **$1.0\text{ s}$** | $10\text{ k}\Omega$ | $100\ \mu\text{F}$ | $1.10\text{ s}$ |
| **$2.5\text{ s}$** | $22\text{ k}\Omega$ | $100\ \mu\text{F}$ | $2.42\text{ s}$ |
| **$5.2\text{ s}$ (Current)** | **$47\text{ k}\Omega$** | **$100\ \mu\text{F}$** | **$5.16\text{ s}$** |
| **$10.0\text{ s}$** | $91\text{ k}\Omega$ | $100\ \mu\text{F}$ | $10.00\text{ s}$ |
| **$15.0\text{ s}$** | $68\text{ k}\Omega$ | $220\ \mu\text{F}$ | $16.46\text{ s}$ |
| **$30.0\text{ s}$** | $130\text{ k}\Omega$ | $220\ \mu\text{F}$ | $31.42\text{ s}$ |
| **$60.0\text{ s}$** | $120\text{ k}\Omega$ | $470\ \mu\text{F}$ | $62.06\text{ s}$ |

---

## 🔋 Power & Current Consumption

Powered from a regulated $5.0\text{V}$ rail:

### 1. Standby State (Locked):
- NE555 Quiescent Supply Current ($I_{CC}$): $\approx 3.0\text{ mA} - 5.0\text{ mA}$ (Bipolar NE555) / $\approx 100\ \mu\text{A}$ (TLC555 CMOS)
- Pull-up Resistors ($R_1, R_2$): Since switches are open, zero static current flows through the switches.
- Indicator LED: OFF ($0\text{ mA}$).
- **Total Standby Power:** $P_{\text{standby}} = 5\text{V} \times 4\text{ mA} \approx \mathbf{20\text{ mW}}$.

### 2. Active State (Unlocked Pulse):
- NE555 Supply Current: $\approx 5.0\text{ mA}$
- LED Driver Current ($I_{\text{LED}}$):
  $$I_{\text{LED}} = \frac{V_{OH} - V_F}{R_4} \approx \frac{3.7\text{V} - 2.0\text{V}}{330\ \Omega} \approx 5.15\text{ mA}$$
- RC Charging Current ($I_{RC}$ peak at $t=0$):
  $$I_{RC(\text{peak})} = \frac{5.0\text{V}}{47\text{ k}\Omega} \approx 106\ \mu\text{A}$$
- **Total Active Current:** $\approx 10.3\text{ mA}$
- **Total Active Power:** $P_{\text{active}} \approx 5\text{V} \times 10.3\text{ mA} \approx \mathbf{51.5\text{ mW}}$.
