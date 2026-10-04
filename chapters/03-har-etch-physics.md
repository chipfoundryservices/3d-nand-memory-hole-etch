# Chapter 3: High-Aspect-Ratio Etch Physics — Ion and Neutral Transport

## Overview

A memory hole is a tube 100 nm wide and 8 µm long, and everything that does the etching has to travel down it. Ions fall in from the sheath, neutral radicals wander in by random collisions with the walls, electrons mostly stay out, and reaction products have to find their way back up. None of these journeys scales the same way with depth. This chapter builds a transport picture of the hole and uses it to explain why the etch rate falls with depth (ARDE), why multi-keV ion energy is needed, why charging twists holes, and why the neutral supply at the bottom is a small fraction of what the plasma provides at the top.

**Learning Objectives:**
- Compute the fraction of ions that reach the hole bottom from the ion angular spread and the geometric acceptance angle
- Estimate neutral transmission to the bottom by Clausing and diffusion-with-sticking models
- Derive and apply the ARDE relation R = R₀/(1 + A/A₀), and integrate it for depth versus time
- Estimate ion deflection from differential charging
- Describe how ion reflection from the sidewall makes bowing
- Estimate product density at the hole bottom

---

## 3.1 The Hole as Three Transport Problems

```
Species            Direction   Driven by                   Scales with depth as
─────────────────────────────────────────────────────────────────────────────────────
Ions (CFx⁺, Ar⁺)   Down        Sheath field, directed      Fraction passing falls with A,
                               at several keV              rises with energy
Neutral radicals   Down        Random walk, wall           Falls exponentially with depth
 (F, CFx, O)                   collisions                  when sticking is non-zero
Electrons          Mostly      Thermal, isotropic          Shadowed by mask: little reaches
                   blocked                                 the bottom
Reaction products  Up          Random walk                 Density at the bottom rises
 (SiF₄, CO, HCN)                                           with depth
Surface species    Down walls  Surface diffusion,          Fraction reaching bottom
                               physisorption               depends on temperature
```

The three that matter most for the etch rate are **ions**, **neutral radicals**, and **surface species**. They fail in different ways, and the recipe has to balance them.

---

## 3.2 Ion Transport

### 3.2.1 Angular Spread

An ion falling through the sheath with energy E_i gains almost all of it along the surface normal. Its transverse energy is set by the ion temperature before the sheath (T_i ≈ 0.3–1 eV in a low-pressure plasma), and collisions inside the sheath. For a Gaussian transverse velocity distribution:

```
σ_θ ≈ √(T_⊥ / E_i)    (radians, per axis)

Reference: T_⊥ = 0.5 eV
  E_i = 2 keV:   σ_θ = 15.8 mrad = 0.91°
  E_i = 4 keV:   σ_θ = 11.2 mrad = 0.64°
  E_i = 10 keV:  σ_θ =  7.1 mrad = 0.41°
```

### 3.2.2 Geometric Acceptance

An ion that enters a hole of mean width w̄ and depth z can reach the bottom without touching the wall only if its angle from the axis is below a limit of roughly

```
θ_c ≈ w̄ / z = 1/A     (rough acceptance half-angle)

At full depth, A = 80:  θ_c = 12.5 mrad = 0.72°
```

For a 2D Gaussian with per-axis spread σ_θ, the fraction of ions within a circular cone of half-angle θ_c is

```
f_pass = 1 − exp(−θ_c² / (2σ_θ²))
```

```
Ion energy    σ_θ       f_pass (A = 80, θ_c = 12.5 mrad)
────────────────────────────────────────────────────────
 2 keV      15.8 mrad       27%
 4 keV      11.2 mrad       47%
 5 keV      10.0 mrad       54%
10 keV       7.1 mrad       79%
```

**This is the reason hole etch uses multi-keV bias.** At conventional etch energies of a few hundred eV, σ_θ is 40 mrad and f_pass at A = 80 is under 5%. Ions that fail the cone do not vanish; they strike the sidewall, where they sputter, reflect, and deposit energy (Section 3.5), and they are the source of bowing.

### 3.2.3 Ion Flux and Yield at the Bottom

