# Chapter 6: Ion Energy, Bias Power & Pulsing

## Overview

Ion energy is the first control knob of the hole etch. Raise it and more ions pass down the hole, the bottom etches faster, and the profile is straighter. Raise it too far and the carbon mask erodes, the chuck cannot take the heat, and charging gets worse. This chapter derives how much ion energy a given aspect ratio demands, tabulates the trade-offs, shows how the broad energy distribution of a low-frequency bias costs ions, and uses pulsed and ramped bias to move the compromise.

**Learning Objectives:**
- Derive the ion energy needed to hold a target pass fraction at aspect ratio A
- Tabulate ion pass fraction, mask erosion, and heat load against ion energy
- Evaluate the cost of IED broadening and what tailored waveforms recover
- Model pulsed bias at different duty cycles and compare bottom delivery to mask loss
- Design a depth-ramped bias profile and estimate its mask saving

---

## 6.1 How Much Energy Does a Hole Need?

From Chapter 3.2, the fraction of ions that pass a hole of aspect ratio A is

```
f_pass = 1 − exp(−θ_c² / (2σ_θ²)),   θ_c = 1/A,   σ_θ² = T_⊥ / E
```

Solving for the energy that gives a target pass fraction f:

```
E_req = 2 · T_⊥ · ln[1/(1 − f)] · A²
```

```
For T_⊥ = 0.5 eV and f = 46.5%:  E_req = 0.625 · A²  (eV)

   A        E_req
 ────────────────────────
   20       250 eV
   40       1.0 keV
   60       2.25 keV
   80       4.0 keV      ← reference hole at full depth
  100       6.25 keV
  120       9.0 keV
  160       16 keV       ← a 16 µm single-deck hole
```

**The required energy rises as the square of the aspect ratio.** Doubling the depth at fixed CD multiplies the needed ion energy by four. This simple relation is the quantitative reason that a single 16 µm hole is impractical: it needs about 16 keV mean energy at the bottom, four times the wafer heat load per ion, and a mask that cannot survive it (Chapter 14).

For a target of 50% instead of 46.5%, the numbers scale by ln 2 / 0.625 = 1.11 (A = 80 needs 4.4 keV).

---

## 6.2 The Trade-Offs of Higher Energy

Hold the ion flux at the reference 6.6×10¹⁵ cm⁻² s⁻¹ and vary the mean energy (mono-energetic for clarity):

```
E (keV)   f_pass(A=80)   Heat load        Wafer power    Mask loss    Bottom delivery per
                         (W/cm²)          (707 cm²)      ∝ √E         unit mask loss
────────────────────────────────────────────────────────────────────────────────────────────
  2         27%           2.1             1.5 kW          0.71         0.38
  4         46%           4.2             3.0 kW          1.00         0.46
  6         61%           6.4             4.5 kW          1.22         0.50
  8         71%           8.5             6.0 kW          1.41         0.50
 10         79%          10.6             7.5 kW          1.58         0.50

(Mask loss assumed to follow physical sputtering ∝ √E at fixed flux, illustrative;
 bottom delivery per unit mask loss = f_pass / relative mask loss.)
```

Three observations:

1. **The pass fraction saturates.** Between 6 and 10 keV it only rises from 61% to 79% while mask loss climbs 30%.
2. **Delivery per unit mask loss plateaus near 6–8 keV.** Beyond that, extra energy buys ions only at proportional mask cost.
3. **Heat load rises in proportion to energy.** Every kV adds about 1 W/cm² at the reference ion flux. The ESC can handle it only if it is built for cryogenic cooling (Chapter 8).

The reference choice of 4 keV mean is a balance for the 8 µm hole, and cryogenic processes shift it upward because they carry more of the heat load.

---

## 6.3 Energy Distribution: The Cost of a Sine Wave

### 6.3.1 Sinusoidal Bias

At 400 kHz, ions follow the instantaneous sheath voltage (Chapter 5.2.1). The energy is E(φ) = E_mean · (1 + sin φ), and the distribution is bimodal, with peaks at the low and high ends.

```
Mean 4 keV, sinusoidal:   f_pass = 41%     (mono-energetic: 46%)
```

A third of the ions arrive below 2 keV where their angular spread (above 15 mrad) puts them outside the acceptance cone. They strike the wall instead of the bottom and are the dominant contributors to bowing and mask faceting from reflection.

