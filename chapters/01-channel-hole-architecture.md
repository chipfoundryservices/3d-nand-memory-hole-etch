# Chapter 1: 3D NAND Architecture & the Memory Channel Hole

## Overview

Every bit in a 3D NAND die is stored on the wall of a hole. A single 512 Gb die carries about 1.3 billion of them, each a narrow vertical cylinder cut through the whole alternating stack, and a 300 mm wafer carries about a trillion. The memory hole (also called the channel hole) is the structure that makes vertical NAND vertical: after the hole is etched, it is lined with a blocking oxide, a charge-trap nitride, a tunnel oxide, and a polysilicon channel, and every word-line layer it crosses becomes one memory cell.

This chapter explains why the hole exists, what it has to deliver, why its aspect ratio sets the difficulty of the whole etch, and where the etch sits in the process flow.

**Learning Objectives:**
- Describe the vertical NAND string and how a memory cell forms where a hole crosses a word line
- Define the aspect ratio of a memory hole and relate it to layer count and hole diameter
- State the geometric targets of the hole etch: top CD, bottom CD, depth, tilt, and shape
- Count holes per die and per wafer, and relate defect rate per hole to die yield
- Place the hole etch in the 3D NAND process flow

---

## 1.1 From Planar NAND to the Vertical String

Planar NAND stored charge in cells laid side by side in one plane. Shrinking those cells below roughly 15 nm made neighboring floating gates talk to each other and left too few electrons per cell to read reliably. 3D NAND changed direction: instead of shrinking cells further, it **stacked** them.

```
Planar NAND                       Vertical (3D) NAND
────────────────────────────────────────────────────────────────────────
Cells in one plane                Cells stacked along a vertical string
Word line = a line on the surface Word line = a buried horizontal plate
Density from lithographic shrink  Density from layer count
Cell size ~15 nm node limit       Cell size set by hole pitch (~200 nm)
                                  and layer pitch (~60 nm)
Cost falls with litho shrink      Cost falls if layers can be added
                                  faster than etch/deposition cost grows
```

In a vertical string, a single channel runs the full height of the stack. Each word-line plate that the channel passes defines one transistor in series. Reading and programming select one cell by turning the others on as pass gates. The bit density of the die is

```
Bits per die = (holes per die) × (word-line layers per hole) × (bits per cell)
```

so density rises with the number of layers per hole and with how closely holes can be packed. The first factor drives hole depth. The second drives hole density and diameter. Both push on the etch.

---

## 1.2 The Memory Cell in a Hole

### 1.2.1 Cross-Section of a Filled Hole

After etch, the hole receives a series of conformal films by atomic layer deposition and CVD. The reference films are:

```
Film (from the wall inward)       Function                    Thickness (reference)
────────────────────────────────────────────────────────────────────────────────────
Blocking oxide (Al₂O₃ or SiO₂)    Stops charge back-tunneling   6 nm
Charge-trap nitride (Si₃N₄)       Stores the bit                6 nm
Tunnel oxide (SiO₂, or ON/ONO)    Lets charge in and out        4 nm
Polysilicon channel               Carries read current          7 nm
Core fill (oxide)                 Fills the remaining space     remainder
───────────────────────────────────────────────────────────────────
Total film per side                                              23 nm
```

```
Reference hole after etch:   CD_top = 120 nm, CD_bottom = 80 nm
Film consumed (both sides):  2 × 23 = 46 nm
Remaining core:              top  74 nm
                             bottom 34 nm
```

The hole etch therefore has to leave **enough room at the bottom** for all films and a core. A bottom CD that comes in 20 nm small leaves only 14 nm for the core, and the film stack can pinch off and trap a void. This is the first reason that bottom CD, rather than top CD, is the number that matters most.

### 1.2.2 Where the Cell Forms

Replacing the nitride layers with tungsten word lines (the replacement-gate flow) leaves each word-line plate wrapped around the hole at one level. The cell is the short region of channel, trap layer, and gate where they overlap:

```
Layer pitch p = 60 nm → cell gate length ≈ 28–32 nm (the replaced nitride)
Oxide layers (28 nm) separate neighboring cells and set the cell-to-cell spacing
```

The etch does not form the cell. It forms the **shape that the cell films will be deposited into**, and any non-uniformity in the hole (bowing, a rough wall, a twist) is copied into every cell it passes.

---

## 1.3 Aspect Ratio: The Number That Sets the Difficulty

### 1.3.1 Definition