```
Reference ion flux at the wafer surface:
  Power to the wafer ≈ 3.0 kW (about 30% of the 10 kW bias, rest in sheath
    and capacitive loss)
  Area 707 cm² → 4.24 W/cm²
  Mean ion energy 4 keV → ion current density = 4.24 / 4000 = 1.06 mA/cm²
  Ion flux Γ_i = 1.06×10⁻³ / 1.60×10⁻¹⁹ = 6.6×10¹⁵ ions cm⁻² s⁻¹

Per hole, at the top (CD 120 nm, area 1.13×10⁻¹⁰ cm²):
  7.5×10⁵ ions/s enter each hole
  47% pass at full depth → 3.5×10⁵ ions/s reach the bottom
```

```
Bottom etch volume rate (hole bottom 80 nm, rate 150 nm/min):
  Area 5.03×10⁻¹¹ cm² × 1.5×10⁻⁵ cm/min = 7.5×10⁻¹⁶ cm³/min = 1.26×10⁻¹⁷ cm³/s
  × 7.5×10²² atoms/cm³ (average ON) = 9.5×10⁵ atoms removed per second

Atoms removed per arriving ion = 9.5×10⁵ / 3.5×10⁵ ≈ 2.7
```

A yield of about 3 atoms per ion at 4 keV is far above pure physical sputtering (about 0.5 for Ar⁺ on SiO₂). The bottom of the hole is therefore running a **chemically enhanced** etch, in which each ion also activates adsorbed fluorocarbon. The chemical part needs a supply of neutral or surface-borne species at the bottom (Sections 3.3 and 3.4).

---

## 3.3 Neutral Transport

### 3.3.1 Clausing Transmission (No Sticking)

For a molecule entering a cylindrical tube, with isotropic (cosine) entry and diffuse reflection at the wall, the probability of reaching the far end without sticking is the Clausing factor. For long tubes:

```
W ≈ 1 / (1 + 3L/(8R))     (L = length, R = radius)

L = 8 µm, R = 50 nm (L/R = 160):   W = 1 / (1 + 60) = 0.0164
L = 8 µm, R = 60 nm (L/R = 133):   W = 0.0196
L = 4 µm, R = 50 nm (L/R = 80):    W = 0.0323
L = 16 µm, R = 50 nm (L/R = 320):  W = 0.0083
```

Even for a neutral that **never sticks**, only about 1.6% of arrivals at the opening make it to the bottom, and the number falls linearly with depth.

### 3.3.2 Diffusion With Wall Loss (Sticking)

If a fraction s of wall collisions removes the neutral (by sticking, reacting, or recombining), its density falls roughly exponentially with depth. Treating the hole as a one-dimensional diffusion problem with Knudsen diffusivity D_K = (2R/3) v̄ and a wall loss rate per unit volume of s n v̄/(2R):

```
n(z) = n₀ exp(−z/λ),     λ = D / √(3s)    (D = 2R hole diameter)

D = 100 nm:
  s = 10⁻¹:    λ = 183 nm
  s = 10⁻²:    λ = 577 nm
  s = 10⁻³:    λ = 1.83 µm   →  n(8 µm)/n₀ = exp(−4.4) = 1.2%
  s = 10⁻⁴:    λ = 5.8 µm    →  n(8 µm)/n₀ = exp(−1.4) = 25%
```

Fluorine atoms on a fluorocarbon-covered wall have s of 0.01 to 0.1, and CFx radicals stick with a probability of a few percent. At those values, essentially **no gas-phase radical survives to the bottom of an 8 µm hole**. Whatever neutral supply exists at the bottom must come by other routes:

1. Ion-driven sputtering and reflection of CFx fragments down the wall
2. **Surface diffusion** of physisorbed species along the sidewall
3. Low-sticking neutral species (HF, O₂) that do not use up their flux on the wall

Route 3 is the main reason hydrogen fluoride chemistry has become attractive at low wafer temperature (Chapter 4.5): HF adsorbs on cold oxide and nitride, does not react with the polymer-covered wall, and survives many wall collisions.

---

## 3.4 Aspect-Ratio-Dependent Etch Rate (ARDE)

### 3.4.1 The Empirical Form

The etch rate of a hole falls with its aspect ratio. A widely used empirical relation is

```
R(A) = R₀ / (1 + A/A₀)

A = z / w̄ (local aspect ratio), R₀ = rate in the limit A → 0,
A₀ = aspect ratio at which the rate has fallen to half
```

Reference values: R₀ = 300 nm/min, A₀ = 80, w̄ = 100 nm.

```
Depth z    A     R (nm/min)   R/R₀
─────────────────────────────────────
 0.0 µm     0       300       1.00
 2.0 µm    20       240       0.80
 4.0 µm    40       200       0.67
 6.0 µm    60       171       0.57
 8.0 µm    80       150       0.50
```

