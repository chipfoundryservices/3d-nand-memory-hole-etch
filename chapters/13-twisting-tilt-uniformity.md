# Chapter 13: Twisting, Tilt & Uniformity Across the Wafer

## Overview

The previous chapters describe one ideal hole. A wafer holds a trillion of them, and no two see exactly the same plasma. Holes near the wafer edge see a bent sheath and lean. Holes near an array boundary see more radicals and etch deeper. Holes anywhere may wander sideways as charging deflects ions. These effects separate a process that makes a good hole from a process that makes a good wafer. This chapter treats three: **twisting** (random wandering of the hole axis), **edge tilt** (systematic leaning), and **local and radial uniformity** (CD and depth that depend on where on the wafer, and where in the die, a hole sits).

**Learning Objectives:**
- Model twisting as a random walk of the hole axis and relate it to slope noise and correlation length
- Describe edge tilt with a radial decay model and estimate the fraction of die affected
- Identify the knobs for radial CD and depth tuning and apply them to an example
- Explain array-edge and density effects and why dummy holes are used
- Convert CD errors into depth errors using the sensitivity from Chapter 12

---

## 13.1 Twisting

### 13.1.1 What Twisting Is

An ideal hole is a vertical cylinder. A twisted hole has an axis that wanders laterally with depth, in a path that differs from hole to hole. The cause is the sub-volt charging asymmetry that deflects fast ions (Chapter 3.5.2). The hole axis follows the net ion direction averaged over many ions and RF cycles. If that average direction has a small random error that changes with depth, the axis traces a random path.

### 13.1.2 A Random-Walk Model

Let the local axis slope s(z) have rms σ_s and an exponential correlation length ℓ (the depth over which the charging environment stays the same). The lateral displacement x(z) = ∫s dz then has

```
σ_x²(z) ≈ 2 σ_s² · ℓ · z      (z ≫ ℓ)

Reference: ℓ = 500 nm, z = 8000 nm
  σ_s = 0.5 mrad:   σ_x² = 2 × 2.5×10⁻⁷ × 500 × 8000 = 2.0 nm²    σ_x = 1.4 nm
  σ_s = 0.75 mrad:  σ_x = 2.1 nm
  σ_s = 1.0 mrad:   σ_x = 2.8 nm       ← at the 3 nm rms spec
```

Twist grows as the square root of depth and linearly with slope noise. The spec of 3 nm rms (Chapter 1.4) corresponds to an axis slope noise of about 1 mrad (0.06°) for a correlation length of 0.5 µm.

### 13.1.3 Why It Gets Worse With Depth

A single charging-induced ion deflection is of the order of 5–15 mrad (Chapter 3.5.2). The hole axis responds to the average, so the effective slope noise is much smaller than the single-event deflection, but the ratio of charge-induced deflection to the acceptance cone (0.7° = 12.5 mrad at A = 80) grows with A:

```
Acceptance cone θ_c = 1/A:   A = 40: 25 mrad     A = 80: 12.5 mrad     A = 120: 8.3 mrad

Fixed charging deflection of 5 mrad:
  A = 40:   20% of the cone     → a few ions strike the wall, most reach the bottom
  A = 80:   40% of the cone
  A = 120:  60% of the cone     → ions are routinely lost; twisting and blind holes rise
```

### 13.1.4 Reducing Twist

```
Lever                          Effect on σ_s (illustrative)    Cost
─────────────────────────────────────────────────────────────────────────────
Higher ion energy              δθ ∝ E^(-1/2): +30% E → −12%    Mask, heat
Pulsed bias (50% duty)         −20 to −40%                     Rate, complexity
Lower pressure                 Narrower sheath angle           Mask protection
Warmer sidewall (leakier)      Charge drains faster            Bowing
Conductive polymer / additives Lower surface resistivity       Chemistry window
Cleaner mask (no striations)   Fewer local charging sources    Litho/mask-open
```

---

## 13.2 Edge Tilt

### 13.2.1 Radial Decay

Near the wafer edge, the sheath bends and ions arrive tilted (Chapter 5.4). The tilt is largest at the edge and decays inward over a few millimeters:

