# Chapter 5: Reactor Architecture for Multi-keV High-Aspect-Ratio Etch

## Overview

A memory-hole reactor does something unusual among etch tools: it delivers ten kilowatts of bias power to a 300 mm wafer, builds a sheath over a centimeter thick, and holds the wafer at a cryogenic temperature while doing so. The architecture that results is capacitively coupled (CCP), driven at several frequencies, and built around a very robust electrostatic chuck. This chapter explains why the reactor looks the way it does, what the sheath at several kilovolts does to the ions, how the edge of the wafer is handled, and how a platform of such chambers is matched.

**Learning Objectives:**
- Compare CCP, ICP, and hybrid sources for high-aspect-ratio etch
- Estimate sheath thickness at multi-kV bias and the number of ion collisions in it
- Explain the roles of the source and bias frequencies and the ion transit time
- Quantify the edge-ring height effect on ion tilt and hole displacement
- Describe the power-delivery and arcing constraints of a 10 kW bias system
- Specify chamber-matching targets for a multi-chamber platform

---

## 5.1 Why Capacitively Coupled

```
Property                    CCP (dual/triple freq.)     ICP                 Hybrid
─────────────────────────────────────────────────────────────────────────────────────────
Plasma density              1×10¹⁰–1×10¹¹ cm⁻³          1×10¹¹–1×10¹² cm⁻³   Variable
Dissociation                Moderate (limited)          High                High
Bias power capability       Very high (10–30 kW)        High, but limited   High
                                                        by source coupling
Ion energy range            0.3–10 keV                  0.05–2 keV typical  Wide
Gas chemistry               Heavier fragments retained  More atomic F, C    Mixed
                            (fewer small radicals)      (more dissociated)
Neutral radical bias        Larger CxFy, polymerizing   Smaller, F-rich     Mixed
Wall heating                Moderate                    Window/coil
Typical use in this book    Reference hole etch         Mask open, cleanup  Alternatives
```

The hole etch needs **high ion energy with a modest radical supply**. A CCP gives the first and avoids the second. A dense ICP creates a lot of atomic fluorine that attacks the sidewall and mask, and its coil coupling does not scale to the bias power the hole demands. Multi-keV bias plus a plasma density of 5×10¹⁰ cm⁻³ is the combination the CCP delivers.

---

## 5.2 The Frequency Stack

```
Role                    Frequency        Power         Function
─────────────────────────────────────────────────────────────────────────────
Plasma generation       60 MHz (or 100)  3 kW          Sets density and radical
(source, top or bottom)                                  composition
Mid-frequency           2–13.56 MHz      0–3 kW        IED shaping, tuning
(optional)
Low-frequency bias      400 kHz          10 kW         Ion energy, multi-keV
(wafer electrode)                        (peak 20 kW   (pulsed option)
                                          pulsed)
```

The **source** frequency is high so the plasma density is generated efficiently without large sheath voltages. The **bias** frequency is low so that the kV-scale voltage can be developed with an acceptable current through the chuck and matching network, and so that ions respond to the instantaneous sheath voltage.

### 5.2.1 Ion Transit Time Versus RF Period

Whether ions see the average sheath voltage or the instantaneous voltage depends on the ratio of their transit time to the RF period.

```
Collisionless Child–Langmuir sheath: τ_i ≈ 3 s / v_f

Reference: CF₃⁺ (69 u) at 4 keV  →  v_f = √(2E/m) = 1.06×10⁵ m/s
           s = 1.2 cm (Section 5.3)
  τ_i = 3 × 0.0119 m / 1.06×10⁵ m/s = 0.34 µs

Bias period at 400 kHz: T = 2.5 µs      →  τ_i / T = 0.14
Source period at 60 MHz: T = 16.7 ns    →  τ_i / T = 20
```

At 400 kHz, ions cross in a small fraction of the bias period and **follow the instantaneous sheath voltage**. The ion energy distribution (IED) is broad, bimodal, and spans from near zero to twice the mean energy. At 60 MHz, ions see only the time-averaged sheath, so the source frequency does not broaden the IED.

### 5.2.2 Consequence for Hole Etch

For a sinusoidal 400 kHz bias with a mean ion energy of 4 keV, the energy varies as E(φ) = 4000 (1 + sin φ) eV, from 0 to 8 keV. Averaging the pass fraction of Chapter 3.2.2 over that distribution:

```
Mono-energetic 4 keV:               f_pass = 46%
Sinusoidal IED, mean 4 keV:         f_pass = 41%
  (33% of the ions have E < 2 keV and carry the poor angular spread)
Sinusoidal IED, mean 5 keV:         f_pass = 47%
Sinusoidal IED, mean 6 keV:         f_pass = 52%
```

