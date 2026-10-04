# Appendix F: Endpoint & Metrology Reference

Reference tables for optical-emission endpoint, metrology selection, sampling, and calibration. Precision values are representative and depend on tool, model, and target.

---

## F.1 Optical Emission Lines

```
Species    λ (nm)          Source                       Use in hole etch
─────────────────────────────────────────────────────────────────────────────────────
CN         388.3           Nitride etch product         Falls at landing; stage trend
CO         483.5, 519.8    Oxide etch; mask burn        Main-etch stability; mask state
F          703.7           Etchant                      Rises as loading falls; clean endpoint
Ar         750.4           Actinometer                  Normalize intensity
Si         288.2           Exposed poly                 Landing signal
SiF        440.1           Silicon + fluorine           Landing signal (alternative)
C₂         516.5           Carbon-rich plasma           Polymer state
H (Hα)     656.3           Hydrogen species             HF/H₂ chemistry state
```

### F.1.1 Signal Budget

```
Etch product flux:  9.5×10¹⁷ atoms/s (10¹² holes × 9.5×10⁵ atoms/s);  Si-bearing ≈ 3.3×10¹⁷ /s
Gas flow 180 sccm = 8.1×10¹⁹ molecules/s;  product ≈ 0.4% of exhaust
```

### F.1.2 Endpoint Parameters

```
Parameter                      Typical setting
─────────────────────────────────────────────────────────────────
Signal                         Si 288 nm (rise) or CN 388 nm (fall), normalized to Ar 750 nm
Smoothing                      1–3 s moving average
Trigger                        50% of the transition (rise or fall)
Fixed overetch after trigger   66–82 s (Chapter 12.2, 12.3)
Maximum etch time              Stage-4 time + 25% (fail-safe)
Minimum signal change          > 5× noise (3σ); else fall back to time
```

---

## F.2 Metrology Selection

```
Measurand                 Preferred method           Backup / calibration       P/T target
─────────────────────────────────────────────────────────────────────────────────────────────
Top CD                    CD-SEM                     OCD                        ≤ 0.1
Bottom CD                 OCD (array average)        CD-SAXS, HV-SEM            ≤ 0.2
Depth                     OCD                        Cross-section              ≤ 0.2
Taper / bowing            CD-SAXS, OCD               Cross-section TEM          ≤ 0.25
Tilt                      HV-SEM                     Cross-section              ≤ 0.25
Twist, ellipticity        HV-SEM                     Cross-section              ≤ 0.3
Wall roughness            CD-SEM (top), TEM          —                          ≤ 0.3
Blind holes / shorts      E-beam voltage contrast    Electrical test            n/a (count)
Mask remaining            OCD or x-ray reflectivity  Cross-section              ≤ 0.2
Bow / stress              Optical bow tool           —                          ≤ 0.1
Particles                 Blanket particle scanner   —                          n/a
```

```
P/T = 6σ_meas / (USL − LSL)
Bottom CD ±4 nm:  OCD σ = 0.17 nm → 6σ = 1.0 nm → P/T = 0.125
```

---

## F.3 Sampling Plan (Illustrative)

```
Monitor                         Frequency                  Sites           Action on fail
──────────────────────────────────────────────────────────────────────────────────────────────
FDC traces                      Every wafer                n/a             Hold wafer; alarm
OCD (depth, CD, taper)          1 wafer per lot per chamber 9–13 sites     R2R update; hold if OOS
HV-SEM (tilt, twist, bottom)    1 wafer per chamber per day 5 sites + edge Tilt/twist trend
CD-SAXS / cross-section         Weekly, and after PM        Calibration     Recalibrate OCD
Particle monitor wafer          Daily and after PM          Blanket         Clean / PM
E-beam inspection               1 wafer per lot per week    1–2 die         Systematic defect review
Electrical sort                 All wafers                  All die         Yield feedback (weeks)
```

---

## F.4 OCD Model Parameters (Typical Fit Set)

```
Parameter                        Fit range              Sensitivity
──────────────────────────────────────────────────────────────────────
Top CD                           110–130 nm             High
Bottom CD                        70–90 nm               Moderate (needs IR)
Depth (stack height)             7.5–8.5 µm             High (stack period)
Bow amplitude / depth            0–8 nm / 1–4 µm        Moderate
Layer thickness (O, N)           ± 3 nm                 Periodic stack signal
Mask thickness remaining         0.2–1.0 µm             High
Hole pitch                       fixed (200 nm)         Constant
```

Models are tuned on cross-sections and CD-SAXS. A change in film optical properties (nitride hydrogen, oxide density) changes the model, so OCD recipes are re-verified when deposition conditions change.

---

## F.5 E-Beam Throughput and Detectability

```
Holes per mm² = 1 / 0.03464 µm² × 10⁶ = 2.9×10⁷
Inspection speed 100 mm²/h → 2.9×10⁹ holes/h;  one die (1.33×10⁹ holes) ≈ 28 min

Holes to inspect to expect 3 failures:   N = 3/p
  p = 10⁻⁹:   3×10⁹  → 1.0 h
  p = 10⁻¹⁰:  3×10¹⁰ → 10 h
  p = 10⁻¹¹:  3×10¹¹ → 103 h
```

---

## F.6 Calibration Checklist

```
1  Cross-section reference wafers (stack, hole profile) for each product.
2  OCD model validated against CD-SAXS for bottom CD and taper (≤ 0.5 nm bias).
3  HV-SEM tilt verified against cross-section on at least 3 edge sites.
4  Gauge R&R per tool: precision, matching between tools (TMU).
5  OES: lines verified with Ar actinometry; window cleanliness check.
6  Sensor-wafer thermal map for ESC zones (Appendix C.5).
7  Revalidate after PM, after a deposition change, and quarterly.
```

---

**Appendix F Version:** 1.0  
**Last Updated:** 2026-10-04
