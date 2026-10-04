# Chapter 7: Gas Delivery, Pressure & Multi-Step Recipes

## Overview

A hole 8 µm deep is not one etch but several. At the top, the hole is short and wide relative to its length, radicals arrive freely, and the problem is protecting the mask. At the bottom, almost nothing arrives and the problem is keeping the hole open. No single set of gas, pressure, and bias is right for both. Production recipes therefore run in **stages** that change composition, pressure, and ion energy as the hole deepens. This chapter builds the stage structure from the physics, gives a reference stepped recipe, quantifies how sensitive it is to small gas-flow errors, and describes the mechanics of switching between stages.

**Learning Objectives:**
- Explain why a hole etch is divided into stages and what changes at each
- Read and construct a stepped reference recipe, with stage times from the ARDE model
- Estimate mask loss for a stepped recipe
- Quantify sensitivity of selectivity, bottom opening, and mask loss to gas-flow errors
- Specify mass-flow controller accuracy and transition behavior
- Relate pressure to ion transport, radical supply, and ARDE

---

## 7.1 Why the Recipe Has Stages

```
Depth zone      A range    What dominates             What the recipe must do
──────────────────────────────────────────────────────────────────────────────────────
Top (0–2 µm)    0–20       Radicals plentiful;        Protect the mask (carbon-rich),
                           ions pass easily           set top CD, avoid top bowing
Middle          20–50      Neutral supply falling;    Maintain passivation, keep
(2–5 µm)                   passivation thinning       R_N/R_ox ≈ 1, start raising
                                                       ion energy
Deep            50–75      Few radicals; ion cone     Lower pressure, raise bias,
(5–7.5 µm)                 narrowing; charging        add O₂ to avoid clogging,
                                                       protect bottom CD
Bottom          75–80      Ion-starved; products      Highest bias, lowest pressure,
(7.5–8 µm)                 accumulating               open the bottom
Overetch        > 80       Landing on poly            Selectivity to poly-Si
```

The same stage logic appears in all high-aspect-ratio etches (contact holes, trenches), but the 8 µm depth of the memory hole stretches every stage.

---

## 7.2 A Reference Stepped Recipe

### 7.2.1 Stage Times From the ARDE Model

Using t(A) = 0.333 min × (A + A²/160) (Chapter 3.4), the time spent in each depth zone is:

```
Zone boundary     A      t(A) (min)    Stage time (min)
────────────────────────────────────────────────────────
 0 µm             0         0
 2.0 µm          20        7.5               7.5
 5.0 µm          50       21.9              14.4
 7.5 µm          75       36.7              14.8
 8.0 µm          80       40.0               3.3
Overetch (66 s, Ch. 2.4.2)                    1.1
                                       Total ≈ 41.1 min
```

### 7.2.2 Reference Recipe

```
Parameter         Break-    Stage 1   Stage 2   Stage 3   Stage 4   Over-
                  through   (top)     (middle)  (deep)    (bottom)  etch
────────────────────────────────────────────────────────────────────────────
Depth zone (µm)   Cap       0–2       2–5       5–7.5     7.5–8     landing
Time              8 s       7.5 min   14.4 min  14.8 min  3.3 min   66 s
Pressure (mTorr)  20        20        15        10        6         6
Ar (sccm)         200       150       150       150       150       150
C₄F₆ (sccm)       —         15        14        12        10        8
CH₂F₂ (sccm)      —         8         8         7         6         —
CF₄ (sccm)        40        —         —         —         —         —
O₂ (sccm)         —         8         9         10        12        14
Source 60 MHz     1.5 kW    3.0 kW    3.0 kW    3.0 kW    3.0 kW    2.0 kW
Mean ion energy   0.5 keV   1.0 keV   1.5 keV   3.0 keV   4.5 keV   4.0 keV
ESC setpoint (°C) −70       −70       −70       −70       −70       −70
(Illustrative cryogenic process. The ESC setpoint is constant; the wafer runs warmer
 by an amount that follows the stage's ion heat load, −63 °C in Stage 1 to −38 °C in
 Stage 4, Chapter 8.3)
```

Trends worth noting:

1. **Pressure falls** from 20 to 6 mTorr, widening the collisionless fraction of ions in the sheath (Chapter 5.3.2).
2. **Fluorocarbon share falls with depth** while O₂ rises. The carbon-rich polymer that protected the mask at the top is no longer wanted at the bottom, where it would close the hole. The O₂ rise is bounded by mask erosion.
3. **Ion energy steps up** with depth, following E_req ≈ 0.625 A² with margin (Chapter 6.5).
4. **The overetch has no nitride-balancing gas** (CH₂F₂ is removed) and has high O₂ and lower C₄F₆ to keep the bottom open while limiting polymer on the exposed silicon. The overetch is the stage most often tuned for the landing layer (Chapter 12).