```
θ(r) = θ_e · exp[ −(R − r) / λ_t ]      R = 150 mm,  λ_t = 6 mm,  θ_e = 0.20° (illustrative)

Distance from edge   Tilt θ      Bottom displacement (H = 8 µm)
────────────────────────────────────────────────────────────────
 0 mm                0.200°      28 nm
 3 mm                0.121°      17 nm
 6 mm                0.074°      10 nm
10 mm                0.038°       5 nm
15 mm                0.016°       2 nm
```

With a spec of 0.1° (14 nm), the tilt exceeds spec within R − r < λ_t ln(θ_e / 0.1°) = 6 × ln 2 = 4.2 mm of the edge.

### 13.2.2 Share of Die Affected

```
Fraction of wafer area within distance d of the edge: 1 − ((150 − d)/150)²

  d = 4.2 mm:  1 − (145.8/150)² = 5.5%
  d = 10 mm:   1 − (140/150)²   = 12.9%
  d = 15 mm:   1 − (135/150)²   = 19.0%
```

Edge die are a meaningful share of the wafer. A tilt that is out of spec in the outer 4.2 mm affects about 5% of the wafer area. If the edge die are partially usable, how much of the area is lost depends on the contact-landing tolerance of the product. This is why the edge ring is among the most carefully designed and monitored parts of the chamber.

### 13.2.3 Correcting Edge Tilt

```
Method                          Action
──────────────────────────────────────────────────────────────────────────────
Ring height (adjustable)        Re-set ring-to-wafer height as ring wears
Edge RF / bias tuning           Separate edge electrode or ring bias to flatten the
                                sheath at the edge
Ring geometry                   Profile of ring surface: inner step, taper
Edge temperature offset         Compensates CD/depth shift (not tilt)
Edge exclusion                  Define yield-critical area inside the tilt zone
```

---

## 13.3 Radial Tuning of CD and Depth

### 13.3.1 Typical Signatures

```
Symptom (relative to center)         Likely cause                          Chapter
────────────────────────────────────────────────────────────────────────────────────
Edge CD small, edge depth low         Edge neutral supply low;              7.6
                                       edge wafer cooler than center
Edge CD large, depth high             Edge hotter (bow, leak) or            8.5
                                       edge gas rich
Mid-radius ring of CD change          Plasma density profile (RF)           5
Whole-wafer bowl or dome              Temperature profile through zones     8.3
```

### 13.3.2 Knob Library

```
Knob                          CD sensitivity       Depth sensitivity     Region
───────────────────────────────────────────────────────────────────────────────────
Edge gas fraction f_e         1.5 nm per 0.1       1.0% per 0.1          Mid to edge
ESC zone offset               0.6 nm per K         2% per K              By zone
He pressure, edge zone        0.6 nm per K         2% per K              Edge
Bias power distribution       Variable             Variable              Radial
Edge ring height              Tilt; CD at edge     Edge depth            Outer 10 mm
```

### 13.3.3 Worked Example

```
Measured: center–edge: CD −2.4 nm (edge small), depth −1.2% (edge shallow)
          (edge-to-center difference)

One knob alone cannot fix both:
  Gas only:         Δf_e = +0.16 closes the CD (+2.4 nm) but adds +1.6% depth (0.4% too much)
  Temperature only: ΔT = +0.6 K closes the depth (+1.2%) but adds only +0.36 nm of CD
                    (2.0 nm of CD error remains)
```

Because **CD and depth are coupled by the same knobs**, a tuning plan has to solve for both simultaneously: with two knobs (gas fraction and zone temperature) and two targets, the solution is a 2 × 2 linear system:

```
[1.5 nm/0.1  0.6 nm/K ] [Δf_e/0.1]   [+2.4 nm ]
[1.0 %/0.1   2.0 %/K  ] [ΔT       ] = [+1.2 %  ]

Determinant: 1.5 × 2.0 − 0.6 × 1.0 = 2.4
Δf_e/0.1 = (2.4 × 2.0 − 0.6 × 1.2) / 2.4 = (4.8 − 0.72)/2.4 = 1.70  → Δf_e = +0.17
ΔT       = (1.5 × 1.2 − 1.0 × 2.4) / 2.4 = (1.8 − 2.4)/2.4 = −0.25 K
```

So raise the edge gas fraction by 0.17 and cool the edge zone by 0.25 K to bring both back to center. The temperature change is small, within the 0.3 K calibration resolution of the zones, so in practice the correction would rely on gas alone and accept a residual depth error of about 0.4% (Δf_e = +0.16 alone).

