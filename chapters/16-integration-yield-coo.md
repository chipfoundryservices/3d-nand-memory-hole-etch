# Chapter 16: Integration, Yield & Cost of Ownership

## Overview

The memory hole is judged downstream. The films deposited into it, the bottom punch, the epitaxial or polysilicon seed, the channel, the plug, and the electrical test each convert the hole's shape into a pass or a fail. This chapter follows the hole through those steps, builds a yield budget from the killer modes in the earlier chapters, and then prices the etch: at 45 minutes per wafer per deck, hole etch consumes more chamber time than any other etch in the array flow, so its cost of ownership is dominated by capital and throughput.

**Learning Objectives:**
- Map hole-etch outcomes (bottom CD, twist, recess, residue) to downstream failure modes
- Compute web margin between neighboring holes and its sensitivity to twist
- Assemble an illustrative yield budget from independent killer modes
- Build a per-wafer cost of ownership and size the chamber count for a fab
- Evaluate a faster cryogenic process against a higher chamber capital cost
- Read wafer-map signatures back to their etch causes

---

## 16.1 What the Hole Hands to the Next Steps

```
Etch outcome                  Downstream step affected          Failure if out of spec
────────────────────────────────────────────────────────────────────────────────────────────
Bottom CD small               ONO + poly deposition             Film pinch-off, core void, string open
Bottom recess shape/residue   Bottom clean, SEG / poly seed     Poor epitaxy, high contact resistance
Twist, ellipticity            Conformal films                   Non-uniform film, Vt spread, read current
Wall roughness                Trap layer and tunnel oxide       Charge-trap variation, retention
Bowing (web)                  Neighbor spacing                  Hole-to-hole short at pitch limit
Tilt                          Bottom contact, joint             Misland, ledge
Top CD, mask remnant          Top plug and bit-line contact     Contact misalignment, CMP dishing
Blind hole                    —                                  Dead string (ECC / spare handles a few)
```

### 16.1.1 Film Step Coverage

The ONO and polysilicon films are deposited by ALD and CVD in an 80:1 hole. The thickness at the bottom relative to the top must be close to 1:

```
Step coverage (bottom / top) target ≥ 95%
Film per side 23 nm at top → ≥ 21.9 nm at bottom
Film both sides at bottom: ≥ 43.7 nm → core at 80 nm bottom CD: 36 nm (spec min 20)
```

A hole whose bottom is narrow or rough does not just get a smaller core. It also changes the local deposition rate (the precursor starves), so the films at the bottom of a small hole are thinner, which affects the cell.

---

## 16.2 Web Margin Between Neighbors

The silicon-oxide/nitride web between two adjacent holes carries the cell's structural support and insulation. For a hex array at pitch a = 200 nm:

```
Web = a − (r₁ + r₂) − (relative displacement along the line of centers)
    = 200 − 124 − Δ     at the bow depth: CD_max = 120 + 4 = 124 nm (Chapter 10.3)
    Mean web = 76 nm (Δ = 0)
```

The spread comes from hole-to-hole CD (σ_CD = 1.0 nm → radius σ = 0.5 nm each), twist (σ_x = 3 nm per axis per hole), and lithographic placement (σ ≈ 1.7 nm from 5 nm 3σ):

```
σ_web = √[ (3√2)² + (0.5√2)² + 1.7² ] = √[18.0 + 0.5 + 2.9] = 4.6 nm
```

Failure when the web falls below a minimum, e.g., 20 nm (electrical breakdown and leakage):

```
Mean web    z = (mean − 20)/4.6     Tail probability per pair
─────────────────────────────────────────────────────────────────
 76 nm       12.2                    < 10⁻³⁰  (negligible)
 56 nm        7.8                    3×10⁻¹⁵
 46 nm        5.6                    9×10⁻⁹
 40 nm        4.3                    8×10⁻⁶
 36 nm        3.5                    3×10⁻⁴   (catastrophic)
```