### 3.4.2 Depth Versus Time

Since dz/dt = R(A) and z = A w̄:

```
dt = (w̄ / R₀) · (1 + A/A₀) dA
t(A) = (w̄/R₀) · (A + A²/(2A₀))

Reference (w̄/R₀ = 100/300 = 0.333 min):
  t(40) = 0.333 × (40 + 10)  = 16.7 min   (half depth)
  t(80) = 0.333 × (80 + 40)  = 40.0 min   (full depth)
```

The first half of the hole is etched in 42% of the time and the second half in 58%. At constant total width, doubling the depth to A = 160 gives t = 0.333 × (160 + 160) = 107 min, about 2.7 times the time, which is why the industry stacks decks rather than etching deeper (Chapter 14).

### 3.4.3 Relation to the Transport Picture

ARDE is not a single mechanism. The neutral flux falls exponentially with depth (Section 3.3), the ion flux falls as the cone narrows (Section 3.2), and the surface-borne supply is shared along a longer wall. The empirical form R₀/(1 + A/A₀) fits all three over the range of production interest and is convenient for planning. Recipes can shift A₀ (Chapters 6 and 7) by changing ion energy, wall temperature, and chemistry, but it cannot be removed.

```
Lever                         Effect on A₀ (illustrative)
──────────────────────────────────────────────────────────────
Ion energy 4 → 6 keV          A₀ +15–25%  (more ions pass the cone)
Wafer temperature −20 °C       A₀ +10–20%  (more HF/precursor adsorbed;
                               lower wall loss)
Hole CD +10%                  A₀ effectively unchanged, A falls 10%
Pulsed bias (Ch. 6)           A₀ +10–30%  (less charging, reduced
                               neutral depletion)
```

---

## 3.5 Differential Charging

### 3.5.1 The Mechanism

At the surface of the hole, ions arrive in a narrow cone aimed down the axis, and electrons arrive isotropically with thermal energy (T_e ≈ 3–4 eV). The hole's high aspect ratio **shadows the electrons**: only those entering nearly vertically reach the bottom. The mask top charges negative, the bottom charges positive, and a field develops in the hole.

During the RF cycle, positive ions are accelerated by bias that varies from the peak to near zero, so ions arriving at low sheath voltage are slow and easily deflected. At the reference 400 kHz bias frequency, ions in the hole experience a sheath that changes slowly compared with their transit time, so the charging pattern is stable on the timescale of the hole.

### 3.5.2 Deflection of a Fast Ion by a Small Potential

A potential difference ΔV across the hole width gives the ion a transverse kick. The ion angle changes by

```
δθ ≈ √(e·ΔV / E_i)

E_i = 4 keV:
  ΔV = 0.1 V:  δθ = 5.0 mrad  (0.29°)
  ΔV = 0.5 V:  δθ = 11.2 mrad (0.64°)    ← equal to σ_θ
  ΔV = 1.0 V:  δθ = 15.8 mrad (0.91°)    ← larger than the whole acceptance cone
```

**A potential asymmetry of half a volt across a hole bends a 4 keV ion by as much as its entire thermal spread.** Since charge on a dielectric wall is local and random, the deflection is random from hole to hole and from layer to layer. This is the origin of **twisting** (Chapter 13) and of the near-bottom deflection that gives elliptical bottoms.

### 3.5.3 Counteracting Charging

```
Method                               Mechanism                      Chapter
──────────────────────────────────────────────────────────────────────────────
Higher ion energy                    Smaller δθ for a given ΔV      6
Pulsed bias, afterglow               Negative ions and electrons    6
                                     enter the hole in the off-time
                                     and neutralize charge
Wall conductivity (polymer, heat)    Charge leaks                   4, 8
Lower pressure                       Longer mean free path in the   7
                                     sheath → narrower angle
```

---

## 3.6 Ions at the Sidewall: Reflection and Bowing

An ion outside the acceptance cone strikes the wall at a **grazing angle**. For a hole 100 nm wide and an ion tilted 1° from the axis, the angle of incidence on the wall (measured from the surface normal) is about 89°. At such angles:

```
Angle from the surface normal    Behavior
──────────────────────────────────────────────────────────────────────
0–40°                            Absorbed, physical sputtering yield low
40–70°                           Sputter yield peaks (3× normal at ~60°)
> 80°                            Specular reflection, little energy lost
```

