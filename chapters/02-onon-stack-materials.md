# Chapter 2: The ONON Stack — Materials, Deposition & Stress

## Overview

The hole etch cuts through 256 alternating films of silicon dioxide and silicon nitride. Neither film is a pure, textbook material. Both are PECVD layers with hydrogen in them, different densities, different stress, and thickness variation that adds up over 8 µm. How well the etch performs depends heavily on those properties: whether oxide and nitride etch at the same rate, how much the wafer bows on the chuck, how far the bottom of the stack is from the top, and what is underneath when the etch arrives.

This chapter describes the stack the etch inherits, and what it asks of the plasma.

**Learning Objectives:**
- Describe the SiO₂/Si₃N₄ (ON) and SiO₂/poly-Si (OP) stacks and their top and bottom layers
- Relate PECVD film properties (density, hydrogen, composition) to etch rate
- Compute wafer bow from stack stress with the Stoney equation
- Estimate how stack height variation maps into etch time and overetch margin
- Explain why a non-selective etch of two different films leaves wall modulation

---

## 2.1 The Two Stack Types

### 2.1.1 ON Stacks (Replacement Gate)

The dominant stack alternates **SiO₂ (O)** and **Si₃N₄ (N)**. The nitride is a placeholder: after the holes are etched and filled, slits are etched, the nitride is removed by hot phosphoric acid, and tungsten is deposited into the gaps as the word lines.

```
Reference ON pair:      SiO₂  28 nm
                        Si₃N₄ 32 nm        →  pitch p = 60 nm
Reference deck:         128 pairs
                        + select gates, dummy layers, bottom and top caps
                        = 0.32 µm
Total stack height H:   128 × 60 nm + 0.32 µm = 8.0 µm
```

### 2.1.2 OP Stacks (Gate-First, Floating-Gate)

Some devices alternate **SiO₂ and doped polysilicon**. The poly layers become the gates directly, so no replacement step is needed. For the hole etch, this changes the chemistry sharply: silicon etches with halogen chemistry (HBr, Cl₂, SF₆) rather than fluorocarbons, and the sidewall passivation is silicon oxide and bromide rather than fluorocarbon polymer (Chapter 4.6). The plasma physics (ion angular spread, charging, Knudsen transport) are the same.

```
Property              ON stack                   OP stack
────────────────────────────────────────────────────────────────────────
Gate formation        Replacement (W)            Direct (doped poly)
Etch chemistry        Fluorocarbon / HF          Fluorocarbon for oxide,
                                                 HBr / Cl₂ / SF₆ for poly
Layer etch-rate       Close to 1:1 achievable    Hard to match; oxide and
matching                                         poly differ by chemistry
Wall modulation       Small (1–2 nm)             Larger (3–6 nm) without
                                                 careful design
Share of products     Majority                   Minority
```

The rest of this book uses the ON stack as the reference.

---

## 2.2 PECVD Films and Their Etch Properties

```
Property                      PECVD SiO₂ (TEOS)    PECVD Si₃N₄ (SiH₄/NH₃)
────────────────────────────────────────────────────────────────────────────
Thickness (reference)         28 nm                32 nm
Density                       2.2 g/cm³            2.7–2.9 g/cm³
Atom density                  6.6×10²² cm⁻³        8.4×10²² cm⁻³
Refractive index (633 nm)     1.46                 1.90–2.00
Hydrogen content              1–3 at%              15–20 at%
Stress (single film)          −180 MPa             +100 MPa
Dep. temperature              350–400 °C           350–400 °C
Wet etch rate (BOE 10:1)      ~ 1 (reference)      low
Plasma etch behavior          Ion-assisted,        Ion-assisted, needs
                              fluorocarbon-driven  hydrogen or CHxFy to
                                                   remove N as HCN/CN
```

(Illustrative values; real films vary with deposition conditions.)

### 2.2.1 What the Etch Must Do With Two Films

The two films need different chemistry in their own right. Oxide etches by fluorocarbon radicals chemisorbing on a CFx-covered oxide surface, with oxygen removed as CO and CO₂. Nitride needs hydrogen to take nitrogen away as HCN and NH-containing products, so hydrofluorocarbons (CH₂F₂, CH₃F, CHF₃) help. A **non-selective** hole etch has to remove both at about the same rate, in a single plasma, for the entire 8 µm.

```
Target for the hole etch:   R_oxide : R_nitride = 1.00 ± 0.05  (vertical and lateral)
Why:                        Any mismatch leaves wall steps, and the steps are
                            repeated 128 times.
```

### 2.2.2 Wall Modulation From Rate Mismatch

A sidewall that sees mean lateral etch d per side, with nitride-to-oxide lateral rate ratio ρ, ends up with a step per side of

```
Step per side = 2d · (ρ − 1) / (ρ + 1)

Example: d = 5 nm lateral etch, ρ = 1.3
  step = 2 × 5 × 0.3 / 2.3 = 1.3 nm per side  →  2.6 nm in CD

Example: ρ = 1.1:  step = 10 × 0.1/2.1 = 0.48 nm per side
```

