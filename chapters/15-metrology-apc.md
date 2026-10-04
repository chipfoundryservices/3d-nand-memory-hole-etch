# Chapter 15: Metrology, Inspection & Advanced Process Control

## Overview

A memory hole is hidden at the moment it matters. Its bottom is 8 µm below a surface that an electron beam cannot see through, its profile is revealed only by cutting the wafer, and a failure rate of one per ten billion cannot be checked by looking at holes. Control of the etch therefore depends on a layered system: in-chamber signals (optical emission, RF, temperature, helium leak) that are available on every wafer and in real time; non-destructive measurements (optical, X-ray, high-voltage electron beam) that sample a few sites per wafer; destructive cross-sections for calibration; and electrical tests that finally count what the others could not. This chapter describes each layer, what it can resolve, what it costs in time, and how feed-forward and feedback control use the results.

**Learning Objectives:**
- Estimate the signal available to optical-emission endpoint from the etch product flux
- Compare metrology methods by what they measure and their precision against the tolerance
- Show why inspection cannot verify a 10⁻¹¹ hole-failure rate directly
- Design a feedback update with an EWMA law and evaluate its noise penalty
- Quantify the number of wafers at risk for a given detection lag
- Specify the FDC variables that protect a 45-minute process

---

## 15.1 Optical Emission and Endpoint

### 15.1.1 How Much Product Is There?

```
Atoms removed per second (Chapter 3.2.3): 9.5×10⁵ per hole at the bottom rate
Holes per wafer: 1.0×10¹²
Total: 9.5×10¹⁷ atoms/s from the hole bottoms

Fraction as silicon: ~0.35 → Si-bearing product ≈ 3.3×10¹⁷ molecules/s (SiF₄ etc.)

Total gas flow 180 sccm × 4.48×10¹⁷ molecules/(s·sccm) = 8.1×10¹⁹ molecules/s
Product concentration in the exhaust ≈ 3.3×10¹⁷ / 8.1×10¹⁹ = 0.4%
```

Etch products make up roughly half a percent of the gas leaving the chamber, which is large enough to see in the plasma emission, even though each hole contributes only a million molecules per second.

### 15.1.2 Species and Lines

```
Species    Wavelength (nm)   Behavior in the hole etch
─────────────────────────────────────────────────────────────────────────────
CN         388.3             Nitride etch product; falls as the bottom reaches poly
CO         483.5, 519.8      Oxide etch product, mask erosion; stable in main etch
F          703.7             Etchant; rises when etch products fall (loading)
Ar         750.4             Actinometry reference
Si         288.2             Rises when poly is exposed (landing)
SiF        440.1             Silicon exposed to fluorine
```

### 15.1.3 What the Signal Can and Cannot Do

```
Can:     Detect the average arrival at the polysilicon (Chapter 12.5)
         Detect chamber state (wall condition, window coating)
         Trend stage transitions and flag gas or power faults
Cannot:  See the tail of the depth distribution (the slowest holes)
         See differences between center and edge unless multi-point OES is used
         Resolve a few nm of CD change (the signal is too weak)
```

---

## 15.2 Metrology Methods

```
Method                       Measures                         Precision (3σ,       Speed / notes
                                                              illustrative)
────────────────────────────────────────────────────────────────────────────────────────────────────
CD-SEM (top-down)            Top CD, ellipticity, roughness   0.3 nm               Fast; top only
OCD / scatterometry          Average CD, depth, taper over    0.5 nm CD,           Fast; model-based;
(IR spectroscopic)           an array area                    0.5% depth           averages over holes
CD-SAXS (X-ray)              Full profile, bow, taper         0.4 nm               Slow, high accuracy;
                                                                                    calibration
High-voltage SEM             Bottom CD, tilt, twist, bottom   1.0 nm CD,           Slow; sees through 8 µm
(backscatter, 30–100 keV)    opening                          0.03° tilt,          at high voltage
                                                              1.5 nm twist
Cross-section SEM / TEM      Reference profile, layers        0.5 nm               Destructive; hours
E-beam voltage contrast      Blind and shorted holes          Count only           ~ 100 mm²/h
Electrical test (wafer sort) String continuity, leakage       —                    Whole die; post-process
```