Most off-axis ions strike at > 85° and **reflect specularly with energy retained**, so they travel on down the hole, hitting the wall again lower. The impact point of an ion deflected near the top is far below, and that is where bowing appears: the wall at depth z_b receives ions reflected from above, with enough energy to sputter or remove polymer. Mask sidewalls produce a related effect: ions reflect from a faceted mask edge onto the upper hole wall, giving **top bowing** and **top CD growth** (Chapter 10).

```
Roughly: bowing position z_b ≈ w̄ / (2·tan θ_r) with θ_r the reflection angle
  θ_r = 1° → z_b ≈ 100 nm / (2 × 0.0175) = 2.9 µm
  θ_r = 2° → z_b ≈ 1.4 µm
```

---

## 3.7 Reaction Products

For each ion that reaches the bottom, the etch generates volatile products (SiF₄, CO, CO₂, HCN, NF₃ depending on chemistry) that must leave through the same 8 µm tube. At the reference rate:

```
Products generated: ~10⁶ molecules/s per hole (Section 3.2.3)
Conductance of the hole for SiF₄ (v̄ = 247 m/s at 300 K, R = 50 nm, W = 0.018):
  C = W · A_h · v̄ / 4 = 0.018 × 7.85×10⁻¹⁵ m² × 247 / 4 = 8.7×10⁻¹⁵ m³/s

Density rise at the bottom = flux / C = 10⁶ / 8.7×10⁻¹⁵ = 1.1×10²⁰ m⁻³
Gas density at 20 mTorr, 300 K:  n = p / k_B T = 6.4×10²⁰ m⁻³
Ratio: 17%  (equivalent to about 3.4 mTorr of product at the hole bottom)
```

The product density at the bottom is **not negligible**. It dilutes the local etchant and favors **redeposition** on the lower wall, which is a source of hole clogging (Chapter 11). Products are also partly ionized and re-sputtered, so the true figure depends on the chemistry; the estimate shows the order of magnitude, not a prediction.

---

## 3.8 Summary & Key Takeaways

1. **Ion transport is angle-limited.** At A = 80, the acceptance cone is 0.72°. Only multi-keV ions with σ_θ near 0.6° or less deliver a useful fraction (47% at 4 keV).

2. **Neutral transport is sticking-limited.** Even a non-sticking neutral is transmitted at 1.6%. A radical with s = 0.01 is gone within 600 nm. The bottom runs on ion-driven and surface-borne chemistry.

3. **ARDE follows R₀/(1 + A/A₀).** With R₀ = 300 nm/min and A₀ = 80, the rate halves between the top and bottom and the etch takes 40 minutes.

4. **Charging is a sub-volt effect for fast ions.** 0.5 V deflects a 4 keV ion by its full thermal spread, making twisting a statistical, hole-by-hole process.

5. **Off-axis ions bounce.** Grazing reflection carries energy far down the wall and places bowing where they finally land.

6. **Products accumulate at the bottom.** At 17% of the chamber gas density, they dilute chemistry and feed redeposition.

---

## Study Questions

1. For an ion energy of 6 keV and T_⊥ = 0.6 eV, compute σ_θ. For a hole with w̄ = 90 nm and depth 7.5 µm, compute the acceptance angle and the fraction of ions passing.

2. A neutral radical has a sticking coefficient s = 0.003 in a hole of D = 90 nm. Compute the decay length λ and the fraction of the opening density that reaches 6 µm. How low would s have to be for 50% to survive 6 µm?

3. With R₀ = 280 nm/min, A₀ = 70, and w̄ = 95 nm, compute the time to etch 7.0 µm. If A₀ is raised to 90 by a recipe change (all else equal), what is the new time and the percentage saving?

4. A hole bottom has a lateral potential difference of 0.3 V. For a 3 keV ion, compute the deflection angle. What lateral displacement results over the last 300 nm of travel?

5. Compute the product density at the bottom if the etch rate at the bottom falls to 100 nm/min for a bottom CD of 70 nm, assuming the same atom density and 3 atoms removed per molecule (W = 0.018, R = 45 nm). Compare with the 20 mTorr gas density.

---

**Previous Chapter:** [Chapter 2: The ONON Stack — Materials, Deposition & Stress](./02-onon-stack-materials.md)  
**Next Chapter:** [Chapter 4: Etch Chemistries — Fluorocarbons, Hydrofluorocarbons & Cryogenic HF](./04-etch-chemistries.md)

---

**Chapter 3 Development Status:** Complete  
**Version:** 1.0
