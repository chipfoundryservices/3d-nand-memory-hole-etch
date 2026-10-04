# Chapter 11: Mask Budget — Selectivity, Faceting & Clogging

## Overview

The mask has to survive the whole etch with its openings intact. For the reference hole that means 1.8 µm of amorphous carbon is exposed to the same 4 keV ion flux for 41 minutes, losing about a micron or more, while its top edges are sputtered back and polymer builds on its sidewalls. If the mask runs out, the top CD blows up and the hole etch loses its pattern. If too much polymer is used to protect it, the hole closes at the bottom. This chapter sets the mask-thickness budget, explains why mask thickness grows with hole depth, treats faceting and top-CD growth, and covers clogging, the closing of the hole by its own passivation.

**Learning Objectives:**
- Compute the minimum mask thickness for a given depth, selectivity, and margin
- Describe the mask stack and the aspect ratio of the mask opening
- Estimate top-CD growth from mask faceting
- Express the clogging limit as a margin on polymer deposition at the etch front
- Compare mask materials (a-C, doped carbon) on selectivity, thickness, and cost of removal
- Build a mask budget with statistical margin

---

## 11.1 The Mask Stack

```
Layer (top to bottom)     Reference thickness   Role
─────────────────────────────────────────────────────────────────────────
Photoresist               ~ 0.3 µm              Pattern (gone after mask open)
Bottom antireflective     ~ 0.05 µm             Litho control (part of the mask open)
SiON cap / hard mask      0.06 µm               Opens the carbon; ARC
Amorphous carbon (a-C)    1.8 µm                Main etch mask
Stack (ON)                8.0 µm                Memory-hole target
```

The carbon is opened in its own etch before the hole etch (mask-open step), with oxygen-based chemistry and hydrogen bromide or carbonyl sulfide for sidewall protection. The mask open itself is a high-aspect-ratio etch:

```
Mask opening aspect ratio = t_mask / CD_open = 1800 / 110 ≈ 16:1
```

The mask opening CD, together with how much it grows during the hole etch, sets the top CD (Section 11.3).

---

## 11.2 How Thick Must the Mask Be?

### 11.2.1 The Thickness Equation

```
t_mask ≥ t_loss + t_cap + t_margin

  t_loss = H / S_avg           (mask lost over the etch; S_avg = average selectivity)
  t_cap  = 0.06 µm             (SiON breakthrough)
  t_margin ≥ 0.3 µm            (minimum remaining to keep the opening intact)
```

```
Reference: H = 8.0 µm, S_avg = 6:1 → t_loss = 1.33 µm
  t_mask ≥ 1.33 + 0.06 + 0.30 = 1.69 µm  → 1.8 µm chosen

With the stepped recipe (S_avg = 8:1, Chapter 7.2.3): t_loss = 1.00 µm
  t_mask ≥ 1.00 + 0.06 + 0.30 = 1.36 µm
```

### 11.2.2 Mask Thickness Versus Hole Depth

```
H (µm)    t_loss at S = 6:1    Required t_mask    Mask AR (CD_open = 110 nm)
────────────────────────────────────────────────────────────────────────────
 4.0       0.67                 1.03               9:1
 8.0       1.33                 1.69              15:1
12.0       2.00                 2.36              21:1
16.0       2.67                 3.03              28:1
```

The mask has to be thicker as the hole gets deeper, and its own opening gets deeper and harder to etch. A 3.0 µm carbon mask for a 16 µm hole would need a mask-open etch at 28:1, and its opening would be roughly as hard to control as the hole itself. This is the second reason (after ARDE and ion energy) that very deep single-deck holes are impractical.

### 11.2.3 Instantaneous Selectivity

Mask loss runs at roughly constant rate (the mask sees the full ion flux regardless of hole depth) while the hole rate falls with depth, so selectivity is highest at the start:

```
R_mask = 33.3 nm/min (constant; reference constant-condition recipe)
S(A) = R(A) / R_mask = 300 / [33.3 (1 + A/80)]

  A = 0:   9.0:1
  A = 40:  6.0:1
  A = 80:  4.5:1
```

By the bottom of the hole, selectivity has fallen to 4.5:1, so the **last minutes of the etch consume mask fastest per unit depth**. This is another reason the stepped recipe reduces ion energy and flux early and keeps them for the bottom stage.

---

## 11.3 Faceting and Top-CD Growth

### 11.3.1 Mechanism