```
Aspect ratio (top CD):   AR_top  = H / CD_top
Aspect ratio (mean CD):  A       = z / w̄       (used in transport models; w̄ = mean CD)

Reference: H = 8.0 µm, CD_top = 120 nm, CD_bottom = 80 nm, w̄ = 100 nm
  AR_top = 8000 / 120 ≈ 67:1
  A (at full depth) = 8000 / 100 = 80
```

The quoted "67:1" uses the top CD, the figure most often seen in the literature. The transport models in this book use A = z/w̄, because what the species moving in the hole see is the width of the hole they travel through, not the width of the opening.

### 1.3.2 How Aspect Ratio Scales

The stack height grows with layer count, but the hole cannot widen to compensate, because the holes must stay on a fixed pitch to keep density.

```
Layers    Stack height H     CD_top    AR_top
(pairs)   (pitch 60 nm)      (nm)      (H / CD_top)
──────────────────────────────────────────────────
  32      ~2.3 µm            ~ 90 nm     ~ 25
  64      ~4.2 µm            ~100 nm     ~ 42
  96      ~6.1 µm            ~110 nm     ~ 55
 128      ~8.0 µm            ~120 nm     ~ 67
 256 (2 decks of 128)  2 × 8.0 µm        2 × 67   (Chapter 14)
```

```
Per-layer cost to the etch: every extra pair adds 60 nm of depth at a CD that
cannot grow. At A = 80, doubling depth at constant width roughly
doubles A and increases etch time by more than 2× (Chapter 3.5).
```

This is why the industry moved to **string stacking** (multi-deck): etching a 16 µm hole in one pass would need A ≈ 160 and a mask nearly 3 µm thick (Chapter 14).

### 1.3.3 What Aspect Ratio Does to the Etch

A deep, narrow hole changes the physics of every species in it:

```
Species          Problem at high A                           Consequence
───────────────────────────────────────────────────────────────────────────────
Ions             Must be nearly vertical to reach the bottom  Needs multi-keV energy
                                                              and tight angular spread
Neutral          Reach the bottom only by repeated wall       Etch-rate decay (ARDE),
 radicals         collisions (Knudsen transport)              neutral starvation
Polymer          Must reach the sidewall at every depth       Bowing or clogging
 precursors
Charge           Electrons are shadowed; ions are not         Positive bottom, deflected
                                                              ions, twisting
Reaction         Must leave through the same narrow hole      Redeposition, clogging
 products
```

Chapter 3 develops each of these in turn.

---

## 1.4 The Specification Sheet of a Memory Hole

```
Parameter                      Reference target          Why it matters
──────────────────────────────────────────────────────────────────────────────────────
Depth H                        8.0 µm ± 2%               Must reach the bottom
                                                         landing layer everywhere
CD_top (at stack top)          120 nm ± 3 nm             Sets film fit and pitch
CD_bottom                      80 nm ± 4 nm              Sets film fit, cell current
Bottom/top CD ratio            ≥ 0.65                    Taper limit
Max bowing (CD_max − CD_top)   ≤ +4 nm                   Limits web between holes
Sidewall angle (average)      89.85–89.90° (taper       Controls bottom CD
                               0.10–0.15° per side)
Tilt at the wafer edge         ≤ 0.1° (≤ 14 nm shift     Landing on bottom contact
                               at the bottom)
Twist / distortion             ≤ 3 nm rms (3σ ≤ 9 nm)    Cell-to-cell uniformity
Ellipticity                    ≤ 3%                      Film fit and read current
Hole-to-hole CD (within die)   3σ ≤ 3 nm                 Cell Vt distribution
Wafer-to-wafer CD              3σ ≤ 3 nm
Wall roughness (LWR-like)      ≤ 2 nm rms                Charge-trap uniformity
Bottom into poly/SEG recess    30–60 nm                  Contact to the source line
Missing / blind holes          < 10 ppb of holes         Yield (Section 1.6)
Mask remaining at end          ≥ 0.3 µm                  Margin (Chapter 11)
```

These are reference targets for the book's worked examples, not qualified specifications for any product.

---

## 1.5 Where the Hole Etch Sits in the Flow

```
Step                                             Notes
───────────────────────────────────────────────────────────────────────────────
 1  CMOS periphery (or separate wafer for        CMOS-under-array, or array-on-
    bonding)                                      CMOS wafer-bonded
 2  Source line / bottom poly or SEG base layer  Landing layer for the hole
 3  Alternating SiO₂ / Si₃N₄ stack (PECVD)       128 pairs for the reference
 4  Hard mask: amorphous carbon + SiON cap       1.8 µm a-C
 5  Lithography: hole pattern                    Dense hex-staggered array
 6  Hard-mask open                               Transfers resist → a-C
 7  ★ MEMORY HOLE ETCH ★                         This book
 8  Hole clean, sacrificial or epitaxial bottom  SEG or poly pillar growth
 9  ONO (blocking/trap/tunnel) + poly channel    Conformal ALD/CVD
10  Bottom punch (ONO spacer etch) + channel     Contact to the source
11  Core fill, plug, bit-line contact
12  Staircase etch, slit etch, nitride removal   Replacement gate (Book #23, slit-etch)
13  Word-line fill (W), contacts, metal
```