### 15.2.1 Precision-to-Tolerance

A measurement is useful when its uncertainty is small compared with the specification range:

```
P/T = 6 σ_meas / (USL − LSL) = 2 × (3σ_meas) / (tolerance range)

Bottom CD, spec 80 ± 4 nm (range 8 nm):
  HV-SEM, 3σ = 1.0 nm:    P/T = 2.0 / 8 = 0.25   (marginal; target ≤ 0.2)
  CD-SAXS, 3σ = 0.4 nm:   P/T = 0.8 / 8 = 0.10   (good, but slow)
  OCD, 3σ = 0.5 nm:       P/T = 0.125            (good, array average only)

Top CD, spec 120 ± 3 nm (range 6 nm):
  CD-SEM 3σ = 0.3 nm:     P/T = 0.10
```

The practical metrology scheme is **OCD for routine bottom CD and depth, calibrated against CD-SAXS and cross-sections**, with HV-SEM at a few sites for tilt, twist, and bottom shape.

---

## 15.3 Why Inspection Cannot Verify the Defect Rate

Voltage-contrast e-beam inspection can detect a blind hole (the bottom is not conducting to the substrate) by its contrast. At about 100 mm² per hour:

```
Holes per mm² = 1 / 0.03464 µm² × 10⁶ µm²/mm² = 2.9×10⁷
Throughput = 100 mm²/h × 2.9×10⁷ = 2.9×10⁹ holes per hour
One whole die (1.33×10⁹ holes): 28 min

To see N_f failures at rate p, one must examine N_f / p holes:
  p = 10⁻⁹:    3 failures needs 3×10⁹ holes → 1.0 h  (about 2.3 die)
  p = 10⁻¹⁰:   3×10¹⁰ holes → 10.3 h  (23 die)
  p = 10⁻¹¹:   3×10¹¹ holes → 103 h   (225 die)
```

Inspection can find systematic failures (a particle, a zone of blind holes), and it can verify rates down to about 10⁻⁹ in a few hours. **The final proof of 10⁻¹¹ comes from electrical test of whole die.** The etch must therefore be controlled by the physics that determines the distribution (Chapter 12): CD uniformity, overetch, tilt, twist, not by inspection of the tail.

---

## 15.4 Advanced Process Control

### 15.4.1 Feed-Forward

```
Input (measured before the etch)       Etch variable adjusted               Gain
──────────────────────────────────────────────────────────────────────────────────────
Mask-open CD (OCD or CD-SEM)           O₂ / C₄F₆ in stages (bottom CD)      1.0 nm/nm
Stack height (film thickness)          Overetch timing / stage times        ΔH → Δt = ΔH/R
Mask thickness                         Bias ramp (mask loss budget)         —
Wafer bow                              Chuck / He staging, edge-zone offset  Table in Ch. 8.5
```

```
Example: stack height is 60 nm above target (deck 1). Stage times scale with depth:
  Extra time = 60 nm / 150 nm/min = 0.4 min = 24 s, added to Stage 4 before the overetch.
  (The 50% endpoint then also adapts: Chapter 12.5.2.)
```

### 15.4.2 Feedback: EWMA Run-to-Run

After each lot (or wafer), the measured bottom CD error e_k relative to target updates the recipe offset u with an exponentially weighted moving average (EWMA):

```
u_{k+1} = u_k − λ · e_k / G        G = process gain (nm per unit of u)

Bottom CD error e_k = +1.8 nm; actuator = O₂ flow in Stage 3, G = +1.0 nm/sccm; λ = 0.4
  Δu = −0.4 × 1.8 / 1.0 = −0.72 sccm
```

The choice of λ trades tracking speed against measurement noise. If the error is purely noise (no drift), the EWMA causes output variance to increase:

```
Variance factor = 1 + λ / (2 − λ)

  λ = 0.2:  1.11  (σ increases 5.4%)
  λ = 0.4:  1.25  (σ increases 11.8%)
  λ = 0.7:  1.54  (σ increases 24%)
```

For a drifting process, a larger λ tracks drift better. For a stable process with noisy metrology, a small λ (0.2–0.3) is better. Many hole-etch controllers use **separate loops**: a slow loop on chamber offsets from OCD, and fast feed-forward on measured mask CD.

