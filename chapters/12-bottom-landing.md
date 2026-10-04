# Chapter 12: Etch Rate Decay, Bottom Landing & Overetch

## Overview

Every hole must reach the bottom. A hole that stops 50 nm short is a dead string, and a hole that overshoots through the landing layer shorts the source line. With 10¹² holes on a wafer, "every hole" means the extreme tail of the depth distribution, not its mean. This chapter turns the ARDE model of Chapter 3 into a depth-sensitivity relation (how much a 1 nm CD error costs in depth), builds the overetch from the tail of the hole-to-hole distribution, shows how selectivity to the polysilicon landing layer limits the overetch, and describes how endpoint detection removes the wafer-level component of depth variation.

**Learning Objectives:**
- Derive the sensitivity of depth to hole CD from the ARDE model
- Build the overetch requirement from a systematic residual and the tail of a random distribution
- Relate overetch to polysilicon recess through selectivity and check against the landing-layer thickness
- Describe the shape of the bottom and the effect of charging at the landing interface
- Explain how endpoint detection and fixed overetch work together

---

## 12.1 Depth Is Sensitive to CD

### 12.1.1 Derivation

With R = R₀/(1 + A/A₀) and A = z/w̄, integrating gives the time to reach depth z:

```
t = z/R₀ + z² / (2 A₀ R₀ w̄)
```

At fixed etch time t, differentiate with respect to the hole's mean width w̄:

```
dz/dw̄ = [ z² / (2 A₀ w̄²) ] / (1 + A/A₀)
      = [ A² / (2 A₀) ] / (1 + A/A₀)

Reference: A = 80, A₀ = 80
  dz/dw̄ = (6400/160) / 2 = 20     (nm of depth per nm of mean CD)
```

**One nanometer of mean-CD difference between two holes becomes 20 nm of depth difference** at the end of a 40-minute etch. In relative terms, d ln z / d ln w̄ = 20 × 100/8000 = 0.25: a 1% wider hole is 0.25% deeper.

```
Hole-to-hole mean CD, 3σ (spec)    σ_CD     Depth σ       Depth 3σ
─────────────────────────────────────────────────────────────────────
3.0 nm                             1.0 nm   20 nm         60 nm
4.0 nm                             1.33 nm  27 nm         80 nm
6.0 nm                             2.0 nm   40 nm         120 nm
```

This is the link between profile control and landing: the hole-to-hole CD specification of ±3 nm (3σ) in Chapter 1.4 is not only a cell-uniformity requirement. It sets the random depth spread to 20 nm (1σ).

---

## 12.2 Building the Overetch From the Tail

### 12.2.1 Why the Tail

A depth distribution that is Gaussian with σ_r = 20 nm has holes beyond 3σ at the rate of 1.35×10⁻³ per side, which for 10¹² holes is a billion holes. To reach a blind-hole probability of p, the overetch has to cover the distribution out to z_p standard deviations:

```
Tail probability p      z_p
─────────────────────────────
10⁻⁶                    4.75
10⁻⁸                    5.61
10⁻⁹                    6.00
10⁻¹¹                   6.71
```

### 12.2.2 The Overetch Budget

The endpoint signal (Section 12.5) is referenced to the **average** arrival at the bottom on each wafer, so stack-height variation and wafer-to-wafer rate differences are removed by timing the overetch from the endpoint. What remains is:

1. A **systematic within-wafer** residual (center-to-edge depth difference after zone tuning), treated as a range
2. The **random hole-to-hole** spread σ_r, treated as a distribution tail

```
Reference:
  Systematic residual after zone tuning:      30 nm (the range, not 3σ)
  Random σ_r from CD variation:               20 nm  (Section 12.1)
  Tail coverage for p = 10⁻¹¹ (z = 6.7):      6.7 × 20 = 134 nm
  Overetch depth:                             30 + 134 = 164 nm
```

This reproduces the 166 nm figure used in Chapter 2.4.2 (there as an RSS of stack-height and rate spread). At the bottom rate of 150 nm/min:

```
Overetch time = 164 / 150 = 1.09 min ≈ 66 s
```

### 12.2.3 What Moves the Overetch

