# Appendix A: Stack, Mask & Gas Material Properties

Reference properties used in the worked examples. Film values are representative of PECVD and ALD layers and vary with deposition conditions, so treat them as starting points and replace them with measured values when available.

---

## A.1 Stack Films

```
Property                       PECVD SiO₂ (TEOS)   PECVD Si₃N₄       Poly-Si (landing)
────────────────────────────────────────────────────────────────────────────────────────
Reference thickness            28 nm               32 nm             120 nm
Density (g/cm³)                2.2                 2.7–2.9           2.33
Atom density (cm⁻³)            6.6×10²²            8.4×10²²          5.0×10²²
Refractive index (633 nm)      1.46                1.90–2.00         ~3.9 (k ≠ 0)
Hydrogen content (at%)         1–3                 15–20             < 1
Single-film stress (MPa)       −180 (compressive)  +100 (tensile)    ±100 (anneal-dependent)
Deposition temperature         350–400 °C          350–400 °C        LPCVD ~ 550–620 °C
Average atoms in ON stack      7.5×10²² cm⁻³ (used for removal-rate arithmetic)
```

```
Stack metrics (reference)
  Pair pitch p = 60 nm;  128 pairs = 7.68 µm;  select / dummy / cap = 0.32 µm
  Total stack H = 8.0 µm
  Net stack stress (28/32 weighting) = −30.7 MPa
  Wafer bow (Stoney, 775 µm Si, E′ = 180 GPa) ≈ 150 µm
```

---

## A.2 Silicon Substrate

```
Property                             Value
─────────────────────────────────────────────────────────────
Diameter / thickness                 300 mm / 775 µm
Area                                 707 cm²
Density                              2329 kg/m³
Specific heat (300 K)                700 J/kg·K
Biaxial modulus E′ = E/(1 − ν)       ≈ 180 GPa (Si(100))
Heat capacity per area (ρ c t)       1263 J/m²K
```

---

## A.3 Mask Stack

```
Layer                      Thickness     Notes
───────────────────────────────────────────────────────────────────────────────
Photoresist                ~ 0.3 µm      Consumed in the mask-open step
BARC                       ~ 0.05 µm     Litho control
SiON cap / hard mask       0.06 µm       Opens the carbon; ~0.06 µm lost at breakthrough
Amorphous carbon (a-C)     1.8 µm        6:1 average selectivity (constant), 8:1 stepped
Boron-doped carbon         1.2–1.25 µm   ~9:1 (illustrative)
Tungsten-doped carbon      1.0–1.05 µm   ~12:1 (illustrative); metal-contamination controls required
```

```
Mask thickness vs selectivity (H = 8.0 µm, cap 0.06 µm, margin 0.3 µm)
  S = 4:1: 2.36 µm    5:1: 1.96    6:1: 1.69    8:1: 1.36    9:1: 1.25    12:1: 1.03
```

---

## A.4 Cell Films Deposited in the Hole

```
Film (from the wall inward)        Thickness per side    Both sides
──────────────────────────────────────────────────────────────────
Blocking oxide (SiO₂ / Al₂O₃)      6 nm                  12 nm
Charge-trap nitride (Si₃N₄)        6 nm                  12 nm
Tunnel oxide (SiO₂)                4 nm                   8 nm
Polysilicon channel                7 nm                  14 nm
Total                              23 nm                 46 nm
```

---

## A.5 Process Gases

```
Gas       Formula           Molar mass (g/mol)    Boiling point (°C)    Use
─────────────────────────────────────────────────────────────────────────────────────────
Argon     Ar                39.95                 −186                  Dilution, ion supply
Oxygen    O₂                32.00                 −183                  Polymer control, mask burn
Carbon tetrafluoride  CF₄   88.00                 −128                  Cap / mask breakthrough
Hexafluorobutadiene   C₄F₆  162.03                  6                   Main etch, mask protection
Octafluorocyclobutane C₄F₈  200.03                 −6                   Passivation
Fluoroform            CHF₃  70.01                 −82                   Oxide/nitride balance
Difluoromethane       CH₂F₂ 52.02                 −52                   Nitride etch
Fluoromethane         CH₃F  34.03                 −78                   Nitride etch, thin polymer
Hydrogen fluoride     HF    20.01                  19.5                 Cryogenic etch (toxic, corrosive)
Nitrogen trifluoride  NF₃   71.00                 −129                  Chamber clean
Sulfur hexafluoride   SF₆   146.06                −64 (sublimes)         Chamber clean, poly etch
Hydrogen bromide      HBr   80.91                 −66                   Poly (OP stack) etch
Carbonyl sulfide      COS   60.08                 −50                   Mask-open sidewall protection
```

---

## A.6 Etch Products

```
Product    Mass (u)    Mean speed at 300 K (m/s)    Origin
───────────────────────────────────────────────────────────────
SiF₄       104.1       247                          Oxide and nitride etch
CO         28.0        476                          Oxide etch, mask burn
CO₂        44.0        ~380                         Oxide etch
HCN        27.0        ~485                         Nitride etch with hydrogen
NF₃/N₂     71 / 28     ~300 / 476                   Nitride etch without hydrogen
```

---

## A.7 Plasma Constants (Reference)

```
Quantity                                    Value
──────────────────────────────────────────────────────────────────────────────
Plasma density n                            5×10¹⁰ cm⁻³
Electron temperature T_e                    3 eV
Debye length λ_D                            58 µm
Collisionless sheath at 5 kV                1.2 cm
Ion transverse temperature T_⊥              0.5 eV
Charge-exchange cross-section (CFx⁺)        5×10⁻¹⁵ cm²
Wafer ion flux Γ_i                          6.6×10¹⁵ cm⁻² s⁻¹
Wafer ion heat at 4 keV                     4.2 W/cm² (3.0 kW over 707 cm²)
```

---

**Appendix A Version:** 1.0  
**Last Updated:** 2026-10-04
