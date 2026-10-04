# Appendix D: Process Windows & Lookup Tables

Lookup tables built from the reference process. All entries come from the models in Chapters 3, 6, 11, and 12 and are consistent with the worked examples.

---

## D.1 Reference Process Summary

```
Stack:      SiO₂ 28 nm / Si₃N₄ 32 nm per pair (p = 60 nm); 128 pairs + 0.32 µm → H = 8.0 µm
Hole:       CD_top 120 nm, CD_bottom 80 nm (mean 100 nm), AR_top 67:1, A = 80 at full depth
Mask:       1.8 µm a-C + 60 nm SiON
ARDE:       R(A) = 300 nm/min / (1 + A/80),  w̄ = 100 nm,  t(80) = 40 min
Ions:       T_⊥ = 0.5 eV;  mean 4 keV (CW, constant) or stepped 1.0 → 4.5 keV
Die:        512 Gb TLC, 1.33×10⁹ holes, 200 nm hex pitch, ~46 mm² array
Wafer:      ~750 die, ~10¹² holes
```

---

## D.2 Ion Pass Fraction f_pass (%) by Aspect Ratio and Mean Ion Energy

Mono-energetic ions, T_⊥ = 0.5 eV, f = 1 − exp(−E/(2 T_⊥ A²)).

```
A      1 keV   2 keV   4 keV   6 keV   8 keV   10 keV    E_req for 46.5% (keV)
──────────────────────────────────────────────────────────────────────────────────
 20     92      99     100     100     100     100        0.25
 40     46      71      92      98      99     100        1.0
 60     24      43      67      81      89      94        2.25
 80     14      27      46      61      71      79        4.0
100     10      18      33      45      55      63        6.25
120      7      13      24      34      43      50        9.0
160      4       8      14      21      27      32        16.0
```

(For a sinusoidal 400 kHz bias with the same mean energy, subtract about 5 points at A = 80, 4 keV; Chapter 5.2.2.)

---

## D.3 ARDE Lookup (R₀ = 300 nm/min, A₀ = 80, w̄ = 100 nm)

```
Depth (µm)   A     R (nm/min)   t(A) (min)    Stage (Chapter 7.2)
─────────────────────────────────────────────────────────────────
 1           10    267           3.5
 2           20    240           7.5          End of Stage 1
 3           30    218          11.9
 4           40    200          16.7
 5           50    185          21.9          End of Stage 2
 6           60    171          27.5
 7           70    160          33.5
 7.5         75    155          36.7          End of Stage 3
 8           80    150          40.0          End of Stage 4 (+1.1 min overetch)
```

---

## D.4 Recipe Window (Stage 2, Reference ± Limits)

```
Parameter                   Nominal     Window (±)        Effect at window edge
──────────────────────────────────────────────────────────────────────────────────────
Pressure                    15 mTorr    ± 1.0             Collisionless fraction ± 3 pts
Ar                          150 sccm    ± 3               Negligible
C₄F₆                        14 sccm     ± 0.3             Mask selectivity ± 2.4%
CH₂F₂                       8 sccm      ± 0.3             R_N/R_ox ± 0.014
O₂                          9 sccm      ± 0.3             Mask loss ± 3.6%, bottom CD ± 0.3 nm
Source power (60 MHz)       3.0 kW      ± 2%              Ion flux ± 1–2%
Bias power (mean 1.5 keV)   ~ 3 kW      ± 2%              Bias voltage ± 1–2%
ESC setpoint                −70 °C      ± 0.3 K           Rate ± 0.6%
Backside He                 20 Torr     ± 1               h ± 2–3%
Stage time                  14.4 min    ± 3%              Depth ± 0.4 µm (this stage)
```

---

## D.5 Mask Thickness and Selectivity (H = 8.0 µm, cap 0.06 µm, margin 0.3 µm)

```
S_avg    t_loss (µm)    t_mask,min (µm)    Mask AR (110 nm opening)
─────────────────────────────────────────────────────────────────────
 4        2.00           2.36                21:1
 5        1.60           1.96                18:1
 6        1.33           1.69                15:1
 8        1.00           1.36                12:1
 9        0.89           1.25                11:1
12        0.67           1.03                 9:1
```

---

## D.6 Overetch and Landing (Chapter 12)

```
Tail probability    z      Overetch for σ_r = 20 nm,         Time at 150 nm/min
                           30 nm systematic residual
──────────────────────────────────────────────────────────────────────────────
10⁻⁶                4.75   125 nm                            50 s
10⁻⁸                5.61   142 nm                            57 s
10⁻⁹                6.00   150 nm                            60 s
10⁻¹¹               6.71   164 nm                            66 s

Recess_max = 30 nm + OE / S_Si:
  S_Si = 8: 50 nm     6: 57     4: 71     3: 85     2: 112  (120 nm poly layer)
```

---

## D.7 Heat Load and Wafer Temperature (Γ_i = 6.6×10¹⁵ cm⁻² s⁻¹, h = 1500 W/m²K, ESC −70 °C)

```
Mean ion energy   Heat (W/cm²)   Wafer power (kW)    ΔT (K)    Wafer T (°C)
───────────────────────────────────────────────────────────────────────────────
1.0 keV           1.06           0.75                7.1       −62.9
1.5               1.59           1.12                10.6      −59.4
3.0               3.17           2.24                21.1      −48.9
4.0               4.23           2.99                28.2      −41.8
4.5               4.76           3.36                31.7      −38.3
```

---

## D.8 Specification Summary (Reference Targets)

```
Parameter                      Target                         Chapter
──────────────────────────────────────────────────────────────────────────
Depth                          8.0 µm ± 2%                    1.4
CD_top                         120 nm ± 3 nm                  1.4
CD_bottom                      80 nm ± 4 nm                   10.4
Bowing                         ≤ +4 nm                        10.3
Taper (per side)               0.10–0.15° (≈ 20 nm)           10.1
Edge tilt                      ≤ 0.1° (≤ 14 nm at bottom)     13.2
Twist                          ≤ 3 nm rms                     13.1
Ellipticity                    ≤ 3%                           10.5
Wall roughness                 ≤ 2 nm rms                     10.5
Hole-to-hole CD (3σ)           ≤ 3 nm                         12.1
Mask remaining                 ≥ 0.3 µm                       11.2
Poly recess                    30–60 nm                       12.3
Particle adders                ≤ 5 per wafer (≥ 80 nm)        9.3.2
ESC uniformity                 ± 0.5 K                        8.3
```

---

**Appendix D Version:** 1.0  
**Last Updated:** 2026-10-04