### 7.2.3 Mask Loss of the Stepped Recipe

Taking mask loss as proportional to √E × time (Chapter 6.2) and calibrating to 1.33 µm for 40 min at constant 4 keV (80 √keV·min):

```
Stage         E (keV)   √E      Time (min)   √E × t
────────────────────────────────────────────────────
Stage 1       1.0       1.00     7.5           7.5
Stage 2       1.5       1.22    14.4          17.6
Stage 3       3.0       1.73    14.8          25.6
Stage 4       4.5       2.12     3.3           7.0
Overetch      4.0       2.00     1.1           2.2
                                       Total   59.9

Mask loss = 1.33 µm × 59.9 / 80 = 1.00 µm
Mask remaining = 1.8 − 1.0 = 0.8 µm  (before the breakthrough, which takes ~0.06 µm of SiON cap)
Average selectivity = 8.0 / 1.0 = 8:1  (versus 6:1 at constant conditions)
```

The stepped recipe holds more mask. It still ignores the lower rate at low early-stage energy and the effect of changing chemistry on mask erosion, so a real stepped recipe lands between this bound and the constant-condition result; Chapter 11 develops the mask budget in full.

---

## 7.3 Sensitivity to Gas Flow

### 7.3.1 Sensitivity Coefficients

```
Parameter (stage 2 reference)       Change        Effect (illustrative)
──────────────────────────────────────────────────────────────────────────────────
O₂ (9 sccm)                         +1 sccm       Mask loss +12%; bottom CD +1.0 nm;
                                                  clogging risk lower
O₂ (9 sccm)                         −1 sccm       Mask loss −12%; polymer thicker;
                                                  neck/clog risk higher, bottom CD −1.5 nm
CH₂F₂ (8 sccm)                      +1 sccm       R_N/R_ox +0.045; mask loss +4%
C₄F₆ (14 sccm)                      −1 sccm       Mask selectivity −8%; bowing +0.4 nm
Ar (150 sccm)                       +5 sccm       Ion fraction up slightly; negligible
                                                  chemistry change; pressure +0.3 mTorr
Pressure                            +1 mTorr      Collisionless fraction −3 pts; A₀ −2%
                                                  (Section 7.4)
```

### 7.3.2 Implications

1. **Small flows control the profile.** O₂, CH₂F₂, and C₄F₆ are flows of 8–15 sccm. An MFC sized for 100 sccm with ±1% of full-scale accuracy has ±1 sccm error at 10 sccm setpoint, which is a ±10% effect on mask loss. Specify MFCs sized to the flow with accuracy in percent of reading.

2. **Errors are systematic.** An MFC that reads 1 sccm low does so every wafer, and the effect accumulates over a 40-minute etch. A rate-of-rise check belongs in the daily chamber check (Appendix C).

3. **Mass-flow stability beats absolute accuracy.** A chamber that is off by 3% but repeatable can be offset by recipe. A chamber that wanders by ±1% cannot.

```
Worked example: O₂ MFC reads 0.8 sccm low in Stage 3 (setpoint 10 sccm):
  Effect on mask loss in Stage 3: −9.6%  → Stage 3 loses 0.43 µm of mask (Section 7.2.3),
    so the change is 0.43 × 9.6% ≈ 0.04 µm
  Effect on bottom opening: clog risk up → bottom CD falls ≈ 1.2 nm per 0.8 sccm
  Combined with R_N/R_ox shift: negligible
  Net: mask unaffected, but bottom CD error of 1.2 nm is 30% of the ±4 nm spec
```

---

## 7.4 Pressure

### 7.4.1 What Pressure Changes

```
Effect                        Low pressure (5 mTorr)         High pressure (20 mTorr)
──────────────────────────────────────────────────────────────────────────────────────────
Collisionless ion fraction    38%                             2%
Radical density at the wafer  Lower                           Higher
Residence time (30 L, 180 sccm flow total)   0.07 s           0.26 s
Polymer deposition            Lower                           Higher
Mask protection               Lower                           Higher
Sheath thickness              Larger                          Smaller
ARDE (A₀)                     Higher (better)                 Lower
```

```
Residence time τ = p V / Q
  At 20 mTorr, 30 L, Q = 180 sccm = 2.28 Torr·L/s:  τ = 0.020 × 30 / 2.28 = 0.26 s
  At 6 mTorr, 30 L, Q = 180 sccm:                    τ = 0.006 × 30 / 2.28 = 0.08 s
```

Residence times are short, so the plasma composition follows gas changes within a few tenths of a second. Transitions are limited by the MFC and throttle-valve settling (about 1–3 s), not by gas exchange.

