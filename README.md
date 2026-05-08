# 🎲 Digital Dice — Digital Logic Design Project

> A microcontroller-free digital dice simulator using a 555 Timer, 4017 Decade Counter, and 74LS47 BCD Decoder, built and simulated in Proteus.

**Course:** Digital Logic Design | **Class:** BSCS-14-D  
**Institution:** NUST — School of Electrical Engineering and Computer Sciences  
**Lab Engineer:** Maam Rafia Ahmad | **Date:** 18/05/2025

---

## 📋 Table of Contents

- [Overview](#overview)
- [How It Works](#how-it-works)
- [Components](#components)
- [Circuit Operation](#circuit-operation)
- [Truth Table](#truth-table)
- [Simulation](#simulation)
- [Known Limitations](#known-limitations)
- [References](#references)

---

## Overview

This project simulates a standard 6-sided die using only discrete digital logic ICs — no microcontroller required. Pressing and releasing a push button generates a pseudo-random number from **1 to 6**, displayed on a 7-segment display.

The randomness comes from the fact that a 555 Timer drives the counter at a frequency far too fast for the human eye to track. Wherever the counter lands when the button is released becomes the dice result.

---

## How It Works

```
[9V Supply] → [555 Timer (Astable)] → [4017 Decade Counter] → [Diode Logic (BCD)] → [74LS47 Decoder] → [7-Segment Display]
                                              ↑
                                       [Push Button]
                                    (enables/disables clock)
```

1. **Button HELD** → 555 Timer clocks the 4017 counter at high speed. Display cycles through 1–6 rapidly.
2. **Button RELEASED** → Clock stops. Counter freezes. Display holds the final digit.

---

## Components

| # | Component | Qty | Value / Notes |
|---|-----------|-----|---------------|
| 1 | Solderless Breadboard | 1 | Full-size |
| 2 | 555 Timer IC | 1 | Astable mode |
| 3 | 4017 Decade Counter IC | 1 | Counts Q0–Q5 (reset at Q6) |
| 4 | 74LS47 BCD-to-7-Seg Decoder | 1 | Common anode driver |
| 5 | 7-Segment Display | 1 | Common anode, red |
| 6 | Push Button | 1 | Momentary tactile switch |
| 7 | 1N4148 Diode | 9 | Diode OR-gate BCD logic |
| 8 | Red LED | 1 | Power indicator |
| 9 | Resistor | 1 | 100Ω (segment current limiting) |
| 10 | Resistor | 6 | 1kΩ (pull-down / logic lines) |
| 11 | Resistor | 2 | 10kΩ (555 timer timing) |
| 12 | Capacitor | 1 | 100µF electrolytic (555 timing) |
| 13 | Capacitor | 1 | 104pF ceramic (noise bypass) |
| 14 | Power Supply | 1 | 9V DC |

---

## Circuit Operation

### 555 Timer (Astable Mode)
- Configured with R1 = R2 = 10kΩ and C = 100µF → ~4.8 Hz (visible in simulation)
- Replace C with 104pF for hardware → very high frequency (imperceptible cycling)
- Output on Pin 3 drives the 4017 clock input

### 4017 Decade Counter
- Counts from Q0 to Q5 (6 states = dice faces 1–6)
- Q6 output (Pin 5) is wired back to Reset (Pin 15) → auto-resets after 6
- Clock Enable (Pin 13) tied to GND → always enabled

### Diode Logic (BCD Encoding)
- 4017 outputs are one-hot (only one high at a time)
- 9× 1N4148 diodes implement OR-gate logic to produce the correct **BCD (DCBA)** code for each counter state
- 1kΩ resistors pull BCD lines low by default

### 74LS47 Decoder + Display
- Decodes BCD input to 7-segment signals (a–g)
- 100Ω resistors limit current to each segment
- LT, RBI tied high; BI/RBO tied to VCC → display always active

---

## Truth Table

| Counter State | Active Output | BCD (DCBA) | Display |
|---------------|---------------|------------|---------|
| Q0 | Q0 = 1 | 0001 | **1** |
| Q1 | Q1 = 1 | 0010 | **2** |
| Q2 | Q2 = 1 | 0011 | **3** |
| Q3 | Q3 = 1 | 0100 | **4** |
| Q4 | Q4 = 1 | 0101 | **5** |
| Q5 | Q5 = 1 | 0110 | **6** |
| Q6 | Q6 = 1 | — | Reset → Q0 |

---

## Simulation

The circuit was fully simulated in **Proteus Design Suite** before hardware assembly.

**Simulation file:** `digital_dice_proteus.pdsprj`  
**Snapshot:** See `digital_dice_proteus.jpeg` in this repository

Simulation confirmed:
- ✅ Correct 555 astable clock output
- ✅ Q0–Q5 sequential counting with auto-reset at Q6
- ✅ Accurate BCD encoding via diode logic
- ✅ Correct segment activation on 7-segment display
- ✅ Clean freeze on button release

---

## Known Limitations

- **Pseudo-random only** — result depends on exact button release timing; not cryptographically random
- **No hardware debouncing** — rapid presses can cause occasional double-counts
- **Single die only** — dual dice would require circuit duplication
- **Fixed speed** — dice roll speed is set by R/C values; no runtime speed adjustment

---

## References

1. NE555 / LM555 Timer Datasheet — Texas Instruments
2. CD4017B Decade Counter Datasheet — Texas Instruments
3. SN74LS47 BCD-to-Seven-Segment Decoder Datasheet — Texas Instruments
4. 1N4148 Signal Diode Datasheet — Fairchild Semiconductor
5. [Electronics Tutorials — 7-Segment Display Counter](https://www.electronics-tutorials.ws)
6. [555 Timer Astable Calculator](https://www.555-timer-circuits.com)
7. Proteus Design Suite — Labcenter Electronics