### 15.4.3 Virtual Metrology

Because HV-SEM and CD-SAXS sample few wafers, **virtual metrology** estimates each wafer's CD and depth from the chamber trace data (power, voltage, temperature, OES) using a model calibrated against measurements. Its accuracy is judged against the OCD sample, and its role is to fill in the unsampled wafers.

---

## 15.5 Fault Detection

### 15.5.1 The Variables

```
Variable                                  Fault it reveals                        Typical limit
─────────────────────────────────────────────────────────────────────────────────────────────────
Bias voltage at fixed power               Ring wear, wall film, electrode          ±2%
Reflected power, match positions          Impedance shift, arcing                  < 1% reflected
He backside leak per zone                 Bow, ESC seal, particle under wafer      Zone-specific
ESC zone temperatures                     Chiller drift, heater fault              ±0.3 K
MFC deviation (flow vs setpoint)          Gas-delivery fault                       ±1% of setpoint
Pressure vs setpoint                      Throttle, leak                           ±2%
OES intensities (CN, CO, F, Ar)           Wall state, chemistry shift              ±3–5%
Arc counts                                Particles, damage                        0 per wafer
Stage times / endpoint times              Rate change                              ±3%
```

### 15.5.2 Detection Lag and Wafers at Risk

A wafer takes 45 minutes in a chamber, and a lot of 25 wafers takes about 3.1 hours across a six-chamber platform. If an excursion is only detected when the first post-etch measurement of the lot returns after, say, 4 hours:

```
Wafers per chamber during the lag:   4 h / 0.75 h = 5.3
Wafers at risk across six chambers if all are affected: 6 × 5.3 = 32 wafers (1.3 lots)
Wafer value: stack deposit, litho, mask-open and 45 min of the longest etch in the flow
```

Compare with in-situ FDC, which reports on each wafer in real time. For a fault visible in the trace, **at most one wafer per chamber is at risk**. The economics favor investing in per-wafer FDC rather than in more post-etch metrology capacity.

---

## 15.6 Summary & Key Takeaways

1. **Etch products are about 0.4% of the exhaust.** That is enough for OES endpoint, but only for the average arrival.

2. **OCD with calibration is the workhorse.** HV-SEM and CD-SAXS calibrate and check tilt, twist, and bottom shape.

3. **Inspection cannot verify 10⁻¹¹.** It would take about 100 hours of e-beam time; electrical test closes the loop.

4. **EWMA control trades speed for noise.** λ = 0.4 adds 12% to the measured σ if the error is noise.

5. **Feed-forward uses what is known before the etch.** Mask CD and stack height are corrected before the wafer enters the chamber.

6. **Per-wafer FDC limits risk.** Detection at the end of a 4 h lag puts about 32 wafers at risk; FDC puts one per chamber.

---

## Study Questions

1. A hole etch removes 8×10¹⁷ atoms/s, 35% as Si-bearing molecules, in a total flow of 150 sccm. Compute the product concentration in the exhaust.

2. A bottom CD spec of 78 ± 3.5 nm is measured with a tool of 3σ = 0.9 nm. What is P/T? What measurement 3σ is needed for P/T = 0.15?

3. An e-beam inspector runs at 150 mm²/h on a die with 2.5×10⁷ holes/mm². How long to inspect 4×10¹⁰ holes? At p = 5×10⁻¹⁰, how many failures would that find on average?

4. A feedback loop uses λ = 0.3, G = 0.8 nm/sccm. The error is +2.4 nm. Compute the first update in sccm. What is the variance factor for noise-only error?

5. An excursion is detected at lot-level metrology 5 h after it began, in a platform of 5 chambers at 0.8 h per wafer. How many wafers are at risk? How many if FDC detects it after one wafer?

---

**Previous Chapter:** [Chapter 14: Advanced Schemes — Multi-Deck, Joints & 400-Layer Stacks](./14-advanced-multideck.md)  
**Next Chapter:** [Chapter 16: Integration, Yield & Cost of Ownership](./16-integration-yield-coo.md)

---

**Chapter 15 Development Status:** Complete  
**Version:** 1.0
