# Chapter 4: Etch Chemistries — Fluorocarbons, Hydrofluorocarbons & Cryogenic HF

## Overview

The chemistry of a memory-hole etch does four jobs at once. It etches oxide and nitride at the same rate. It protects the amorphous carbon mask. It passivates the sidewall so the hole does not bow. And it does all three in a hole where almost no gas-phase radical reaches the bottom (Chapter 3.3). Fluorocarbon plasmas, long the workhorse of oxide etch, were extended to this problem by adding hydrofluorocarbons for the nitride, oxygen for polymer control, and heavy additives for mask protection. More recently, cryogenic wafer temperatures and **hydrogen fluoride (HF)** based chemistry have shifted the balance, because HF has a very low sticking probability on the wall and condenses strongly on a cold surface.

This chapter covers each ingredient, the physical reason for its role, and the cryogenic shift.

**Learning Objectives:**
- Explain the role of fluorocarbon F/C ratio in oxide etch, polymer formation, and mask selectivity
- Explain how hydrofluorocarbons remove nitrogen and balance the oxide and nitride etch rates
- Use an adsorption model to explain the temperature dependence of HF-assisted etching
- Compare room-temperature and cryogenic recipes on rate, selectivity, and bowing
- Describe how sidewall passivation is delivered deep in the hole
- Outline the chemistry changes for an oxide/polysilicon stack

---

## 4.1 What the Chemistry Must Do

```
Job                      Requirement                        Chemical lever
──────────────────────────────────────────────────────────────────────────────────
Etch SiO₂                R_ox high, ion-assisted            CFx, F, HF
Etch Si₃N₄               R_N ≈ R_ox                         CHxFy (H removes N)
Protect the a-C mask     Mask loss ≤ 33 nm/min (S ≥ 6:1)    C-rich polymer; low O₂
Passivate the sidewall   Wall polymer at every depth        Heavy CxFy, redeposition,
                                                            low temperature
Remove products          Volatile, non-redepositing         SiF₄, CO, CO₂, HCN
Keep the bottom open     No etch stop (clogging)            O₂, lower C/F at depth
```

Several of these pull against each other. More polymer helps the mask and wall but starves the bottom. More oxygen opens the bottom but burns the mask. The recipe is a compromise that varies with depth (Chapter 7).

---

## 4.2 Fluorocarbon Plasmas

### 4.2.1 F/C Ratio

In a fluorocarbon plasma, the effective fluorine-to-carbon ratio of the gas-phase fragments sets the balance between etching and polymer deposition. Heavier, carbon-rich precursors give a lower F/C and more polymer:

```
Precursor     F/C ratio    Polymer tendency    Typical use
─────────────────────────────────────────────────────────────────────
CF₄           4            Low (etchant-rich)  Mask open, cap etch
CHF₃          3            Moderate            Nitride/oxide balance
C₄F₈          2            High                Main etch, wall passivation
C₄F₆          1.5          Very high           Selective to a-C, deep etch
C₅F₈          1.6          High                Alternative to C₄F₆
```

### 4.2.2 Why Low F/C Helps Mask Selectivity

Silicon dioxide and nitride etch in the presence of a thin CFx layer, which ions drive into reaction; the oxygen in SiO₂ leaves as CO and CO₂ and so consumes carbon. The oxide itself supplies a carbon sink. The amorphous carbon mask has no oxygen and no carbon sink: the polymer grows on it instead. A **higher C content in the gas** means more polymer growth on the mask and a thicker protective layer, and a lower etch rate for the mask.

```
Mask selectivity trend (ON : a-C), illustrative
  CF₄-based:   2:1 – 3:1
  C₄F₈-based:  4:1 – 6:1
  C₄F₆-based:  6:1 – 9:1
  Cryo HF:     8:1 – 12:1  (Section 4.5)
```

Reference value in this book: **6:1 averaged over the etch** (9:1 at the top, falling to 4.5:1 at the bottom, Chapter 3.4).

### 4.2.3 Oxygen and Argon

