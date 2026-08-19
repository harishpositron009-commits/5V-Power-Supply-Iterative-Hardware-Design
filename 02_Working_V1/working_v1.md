# 02 — Building the First Working V1

## Objective

With the problem and functional architecture defined, the next step was to give the proposal life by converting each functional block into an actual electrical component.

The objective at this stage was not maximum efficiency or optimization.

The objective was to build the **first living specimen** — a circuit capable of producing the required output and providing a physical and electrical reference for further analysis.

The functional architecture developed in the previous stage was:

230 V AC → Voltage Reduction → Rectification → Filtering → Regulation → 5 V DC

This now had to be translated into:

230 V AC → Transformer → Bridge Rectifier → Filter Capacitor → Voltage Regulator → 5 V DC

---

# Establishing the Design Conditions

Before selecting components, some basic operating conditions had to be established.

### Input

**230 V AC, 50 Hz**

### Required Output

**5 V regulated DC**

### Initial Design Load

For the first version, the circuit was analyzed around a maximum load current of approximately:

**500 mA**

This value provides a reference for component selection, power-loss calculations, ripple estimation, and thermal analysis.

The first design can therefore be considered around:

**5 V × 0.5 A = 2.5 W maximum DC output**

---

# Stage 1 — Selecting the Transformer

The first task is to reduce the 230 V mains voltage to a level suitable for rectification and regulation.

A:

**230 V AC → 12 V AC step-down transformer**

was selected.

The transformer provides two important functions:

1. Voltage reduction
2. Galvanic isolation between the mains supply and the low-voltage circuit

The 12 V secondary provides sufficient voltage headroom for the bridge rectifier, filter capacitor, and 7805 regulator.

However, an important realization was that:

> **12 V AC does not become 12 V DC after rectification and filtering.**

The transformer rating is specified in RMS voltage.

The approximate peak voltage is:

V_peak = V_RMS × √2

Therefore:

V_peak = 12 × 1.414

V_peak ≈ 16.97 V

This becomes important later when analyzing regulator power dissipation.

---

# Stage 2 — Selecting the Rectifier

The reduced AC voltage must next be converted into DC.

A **full-wave bridge rectifier** was selected using four diodes.

### Selected Diode

**1N4007**

During each half-cycle, two diodes in the bridge conduct.

Assuming an approximate forward voltage drop of:

**0.7 V per diode**

the total bridge drop is approximately:

2 × 0.7 = 1.4 V

The approximate peak voltage after the bridge therefore becomes:

16.97 - 1.4 ≈ 15.57 V

This is still pulsating DC and therefore requires filtering before being supplied to the regulator.

---

# Stage 3 — Selecting the Filter Capacitor

The output of the bridge rectifier is not smooth DC.

A capacitor is therefore placed across the rectifier output.

The capacitor charges near the peaks of the rectified waveform and supplies energy to the load while the rectified voltage falls between peaks.

For a full-wave rectifier operating from a 50 Hz supply:

**Ripple frequency = 2 × mains frequency**

Therefore:

**f_ripple = 100 Hz**

The approximate ripple relationship is:

ΔV ≈ I / (f × C)

For the initial design:

- I = 0.5 A
- f = 100 Hz
- C = 1000 µF = 0.001 F

Therefore:

ΔV ≈ 0.5 / (100 × 0.001)

ΔV ≈ 5 V

This is a relatively large theoretical ripple.

However, the purpose of V1 is not to immediately eliminate every weakness.

The **1000 µF capacitor was retained as the first working reference** so that its behavior could later be analyzed through theory and simulation.

This is an important part of the methodology:

> A component should not automatically be made larger simply because a larger value appears better.

Increasing capacitance may reduce ripple, but it can also increase:

- Physical size
- Cost
- Stored energy
- Charging current
- Inrush current
- Rectifier stress
- Transformer stress

The appropriate value therefore depends on the requirements and acceptable trade-offs of the system.

---

# Stage 4 — Selecting the Voltage Regulator

The final stage must provide approximately:

**5 V DC**

A **7805 linear voltage regulator** was selected for the first implementation.

The 7805 provides a simple way of obtaining a regulated 5 V output from a sufficiently high DC input.

Its simplicity makes it useful for the first living specimen because the regulator behavior can be understood using relatively straightforward electrical relationships.

However, the simplicity comes with an important disadvantage.

A linear regulator dissipates the voltage difference between its input and output as heat.

The approximate regulator loss is:

P_loss = (V_in - V_out) × I_load

For example, if:

- V_in ≈ 16 V
- V_out = 5 V
- I_load = 0.5 A

then:

P_loss = (16 - 5) × 0.5

P_loss ≈ 5.5 W

This immediately reveals one of the major weaknesses of the V1 architecture.

The regulator may successfully produce 5 V, but a significant amount of energy can be converted into heat.

This problem will be analyzed in greater detail in the **Performance Analysis** stage.

---

# Stage 5 — Output Indication

An LED was included to provide a simple visual indication that the regulated output is present.

A current-limiting resistor is required in series with the LED.

For an approximate:

- Supply voltage = 5 V
- LED forward voltage ≈ 2 V
- Desired LED current ≈ 10 mA

the resistor can be estimated using Ohm's law:

R = (V_supply - V_LED) / I_LED

R = (5 - 2) / 0.01

R ≈ 300 Ω

A nearby standard resistor value of:

**330 Ω**

was selected.

---

# V1 Component Selection

The first living specimen therefore uses:

| Function | Selected Component |
|---|---|
| Voltage reduction | 230 V / 12 V step-down transformer |
| Rectification | 4 × 1N4007 diodes |
| Filtering | 1000 µF electrolytic capacitor |
| Regulation | 7805 linear regulator |
| Output indication | LED + 330 Ω resistor |
| Output | 5 V DC |

The architecture becomes:

230 V AC
   ↓
230/12 V Transformer
   ↓
4 × 1N4007 Bridge Rectifier
   ↓
1000 µF Filter Capacitor
   ↓
7805 Linear Regulator
   ↓
5 V DC Output

---

# What Have We Built?

At this point, the circuit has moved from an idea into a complete first-pass electrical architecture.

But this does **not** mean the design is finished.

In fact, several weaknesses are already visible:

- Large theoretical ripple
- Significant voltage across the 7805
- Potentially high regulator power dissipation
- Thermal limitations
- Rectifier losses
- Transformer losses
- Inrush current
- Component-rating considerations
- Overall efficiency concerns

This is exactly why the first living specimen is useful.

Before V1 existed, these were mostly abstract design questions.

Now they can be associated with an actual circuit.

> **The imperfections of V1 become the inputs for V2.**

---

# Moving to the Next Stage

The first functional design has now been established.

The next question is no longer:

> **Can this architecture produce 5 V?**

The next question becomes:

> **What is actually happening to the power as it moves through the system?**

The next stage therefore investigates:

- Transformer losses
- Rectifier losses
- Ripple
- Capacitor behavior
- Regulator losses
- Thermal behavior
- Wiring losses
- Overall efficiency
- Component stress

## → 03 — Performance Analysis

The goal is to understand the weaknesses of the first living specimen before deciding what should be changed.
