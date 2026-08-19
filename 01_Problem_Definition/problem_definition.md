# 01 — Problem Definition

## The Problem

The objective of this project is to design a power supply capable of converting:

**230 V AC → Regulated 5 V DC**

The circuit should take the available mains supply, reduce it to a usable voltage, convert the AC waveform into DC, smooth the resulting waveform, and finally regulate it to approximately 5 V.

However, the purpose of this project is not simply to reproduce an existing 5 V power-supply circuit.

The larger objective is to use this relatively simple problem to develop and understand a **repeatable engineering approach for taking an electronic system from an idea to a PCB**.

---

## Defining the Required Function

Before selecting individual components, the required functions of the system were separated into stages.

The system needs to:

1. Reduce the 230 V AC input to a lower and safer voltage.
2. Convert the reduced AC voltage into DC.
3. Smooth the pulsating DC produced by rectification.
4. Regulate the resulting voltage to approximately 5 V.
5. Provide a usable 5 V DC output.
6. Provide a foundation that can later be analyzed, protected, simulated, implemented on a PCB, and improved.

From these requirements, the initial functional architecture became:

```text
230 V AC
    ↓
Voltage Reduction
    ↓
Rectification
    ↓
Filtering
    ↓
Voltage Regulation
    ↓
5 V DC