```
Gas     Role                              Typical share of flow
─────────────────────────────────────────────────────────────────
Ar      Dilution, ion generation, heavy   40–70%
        ion for sputter assist
O₂      Reduces polymer, opens the        2–10%
        bottom; burns carbon
```

The O₂ flow is a **knob with a narrow range**. Raising O₂ by 1 sccm in 20 sccm of fluorocarbon can open a clogging bottom, but a few sccm more starts to erode the carbon mask at an accelerating rate (the burn rate of a-C in O atoms is high). The rate of mask loss rises roughly in proportion to the oxygen atom flux, so the practical O₂ share is set by the mask budget (Chapter 11).

---

## 4.3 Hydrofluorocarbons and the Nitride

### 4.3.1 Why Nitride Needs Hydrogen

Removing silicon nitride in a fluorine plasma requires taking nitrogen away. Without hydrogen, nitrogen leaves as N₂ or NF₃ with some difficulty. With hydrogen available (from CHxFy, H₂, or HF), nitrogen leaves as **HCN** and **NH-containing products**, which are volatile at low energy. In practice:

```
Gas       H : F : C     Effect on SiN
─────────────────────────────────────────────────────
CHF₃      1 : 3 : 1     Moderate SiN rate, moderate polymer
CH₂F₂     2 : 2 : 1     High SiN rate, lower selectivity to oxide
CH₃F      3 : 1 : 1     Highest SiN rate, thin polymer; more mask erosion
H₂        direct        Strong SiN enhancement; adds H to the wall
```

### 4.3.2 Balancing Oxide and Nitride

Because the oxide etch is favored by C₄F₆/C₄F₈ and the nitride by CH₂F₂/CH₃F, the recipe blends them to hold R_ox : R_N = 1.00 ± 0.05 (Chapter 2.2.1):

```
Illustrative blend at 20 mTorr, Ar 150 sccm, O₂ 8 sccm:
  C₄F₆ 15 sccm, CH₂F₂ 6 sccm:  R_N / R_ox = 0.92
  C₄F₆ 15 sccm, CH₂F₂ 8 sccm:  R_N / R_ox = 1.01
  C₄F₆ 15 sccm, CH₂F₂ 10 sccm: R_N / R_ox = 1.10

Sensitivity ≈ +0.045 per sccm of CH₂F₂ near the target
```

```
Worked example: R_N/R_ox drifts to 1.08 from a 0.7 sccm MFC offset.
  Wall step per side (d = 5 nm, ρ = 1.08):
    2 × 5 × 0.08 / 2.08 = 0.38 nm per side
  Equivalent offset in CH₂F₂ flow: 0.08/0.045 = 1.8 sccm
```

Flow errors of a sccm are therefore visible in the wall. Mass-flow controllers for these gases are sized for the flow, and their accuracy is specified in percent of setpoint (Chapter 7.4).

### 4.3.3 Hydrogen and the Mask

Hydrogen-containing plasmas attack the carbon mask by forming hydrocarbons, which are volatile. The CH₃F and H₂ shares that help the nitride therefore cost mask thickness. The trade-off is built into the recipe: nitride-rich H content is chosen with the mask budget in mind (Chapter 11).

---

## 4.4 Passivation: Protecting the Wall

### 4.4.1 Where the Passivant Comes From

At the top of the hole, polymer precursors (CFx, CxFy radicals) arrive from the gas phase. Their sticking probability is high (s ≈ 0.05), so their decay length in a 100 nm hole is

```
λ = D / √(3s) = 100 nm / √(0.15) = 258 nm
```

Beyond about a micron, practically no gas-phase passivant arrives. The wall deeper in the hole is protected by **four routes**:

1. **Ion-driven redeposition**: fluorocarbon fragments sputtered from the upper wall travel down and re-stick
2. **Reflected ion flux**: ions that carry a CFx⁺ charge deposit their carbon on the lower wall
3. **Surface diffusion**: physisorbed fluorocarbons move down the film, especially when the wall is cold
4. **Etch-product redeposition**: SiFx, CO-derived species deposit on the lower wall (Chapter 3.7)

