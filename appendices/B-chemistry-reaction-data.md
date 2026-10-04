# Appendix B: Chemistry & Reaction Data

Reaction pathways, transport constants, and adsorption data for the chemistries of Chapter 4. Rate constants are not given: rates in a hole are set by transport and ion flux more than by gas-phase kinetics, so the tables below focus on the quantities the models use.

---

## B.1 Main Reaction Pathways

```
Process                               Net reaction (schematic)
──────────────────────────────────────────────────────────────────────────────────
Oxide etch (ion-assisted)             SiO₂ + CFx* → SiF₄ + CO / CO₂   (ion-driven)
Nitride etch (hydrogen-assisted)      Si₃N₄ + F*, H* → SiF₄ + HCN / NH₃   (ion-driven)
Mask burn by oxygen                   C(a-C) + O* → CO / CO₂
Mask erosion by hydrogen              C(a-C) + H* → CxHy (volatile)
Polymer growth on surfaces            CFx* + surface → (CF₂)ₙ
Polymer removal by oxygen             (CF₂)ₙ + O* → COF₂ / CO / CO₂ / F*
HF-assisted etch (cold)               SiO₂ + 4HF (ads) → SiF₄ + 2H₂O  (ion/thermal-assisted)
Poly etch (OP stack)                  Si + Br*, Cl*, F* → SiBrx, SiClx, SiF₄
Sidewall film in halogen etch         SiBrx + O₂ → SiOxBry (wall passivation)
Waferless autoclean                   (CF₂)ₙ + O* → CO, COF₂;  SiOxFy + NF₃* → SiF₄
```

---

## B.2 Transport Data

### B.2.1 Mean Thermal Speeds at 300 K

```
Species    Mass (u)    v̄ (m/s)         Species    Mass (u)    v̄ (m/s)
────────────────────────────────────────────────────────────────────────
F          19.0        578              CF₃        69.0        303
O          16.0        630              Ar         39.95       399
HF         20.0        563              SiF₄       104.1       247
CF₂        50.0        356              C₄F₆       162         198
CO         28.0        476              O₂         32.0        446
```

### B.2.2 Clausing Transmission, No Sticking (W = 1/(1 + 3L/(8R)))

```
Hole depth    D = 100 nm (L/R=2L/D)    W
──────────────────────────────────────────────────
 2 µm         40                       0.063
 4 µm         80                       0.032
 6 µm        120                       0.022
 8 µm        160                       0.016
12 µm        240                       0.011
16 µm        320                       0.008
```

### B.2.3 Decay Length With Sticking (λ = D/√(3s))

```
s          D = 80 nm    D = 100 nm    D = 120 nm
────────────────────────────────────────────────────
0.10       146 nm       183 nm        219 nm
0.05       207          258           310
0.01       462          577           693
10⁻³       1.46 µm      1.83 µm       2.19 µm
10⁻⁴       4.62 µm      5.77 µm       6.93 µm
```

Typical sticking: F on polymer-covered wall 0.01–0.1; CFx 0.01–0.1; O 0.01–0.1 on clean oxide (lower on polymer); HF on warm walls ~10⁻⁴ (illustrative).

---

## B.3 Adsorption and Temperature Data

```
Physisorption constant K ∝ exp(E_ads / k_B T);  θ = Kp/(1 + Kp)

Ratio K(T₂)/K(T₁) = exp[(E_ads/k_B)(1/T₂ − 1/T₁)],   E_ads/k_B = 11,600 K per eV

E_ads (eV)    K(223 K)/K(293 K)    K(210 K)/K(235 K)
─────────────────────────────────────────────────────
0.20          12                   3.2
0.25          22                   4.4
0.30          42                   5.8
0.40          144                  10.5
```

```
Spontaneous lateral etch ∝ exp(−E_a/k_B T):  E_a = 0.15 eV → ratio 223 K / 293 K = 0.155
                                              E_a = 0.20 eV → 0.083
```

---

## B.4 Ion Data

```
Quantity                                 Value / relation
──────────────────────────────────────────────────────────────────────────
Angular spread per axis                  σ_θ ≈ √(T_⊥ / E_i)
Acceptance half-angle                    θ_c ≈ 1/A
Pass fraction (2D Gaussian)              f = 1 − exp(−θ_c² / 2σ_θ²) = 1 − exp(−E_i /(2 T_⊥ A²))
Energy for target f                      E_req = 2 T_⊥ ln[1/(1 − f)] A²
Deflection from lateral potential ΔV     δθ ≈ √(eΔV / E_i)
Sputter yield vs. angle                  Peak 2–3× normal incidence at 45–70°
Sheath thickness (collisionless)         s = (√2/3) λ_D (2V_s / T_e)^(3/4)
Transit time (collisionless)             τ_i ≈ 3 s / v_f
```

---

## B.5 Gas Blend Sensitivities (Reference Chemistry)

```
Parameter                  Change      Effect (illustrative)
──────────────────────────────────────────────────────────────────────────
CH₂F₂ (8 sccm)             +1 sccm     R_N/R_ox +0.045
O₂ (9 sccm)                +1 sccm     Mask loss +12%, bottom CD +1.0 nm
C₄F₆ (14 sccm)             −1 sccm     Mask selectivity −8%, bowing +0.4 nm
Pressure                   +1 mTorr    Collisionless fraction −3 pts, A₀ −2%
Wafer temperature          +1 K        Etch rate +2%, bottom CD +0.6 nm, mask loss −1%
Ion energy                 +5%         Pass fraction +1.7 pts, bottom CD +0.8 nm
```

---

**Appendix B Version:** 1.0  
**Last Updated:** 2026-10-04