---

## 13.4 Local Effects: Array Edge and Density

### 13.4.1 Why Edge Holes Differ

A hole at the boundary of an array, next to an open area or a slit, has fewer neighbors competing for neutral flux and shares ion flux over a different area. It etches faster and wider than a hole in the middle of a dense array, the opposite of ARDE's rule that a narrow, crowded feature is slow. The effect is a **microloading** one: local open area changes the local supply of radicals.

```
Illustrative: outermost row of holes sees 1.5–2× the radical flux of an interior hole.
  CD difference: +3 to +5 nm
  Depth difference (dz/dw̄ = 20, Chapter 12.1):  +60 to +100 nm deeper
```

A depth 60–100 nm too deep punches through the polysilicon landing layer in the worst case (the overetch for the whole distribution was 164 nm, which with S_Si = 6 gave a 57 nm recess; an additional 80 nm at S_Si = 6 adds another 13 nm: recess 70 nm). The edge-row holes may be fine for depth but have off-spec CD and bowing.

### 13.4.2 Dummy Holes

The standard fix is to extend the pattern beyond the active array with **dummy holes** (not part of any memory string) so that the outermost active hole has the same environment as an interior one.

```
Rows of dummy holes needed ≈ (neutral diffusion range) / pitch
  Lateral range of radical shadowing ~ 0.5–1 µm; pitch 200 nm → 3–5 rows
Area cost: 4 rows × 0.2 µm = 0.8 µm per side of each block edge
```

At the edges of the slit, the block boundary, and the staircase region, the same logic applies, and the density of the pattern near them has to be handled in layout rules.

### 13.4.3 Dense versus Sparse Regions

A die also has regions with different pattern densities: array interior, periphery, scribe lines. Open area fraction changes the total radical consumption. In this book's reference, 32.6% of the array is open, which is very high compared with contact etch, so ion and radical consumption is a significant fraction of the supply, and the plasma properties depend on loaded area.

---

## 13.5 Summary & Key Takeaways

1. **Twist is a random walk.** σ_x = √(2 σ_s² ℓ z); 1 mrad of slope noise and 0.5 µm correlation length gives 2.8 nm over 8 µm.

2. **Acceptance cone shrinks as 1/A.** A fixed charging deflection of 5 mrad uses 40% of the cone at A = 80 and 60% at A = 120.

3. **Edge tilt decays over a few mm.** With λ_t = 6 mm, the outer 4.2 mm (5.5% of the area) exceeds a 0.1° spec.

4. **CD and depth are coupled by the same knobs.** Radial tuning is a 2 × 2 problem in gas fraction and zone temperature.

5. **Array-edge holes are faster and wider.** +4 nm of CD is +80 nm of depth; dummy holes remove the difference.

6. **Hole-to-hole CD becomes depth variation.** Chapter 12's 20 nm of depth per nm of CD links every uniformity result in this chapter to landing.

---

## Study Questions

1. A hole has slope noise σ_s = 0.8 mrad and correlation length 0.4 µm. Compute twist at z = 7 µm and 9 µm. What slope noise gives 2.5 nm rms at 9 µm?

2. With θ_e = 0.3°, λ_t = 5 mm, find the distance from the edge at which tilt falls to 0.1°, and the fraction of wafer area inside that distance.

3. An edge-to-center signature is CD +1.8 nm, depth +1.5%. Using the sensitivity matrix in Section 13.3.3, solve for Δf_e and ΔT.

4. Edge-row holes are 4 nm wider than interior holes. Using dz/dw̄ = 20, find the extra depth. If S_Si = 6 and d_nominal is 30 nm, find the extra recess.

5. For a dummy-hole ring with a radical shadowing range of 0.8 µm and 200 nm pitch, how many rows are needed? What area penalty results on a block edge 5 mm long?

---

**Previous Chapter:** [Chapter 12: Etch Rate Decay, Bottom Landing & Overetch](./12-bottom-landing.md)  
**Next Chapter:** [Chapter 14: Advanced Schemes — Multi-Deck, Joints & 400-Layer Stacks](./14-advanced-multideck.md)

---

**Chapter 13 Development Status:** Complete  
**Version:** 1.0
