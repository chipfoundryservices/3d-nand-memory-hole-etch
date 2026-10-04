# Chapter 10: Profile Control — Taper, Bowing & Bottom CD

## Overview

A memory hole is judged by its shape from top to bottom. The reference hole narrows from 120 nm at the top to 80 nm at the bottom, a taper of only 0.14° per side. In that same 8 µm it must not bulge by more than 4 nm, and it must keep its bottom open wide enough for the cell films. Taper, bowing, and bottom CD all come from the same competition: sidewall passivation that narrows the hole against lateral etch from neutrals and off-axis ions that widens it. This chapter turns that competition into numbers, shows how to estimate bowing from the ion physics of Chapter 3, and builds a bottom-CD error budget.

**Learning Objectives:**
- Describe the hole profile with CD(z) and relate taper to net lateral growth rate
- Estimate bowing amplitude from off-axis ion flux and wall sputter yield
- Identify the levers that move taper, bowing, and bottom CD, and their sensitivities
- Explain ellipticity and wall roughness and their transfer from the mask
- Build an RSS error budget for bottom CD

---

## 10.1 Describing the Profile

```
CD(z) = CD_top − 2·T(z) + B(z)

  T(z)  Taper: cumulative net lateral growth per side down to depth z
  B(z)  Bow: localized excess lateral etch (positive widens)
```

```
Reference: CD_top = 120 nm, CD_bottom = 80 nm, H = 8.0 µm
  Taper per side:      (120 − 80)/2 = 20 nm over 8000 nm
  tan α = 20/8000 = 0.0025  →  α = 0.143° from vertical (sidewall angle 89.86°)
  Mean CD:             100 nm
```

At this depth, a 1° sidewall angle (89°) would mean a taper of 140 nm per side, so a 120 nm hole would pinch shut after only 60 nm / tan 1° ≈ 3.4 µm. **Memory holes are almost vertical.** Differences of a few hundredths of a degree separate a good hole from a bad one, which makes the angle itself a poor control metric. Bottom CD and the ratio CD_bottom/CD_top are used instead.

---

## 10.2 Taper: Net Lateral Growth

### 10.2.1 The Etch Front Narrows as It Advances

The width of the hole at depth z is set where the etch front passes through z. At the front, ions are scarce and passivation is not opposed by much sidewall bombardment, so the width of the front shrinks as the hole deepens. Let g_f be the net lateral growth rate (per side) at the front, positive when the front narrows. Since the front moves at the vertical rate R(z), the taper accumulated by depth z is

```
T(z) = ∫₀^z (g_f / R) dz' = ∫₀^t g_f dt'

Reference: g_f = 0.5 nm/min (constant)
  T(H) = 0.5 nm/min × 40 min = 20 nm per side    → CD: 120 nm → 80 nm
```

The wall behind the front is then largely frozen: it sees few ions and little neutral flux, so it changes only through the bowing mechanism of Section 10.3.

### 10.2.2 Why a Slow Etch Rate Matters

Because the front moves at half the rate near the bottom (150 nm/min) as near the top (300 nm/min), the same net lateral growth per minute narrows the hole twice as much per µm of depth:

```
Taper per side per µm of depth = g_f / R

  Near the top (R = 300 nm/min):    0.5 / 300 = 1.7 nm/µm   (0.095° from vertical)
  Near the bottom (R = 150 nm/min): 0.5 / 150 = 3.3 nm/µm   (0.19° from vertical)
```

**The hole closes faster per µm at the bottom than at the top.** This is why bottom-CD margin is lost fastest to any extra passivation (Chapter 11, clogging), and why anything that speeds the bottom etch (more bottom ions, cold HF chemistry) reduces taper without changing the passivation.

### 10.2.3 Levers on Taper

