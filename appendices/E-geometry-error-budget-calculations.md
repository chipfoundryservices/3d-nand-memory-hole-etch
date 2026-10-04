# Appendix E: Geometry & Error-Budget Calculations

Worked derivations and templates for the quantitative relationships used in this book. Each section states the model, its assumptions, and a worked example using the reference process.

---

## E.1 Aspect Ratio, Taper, and Sidewall Angle

```
AR_top = H / CD_top;     A = z / w̄,   w̄ = (CD_top + CD_bottom)/2
Taper per side T = (CD_top − CD_bottom)/2
Sidewall angle = 90° − arctan(T / H)

Example: H = 8000, CD_top = 120, CD_bottom = 80
  AR_top = 66.7;   w̄ = 100;   A = 80
  T = 20 nm;   arctan(20/8000) = 0.143°;   sidewall angle = 89.857°
```

---

## E.2 ARDE Integration

```
R(A) = R₀ / (1 + A/A₀),   dz/dt = R,   z = A·w̄

t(A) = (w̄/R₀)(A + A²/(2A₀))        (Chapter 3.4.2)
A(t): solve A² + 2A₀A − 2A₀R₀t/w̄ = 0   →   A = −A₀ + √(A₀² + 2A₀R₀t/w̄)

Example: R₀ = 300, A₀ = 80, w̄ = 100, t = 40 min
  2A₀R₀t/w̄ = 2×80×300×40/100 = 19,200
  A = −80 + √(6400 + 19,200) = −80 + 160 = 80  ✓
```

### E.2.1 Depth Sensitivity to CD

```
t = z/R₀ + z²/(2 A₀ R₀ w̄)  →  dz/dw̄ = [A²/(2A₀)] / (1 + A/A₀)

Example: A = A₀ = 80:  dz/dw̄ = 40 / 2 = 20  (nm depth per nm CD)
Sensitivity to time: dz/dt = R(A) = 150 nm/min at the bottom
```

---

## E.3 Ion Transport

```
σ_θ² = T_⊥ / E;   θ_c = 1/A;   f = 1 − exp(−E/(2 T_⊥ A²))
E_req(f) = 2 T_⊥ ln[1/(1 − f)] · A²

Example: A = 80, T_⊥ = 0.5 eV, f = 50%:  E_req = 2 × 0.5 × 0.693 × 6400 = 4.43 keV

IED-averaged pass fraction for sinusoidal bias, E(φ) = E_m (1 + sin φ):
  f̄ = (1/2π) ∫₀^{2π} f(E_m(1 + sin φ)) dφ          (numerical)
  Example: E_m = 4 keV, A = 80: f̄ = 41%   (mono-energetic: 46%)

Charge deflection:  δθ = √(eΔV/E);   E = 4 keV, ΔV = 0.5 V → 11.2 mrad
```

---

## E.4 Neutral Transport

```
Clausing (no sticking):   W = 1/(1 + 3L/(8R))
Decay length (sticking):  λ = D/√(3s);   n(z) = n₀ exp(−z/λ)
Product density at the bottom:  Δn = Φ_p / (W · A_h · v̄ / 4)

Example: Φ_p = 10⁶ /s, W = 0.018, A_h = 7.85×10⁻¹⁵ m², v̄ = 247 m/s
  C = 0.018 × 7.85×10⁻¹⁵ × 247/4 = 8.7×10⁻¹⁵ m³/s
  Δn = 10⁶ / 8.7×10⁻¹⁵ = 1.1×10²⁰ m⁻³   (17% of 6.4×10²⁰ m⁻³ at 20 mTorr)
```

---

## E.5 Mask Budget

```
t_mask ≥ H/S_avg + t_cap + t_margin
For a stepped recipe: loss = L_ref × Σ(√E_i t_i) / (√E_ref t_ref)

Example: L_ref = 1.33 µm for 40 min at 4 keV (80 √keV·min);
         stages: (1.0, 7.5), (1.5, 14.4), (3.0, 14.8), (4.5, 3.3), (4.0, 1.1)
  Σ√E t = 7.5 + 17.6 + 25.6 + 7.0 + 2.2 = 59.9
  Loss = 1.33 × 59.9/80 = 1.00 µm
Statistical remaining: nominal − √[(σ_t)² + (σ_loss)²]
```

---

## E.6 Overetch and Landing

```
OE = d_sys + z_p · σ_r,    σ_r = (dz/dw̄) · σ_CD
Recess_max = d_nominal + OE / S_Si

Example: d_sys = 30 nm, σ_CD = 1.0 nm → σ_r = 20 nm, z_p = 6.7
  OE = 30 + 134 = 164 nm;  time = 164/150 = 1.09 min
  S_Si = 6:  Recess_max = 30 + 27 = 57 nm
```

---

## E.7 Stress, Bow, and Heat

```
Net stress:     σ = (t_O σ_O + t_N σ_N)/(t_O + t_N)
Stoney:         1/R = 6 σ t_f / (E′ t_s²);   bow δ = D²/(8R)
Heat flux:      q = Γ_i e E_i
Wafer ΔT:       ΔT = q/h;   τ = ρ c t / h
Clamp:          P = ½ ε₀ ε_r (V/d)²
Adsorption:     K(T₂)/K(T₁) = exp[(E_ads/k_B)(1/T₂ − 1/T₁)]

Example: σ = −30.7 MPa, t_f = 8 µm, t_s = 775 µm, E′ = 180 GPa
  1/R = 6 × 30.7×10⁶ × 8×10⁻⁶ / (180×10⁹ × (775×10⁻⁶)²) = 0.0136 m⁻¹ → R = 73 m
  δ = 0.09 / (8 × 73) = 154 µm
```

---

## E.8 Twist, Tilt, and Web

```
Twist:   σ_x² = 2 σ_s² ℓ z
Tilt:    displacement = H tan θ;  θ(r) = θ_e exp[−(R − r)/λ_t]
Web:     W = a − (r₁ + r₂) − Δ;   σ_W² = 2σ_x² + 2σ_r,rad² + σ_placement²
         z = (W̄ − W_min)/σ_W,   p = Φ(−z)

Example: a = 200, CD_max = 124 nm, σ_x = 3, σ_r,rad = 0.5, σ_placement = 1.7
  W̄ = 76 nm;  σ_W = √(18 + 0.5 + 2.9) = 4.6 nm;  W_min = 20 → z = 12.2
```

---

## E.9 Joint Alignment and Cost

```
Joint margin per side = (CD_plug,top − CD_deck2,bottom)/2
Misalignment (RSS)    = √(δ_OL² + δ_tilt² + δ_twist²)
Cost per wafer        = (Capex/years)/(wafers per year) + consumables + utilities + facility
Wafers per year       = (60 / cycle min) × 8760 × utilization

Example: (118 − 80)/2 = 19 nm margin;  √(5² + 14² + 9²) = 17.4 nm;  net margin 1.6 nm
         $4.0M / 5 / (1.333 × 8760 × 0.85) = $80.6 per wafer
```

---

**Appendix E Version:** 1.0  
**Last Updated:** 2026-10-04