At the reference pitch the web is safe by a large margin, but **pitch scaling** moves a design down this table quickly: a 36 nm reduction in pitch (from 200 to 164 nm, at constant CD) would put the web at 40 nm and give a failure rate of about 10⁻⁵ per pair. Because the failure probability depends on the twist σ through z, **reducing twist from 3 nm to 2 nm** (σ_web 4.6 → 3.4 nm) moves a 40 nm web from z = 4.3 to z = 5.9 (about 2×10⁻⁹), a factor of about 5,000 in failure rate.

---

## 16.3 An Illustrative Yield Budget

Using Poisson statistics (Chapter 1.6) with the reference N = 1.33×10⁹ holes per die:

```
Killer mode                         Per-hole p         N·p      Source (Chapter)
──────────────────────────────────────────────────────────────────────────────────────────
Etch-tail blind holes               1.0×10⁻¹¹          0.0133   Overetch tail at 6.7σ (12.2)
Stack particles (blind)             1.4×10⁻¹¹          0.0187   0.01 /cm² adders (2.6)
Etch-chamber particles (blind)      1.0×10⁻¹¹          0.0133   5 adders/wafer (9.3.2)
Film pinch-off, web shorts          < 10⁻¹²            ~0.001   Bottom CD, web (10.4, 16.2)
─────────────────────────────────────────────────────────────────────────────────────────
Total N·p                                              ≈ 0.046
Y_random = exp(−0.046) = 95.5%

Edge tilt: dies in the outer 4.2 mm are 5.5% of wafer area (13.2.2); suppose 25% of those
  fail on landing/joint tolerance → 1.4% of dies lost
Y_total ≈ 95.5% × (1 − 0.014) = 94.2%
```

With error-correction and spare blocks, some of the random failures can be absorbed, so 94% is the yield **contribution of the hole module without redundancy**. The budget identifies where improvement is available:

```
Mode                   Contribution to loss     Lever
────────────────────────────────────────────────────────────────────
Stack particles        1.9%                     Deposition-tool cleanliness (not the etch)
Etch tail              1.3%                     σ_CD → overetch (12.2.3)
Etch particles         1.3%                     Clean interval, arc control (9.3)
Edge tilt              1.4%                     Edge ring and bias (5.4, 13.2)
```

Each is of the same order, which is typical for a mature process: no single mode dominates, and each of the many contributors is held to about one percent.

---

## 16.4 Cost of Ownership

### 16.4.1 Throughput

```
Cycle per wafer per deck:  41 min etch + ~4 min overhead (transfer, clean, season) = 45 min
Wafers per hour per chamber: 60 / 45 = 1.33
Wafers per year per chamber (8760 h, 85% utilization): 1.33 × 8760 × 0.85 = 9,930
```

### 16.4.2 Per-Wafer Cost

The values below are **placeholders** to show the structure. Replace them with a fab's own figures.

```
Cost element                          Basis                                   $/wafer
─────────────────────────────────────────────────────────────────────────────────────
Chamber depreciation                  $4.0M, 5 years, 9,930 wafers/yr          80.6
Edge ring                             $3,000 per ring, 730 wafers/ring          4.1
Upper electrode                       $8,000 per part, 2,170 wafers/part        3.7
Process gases                         Placeholder                               2.0
Power (≈ 20 kW × 0.75 h)              15 kWh × $0.10                            1.5
Cooling, abatement, HF handling       Placeholder                               2.0
Cleanroom area and labor              Placeholder                               5.0
────────────────────────────────────────────────────────────────────────────────────
Total per deck                                                                  ≈ 99
```

Capital depreciation is about 81% of the cost. This means that **throughput per chamber is the main lever**: halving chamber time nearly halves the cost per wafer.

### 16.4.3 Chambers for a Fab

```
Fab output: 100,000 wafer starts per month = 1.2×10⁶ wafers per year
Chambers per deck: 1.2×10⁶ / 9,930 = 121
Two decks: 242 chambers;  three decks: 363 chambers
```

For two decks, the hole etch alone would require some 240 chambers, which makes the **hole etch the single largest etch capital item** in a high-layer-count flow.