### 4.4.2 Consequence for Profile

Wall protection is strongest at the top and must be supplied by indirect routes at depth. If it is insufficient at depth, neutrals that do reach the wall (F, O) etch it laterally and the profile **bows**. If it is excessive, it narrows the hole and the bottom closes (**clogging**, **necking**, Chapter 11). Cryogenic temperature helps routes 1 to 3 (more adsorption, slower desorption), which is one reason the cryogenic hole has lower bowing and a larger bottom CD.

---

## 4.5 Cryogenic Etching and HF Chemistry

### 4.5.1 Why Cold Helps

A lower wafer temperature affects the etch in three ways:

1. **Adsorption rises exponentially.** Species that physisorb (HF, CxFy, H₂O) cover more of the surface at low temperature.
2. **Spontaneous (ion-free) chemical etching falls.** The sidewall, which sees few ions, etches less by neutrals.
3. **Polymer deposition and surface diffusion increase.** Passivation extends deeper.

### 4.5.2 Adsorption Model

For an adsorbate with adsorption energy E_ads in equilibrium with gas at partial pressure p:

```
θ = K·p / (1 + K·p),     K = K₀ · exp(E_ads / k_B T)

Reference: E_ads = 0.30 eV (physisorbed HF on oxide/nitride; illustrative)
  K(223 K, i.e. −50 °C) / K(293 K, 20 °C) = exp[(0.30 / 8.617×10⁻⁵) × (1/223 − 1/293)]
                                          = exp[3481 × 1.071×10⁻³] = exp(3.73) ≈ 42

If K·p = 0.020 at 20 °C:
   θ(20 °C)  = 0.020 / 1.020 = 2.0%
   θ(−50 °C) = 0.84 / 1.84   = 46%
```

The cold wafer carries over 20 times more adsorbed HF per unit area.

### 4.5.3 Sidewall Etch Versus Bottom Etch

The ion-activated reaction at the bottom needs adsorbate. More adsorbate there means a higher rate. At the wall, the ion flux is low, so the high coverage does not translate into much etching, while the thermally activated spontaneous etch is cut by the drop in temperature:

```
Spontaneous lateral etch ∝ exp(−E_a / k_B T),  E_a = 0.15 eV (illustrative)
  Ratio, 223 K / 293 K = exp[−(0.15/8.617×10⁻⁵)(1.071×10⁻³)] = exp(−1.86) = 0.155

Lateral spontaneous etch is 6.4× slower at −50 °C than at 20 °C.
```

Together, these two effects raise the bottom rate and lower the sidewall attack, which is the profile win.

### 4.5.4 Low Sticking Brings HF to the Bottom

HF is a closed-shell molecule that does not bond to the polymer-covered wall in the way a CFx radical does. If its wall-sticking coefficient is 10⁻⁴ (versus 5×10⁻² for CFx), its decay length is

```
λ_HF = 100 nm / √(3 × 10⁻⁴) = 5.8 µm → density at 8 µm = exp(−8/5.8) = 25% of the top
λ_CFx = 258 nm                        → density at 8 µm = exp(−31) ≈ 3×10⁻¹⁴
```

HF reaches the bottom at a density many orders of magnitude above that of CFx, so the chemically active supply is no longer purely ion- and surface-driven. That is the main reason the cold HF process shows a smaller ARDE (Chapter 6, Chapter 8).

### 4.5.5 Room Temperature Versus Cryogenic

```
Metric (reference-like, illustrative)     Room-temp fluorocarbon     Cryogenic HF-based
─────────────────────────────────────────────────────────────────────────────────────────
Wafer temperature                         +20 to +60 °C              −60 to −30 °C
R₀ (A → 0)                                300 nm/min                 400–500 nm/min
A₀                                        80                         110–150
Total etch time (8 µm)                    40 min                     25–30 min
Mask selectivity (ON : a-C)               6:1                        9:1–12:1
Bottom CD / top CD                        0.67                       0.75–0.85
Bowing                                    + 4 nm                     +1–2 nm
Wafer heat load                           Moderate                   Very high; needs ESC at ≈ −70 °C
Hardware                                  Standard                   Cryo ESC, chiller, condensation
                                                                     control
```