```
Change                              Overetch required
────────────────────────────────────────────────────────────────────────
σ_CD 1.0 → 1.5 nm (σ_r 20 → 30 nm)  30 + 6.7 × 30 = 231 nm  (+41%)
Systematic residual 30 → 60 nm      60 + 134 = 194 nm  (+18%)
Tail target 10⁻¹¹ → 10⁻⁸            30 + 5.6 × 20 = 142 nm  (−13%)
(ECC absorbs part of the failures)
```

Every nanometer of hole-to-hole CD variation costs about 6.7 × 20 = 134 nm of overetch. **Improving CD uniformity is the most effective way to shrink the overetch.**

---

## 12.3 Selectivity to the Landing Layer

### 12.3.1 Recess in Polysilicon

During the overetch, the holes that have already reached the polysilicon continue to be exposed to the plasma while the slowest holes catch up. The recess in the poly is the overetch depth divided by the selectivity of the oxide/nitride etch to polysilicon (S_Si), plus the nominal recess for the first hole to arrive.

```
Recess_max = d_nominal + OE / S_Si,   d_nominal ≈ 30 nm,   OE = 164 nm

S_Si     OE/S_Si      Recess_max     Fraction of 120 nm poly
─────────────────────────────────────────────────────────────
 8        20 nm        50 nm          42%
 6        27 nm        57 nm          48%
 4        41 nm        71 nm          59%
 3        55 nm        85 nm          71%
 2        82 nm       112 nm          93%    ← punch-through risk
```

(The rule of thumb that the recess should stay below about 80% of the landing layer gives S_Si ≥ 2.5.)

### 12.3.2 Why Selectivity Is Hard at the Bottom

A fluorocarbon plasma etches silicon much more slowly than oxide because the polymer on silicon is not consumed by oxygen from the substrate. But at the bottom, with high ion energy (4–4.5 keV), a high O₂ flow to keep the hole open, and low polymer deposition, the polymer that protects the silicon is thin or absent, and S_Si falls toward 3–4. The **overetch stage** is therefore tuned differently from the main etch (Chapter 7.2.2): lower ion energy, lower O₂, higher polymer share, trading some etch rate in the last minute for selectivity.

```
Overetch tuning, illustrative:
  Main stage 4:   4.5 keV, O₂ 12 sccm, S_Si = 3.5, R_bottom = 150 nm/min
  Overetch:       4.0 keV, O₂ 14 sccm → 10 sccm with more C₄F₆, S_Si = 6, R_bottom = 120 nm/min
  Overetch time at 120 nm/min: 164/120 = 1.37 min = 82 s (vs 66 s)
  Recess_max = 30 + 164/6 = 57 nm (vs 77 nm at S_Si = 3.5)
```

---

## 12.4 The Bottom Shape

### 12.4.1 Charging at the Interface

Before the hole reaches the polysilicon, the bottom is an insulating oxide. It charges positive from the unopposed ion flux (Chapter 3.5), which deflects ions to the wall and tends to cut a **notch** at the interface once the etch is conducting. When the hole opens onto conducting polysilicon, the charge drains, and the field disappears.

```
Bottom feature      Cause                              Spec / effect
───────────────────────────────────────────────────────────────────────────────
Notch / foot        Lateral ion deflection at the      Widens the bottom 2–5 nm;
                    oxide/poly interface               seeds film defects
Recess shape        Ion angle and polymer at the       V-shape: weak SEG growth;
                    bottom                             U-shape: preferred
Bottom rounding     Mix of ion reflection and          Lowers contact area
                    chemistry                          to the source
Residue in recess   Polymer or SiOxFy not removed      Blocks epitaxial growth
                                                        (cleaned afterwards)
```

### 12.4.2 After the Etch

The bottom of the hole is cleaned, the polymer and fluorinated residue are removed (dry and wet), and a **selective epitaxial growth (SEG)** layer or a polysilicon seed grows from the exposed silicon to form the base of the channel. The recess shape and cleanliness decide whether the SEG grows with the right thickness and no voids. These are downstream processes (Chapter 16), but the hole etch has to hand over a clean, U-shaped, silicon-exposed bottom.