### 6.3.2 Tailored Waveforms

Instead of a sine wave, the bias voltage can be shaped so that the sheath voltage is nearly constant during most of the cycle: a **pulsed-DC** waveform with a ramp to compensate for charging of the chuck surface, or a **tailored-waveform** generator that synthesizes the harmonics of the fundamental.

```
Waveform                 IED width (FWHM / mean)    f_pass (mean 4 keV)
─────────────────────────────────────────────────────────────────────────
Sine, 400 kHz            ~ 200%  (bimodal 0–8 keV)   41%
Sine, 13.56 MHz          ~ 15–30%                    45%  (but no multi-kV at this frequency)
Pulsed DC with ramp      ~ 10–20%                    45–46%
Tailored (multi-harm.)   ~ 20–40%                    44–45%
```

The recovered pass fraction is about five percentage points, which is about 12% more ions at the bottom, equivalent to roughly 0.8 keV of extra mean energy on a sine wave. Hardware complexity and arc risk at the chuck are the cost.

---

## 6.4 Pulsed Bias

### 6.4.1 Why Pulse

With continuous bias, positive charge builds at the hole bottom over the RF cycle (Chapter 3.5). When the bias is pulsed, the plasma afterglow in the off-time lets electrons and negative ions enter the hole and neutralize the charge. The aims are:

1. Reduce charge-induced deflection (twisting, ellipticity)
2. Raise A₀ by allowing the neutral and surface-borne supply to recover between pulses
3. Reduce average heat load at the same peak ion energy

### 6.4.2 A Simple Pulse Model

Assume that, for a fixed average bias power P_avg, a pulse of duty cycle d runs at P_on = P_avg/d. Over a CCP, ion energy scales as √P and ion current as √P:

```
E_on = E₀ · √(1/d)       Γ_on = Γ₀ · √(1/d)
Average flux = d · Γ_on = Γ₀ · √d

Duty    E_on      f_pass   Avg flux   Bottom delivery   Mask loss      Delivery / mask loss
 d      (keV)     (A=80)   (rel.)     (rel. to CW)      (rel. to CW)   (rel. to CW)
────────────────────────────────────────────────────────────────────────────────────────────
1.00     4.0       46%      1.00        1.00              1.00            1.00
0.50     5.7       59%      0.71        0.89              0.84            1.06
0.25     8.0       71%      0.50        0.77              0.71            1.08

(Mask loss ∝ avg flux × √E_on.)
```

**Pulsing buys charge relief at a small cost.** At 50% duty, the bottom receives 89% of the continuous-wave ions, mask loss falls to 84%, and delivery per unit mask loss improves by 6%. The cost is more demand on the supply and the matching network (20 kW peak for 10 kW average), and a longer total etch time if the rate follows delivery:

```
Rate scale (bottom-limited) 0.89 → etch time 40 min / 0.89 ≈ 45 min   (if rate ∝ delivery)
```

In practice the lower charging allows a lower overall O₂ and polymer compromise, which can recover some of that time (Chapter 7).

### 6.4.3 Pulse Parameters

```
Parameter                  Typical range       Consideration
─────────────────────────────────────────────────────────────────────────
Pulse frequency            1–20 kHz            Higher: more transitions, more matching
                                                complexity
Duty cycle                 25–80%              Lower duty → more charge relief, higher peak
Source synchronization     Optional            Pulsing the 60 MHz source with the bias
                                               makes the afterglow longer, increases
                                               negative ion flux
Delay between source/bias  0–50 µs             Bias on after the plasma is lit
```

Pulsing the source power in step with the bias is the usual way to lengthen the afterglow, because it gives time for electrons to cool and for negative ions to form in the fluorocarbon gas.

---

## 6.5 Ramping Ion Energy With Depth

### 6.5.1 The Case for a Ramp

The required energy rises as A² (Section 6.1), but the reference recipe uses a constant 4 keV. In the first half of the hole (A < 40), the needed energy is only about 1 keV. Running 4 keV there wastes mask, adds heat, and gives no extra pass fraction (it is already high).

### 6.5.2 Mask Saving From a Ramp

