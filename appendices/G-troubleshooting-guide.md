# Appendix G: Troubleshooting Guide

Symptom-driven guide for memory-hole etch excursions. For each symptom: likely causes ranked from most to least common, checks to separate them, and corrective actions. Chapter references point to the underlying physics.

---

## G.1 Bottom CD Too Small, or Hole Not Opening (Clogging)

```
Likely causes                              Checks                             Actions
─────────────────────────────────────────────────────────────────────────────────────────────
1. Excess polymer at depth (O₂ low,        MFC rate-of-rise; O₂ trend;         Restore O₂; raise stage 3/4
   C₄F₆ high) (Ch. 11.4)                   OES CO/C₂ trend                     O₂; recalibrate MFC
2. Wafer temperature low (He leak low,     Zone temps, He pressure/leak;        Fix He; zone offset;
   chiller) (Ch. 8)                        sensor wafer                         chiller service
3. Ion energy low (bias voltage drift)     Bias V at fixed power; ring hours   Power offset; replace ring
   (Ch. 6)
4. Mask CD small (upstream)                OCD on mask-open wafer               Feed-forward; mask-open fix
5. Fresh chamber / first wafer             Wafer index after clean              Season (Ch. 9.2)
6. Pressure high at depth                  Throttle position, gauge             Calibrate gauge; throttle
```

## G.2 Top CD Too Large, Mask Remaining Too Small

```
Likely causes                              Checks                             Actions
─────────────────────────────────────────────────────────────────────────────────────────────
1. O₂ high or CH₂F₂/H₂ high (Ch. 4.3.3)   MFC checks; mask remaining by OCD   Restore flows
2. Bias too high early (Ch. 6.5)           Stage 1/2 voltage                   Lower early bias
3. Chamber wall F-rich (post-clean)        First-wafer trend; OES F/Ar         Season
4. Mask thickness low or mask-open CD big  Incoming OCD of mask               Feed-forward
5. Temperature high (less polymer)         Zone temps                          Recalibrate
6. Pressure low at top                     Pressure trace                      Restore
```

## G.3 Bowing Above Spec

```
Likely causes                              Checks                             Actions
─────────────────────────────────────────────────────────────────────────────────────────────
1. Ion energy low → more off-axis ions     Bias V; pass fraction estimate       Raise energy; pulsed bias
   (Ch. 3.6, 10.3)
2. Thin sidewall polymer (O₂ high,         O₂, C₄F₆, temperature trends         Restore polymer; cool
   temperature high)
3. Faceted mask / mask erosion (Ch. 11.3)  Mask remaining, top CD growth        Mask budget; reduce early E
4. Neutral flux high (pressure high)       Pressure trend                       Lower pressure at the bow depth
5. Layer-change step (joint, select gate)  Bow depth vs layer map               Add a stage at that depth
```

## G.4 Depth Too Shallow, Blind Holes

```
Likely causes                              Checks                             Actions
─────────────────────────────────────────────────────────────────────────────────────────────
1. Overetch too short or EP trigger late   Endpoint trace; OES signal level     Adjust OE; recalibrate EP
2. Rate low (wafer cold, ion energy low)   Zone T, voltage; stage times         Correct
3. CD small (RIE lag: 20 nm per nm)        OCD bottom CD, depth correlation     Fix CD (Ch. 12.1)
4. Clogging (G.1)                          Bottom CD                            See G.1
5. Particles (stack or chamber)            Blanket particle; map of blind holes Clean interval; PM; stack tool
6. Edge-region cold (He leak)              Edge zone He; depth map              Edge zone offset
```

## G.5 Poly Recess Too Large or Punch-Through

```
Likely causes                              Checks                             Actions
─────────────────────────────────────────────────────────────────────────────────────────────
1. Overetch too long (EP late or stage      Endpoint log; stage-4 time          Retune OE
   time drift)
2. Selectivity to poly low (O₂ too high,    S_Si on blanket; OE gas             Lower O₂; raise C₄F₆ in OE
   energy too high) (Ch. 12.3)
3. Depth spread large (CD variation)        σ_CD hole-to-hole                   Improve CD uniformity
4. Poly layer thin (incoming)               Thickness record                    Feed-forward OE
5. Edge-row holes deeper (loading)          Map by block position               Dummy holes (Ch. 13.4)
```