The ion sputter yield of a surface peaks at an angle of 45–70° from the normal (about 2–3 times the normal-incidence value). The corner of the mask opening, where the vertical wall meets the flat top, presents just those angles to the incoming ions. The corner is sputtered back faster than the flat top, producing a **facet** (a sloped edge) that moves laterally with time.

Ions that strike the facet also reflect toward the opposite hole wall and into the upper portion of the hole (Chapter 3.6), adding to top bowing.

### 11.3.2 Top-CD Growth

```
Reference: mask opening CD_open = 108 nm
  Facet growth over the etch: ≈ 6 nm per side
  Top CD of the hole at the stack top: 108 + 2×6 = 120 nm  ✓ spec
```

The facet grows with the total mask exposure, so the growth is proportional to mask loss:

```
Mask loss +10% (e.g., +1 sccm O₂, Chapter 7.3.1) → facet growth +10% → top CD +1.2 nm
```

When the remaining mask thickness falls below a limit comparable to the facet height, facets from adjacent sides can reach the opening edge and begin to erode the stack's top. This is **top-CD blowout**, and it marks the lower bound on t_margin (0.3 µm).

### 11.3.3 Controlling Faceting

```
Lever                          Effect
─────────────────────────────────────────────────────────────────────────────
Carbon-rich chemistry          Thicker polymer cap on the corner; slower facet
Lower ion energy early         Smaller sputter yield at the corner
Mask material hardness         Harder (doped) carbon: slower facet
Mask opening slope             Steeper mask sidewall: less reflection onto hole
Pulsed bias                    Lower average flux, lower facet rate (Ch. 6.4)
```

---

## 11.4 Clogging: When the Hole Closes Itself

### 11.4.1 The Mechanism

Polymer from the plasma deposits at the etch front and the lower wall faster than the few ions at the bottom can remove it. When the polymer growth outpaces the ion-driven removal, the front narrows (Chapter 10.2) until the opening is too small for ions to enter, and the etch stops. This is **clogging** (also "etch stop" or "pinch-off").

### 11.4.2 Margin on Polymer Deposition

Using the taper model of Chapter 10.2.1, where the front narrows at g_f per side per minute and the bottom CD is 120 − 2 g_f t:

```
g_f (nm/min)    Bottom CD (nm)    Core after 46 nm films    Status
────────────────────────────────────────────────────────────────────────
0.50              80                34 nm                    Reference
0.60              72                26 nm                    OK
0.65              68                22 nm                    Marginal (min core 20)
0.70              64                18 nm                    Film pinch-off
0.80              56                10 nm                    Fails
0.90              48                 2 nm                    Hole closing
```

**The polymer deposition at the etch front can rise only about 30% (0.50 → 0.65) before the cell films pinch off.** In practice, the response is steeper than the table, because a narrower hole has a larger aspect ratio, a lower etch rate (Chapter 3.4), and a larger taper per µm. The feedback amplifies the effect of any change.

### 11.4.3 Feedback Gain

```
R = R₀ / (1 + A/A₀) with A ∝ 1/w:   d ln R / d ln w = (A/A₀) / (1 + A/A₀) = 0.5  at A = A₀
Taper rate ∝ 1/R:                    d ln(taper rate) / d ln w = −0.5

A 10% reduction in bottom width raises the local taper rate by ≈ 5%, which narrows
the front further. With A₀ = 80 the feedback gain is below 1 and the process is
stable, but the response to a disturbance is about 1.2–1.5 times the first-order estimate.
At lower A₀ (say 40, a poor process) the gain is higher.
```

### 11.4.4 Clogging Remedies

```
Remedy                       Mechanism                            Cost
──────────────────────────────────────────────────────────────────────────────
Raise O₂ at depth            Burns polymer at the bottom         Mask erosion
Raise ion energy at depth    More ions to remove polymer          Mask, heat
Cooler → warmer at depth     Less polymer from adsorbed species   Bowing
Lower pressure at depth      Fewer polymer precursors             Mask protection
Pulsed bias                  Charge relief, neutrals recover      Complexity
Reduce C₄F₆ share at depth   Less carbon                          Mask protection
```

All of these trade clogging against the mask or the profile, which is why the stage recipe (Chapter 7) is built to track them with depth.

---

## 11.5 Mask Materials