### 7.4.2 The Compromise

Low pressure is better for ion transport and ARDE, but it reduces polymer deposition and mask protection. The stepped recipe resolves this by running high pressure at the top (where mask protection matters most and ions pass easily) and low pressure at the bottom (where ion delivery matters most and the hole is already protected by the mask thickness left).

---

## 7.5 Stage Transitions

Each stage change requires the gas flows, pressure, source power, and bias power to move to new set-points. The order matters:

```
Rule                                     Reason
────────────────────────────────────────────────────────────────────────────────────
Gases and pressure settle first          Avoid a high-bias ion beam in the wrong chemistry
Bias ramps after gas composition is      A high bias in an O₂-rich, polymer-poor mix
  within tolerance                       etches the mask quickly
Bias ramps over 0.5–2 s, not stepped     Avoid voltage overshoot at the matching network
Plasma stays on through transitions      No ignition transient; avoids particle fallout
Stage time covers transition             Transition counted in the stage, not added
```

```
Transition overhead: 5 transitions × 2–4 s = 10–20 s per wafer (<1% of 41 min)
```

A transition in which bias leads gas by one second, with 4.5 keV bias in the previous stage's composition, is a self-inflicted mask loss: at 33 nm/min × 1.6 (higher energy) ≈ 53 nm/min, 1 s costs about 0.9 nm. This is small per transition but matters over many wafers if timing drifts to several seconds.

---

## 7.6 Center and Edge Gas Tuning

The hole profile has radial dependence in CD, depth, and tilt. Gas is distributed through a showerhead (or injectors) with independently controlled center and edge zones.

```
Edge gas fraction f_e         Edge CD relative to center (illustrative)     Edge depth
────────────────────────────────────────────────────────────────────────────────────────
0.30                          −1.5 nm                                      −1.1%
0.40                           0.0                                          0.0
0.50                          +1.5 nm                                      +1.0%
Sensitivity ≈ 1.5 nm CD and 1.0% depth per 0.1 change in f_e
```

```
Example: edge top CD is 2.2 nm small and edge depth is 0.8% low.
  Needed Δf_e from CD:    +2.2 / 1.5 × 0.1 = +0.15
  Needed Δf_e from depth: +0.8 / 1.0 × 0.1 = +0.08
  Gas split alone cannot correct both. Use gas for CD (+0.15) and bias or temperature
  zones for depth (Chapter 8).
```

The last 5–10 mm of the wafer is governed by the edge ring and temperature (Chapter 13.4), not by gas split.

---

## 7.7 Summary & Key Takeaways

1. **Hole etch is staged.** Five zones (top, middle, deep, bottom, overetch) each need different pressure, gas, and bias.

2. **Stage times follow ARDE.** For the reference: 7.5, 14.4, 14.8, 3.3, and 1.1 min, totaling about 41 min.

3. **Stepped bias saves mask.** The reference stepped recipe loses about 1.0 µm of mask against 1.33 µm for constant conditions (upper bound on the saving).

4. **Small flows matter most.** O₂ ±1 sccm changes mask loss by 12% and bottom CD by 1–1.5 nm.

5. **Pressure trades ions against polymer.** 20 mTorr at the top for mask protection and 6 mTorr at the bottom for ion delivery.

6. **Transitions are limited by valves, not gas exchange.** Timing matters because bias in the wrong chemistry erodes the mask.

---

## Study Questions

1. A stepped recipe has stage energies 1.0, 2.0, 3.5, 5.0 keV with stage times 8, 12, 14, 6 min. Compute mask loss relative to a constant 4 keV, 40 min recipe using √E × time.

2. Using t(A) = (w̄/R₀)(A + A²/(2A₀)) with w̄ = 95 nm, R₀ = 280 nm/min, A₀ = 70, compute the time to reach 2, 5, and 7 µm.

3. An MFC with ±1% full-scale accuracy on a 50 sccm range is used for a 6 sccm O₂ flow. What is the worst-case flow error? Using the 12%/sccm mask loss sensitivity, what is the mask loss change? What accuracy class would hold the error to ±3% of mask loss?

4. Compute the residence time for a 35 L chamber at 12 mTorr and 250 sccm. How long to reach 1% residual of the previous gas?

5. Edge CD is 3 nm large and edge depth is 1.5% shallow. Using the sensitivities in Section 7.6, can gas split correct both? What second knob is needed?

---

**Previous Chapter:** [Chapter 6: Ion Energy, Bias Power & Pulsing](./06-ion-energy-pulsing.md)  
**Next Chapter:** [Chapter 8: Cryogenic Wafer Temperature, ESC Design & Heat Load](./08-cryogenic-esc-heat.md)

---

**Chapter 7 Development Status:** Complete  
**Version:** 1.0