The broad IED costs about five points of pass fraction relative to a single-energy beam of the same mean energy. Chapter 6 discusses pulsed-DC and tailored waveforms that narrow the IED.

---

## 5.3 The Multi-kV Sheath

### 5.3.1 Thickness

For a collisionless Child–Langmuir sheath with peak voltage V_s above a plasma of density n and electron temperature T_e:

```
s = (√2/3) · λ_D · (2 V_s / T_e)^(3/4),    λ_D = 743 · √(T_e/n) cm  (T_e in eV, n in cm⁻³)

Reference: V_s = 5 kV (a representative sheath voltage; the instantaneous
           value swings over the 400 kHz cycle), T_e = 3 eV
  n = 1×10¹⁰ cm⁻³:  λ_D = 129 µm,  s = 2.7 cm
  n = 5×10¹⁰ cm⁻³:  λ_D = 58 µm,   s = 1.2 cm      ← reference
  n = 1×10¹¹ cm⁻³:  λ_D = 41 µm,   s = 0.84 cm
```

The sheath at the reference conditions is about 1.2 cm thick. It is a significant fraction of the electrode gap, and it is the reason the electrode spacing is typically 2–5 cm and the edge ring is designed carefully (Section 5.4).

### 5.3.2 Collisions in the Sheath

Charge-exchange collisions between fast ions and the neutral gas produce slow ions that are accelerated through whatever is left of the sheath. They arrive with lower energy and wider angle.

```
Charge-exchange cross-section (CFx⁺ in fluorocarbon gas) σ ≈ 5×10⁻¹⁵ cm²
Gas density n_g = p / k_B T,  mean free path λ_i = 1 / (n_g σ)

Pressure   n_g (cm⁻³)    λ_i       N_c = s/λ_i    Collisionless fraction exp(−N_c)
──────────────────────────────────────────────────────────────────────────────────
30 mTorr   9.7×10¹⁴      0.21 cm       5.7             0.3%
20 mTorr   6.4×10¹⁴      0.31 cm       3.8             2.2%
10 mTorr   3.2×10¹⁴      0.62 cm       1.9            15%
 5 mTorr   1.6×10¹⁴      1.24 cm       0.96           38%
```

(The collisionless sheath thickness overstates the real, collisional sheath, so these N_c values are upper bounds. They show the trend.)

**Low pressure is a requirement of the hole etch**, not a preference. At 20 mTorr, nearly every ion has had a charge-exchange event in the sheath, which fills the IED with low-energy ions that cannot enter the hole. Reference hole recipes run at 5–20 mTorr. The cost is a lower radical density and longer residence time (Chapter 7).

---

## 5.4 The Wafer Edge: Ring, Sheath, and Tilt

### 5.4.1 Edge Sheath Bending

Near the wafer edge, the plasma sheath follows the surface. If the surface of the edge ring and the wafer are not at the same height, the sheath bends, and ions are deflected toward or away from the wafer center. Ions arriving at the edge hole carry a tilt θ_e, and the hole they etch follows that tilt:

```
Hole bottom displacement = H · tan θ_e ≈ H · θ_e

H = 8.0 µm:
  θ_e = 0.1° (1.75 mrad):   14 nm
  θ_e = 0.2° (3.5 mrad):    28 nm
  θ_e = 0.5° (8.7 mrad):    70 nm
  θ_e = 1.0° (17.5 mrad):  140 nm   — ion does not even reach the bottom (θ_c = 0.72°)
```

The tilt budget is set by what the bottom contact can tolerate. The reference spec of 14 nm displacement corresponds to a tilt below 0.1°.

### 5.4.2 Edge Ring Wear

The edge ring (focus ring) erodes under ion bombardment, and the sheath height above the ring changes as its thickness changes.

```
Illustrative sensitivity:   tilt ≈ 0.4° per 1 mm of ring-to-wafer height mismatch
Budget:                     tilt ≤ 0.1°  →  mismatch ≤ 0.25 mm
Ring erosion (illustrative): 0.05 mm per 100 RF hours  →  500 RF hours between replacements,
                             or active compensation (ring lifted to follow the wear)
```

Adjustable edge rings, which lift to compensate for wear, are used in many reactors. The alternative is to hold the replacement interval and apply an edge-process offset (bias power, gas) to flatten the tilt (Chapter 13.4).

---

## 5.5 Power Delivery, Matching, and Arcing

### 5.5.1 The Bias Network

A 10 kW, 400 kHz source driving a wafer through a chuck sees a capacitive load of a few nF and develops several kV of peak voltage:

```
Reference: P_bias = 10 kW, wafer + chuck capacitance C_load ≈ 4 nF (illustrative)
  X_C at 400 kHz = 1 / (2π f C) = 1 / (2π × 4×10⁵ × 4×10⁻⁹) = 99 Ω
  Peak RF current  I_pk ≈ √(2 P / R_load)  (R_load ≈ 5 Ω → I_pk ≈ 63 A)
  Peak voltage     ≈ I_pk · X_C ≈ 63 × 99 ≈ 6.2 kV
```

