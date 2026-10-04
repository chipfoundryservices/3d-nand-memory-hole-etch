# Appendix C: Standard Procedures

Generic procedures for operating and qualifying a memory-hole etch chamber. They illustrate the structure of a qualified procedure. Limits, intervals, and sequences must be replaced with the tool manufacturer's and the fab's own, and any step involving HF, cryogenic hardware, or high-voltage RF must follow the site's safety program.

---

## C.1 Daily Chamber Check

```
Step   Check                                    Method                              Limit (illustrative)
──────────────────────────────────────────────────────────────────────────────────────────────────────────
1      Chamber leak-up rate                     Rate-of-rise, pumped & isolated     < 1 mTorr/min
2      MFC rate-of-rise (O₂, C₄F₆, CH₂F₂, Ar)  Verify against reference volume      ± 1% of setpoint
3      He backside leak (bare chuck/cover)      Pressure hold at 20 Torr            < 0.1 sccm
4      ESC zone temperatures                    Sensor reading vs setpoint          ± 0.3 K
5      RF: forward/reflected, match positions   Dummy-load or plasma test           Within ± 2% of baseline
6      Bias voltage at fixed power              Test plasma                         ± 2% of baseline
7      Arc detector function                    Simulated trigger                   Trips < 10 µs
8      Particle monitor wafer                   Blanket wafer through recipe        ≤ 5 adders ≥ 80 nm
9      OES signal check                         Ar 750 nm intensity vs baseline     ± 10%
```

A failure at any step puts the chamber on hold until corrected and re-verified.

---

## C.2 Waferless Clean and Season

```
Step   Action                                          Notes
────────────────────────────────────────────────────────────────────────────────
1      Clean: O₂ plasma, no wafer, cover wafer on ESC  Remove polymer (≈ 1.5 min per wafer
                                                       inventory)
2      Clean: NF₃/O₂ plasma                            Remove SiOxFy and fluorinated films
3      Purge and pump                                  Remove residual F, O
4      Season: main-etch chemistry, no bias, 30–60 s    Lay ≈ 100 nm of polymer on walls
5      Verify (periodic): blanket rate and CD monitor   First-wafer shift ≤ ±1.0 nm
```

The clean-and-season sequence is qualified as a unit. If the clean time or season time changes, the first-wafer effect (Chapter 9.2) must be re-measured.

---

## C.3 Preventive Maintenance

```
Phase                  Activity
───────────────────────────────────────────────────────────────────────────────────────
Before opening         Run a final clean. Warm the ESC to ambient with dry N₂ purge.
                       Do not vent cold hardware to humid air.
Opening                Lockout/tagout of RF, gas, chiller. HF gas lines purged and verified.
Parts                  Replace edge ring, upper electrode, liner, cover ring, seals as due
                       (Chapter 9.4). Measure ring height and electrode thickness.
Wet clean              Wipe and bake. Verify surface finish on ESC seal band.
Close-up               Torque and alignment checks; He leak check; coolant leak check.
Pump-down and bake     Bake chamber (walls warm); pump to base pressure.
Cool-down              Cool ESC to setpoint with dry purge; verify zone temperatures.
Season                 Extended season (several cycles, longer time).
Qualification          C.4
```

---

## C.4 Qualification After PM

```
Step   Test                                    Pass criterion (illustrative)
──────────────────────────────────────────────────────────────────────────────────────────
1      Leak-up and He leak                      Daily-check limits
2      RF and match characterization            Within baseline ± 2%
3      Blanket oxide and nitride etch rate       R_N / R_ox = 1.00 ± 0.05
4      Blanket a-C rate                          Within ± 3% of baseline
5      Blanket poly rate and S_Si                S_Si ≥ 5 at overetch conditions
6      Particle test                             ≤ 5 adders ≥ 80 nm, 2 consecutive wafers
7      Patterned monitor wafers (n = 4–10)       Top CD ± 1.5 nm; bottom CD ± 2.5 nm;
                                                 depth ± 0.8%; tilt ± 0.05° (edge)
8      Chamber matching check                    Against fleet baseline
9      Release                                   All criteria met on consecutive wafers
```

---

## C.5 Sensor-Wafer and Zone Calibration

```
Step   Action
──────────────────────────────────────────────────────────────────────────────────
1      Load instrumented (thermocouple or fiber) sensor wafer on the ESC.
2      Run a heat-load recipe at the stage-3 bias level, 5 minutes after plasma ignite.
3      Record wafer temperature at center, mid, and edge; compare with zone setpoints.
4      Adjust zone offsets until across-wafer range ≤ ± 0.3 K.
5      Repeat at stage-1 and stage-4 heat loads to confirm the offsets hold across the recipe.
6      Record offsets, He pressures, and the chiller setpoint in the chamber record.
```

---

## C.6 HF and Cryogenic Safety (Outline)

```
Hazard                              Controls (outline; follow the site program)
────────────────────────────────────────────────────────────────────────────────
HF gas and residues                 Dedicated gas panel, leak detection, scrubber, PPE,
                                    trained personnel, calcium gluconate on site
Cold surfaces and cryogen           Insulation, burn-protection PPE, no bare-hand contact
Condensation / ice on cold parts    Dry purge, controlled warm-up before venting
High-voltage RF (kV, tens of A)     Interlocks, lockout/tagout, discharge verification
```

---

## C.7 Excursion Response

```
1  Stop the chamber (hold lots in the affected chamber).
2  Pull the last 5 wafers' FDC traces: bias voltage, He leak, ESC temperature, OES, arcs.
3  Compare with baseline and with sister chambers.
4  Measure suspect wafers (OCD first, then HV-SEM / cross-section if needed).
5  Identify the signature (Appendix G).
6  Corrective action; re-qualify per C.4 at the level appropriate to the repair.
7  Release with enhanced sampling for the next lots.
8  Record the root cause, detection lag, and update FDC limits and APC models.
```

---

**Appendix C Version:** 1.0  
**Last Updated:** 2026-10-04