A wavy wall is a periodic variation of CD with a 60 nm period. It matters for two reasons. The ALD films laid over it carry the modulation into every cell, and the cell-to-cell variation in field and charge trapping follows. The roughness also seeds hole-to-hole variation, because a rough wall catches ions at random angles.

### 2.2.3 Hydrogen Content

PECVD nitride contains 15–20 at% hydrogen. In the etch, this hydrogen is a benefit (it helps remove nitrogen). Later, during high-temperature steps such as the channel anneal, it outgasses and relaxes film stress, changing wafer bow. For the etch, the practical consequence is that **run-to-run changes in nitride hydrogen content change the nitride etch rate** and therefore the oxide-to-nitride ratio. A 1 at% shift in hydrogen can move R_nitride by about 1–2%.

---

## 2.3 Film Stress and Wafer Bow

### 2.3.1 Stack Stress

Oxide is compressive and nitride is tensile (in this reference), so they partially cancel:

```
Layer-weighted average stress:
  σ_stack = (t_O · σ_O + t_N · σ_N) / (t_O + t_N)
          = (28 × (−180) + 32 × (+100)) / 60
          = (−5040 + 3200) / 60 = −30.7 MPa   (net compressive)
```

A change of 10 MPa in the nitride stress changes the net by 5.3 MPa, so a small shift in deposition conditions moves the bow by a noticeable amount.

### 2.3.2 Stoney Equation

For a thin film on a thick substrate:

```
σ_f · t_f = E_s' · t_s² / (6 R),   E_s' = E_s / (1 − ν_s) ≈ 180 GPa for Si(100)

Curvature  κ = 1/R = 6 σ_f t_f / (E_s' t_s²)
Bow (center-to-edge) δ ≈ D² / (8R)
```

```
Reference: σ = −30 MPa, t_f = 8.0 µm, t_s = 775 µm, D = 300 mm

  1/R = 6 × 30×10⁶ × 8×10⁻⁶ / (180×10⁹ × (775×10⁻⁶)²)
      = 1440 / (1.081×10⁵) = 0.0133 m⁻¹  → R ≈ 75 m
  δ = (0.3)² / (8 × 75) = 1.5×10⁻⁴ m = 150 µm
```

Both sides of the wafer matter: films on the backside add a counter-bow, and many 3D NAND flows deposit a **backside film** to reduce the front-side bow before the etch.

### 2.3.3 What Bow Does to the Etch

```
Consequence                      Mechanism                         Chapter
──────────────────────────────────────────────────────────────────────────────
Poor edge contact with the ESC   Helium leak and hot edge          8
Wafer temperature non-uniform    Edge hotter → faster polymer      8, 10
                                 loss → CD and profile shift
Edge tilt                        Wafer lifts at the edge, so the   13
                                 sheath meets the surface at an
                                 angle
Overlay error                    Wafer shape changes between       Lithography
                                 litho and etch
Changes during the etch          Removing 0–8 µm of stack changes  8
                                 stress and bow (a few %)
```

```
Typical handling limit: bow within ±200 µm for chucking with standard
clamp voltage; beyond that, staged or high-voltage chucking is required.
```

---

## 2.4 Layer Thickness and Total Height

### 2.4.1 Uniformity of Deposition

Within-wafer thickness variation of a PECVD pair is typically 1–1.5% (3σ). Random layer-to-layer errors average out over 128 pairs, but systematic shifts (center-thick or edge-thick) do not:

```
Random layer error σ_L = 0.5% per pair (independent):
  Cumulative: σ_H = σ_L · p · √128 = 0.005 × 60 nm × 11.3 = 3.4 nm   (negligible)

Systematic across-wafer shift of 1.5% (3σ) in every pair:
  ΔH = 0.015 × 7.68 µm = 115 nm  (3σ, within wafer)
```

### 2.4.2 Depth Spread and Overetch Margin

The etch must reach the landing layer on **every** hole. Two independent spreads decide how far past nominal the slowest or deepest-target hole must go:

```
Reference (3σ):
  Stack height range, systematic within wafer:      ±115 nm  (Section 2.4.1)
  Etch-depth spread from rate non-uniformity (1.5%
    of 8.0 µm, hole-to-hole and across wafer):      ±120 nm

  Combined (RSS):  √(115² + 120²) = 166 nm
```

The etch is run past nominal depth by this amount (plus margin). In the overetch, the oxide and nitride are being removed faster than the polysilicon landing layer, by the selectivity S_Si:

```
Poly recess spread = 166 nm / S_Si
  S_Si = 6:   28 nm
  S_Si = 3:   55 nm

Nominal recess (hole that lands exactly on time): ~30 nm
Recess range with S_Si = 6:  30 to 58 nm   ← matches the 30–60 nm target
```

At the bottom rate of 150 nm/min, 166 nm of overetch is about 66 s of extra etch for the worst-case hole, during which the best-case holes are already in the landing layer. Depth uniformity is therefore among the tightest specifications in the module: each additional 1% of depth spread (80 nm) costs about 13 nm of recess range at S_Si = 6, or adds 32 s of bottom-condition overetch time that the profile must survive (Chapter 12).