```
Lever                        Direction          Sensitivity (illustrative, bottom CD)
─────────────────────────────────────────────────────────────────────────────────────
O₂ flow +1 sccm              Less polymer       +1.0 nm
C₄F₆ flow −1 sccm            Less polymer       +1.0 nm
Wafer temperature +1 K       Less passivation   +0.6 nm   (Chapter 8.3.1)
Ion energy +5%               More pass, bottom  +0.8 nm
                             etch
Pressure −1 mTorr            Fewer polymers     +0.5 nm
Top CD (mask opening) +1 nm  Direct transfer    +1.0 nm
```

---

## 10.3 Bowing

### 10.3.1 Mechanism

Bowing is lateral etch concentrated at one depth. Chapter 3.6 showed that ions outside the acceptance cone strike the wall at grazing angles, where they reflect, and the sputter yield of the wall rises sharply at 60–70° from the normal and again for ions that arrive at moderate grazing. Neutral fluorine reaching the wall where the polymer is thin adds to the effect.

### 10.3.2 Estimating the Amplitude

```
Reference (Chapter 3.2.3): 7.5×10⁵ ions/s enter each hole; 53% are outside the
acceptance cone at full depth → 4.0×10⁵ ions/s hit the wall
Wall area over the upper 4 µm of the hole (D = 100 nm):
  π × 100 nm × 4000 nm = 1.26×10⁶ nm² = 1.26×10⁻⁸ cm²
Wall ion flux = 4.0×10⁵ / 1.26×10⁻⁸ = 3.2×10¹³ cm⁻² s⁻¹    (0.5% of the 6.6×10¹⁵ top flux)

Wall removal yield (grazing, with polymer present) Y_w ≈ 0.2 atoms/ion
  Atom flux removed = 3.2×10¹³ × 0.2 = 6.4×10¹² cm⁻² s⁻¹
  Lateral etch rate = 6.4×10¹² / 7.5×10²² cm⁻³ = 8.5×10⁻¹¹ cm/s = 0.051 nm/min
  Over 40 min:        2.0 nm per side  →  4 nm in CD
```

The estimate lands on the spec limit (bowing ≤ +4 nm). It also shows what moves it:

```
Bowing ∝ (1 − f_pass) × Y_w × exposure time

  Raise the ion energy 4 → 6 keV: (1 − f) from 0.535 → 0.39 (−27%)
  Cut wall yield by thicker polymer (Y_w 0.2 → 0.12): −40%
  Cut etch time by 25% (cryo, faster): −25%
```

### 10.3.3 Where the Bow Sits

```
Depth of the bow   Likely cause                          Remedy
──────────────────────────────────────────────────────────────────────────
Top 0–1 µm         Mask facet reflection;                Mask CD and slope control
                   high-pressure neutral etch            (Chapter 11); polymer at top
1–3 µm             Ions deflected near the top;          Raise bias earlier; pulsed bias
                   reflected, landing here
(z_b ≈ w̄/(2 tan θ_r), Ch. 3.6)
Deep (> 4 µm)      Charging deflection; polymer lost     Pulsed bias; temperature; O₂
                   to higher temperature at depth        trim at depth
At layer changes   Thick select/joint layer changes      Matched recipe step at the layer
                   local etch
```

---

## 10.4 Bottom CD

The bottom CD sets the room for the cell films (Chapter 1.2.1) and the contact area to the source. It is the narrowest dimension and the one with the least tolerance.

```
CD_bottom = CD_top − 2 · T_total + B(H)

Reference spec: 80 nm ± 4 nm (3σ)
Film allowance: 46 nm for the full ONO + poly stack
Minimum core: 20 nm (to avoid pinch-off and a void) → CD_bottom,min = 66 nm
```

So the lower limit of 76 nm (80 − 4) still leaves a 30 nm core, 10 nm above the pinch-off limit. A bottom-CD error of −14 nm is the point where the cell films begin to pinch off.

---

## 10.5 Ellipticity and Wall Roughness

### 10.5.1 Ellipticity

A hole is elliptical if its CD in one direction differs from the other:

```
Ellipticity = (CD_max − CD_min) / CD_mean
Spec ≤ 3%  →  3 nm on a 100 nm hole
```