### 16.4.4 Faster, Costlier Chamber

Compare the reference process with a cryogenic HF process that etches in 28 min but with a chamber that costs 25% more (Chapter 4.5.5):

```
                          Reference          Cryogenic
Etch time                 41 min             28 min  (R₀ 450, A₀ 130 illustrative)
Cycle per wafer           45 min             33 min
Wafers/year/chamber       9,930              13,540
Chamber capital           $4.0M              $5.0M
Depreciation $/wafer      80.6               73.9     (−8%)
Chambers for 1.2M wafers  121                89       (−26%)
```

The cryogenic chamber costs more but needs fewer of them: depreciation per wafer drops 8%, and the chamber count drops 26%, which also reduces the facility footprint, maintenance, and operators. Yield, mask savings (Chapter 11.5), and fewer decks via lower ARDE could favor it further. The economic comparison depends on the actual capital and consumable figures.

---

## 16.5 Reading Wafer Maps Back to the Etch

```
Signature (failures or CD map)                  Likely cause                        Chapter
─────────────────────────────────────────────────────────────────────────────────────────────
Ring of failures in the outer 5 mm              Edge tilt, ring wear, He leak       5.4, 13.2, 8.5
Center spot                                     Gas distribution, plasma density    7.6, 5
Chamber-specific pattern                        Chamber mismatch, electrode wear    9.4, 5.6
Random scatter, low density                     Particles (stack or chamber)        2.6, 9.3
Failures at array-block edges                   Edge-row loading                    13.4
First wafer after clean                         Seasoning / first-wafer effect      9.2
Wafer-to-wafer drift                            Wall film, ring hours               9.4, 9.6
Cluster by lot                                  Mask CD, stack height, resist lot   15.4.1
```

Appendix G expands each into a troubleshooting sequence.

---

## 16.6 Summary & Key Takeaways

1. **The hole is judged downstream.** Bottom CD, twist, residue, and recess shape each map to a specific failure in the films, seed, or contact.

2. **Web margin is safe at 200 nm pitch but moves fast.** A 36 nm pitch reduction at constant CD takes the web to 40 nm, and a failure rate of about 10⁻⁵ per pair.

3. **Twist is the lever on web failure.** Cutting σ_x from 3 to 2 nm lowers the failure rate at a 40 nm web by a factor of about 5,000.

4. **The module's yield budget is a sum of ~1% items.** Stack particles, etch tail, etch particles, and edge tilt total about 6% loss.

5. **Cost is mostly capital.** About 81% of the $99 per wafer per deck in the reference, so throughput per chamber is the main lever.

6. **A faster chamber can pay for itself.** A 28 min cryogenic process at +25% capital cuts depreciation 8% and chamber count 26%.

---

## Study Questions

1. A product has a hole pitch of 180 nm, CD_top 115 nm, bowing +3 nm. Compute the mean web. If σ_web = 4.2 nm and the failure threshold is 18 nm, find z and the tail probability (use Φ(−z)).

2. A chamber costs $4.5M, depreciated over 5 years, runs 48 min per wafer cycle with 85% utilization on 8760 h. Compute depreciation per wafer. How many chambers are needed for 90,000 wafers per month?

3. For N = 1.6×10⁹ holes per die and the budget in Section 16.3 (per-hole probabilities summing to 3.4×10⁻¹¹), compute yield. What per-hole probability would give 97%?

4. A process change cuts etch time from 41 to 36 min but raises chamber capital from $4.0M to $4.4M. Compute the new depreciation per wafer (cycle = etch + 4 min, 85% utilization, 5 years) and compare.

5. A wafer map shows failures concentrated in the outer 6 mm and in one chamber of a six-chamber platform. Which two causes would you check first, and which monitors from Chapters 8, 9, and 15 would you use?

---

**Previous Chapter:** [Chapter 15: Metrology, Inspection & Advanced Process Control](./15-metrology-apc.md)  
**Next Chapter:** None (see the Appendices)

---

**Chapter 16 Development Status:** Complete  
**Version:** 1.0