---

## 2.5 The Top and Bottom of the Stack

### 2.5.1 The Bottom: Landing Layers

```
Reference bottom structure (top to bottom)

  Alternating ON pairs (128)
  Bottom select gate region            ~120 nm (thicker oxide + dummy)
  Bottom oxide                         ~60 nm
  ─────────────────────────────────────────────────
  Source-line polysilicon (or SEG base) ~120 nm   ← landing layer
  Buried oxide / CMOS dielectric
```

The hole etch must reach the polysilicon or the epitaxial seed and **stop in it**, with a recess of 30–60 nm. Stopping there depends on selectivity of oxide and nitride etch to silicon (typically > 5:1 in fluorocarbon plasma) and the shortness of the overetch step (Chapter 12).

### 2.5.2 The Top: Select Gates and Cap

The top of the stack has thicker layers for the drain-side select gates and a thick cap oxide. The etch starts through the cap, where the rate is highest (A is near zero) and the mask is thickest. The cap and select-gate region account for 0.3–0.4 µm of the 8.0 µm.

### 2.5.3 Special Layers Inside the Stack

```
Layer                      Purpose                         Etch consideration
────────────────────────────────────────────────────────────────────────────────
Dummy word lines           Reduce edge-cell disturb        Same as regular
Thick oxide / nitride      Select gate, joint (multi-deck) Rate change; wall step
Etch-stop nitride          Optional bottom stop            Needs selective step
Joint (multi-deck)         Interface between decks         Chapter 14
```

Any layer thicker than the standard pair is seen by the plasma as a **change in the etch condition** while the profile continues to develop. These are usually the first places where a bowing or a twist begins.

---

## 2.6 Defects Born in the Stack

A particle or void in the stack acts as a masking defect for the etch. A particle of 80 nm embedded at pair 40 in the path of a hole stops the etch locally, producing a **blind hole** that never reaches the bottom. For the reference die:

```
Particle adders: 0.01 /cm² of ≥ 80 nm per stack deposition (illustrative)
Wafer area ≈ 700 cm² → 7 particles per wafer
Fraction of holes affected: each particle blocks ≈ 1–3 holes
→ ~15 blind holes per wafer from stack particles, among 10¹² holes
→ p ≈ 1.5×10⁻¹¹ (contributes ~2% die-yield loss from this mechanism alone, Ch. 1.6)
```

Stack-borne particles are not an etch problem, but the etch is the step that exposes them, and the **blind-hole** signature is the same whether the cause is a particle or etch starvation. Chapter 15 describes how to tell them apart.

---

## 2.7 Summary & Key Takeaways

1. **The stack is two different films.** Oxide and nitride etch by different chemistry, and the hole etch has to match their rates to within about 5%.

2. **Rate mismatch becomes wall modulation.** A ratio of 1.3 on 5 nm lateral etch leaves 2.6 nm of CD modulation, repeated every 60 nm.

3. **Stack stress sets wafer bow.** A net compressive stress of 30 MPa on 8 µm gives about 150 µm of bow, which affects chucking, temperature, and edge tilt.

4. **Height variation sets overetch.** Stack height range and etch-depth spread combine to about 166 nm (3σ), which the landing layer absorbs through its selectivity.

5. **The bottom is a polysilicon landing layer.** The 30–60 nm recess is the target, and it depends on selectivity and a short overetch.

6. **Embedded particles become blind holes.** The etch reveals stack defects, and signatures must be separated from etch-induced starvation.

---

## Study Questions

1. A stack has oxide at −150 MPa (30 nm) and nitride at +60 MPa (30 nm) over 150 pairs. Compute the net stress, total film thickness, and wafer bow with Stoney's equation (wafer 775 µm thick, E′ = 180 GPa).

2. A hole etch has a nitride-to-oxide lateral rate ratio of 1.2 and mean lateral etch of 6 nm per side. Compute the wall step per side. What ratio would be needed to keep the step under 0.3 nm?

3. Within-wafer deposition thickness varies ±1.2% (3σ) systematically over a 7.9 µm ON stack, and etch-depth spread adds ±1.0%. Combine them by RSS, and find the polysilicon recess range if S_Si = 5 and nominal recess is 30 nm. If the bottom etch rate is 140 nm/min, how much extra overetch time does the worst hole need?

4. If the nitride hydrogen content drifts by +2 at% and R_nitride changes by 1.5% per at%, what is the new R_ox/R_N ratio from a starting value of 1.00? What is the resulting wall step per side for d = 5 nm?

5. A deposition tool adds 0.02 particles/cm² at ≥ 80 nm. Estimate blind holes per wafer, assuming each particle blocks 2 holes. Compare with the 10¹² holes per wafer to derive p.

---

**Previous Chapter:** [Chapter 1: 3D NAND Architecture & the Memory Channel Hole](./01-channel-hole-architecture.md)  
**Next Chapter:** [Chapter 3: High-Aspect-Ratio Etch Physics — Ion and Neutral Transport](./03-har-etch-physics.md)

---

**Chapter 2 Development Status:** Complete  
**Version:** 1.0