Sources are asymmetric ion deflection (charging, tilt: Chapter 3.5, 13), mask openings that are not round, and the hex-array neighbor geometry (a hole with six nearest neighbors sees a symmetric environment, but at the array edge the environment is not symmetric).

### 10.5.2 Roughness

The mask's line-edge roughness (LWR) transfers into the hole and is amplified or smoothed depending on the etch:

```
σ_wall = T_r · σ_mask,   T_r = transfer coefficient (0.4–0.8)

Mask roughness 2.5 nm rms, T_r = 0.6 → σ_wall = 1.5 nm (spec ≤ 2.0 nm)
Layer-to-layer modulation (oxide/nitride mismatch, Chapter 2.2.2): +0.5–1.3 nm,
  added in quadrature with the mask-derived roughness:
  √(1.5² + 1.0²) = 1.8 nm
```

---

## 10.6 Bottom-CD Error Budget

```
Contributor (3σ, bottom CD)                          nm
─────────────────────────────────────────────────────────────
Mask opening CD (litho + mask open)                   1.5
Taper (passivation variation, hole-to-hole)            1.5
Wafer temperature (±0.5 K × 0.6 nm/K)                  0.3
Gas flows (O₂, C₄F₆, ±0.3 sccm)                        0.9
Ion energy (±2% bias voltage)                          1.0
Chamber state (first wafer, ageing, matching)          1.0
Residual random (hole-to-hole, stochastic)             1.2
─────────────────────────────────────────────────────────────
RSS = √(1.5² + 1.5² + 0.3² + 0.9² + 1.0² + 1.0² + 1.2²)   = 2.97 nm
Spec ±4.0 nm → margin 1.0 nm (25%)
```

Two items dominate: the mask CD and the taper variation. Both are largely outside the etch's direct control: the first is set upstream (Chapter 11.1), and the second is a statistical variation from polymer redeposition and charging.

---

## 10.7 Summary & Key Takeaways

1. **The hole is almost vertical.** Taper of 0.14° per side takes 120 nm to 80 nm over 8 µm.

2. **Net lateral growth sets taper.** Per µm of depth the bottom narrows about twice as fast as the top, because the etch rate there is half.

3. **Bowing is an ion-reflection effect.** About 0.5% of the ion flux hits the wall and removes 2 nm per side over the etch, close to the spec.

4. **Bottom CD has the least tolerance.** A 76 nm bottom still leaves a 30 nm core; pinch-off begins near 66 nm.

5. **Ellipticity and roughness are copied into the cell.** Mask roughness of 2.5 nm gives 1.5 nm of wall roughness.

6. **Bottom CD budget closes at about 3 nm (RSS).** Mask CD and taper variation dominate.

---

## Study Questions

1. A hole has CD_top = 112 nm, CD_bottom = 68 nm, and H = 7.2 µm. Find taper per side, the sidewall angle, and the mean CD. How does the bottom/top ratio compare to the reference?

2. Using the bowing estimate of Section 10.3.2, compute the bowing per side if the ion energy is raised to 5 keV (f_pass = 54%) with the other parameters unchanged and the etch time falls to 36 min.

3. A recipe change adds 0.7 sccm of O₂ and drops wafer temperature by 1.5 K. Using the sensitivities in Section 10.2.3, find the net bottom-CD change.

4. For a core minimum of 25 nm, film allowance 50 nm per both sides, find the minimum bottom CD. If the bottom-CD budget is RSS 3.2 nm (3σ), what nominal CD is needed to hold a 3σ margin?

5. Mask LWR is 3.0 nm rms and T_r = 0.55. If the layer modulation is 1.2 nm, compute the total wall roughness. Does it meet the 2.0 nm spec?

---

**Previous Chapter:** [Chapter 9: Chamber Conditioning, Wall Memory & Consumables](./09-chamber-conditioning.md)  
**Next Chapter:** [Chapter 11: Mask Budget — Selectivity, Faceting & Clogging](./11-mask-budget-clogging.md)

---

**Chapter 10 Development Status:** Complete  
**Version:** 1.0