These are large currents and voltages for a match box and a chuck, and they define the component ratings (insulation, tuned inductor current, cooling).

### 5.5.2 Arcing

With kV potentials and a helium-filled backside channel, the chuck sits near the minimum of the Paschen curve for helium. Arcing risks are:

```
Location                    Risk                          Mitigation
─────────────────────────────────────────────────────────────────────────────────
Backside He channels        Breakdown through gas         Porous plugs, fine channels,
                            in vias                       controlled He pressure
Edge of wafer / ring gap    Arcing at the sheath          Ceramic barriers, wider ring
                            discontinuity                 gap management
Bias feed / ESC base        Internal breakdown            Potting, insulation,
                                                          pressure-rated feeds
Wafer lift pins             Breakdown path                Retracted state, ceramic pins
```

An arc leaves particles and a scrapped wafer, and it can deposit metal on the chuck. Arc detection (RF and DC monitors) trips the system within microseconds.

---

## 5.6 Platform Configuration and Matching

### 5.6.1 Cluster Layout

A typical hole-etch platform holds four to six process modules on one mainframe. With a 45-minute wafer cycle per chamber (40 min etch + 5 min overhead), a six-chamber tool produces

```
6 chambers × (60 / 45) wafers/h = 8 wafers per hour
```

so the platform spends more time per wafer in the process chamber than almost any other etch, and chamber count rather than robot speed sets throughput.

### 5.6.2 Chamber Matching Targets

```
Metric (reference)                         Target (3σ across chambers)
───────────────────────────────────────────────────────────────────────
Etch depth (8.0 µm)                        ± 0.8%  (±64 nm)
Top CD                                     ± 1.5 nm
Bottom CD                                  ± 2.5 nm
Tilt at wafer edge                         ± 0.05° (± 7 nm at bottom)
Mask loss                                  ± 3%
Bias voltage at fixed power                ± 1.5%
ESC temperature (He-on, cooled wafer)      ± 0.5 °C
```

At 150 nm/min near the bottom, 1% of depth is 80 nm and 32 s of etch, so depth matching is a time-and-power matching problem. Tilt at the edge is the hardest to match because it depends on the wear state of the ring and the shape of the edge assembly (Chapter 9).

---

## 5.7 Summary & Key Takeaways

1. **CCP at several frequencies fits the hole etch.** It gives multi-kV bias with a modest radical supply.

2. **The bias frequency is low.** At 400 kHz, ions follow the instantaneous sheath, so the IED is broad and bimodal, and the pass fraction is 41% instead of 46%.

3. **The sheath is about 1.2 cm thick.** Combined with 5–20 mTorr, that gives a collisionless fraction between 2% and 38%, so low pressure is a requirement.

4. **The edge sets tilt.** 0.1° of tilt moves the bottom of an 8 µm hole by 14 nm, and it corresponds to about 0.25 mm of edge-ring height error.

5. **Power and arcing are first-order design problems.** A 10 kW, 400 kHz system develops about 6 kV peak, in a helium-filled chuck near the Paschen minimum.

6. **Matching is mostly depth and tilt.** A chamber pass takes 45 minutes, so 1% of depth is 32 s of etch.

---

## Study Questions

1. For a plasma density of 8×10¹⁰ cm⁻³, T_e = 3.5 eV, and V_s = 4 kV, compute λ_D and the collisionless sheath thickness. At 8 mTorr with σ = 5×10⁻¹⁵ cm², compute N_c.

2. A 6 keV ion of mass 69 u crosses a sheath of s = 1.0 cm. Compute its transit time. At what bias frequency does τ_i equal one-tenth of the RF period?

3. An edge ring wears at 0.04 mm per 100 RF hours and the tilt sensitivity is 0.35°/mm. How many RF hours until tilt reaches 0.1°? How much bottom displacement does a tilt of 0.1° give for a 9.0 µm deep hole?

4. A 12 kW bias supply drives a 5 nF load at 400 kHz with R_load = 4 Ω. Compute X_C, the peak current, and the peak voltage.

5. A four-chamber platform has a 38 min etch and 6 min overhead per wafer. Compute throughput per hour and how much it improves if the etch time drops to 28 min by moving to a cryogenic recipe.

---

**Previous Chapter:** [Chapter 4: Etch Chemistries — Fluorocarbons, Hydrofluorocarbons & Cryogenic HF](./04-etch-chemistries.md)  
**Next Chapter:** [Chapter 6: Ion Energy, Bias Power & Pulsing](./06-ion-energy-pulsing.md)

---

**Chapter 5 Development Status:** Complete  
**Version:** 1.0