## G.6 Edge Tilt and Edge CD Excursion

```
Likely causes                              Checks                             Actions
─────────────────────────────────────────────────────────────────────────────────────────────
1. Edge ring worn (Ch. 5.4.2, 9.4)         Ring hours; ring height              Adjust or replace ring
2. He leak at the edge / bowed wafer        Edge-zone leak; wafer bow           Staged chucking; edge offset
   (Ch. 8.5)
3. Ring or cover-ring misplacement          Alignment after PM                  Re-seat; re-qualify
4. Edge gas / temperature tuning off        Radial CD/depth map                 Retune (Ch. 13.3)
5. Arcing damage at the edge                Arc log; inspection                 Repair
```

## G.7 Twist or Ellipticity High

```
Likely causes                              Checks                             Actions
─────────────────────────────────────────────────────────────────────────────────────────────
1. Charging (bias CW at low energy)         Pulsed-bias settings; HV-SEM        Pulse; raise energy
   (Ch. 3.5, 13.1)
2. Mask striation or asymmetric opening     CD-SEM of mask                       Litho / mask-open fix
3. Wall resistivity high (cold, polymer-    Temperature, chemistry               Adjust T / O₂
   rich)
4. Edge tilt (systematic, not random)       Direction of displacement            G.6
5. Particle near the hole                    Defect review                       Clean
```

## G.8 First-Wafer or Post-Clean Shift

```
Likely causes                              Checks                             Actions
─────────────────────────────────────────────────────────────────────────────────────────────
1. Insufficient seasoning (Ch. 9.2)        CD vs wafer index                   Lengthen/strengthen season
2. Residual F or O from clean              OES F/Ar at start                    Longer purge; season
3. Wall temperature low                    Wall heater logs                     Restore
```

## G.9 Particles and Arcs

```
Likely causes                              Checks                             Actions
─────────────────────────────────────────────────────────────────────────────────────────────
1. Polymer flaking (long interval)         Particle monitor; liner inspection   Shorten clean interval
2. Cold-surface condensate (post-PM)       Warm-up record; HF residue           Bake; extend warm-up
3. Arcing at ESC or ring                   Arc counter; reflected-power spikes  Inspect; replace
4. Consumable erosion debris               Parts hours                          Replace
5. Plasma-off transitions                  Recipe                               Plasma-on transitions (Ch. 7.5)
```

## G.10 Chamber-to-Chamber Mismatch

```
Likely causes                              Checks                             Actions
─────────────────────────────────────────────────────────────────────────────────────────────
1. Different ring/electrode hours          Consumable logs                      Offset by hours (Ch. 9.6)
2. RF delivery differences                 Bias V at fixed power; match logs    RF calibration
3. ESC zone / He offsets                   Sensor wafer                         Calibrate
4. MFC calibration differences             Rate-of-rise                         Recalibrate
5. Wall state difference                   OES; clean interval                  Align recipes
```

## G.11 Wafer Temperature or He Leak Alarm

```
Likely causes                              Checks                             Actions
─────────────────────────────────────────────────────────────────────────────────────────────
1. Wafer bow beyond chucking limit          Bow data                             Staged chucking; backside film
2. Particle under the wafer                 Leak vs previous wafer               Clean; ESC inspection
3. ESC seal band wear                       Leak trend over hours                Replace ESC cover
4. Chiller setpoint or flow fault           Chiller logs                         Service
5. Clamp voltage low                        Clamp current                        Adjust; check dielectric
```

---

## G.12 Excursion Workflow

```
1. Hold the affected chamber and lots.
2. Collect FDC traces for the last 5–10 wafers; compare with sister chambers.
3. Check recent events: PM, clean, recipe change, new lot of mask or stack, new gas cylinder.
4. Measure suspect wafers (OCD; then HV-SEM and cross-section as needed).
5. Match the signature to G.1–G.11.
6. Fix the cause; re-qualify (Appendix C.4); release with enhanced sampling.
7. Record root cause and detection lag; update FDC limits and APC models.
```

---

**Appendix G Version:** 1.0  
**Last Updated:** 2026-10-04