```
Time spent at each depth (from Chapter 3.4, w̄/R₀ = 0.333 min):
  A 0–40:   t(40) = 16.7 min
  A 40–60:  t(60) − t(40) = 0.333 × (60 + 22.5) − 16.7 = 27.5 − 16.7 = 10.8 min
  A 60–80:  40.0 − 27.5 = 12.5 min

Ramp E(A) = max(1.0 keV, 0.625 A²):
  Stage       A range    E (keV)       √E     time (min)    √E × time
  ─────────────────────────────────────────────────────────────────
  Top half    0–40       1.0          1.00     16.7          16.7
  Mid         40–60      1.56 (A=50)  1.25     10.8          13.5
  Bottom      60–80      3.06 (A=70)  1.75     12.5          21.9
  Ramp total                                                 52.1

Constant 4 keV: 2.0 × 40 min = 80.0
Ramp / constant = 52.1 / 80.0 = 65%     → 35% less mask loss
```

For 1.33 µm of mask loss at constant 4 keV, the ramp would lose 0.87 µm, **saving 0.46 µm of mask** (before accounting for slower rate at low energy).

### 6.5.3 The Catch

Lower energy early also lowers the etch rate (less ion-driven chemistry, lower sputter-assisted removal of polymer) and may change profile and selectivity in the top half. The saving is therefore a bound. Practical recipes use a **stepped bias**: three to six stages whose ion energy tracks the depth, with gas and pressure changes at each step (Chapter 7).

---

## 6.6 Controlling and Monitoring Ion Energy

```
Quantity                      Method                       Use
───────────────────────────────────────────────────────────────────────────
Forward / reflected power     RF sensor at match output    Delivered power control
Peak-to-peak bias voltage     Probe at ESC base            Proxy for E_mean
DC self-bias                  Voltage divider              Ion energy trend
Harmonics of the LF bias      Spectrum analyzer            Plasma impedance, arcing
IED                           RFEA (qualification only)    Direct ion energy profile
```

Hole etch tools control **delivered power** and monitor the peak voltage as a fault and health signal. A 2% drift in voltage at constant power typically means a change in chamber impedance (wall film, ring wear) and shifts the pass fraction by about 0.7 points, because ion energy follows sheath voltage and df/d(ln E) = x·e⁻ˣ ≈ 0.33 at the reference (x = 0.625).

---

## 6.7 Summary & Key Takeaways

1. **Energy requirement scales as A².** E_req = 0.625 A² eV gives 4.0 keV at A = 80 and 16 keV at A = 160.

2. **Pass fraction saturates; mask and heat do not.** Delivery per unit mask loss plateaus at 6–8 keV, and every kV adds about 1 W/cm² of heat.

3. **A sine bias broadens the IED.** It costs about five points of pass fraction (41% versus 46%), recoverable by pulsed-DC or tailored waveforms.

4. **Pulsing trades flux for charge relief.** At 50% duty, delivery falls to 89% and mask loss to 84%.

5. **Ramping the bias with depth saves mask.** About 35% less mask loss in the idealized case.

6. **Control power, monitor voltage.** Voltage at fixed power is the early-warning signal for impedance changes.

---

## Study Questions

1. Using E_req = 2 T_⊥ ln[1/(1 − f)] A², compute the energy needed for f = 60% at A = 70 and T_⊥ = 0.6 eV. What is the energy at A = 140?

2. A bias supply must deliver 12 kW average at 40% duty. What is its peak power? If E ∝ √P, and the continuous-wave mean energy at 12 kW is 4.5 keV, what is the pulse energy?

3. A recipe uses stages at E = 1.0, 2.0, 3.5, 5.0 keV for A ranges 0–30, 30–50, 50–70, 70–90. With w̄/R₀ = 0.35 min and A₀ = 90, compute the time per stage and the mask loss relative to constant 5 keV (mask loss ∝ √E × time).

4. At a given ion flux, the mean energy rises from 4 to 5 keV. By how much does heat load rise? What does the wafer power become if the reference is 3.0 kW?

5. The bias voltage at constant power drifts 3% high over a ring lifetime. Estimate the change in pass fraction and what happens to mask loss.

---

**Previous Chapter:** [Chapter 5: Reactor Architecture for Multi-keV High-Aspect-Ratio Etch](./05-reactor-architecture.md)  
**Next Chapter:** [Chapter 7: Gas Delivery, Pressure & Multi-Step Recipes](./07-gas-pressure-recipes.md)

---

**Chapter 6 Development Status:** Complete  
**Version:** 1.0