These numbers are indicative of the direction of change, not of any particular tool.

### 4.5.6 Hazards of HF

HF is highly toxic and corrosive. HF-based etches require gas-delivery materials, abatement, and monitoring designed for it, and wall films that contain HF-derived fluorides. Cold surfaces trap HF and water. A warm-up after maintenance must be managed to avoid releasing absorbed acid or forming condensate on cold parts. These are equipment and safety design questions, covered in Chapter 8 and in Appendix C (procedures).

---

## 4.6 Oxide/Polysilicon (OP) Stack Chemistry

For OP stacks, the polysilicon layers cannot be etched in fluorocarbon at the oxide rate, so the etch alternates or combines two mechanisms:

```
Layer        Etchants               Passivant
──────────────────────────────────────────────────────────────────────
SiO₂         CxFy, HF               CFx polymer
Poly-Si      HBr, Cl₂, SF₆, NF₃     SiOxBry, SiOxCly, SiOxFy (from products
                                    and O₂)
```

Silicon etch products (SiBrx, SiClx) oxidize with O₂ addition to form a passivation film on the wall (SiOxBry). That film is a **built-in sidewall protection**, and it is why a halogen-based silicon etch bows less than a fluorocarbon oxide etch of the same depth. Matching the oxide and poly lateral rates is the hard part, and wall modulation in OP stacks is typically larger (Chapter 2.1.2).

---

## 4.7 Summary & Key Takeaways

1. **Carbon-rich fluorocarbons protect the mask.** C₄F₆ raises mask selectivity to 6:1 and above, because the polymer it forms on carbon has no oxygen sink.

2. **Hydrogen removes nitrogen.** CH₂F₂/CH₃F balance the nitride rate to the oxide rate within 5%, at a cost in mask erosion.

3. **Oxygen is a narrow knob.** It opens the bottom and burns the mask at the same time.

4. **The wall is passivated by indirect routes at depth.** No gas-phase CFx survives past 1 µm, so redeposition, reflection, and surface diffusion carry the load.

5. **Cold means more adsorbate and less spontaneous etch.** The reference model gives a 42× increase in the adsorption constant and 6.4× lower spontaneous lateral etch between 20 °C and −50 °C.

6. **HF reaches the bottom.** Its low sticking gives a decay length of several µm, which is what lowers ARDE in cryogenic etch.

---

## Study Questions

1. A chemistry has R_N/R_ox = 0.96 at 7 sccm CH₂F₂ with a sensitivity of 0.04 per sccm. What flow gives a ratio of 1.00? What wall step per side results from 6 nm of mean lateral etch if the ratio is 0.96 instead?

2. Compute the ratio K(−40 °C)/K(20 °C) for E_ads = 0.25 eV. If K·p = 0.03 at 20 °C, what are the coverages at both temperatures?

3. For a fluorocarbon neutral with s = 0.02 in a hole of D = 90 nm, compute λ and the fraction of the opening density that reaches 1.5 µm. What sticking coefficient would give 20% survival at 8 µm?

4. Spontaneous lateral etch has E_a = 0.20 eV. By what factor does it fall if the wafer is cooled from 293 K to 233 K? If a room-temperature process bows 4 nm at this lateral etch, what bowing would the cold process show, all else equal?

5. A recipe raises O₂ from 8 to 10 sccm to cure bottom clogging, and the a-C loss rate rises from 33 to 41 nm/min. Over a 40 min etch with 1.8 µm of mask, what mask remains? What is the new average selectivity for an 8.0 µm hole?

---

**Previous Chapter:** [Chapter 3: High-Aspect-Ratio Etch Physics — Ion and Neutral Transport](./03-har-etch-physics.md)  
**Next Chapter:** [Chapter 5: Reactor Architecture for Multi-keV High-Aspect-Ratio Etch](./05-reactor-architecture.md)

---

**Chapter 4 Development Status:** Complete  
**Version:** 1.0