---

## 12.5 Endpoint and Overetch Together

### 12.5.1 The Signal

When holes break through to the polysilicon, silicon-containing species appear in the plasma (Si at 288 nm, SiF) and carbon-nitrogen (CN at 388 nm) falls as the last nitride is consumed. With 32.6% of the die area open at the hole top, the signal from 10¹² holes is large. It rises over a time window because holes arrive at different times:

```
Spread of arrival: ± 164 nm range at 150 nm/min → ± 1.1 min window
Signal transition: ~ 2 min wide (10% to 90%)
```

### 12.5.2 The Endpoint Strategy

```
Method                               Use
──────────────────────────────────────────────────────────────────────────────
Fixed time (all stages)              Simple; no wafer-by-wafer correction;
                                     needs tight depth control
Endpoint at 50% of transition         Marks the average arrival; the overetch is
 + fixed overetch                     then a fixed time (reference 66–82 s)
Endpoint at start of transition       Early; risks under-etch of slow holes
                                      if the overetch is not adapted
```

Production recipes use the **50% point plus a fixed overetch**. The 50% point removes the wafer-level offset (stack height, bulk etch rate) and leaves the fixed overetch to cover the tail. The signal from a small number of the deepest or fastest holes is not visible, which is why the overetch budget must be built from statistics, not from the endpoint trace.

### 12.5.3 Limits of Endpoint

```
Issue                          Consequence
───────────────────────────────────────────────────────────────────────────────
Signal averaged over the wafer  Cannot see the tail; cannot see edge versus center
Weak signal at late stages      Etch product signal low in a 80:1 hole (Ch. 15.1)
Chamber window coating          Signal drift; endpoint false trigger
Drift in the signal baseline    Needs normalization to a stable line (Ar)
```

Chapter 15 covers OES in detail, including multi-position OES and patterned-wafer monitoring of depth.

---

## 12.6 Summary & Key Takeaways

1. **Depth is 20× as sensitive to CD as it looks.** At A = A₀ = 80, 1 nm of CD gives 20 nm of depth.

2. **The overetch covers a tail.** A systematic 30 nm range plus 6.7σ of the 20 nm random spread gives 164 nm, or 66 s at 150 nm/min.

3. **CD uniformity buys overetch.** Each 1 nm of σ_CD costs about 134 nm of overetch.

4. **Selectivity to poly decides the recess.** At S_Si = 6 the maximum recess is 57 nm (48% of 120 nm); at S_Si = 2 it nearly punches through.

5. **The overetch stage is tuned for selectivity.** Lower energy and more polymer; a longer time (82 s) at a lower rate.

6. **Endpoint removes the wafer-level offset only.** The 50% point plus fixed overetch is standard; the tail needs statistics.

---

## Study Questions

1. For A₀ = 100, A = 90 (z = 9 µm, w̄ = 100 nm), compute dz/dw̄. If σ_CD = 1.2 nm, what is the depth σ?

2. A recipe has a systematic depth residual of 40 nm and a random σ_r of 25 nm. What overetch is needed for p = 10⁻⁹ (z = 6.0)? How long is that at 140 nm/min?

3. The polysilicon landing layer is 90 nm thick, d_nominal = 25 nm, and the overetch is 180 nm. What is the minimum S_Si if the recess must stay below 75% of the layer?

4. Compare the overetch times of a chemistry with R_bottom = 150 nm/min and S_Si = 3.5 versus R_bottom = 115 nm/min and S_Si = 6.5 for OE = 164 nm. Which gives a smaller recess? By how much?

5. If the hole-to-hole mean CD σ is reduced from 1.2 nm to 0.9 nm (A = A₀ = 80), by how much is the overetch (6.7σ) reduced in nm and in seconds at 150 nm/min?

---

**Previous Chapter:** [Chapter 11: Mask Budget — Selectivity, Faceting & Clogging](./11-mask-budget-clogging.md)  
**Next Chapter:** [Chapter 13: Twisting, Tilt & Uniformity Across the Wafer](./13-twisting-tilt-uniformity.md)

---

**Chapter 12 Development Status:** Complete  
**Version:** 1.0