```
Mask                        S (ON : mask),    Thickness for       Removal / cost
                            illustrative      8 µm + 0.3 margin
──────────────────────────────────────────────────────────────────────────────────
Amorphous carbon (a-C)      6:1               1.7 µm              Ash (O₂), easy
Boron-doped carbon          9:1               1.25 µm             Ash, some residue
Tungsten-doped carbon       12:1              1.03 µm             Residue after ash;
                                                                  metal contamination
                                                                  concern
Polysilicon (hard mask)     (not typical)     —                   —
```

```
Doped carbon at 9:1: t_loss = 8.0/9 = 0.89 µm; t_mask ≥ 0.89 + 0.06 + 0.30 = 1.25 µm
  Mask AR = 1250/110 = 11:1 (vs 16:1)  → easier mask open, lower mask-open CD variation
```

Higher selectivity masks reduce thickness and aspect ratio, but they introduce cost, defects from dopant residues, and cleaning challenges, particularly where metals such as tungsten in the mask are a contamination risk to the memory-cell films. Adoption depends on the fab's contamination controls.

---

## 11.6 The Mask Budget With Margin

```
                                      Constant-condition    Stepped recipe
─────────────────────────────────────────────────────────────────────────
Starting a-C                           1.80 µm               1.80 µm
SiON cap breakthrough                  −0.06                 −0.06
Etch loss (nominal)                    −1.33                 −1.00
Nominal remaining                       0.41 µm               0.74 µm

Variation (3σ):
  Starting thickness ±3%                ±0.054                ±0.054
  Mask loss ±8% of loss                 ±0.107                ±0.080
  RSS                                   ±0.120                ±0.097

Minimum remaining (nominal − RSS)       0.29 µm               0.64 µm
Margin versus 0.30 µm                   −0.01 (fails)         +0.34 µm
```

A constant-condition recipe at 1.8 µm sits right at the margin limit. The stepped recipe leaves about 0.34 µm of safety, which can absorb a deeper hole or a 10% worse selectivity at the bottom. In production practice the actual margin is usually set by the worst-case hole location (wafer edge, where heat and ion flux are larger) rather than the wafer mean, so edge-zone measurements of remaining mask are part of the qualification.

---

## 11.7 Summary & Key Takeaways

1. **Mask thickness scales with depth.** t_mask ≥ H/S + 0.06 + 0.3 µm: 1.69 µm for 8 µm at 6:1, 3.0 µm for 16 µm.

2. **Selectivity falls through the etch.** From 9:1 at the top to 4.5:1 at the bottom, because the hole rate falls while the mask loss rate stays constant.

3. **Faceting grows top CD.** About 6 nm per side for the reference; 10% more mask loss adds 1.2 nm.

4. **Clogging has a 30% margin.** Polymer growth at the front can rise from 0.50 to 0.65 nm/min before the films pinch off, and feedback makes the real margin tighter.

5. **Doped carbon trades thickness for contamination risk.** 9:1 cuts the mask to 1.25 µm and its AR to 11:1.

6. **The stepped recipe holds the mask.** Remaining mask rises from 0.41 to 0.74 µm.

---

## Study Questions

1. For H = 10 µm, S_avg = 7:1, SiON cap 0.06 µm and margin 0.3 µm, find the minimum mask thickness and the mask-open aspect ratio for a 115 nm opening.

2. A mask loses material at a constant 28 nm/min over a hole whose rate follows R = 320/(1 + A/90) nm/min with w̄ = 95 nm. Find the instantaneous selectivity at A = 0, 45, and 90.

3. Mask loss increases from 1.0 to 1.15 µm because of an O₂ flow change. If the facet growth is proportional to mask loss and was 6 nm per side, find the new top CD for a 108 nm opening.

4. A hole needs a core of at least 24 nm after films of 48 nm total (both sides). With CD_top = 120 nm and g_f at 0.5 nm/min for 40 min, what is the largest g_f before pinch-off, and what percentage increase does it represent?

5. Compare the mask thickness needed for 8 µm at 12:1 (tungsten-doped) vs 6:1 (a-C), margin 0.3 µm, cap 0.06 µm.

---

**Previous Chapter:** [Chapter 10: Profile Control — Taper, Bowing & Bottom CD](./10-profile-control.md)  
**Next Chapter:** [Chapter 12: Etch Rate Decay, Bottom Landing & Overetch](./12-bottom-landing.md)

---

**Chapter 11 Development Status:** Complete  
**Version:** 1.0