The hole etch comes **before** every step that forms the cell, so its errors cannot be corrected later. A twisted hole is twisted in every film that follows. A bottom that is too small cannot be reopened. The hole is also the first and by far the longest of the array etches: about 40 minutes of plasma time per wafer, versus a few minutes for slit etch or contact etch.

---

## 1.6 Holes per Die, Holes per Wafer, and What That Does to Yield

### 1.6.1 Counting

```
Reference die:     512 Gb, 3 bits per cell (TLC), 128 word-line layers
Cells per die:     512×10⁹ / 3 ≈ 1.71×10¹¹
Holes per die:     1.71×10¹¹ / 128 ≈ 1.33×10⁹
Hole pitch:        200 nm, staggered (hex) array
Area per hole:     0.866 × (0.200 µm)² = 0.0346 µm²
Array area:        1.33×10⁹ × 0.0346 µm² ≈ 46 mm²
Open area at top:  π(60 nm)² / 0.0346 µm² = 32.6% of the array
Dies per wafer:    ~750 (about 80 mm² per die including periphery)
Holes per wafer:   750 × 1.33×10⁹ ≈ 1.0×10¹²
Etch time:         40 min = 2400 s → ~4.2×10⁸ holes completed per second
```

### 1.6.2 Yield per Defective Hole

If each hole fails independently with probability p (blind, shorted to a neighbor, wrong shape), and no redundancy absorbs it, die yield is Poisson:

```
Y = exp(−N · p),   N = 1.33×10⁹

  p = 10⁻¹¹ (1 in 100 billion):  Y = exp(−0.0133) = 98.7%
  p = 10⁻¹⁰:                     Y = exp(−0.133)  = 87.5%
  p = 10⁻⁹:                      Y = exp(−1.33)   = 26.4%
```

Real devices have error-correction codes, spare blocks, and bad-block mapping, which tolerate a fraction of failed strings. The result is a looser limit than "zero failures", but the lesson is unchanged: **one failure in 10 billion holes costs several percent of die yield**, so the etch has to be repeatable far beyond what any inspection can sample directly.

---

## 1.7 Summary & Key Takeaways

1. **The hole is the cell's mold.** Every film that stores, tunnels, or conducts charge is deposited on the wall the etch leaves.

2. **Aspect ratio grows with layer count, and the hole cannot widen.** 128 pairs at 60 nm pitch and 120 nm top CD gives an AR_top of about 67:1.

3. **Bottom CD is the critical number.** Films take 46 nm of the width, and the core has to fit in what is left.

4. **The hole etch cannot be repaired downstream.** It precedes every step that builds the cell.

5. **A wafer holds about 10¹² holes.** A defect rate of one per 10 billion holes is a measurable yield loss.

6. **The etch is long and ion-starved at depth.** The 40-minute hole etch is the longest etch in the array flow and the one most strongly limited by transport.

---

## Study Questions

1. A 3D NAND product has 176 word-line layers at a pair pitch of 55 nm, plus 0.4 µm for select gates and the cap. What is the stack height? If CD_top is 115 nm, compute AR_top and A (using a mean CD of 95 nm).

2. A 1 Tb die with 4 bits per cell and 176 layers has an array efficiency of 70% and a die area of 100 mm². Compute holes per die and the area per hole. What is the equivalent staggered pitch?

3. Using the film stack of Section 1.2.1, how large must the bottom CD be to leave a 30 nm core? What bottom CD would result if the poly channel were thickened from 7 nm to 9 nm?

4. A die has 1.33×10⁹ holes. If ECC absorbs up to 1 failed hole per 10⁷ and the rest are independent killers at probability p, how does the allowed p scale? What would you say to a colleague who proposes sampling 10⁴ holes per wafer by SEM to verify p = 10⁻¹¹?

5. Explain why the top CD, rather than the bottom CD, is the number usually quoted for aspect ratio. Why is A = z/w̄ more useful in the transport models?

---

**Previous Chapter:** None (first chapter)  
**Next Chapter:** [Chapter 2: The ONON Stack — Materials, Deposition & Stress](./02-onon-stack-materials.md)

---

**Chapter 1 Development Status:** Complete  
**Version:** 1.0
