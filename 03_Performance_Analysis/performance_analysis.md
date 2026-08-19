# 03 — Performance Analysis

## Objective

The first living specimen established that the proposed architecture could theoretically provide the required 5 V output.

However, functionality alone does not determine whether a design is good.

The next step is therefore to examine the system and ask:

> **Where is energy being lost, what limitations exist, and which of those limitations are acceptable for this application?**

The V1 architecture being analyzed is:

230 V AC  
↓  
230/12 V Transformer  
↓  
Bridge Rectifier  
↓  
1000 µF Filter Capacitor  
↓  
7805 Linear Regulator  
↓  
5 V DC Output

The purpose of this analysis is not to immediately eliminate every loss.

It is to understand them.

---

# 1 — Transformer

The transformer performs two essential functions:

- Voltage reduction
- Galvanic isolation

However, a real transformer is not ideal.

Loss mechanisms include:

- Copper losses in the windings
- Core losses
- Leakage flux
- Internal resistance
- Voltage regulation under load

This means the transformer secondary voltage will not remain perfectly constant under all operating conditions.

The transformer must therefore be considered as a real electrical component rather than an ideal 230/12 V conversion block.

---

# 2 — Bridge Rectifier Losses

A full-wave bridge rectifier uses four diodes, with two conducting during each half-cycle.

For silicon diodes, an approximate forward drop of:

0.7 V per diode

gives:

Total conducting drop ≈ 1.4 V

This voltage drop produces power loss in the rectifier.

The loss depends on the actual current waveform through the diodes.

An important observation is that the bridge current is not necessarily a smooth constant current.

When a filter capacitor is present, the diodes conduct strongly during portions of the waveform when the rectified source voltage exceeds the capacitor voltage.

This creates charging-current pulses that must be considered when selecting diode and transformer ratings.

---

# 3 — Filter Capacitor & Ripple

The 1000 µF capacitor stores energy near the peaks of the rectified waveform and supplies the load between those peaks.

For a full-wave rectifier connected to a 50 Hz supply:

f_ripple = 100 Hz

The approximate ripple relationship is:

ΔV ≈ I / (f × C)

Using the initial V1 design assumption:

I = 0.5 A  
f = 100 Hz  
C = 1000 µF = 0.001 F

Therefore:

ΔV ≈ 0.5 / (100 × 0.001)

ΔV ≈ 5 V

This represents a relatively large theoretical ripple.

Increasing capacitance could reduce ripple, but doing so introduces other trade-offs:

- Larger physical size
- Higher cost
- Increased stored energy
- Increased charging current
- Higher startup/inrush current
- Greater rectifier stress
- Greater transformer stress

Therefore:

> **Lower ripple is desirable, but increasing capacitance indefinitely is not automatically a better design.**

The appropriate capacitance depends on the acceptable ripple and the constraints of the system.

---

# 4 — 7805 Regulator Loss

The 7805 is one of the most important sources of inefficiency in the V1 architecture.

A linear regulator approximately dissipates:

P_loss = (V_in - V_out) × I_load

If the regulator receives approximately 16 V and supplies:

5 V at 0.5 A

then:

P_loss = (16 - 5) × 0.5

P_loss ≈ 5.5 W

Meanwhile, the useful output power is:

P_out = V_out × I_out

P_out = 5 × 0.5

P_out = 2.5 W

This reveals a fundamental limitation of the architecture.

The regulator can successfully produce 5 V while simultaneously dissipating more power as heat than it supplies to the load.

---

# 5 — Thermal Behaviour

Electrical losses eventually appear primarily as heat.

The 7805 therefore requires thermal consideration.

Factors affecting regulator temperature include:

- Input voltage
- Load current
- Ambient temperature
- Package thermal resistance
- PCB copper area
- Airflow
- Heatsinking

A circuit that produces the correct voltage but operates beyond the safe thermal limits of its components cannot be considered a reliable design.

Thermal behaviour therefore becomes part of electrical design rather than a separate problem.

---

# 6 — Wiring & PCB Resistance

Real conductors are not ideal.

Wires and PCB traces have resistance.

The approximate relationship is:

R = ρL / A

where:

- ρ = conductor resistivity
- L = conductor length
- A = conductor cross-sectional area

Therefore:

Longer conductor → Higher resistance

Smaller cross-sectional area → Higher resistance

The associated power loss is:

P = I²R

For a low-power prototype these losses may be relatively small compared with regulator losses, but they still influence PCB decisions such as:

- Trace width
- Trace length
- Current routing
- Ground-return paths

---

# 7 — Inrush Current

The filter capacitor initially behaves differently during startup because it begins in an uncharged state.

When power is first applied, the capacitor can draw a relatively large charging current.

The magnitude depends on:

- Transformer impedance
- Rectifier impedance
- Capacitor value
- Source impedance
- Wiring resistance
- Point on the AC waveform at switch-on

This is another example of why increasing filter capacitance cannot be treated as a free improvement.

A larger capacitor may reduce steady-state ripple while increasing startup stress.

---

# 8 — Efficiency

The useful output power for the initial 5 V, 0.5 A design condition is:

P_out = 5 × 0.5

P_out = 2.5 W

However, power is lost throughout the system through:

- Transformer losses
- Rectifier losses
- Regulator dissipation
- Capacitor ESR
- Wiring resistance
- PCB resistance

The overall efficiency is:

η = P_out / P_in × 100%

The exact efficiency of the physical system should ultimately be determined using measured input and output power.

At the V1 analysis stage, theoretical and simulated values are used to identify where the dominant losses are likely to occur.

The analysis clearly indicates that the linear regulator is a major source of loss.

---

# What Does This Tell Us?

The first living specimen can perform its required function:

**230 V AC → 5 V DC**

But it also contains several weaknesses:

- Significant regulator dissipation
- Thermal stress
- Rectifier losses
- Transformer losses
- Ripple
- Inrush current
- Conductor losses
- Limited overall efficiency

This does not automatically mean V1 is a failed design.

It means that the design now provides enough information to make better engineering decisions.

---

# Engineering Trade-offs

This stage reinforced one of the central lessons of the project:

> **There is no perfect design.**

Every improvement introduces some combination of:

- Cost
- Complexity
- Size
- Heat
- Efficiency
- Reliability
- Manufacturability
- Component stress

The objective is therefore not to remove every imperfection.

The objective is to determine:

> **Which inefficiencies are acceptable for the requirements of this system, and which ones justify another iteration?**

That decision forms the basis of engineering design.

---

# Moving Forward

The weaknesses of V1 now become inputs to the next stage.

Instead of asking:

> "What is wrong with my circuit?"

the process becomes:

> "What did the circuit teach me, and what should I change because of it?"

The next stage documents the actual design changes, mistakes, corrections, and iterations that occurred during development.

## → 04 — Design Iterations
