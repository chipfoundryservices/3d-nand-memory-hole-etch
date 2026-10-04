# Chapter 9: Chamber Conditioning, Wall Memory & Consumables

## Overview

A hole etch runs for 41 minutes per wafer in a fluorocarbon plasma that deposits polymer on every surface it touches. By the end of a wafer, the walls hold a layer that was not there at the start. That layer decides how much polymer precursor reaches the wafer, how much fluorine the walls release, and how much particulate the chamber will drop on the next wafer. The chamber also contains parts that are worn away by ion bombardment: the edge ring, the upper electrode, the liner. Each wears in a way that changes the plasma, and each has a life measured in RF hours.

This chapter treats the chamber as a slowly changing component of the process. It models wall inventory, explains the first-wafer effect and how seasoning removes it, sets particle limits, and lists the consumables and how their wear shows up in the hole.

**Learning Objectives:**
- Model polymer inventory on chamber walls and choose a clean interval
- Describe the first-wafer effect and design a seasoning step
- Relate wall film to particle adders and to blind-hole density
- Identify the main consumables, wear mechanisms, and effects on the hole
- Plan preventive-maintenance recovery and qualification
- Use RF hours and fault-detection data to compensate for chamber ageing

---

## 9.1 Wall Inventory

### 9.1.1 Deposition per Wafer

The hole etch consumes a few percent of its fluorocarbon feed into polymer on the chamber surfaces. For the reference chamber:

```
Polymer deposition on warm (80 °C) liner:   ~ 3.7 nm/min (illustrative)
Per 41 min wafer:                            0.15 µm
Cold surfaces (ESC edge, ring, lift pins)    several times higher (cryogenic, HF-bearing)
```

### 9.1.2 Accumulation and Cleaning

```
Without cleaning:   inventory after N wafers = N × 0.15 µm
Flake limit (illustrative): 5 µm  →  33 wafers maximum

Waferless autoclean (WAC): O₂ + fluorine (NF₃ or SF₆) plasma
  Removal rate ≈ 100 nm/min on the liner
  To clear one wafer's inventory: 0.15 µm / 0.1 µm/min = 1.5 min
  Overhead: 1.5 / 41 = 3.7% of the wafer time

Clean every k wafers:   overhead = 1.5 k min per k wafers
  k = 1: 1.5 min per wafer;   k = 5: 7.5 min per 5 wafers  (same per wafer)
  but the inventory at the end of the interval is k × 0.15 µm = 0.15 – 0.75 µm
```

A clean interval is a trade between a first-wafer effect and flake risk (Sections 9.2 and 9.3). Because the overhead per wafer is about the same at any interval, the choice is made by the process.

### 9.1.3 What the Wall Does to the Plasma

```
Wall state              Effect on the plasma and the hole
─────────────────────────────────────────────────────────────────────────────────
Clean (just after WAC)  F-rich: walls release fluorine, scavenge CFx radicals
                        → less polymer to the wafer → thinner passivation,
                        mask loss up, CD down at the top
Polymer-coated          Steady-state: polymer deposition and release in balance
                        → nominal polymer supply
Thick or flaking        Particles; changes in wall conductance for RF currents;
                        unstable plasma impedance
```

---

## 9.2 First-Wafer Effect and Seasoning

A chamber coming out of WAC or maintenance has no polymer, and for the first wafer or two, the walls absorb precursors that would otherwise reach the wafer. The result is a drift in CD, mask loss, and profile that decays as the walls re-coat.

```
Model:  ΔCD_k = ΔCD₁ · exp[−(k − 1) / n_s],   k = wafer index after clean

Reference: bottom CD shift ΔCD₁ = −3.0 nm, n_s = 2 wafers
  k = 1:  −3.0 nm
  k = 2:  −1.8 nm
  k = 3:  −1.1 nm
  k = 4:  −0.7 nm
  k = 5:  −0.4 nm      (spec ±1.0 nm: reached at k = 4)
```

**Seasoning** deposits a polymer film on the walls before product wafers run. A waferless step of 30–60 s in the main-etch chemistry with no bias (or low bias on a bare chuck cover) lays down about 100 nm.

```
Seasoning effect on the model:  n_s remains, ΔCD₁ falls in proportion to the
  inventory deposited relative to the steady-state film

  Season to 70% of steady-state film → ΔCD₁ = −3.0 × 0.3 = −0.9 nm → inside spec for all k
  Cost: 45 s per clean = 1.8% of wafer time
```

The seasoning step is part of the recipe, and the **clean + season** sequence is qualified together. Skipping seasoning to save 45 seconds shifts the first wafer by 3 nm, which is three-quarters of the bottom-CD budget.

---

## 9.3 Particles

### 9.3.1 Sources

```
Source                      Mechanism                        Mitigation
─────────────────────────────────────────────────────────────────────────────────
Polymer flakes              Stress cracking of thick film    Clean interval; warm liner
Cold-surface condensate     HF, water, polymer on cold       Warm-up procedure; dry purge
                            parts
Arc debris                  Arc at ESC or ring               Arc detection, ring condition
Consumable erosion          Ceramic or silicon particles     Coating quality, replacement
Plasma-off drop             Particles trapped at sheath      Plasma-on transitions
                            fall at RF off                   (Chapter 7.5)
```

### 9.3.2 Particle Budget

A particle on the wafer surface during etch masks the etch under it and can produce a blind hole (like a particle in the stack, Chapter 2.6). For the reference:

```
Wafer area ≈ 700 cm²;  each ≥ 80 nm adder blocks ≈ 2 holes
Blind-hole budget: p ≤ 1×10⁻¹¹ per hole → ≤ 10 blind holes per wafer (of 10¹²)
Adders allowed from the etch: ≤ 5 per wafer (≥ 80 nm)  = 0.007 /cm² per pass
```

Etch-chamber adder counts of a few per wafer are routine, and particle monitors (blanket wafers after etch) track them. A rise of a few adders above baseline is an early signal of flaking or arcing.

---

## 9.4 Consumables

```
Part                  Wear mechanism                Life (RF h,       Effect when worn
                                                    illustrative)
────────────────────────────────────────────────────────────────────────────────────────────
Edge / focus ring     Ion erosion at the ring        500               Edge tilt grows 0.4°/mm;
                      (Chapter 5.4.2)                                  edge CD and depth shift
Upper electrode       Ion/RF erosion of Si or SiC    1500              F scavenging falls;
(showerhead)          plate, hole enlargement                          gas distribution shifts
Chamber liner         Coating, ion damage            2000 (clean       Impedance and wall
                                                     cycles)           film changes
ESC ceramic surface   Wear of seal band, He          3000+             Leak, clamp change,
                      channel plugs                                    edge temperature
Cover ring, pins      Erosion and particles          1000              Edge field, particles
Window / sensor ports Coating                        500               OES signal loss
                                                                       (Chapter 15)
```

**Edge ring** replaced every 500 RF hours at 0.69 h per wafer is about 730 wafers per ring, so a six-chamber platform consumes roughly one ring per 730 wafers processed (six rings per 4,400 wafers). At that frequency, ring cost and downtime matter, and adjustable rings that follow the wear extend the life.

### 9.4.1 Upper Electrode Wear and Chemistry

A silicon upper electrode reacts with fluorine to make SiF₄ and so acts as a **fluorine scavenger**. As it wears thin, or as polymer covers it, the scavenging changes and so does the effective F/C ratio of the plasma.

```
Illustrative: R_N / R_ox drifts by +0.008 per 100 RF hours as the electrode
  surface roughens, and the oxide mask erosion rate by +0.5% per 100 h

  After 1000 RF hours:  R_N/R_ox: 1.00 → 1.08 → wall step per side (d = 5 nm): 0.38 nm
                        mask loss +5%
```

Drifts of this kind are slow enough to track by RF-hour-indexed recipe offsets (Section 9.6), not fast enough to appear as a wafer-to-wafer jump.

---

## 9.5 Preventive Maintenance and Qualification

```
Step                                   Notes
──────────────────────────────────────────────────────────────────────────────────
1  Cool-down and vent to dry N₂        Avoid humid air on cold parts
2  Part replacement and wet clean      Ring, electrode, liner, seals as due
3  Leak check, He leak, RF check       Bias path, match, arc detector
4  Pump-down and bake                  Remove water; chuck warmed for HF-residue
5  Seasoning (extended)                Multiple conditioning runs, longer than daily
6  Qualification wafers                Blanket rates, selectivity, particles
7  Patterned monitor wafers            Top CD, bottom CD, depth, tilt, twist
8  Release                              All within matching targets (Chapter 5.6.2)
```

Recovery after PM is measured in qualification wafers (typically 4–10), which cost 3–7 hours of chamber time. At 41 min per wafer, each qualification wafer costs 0.7 h, so the **hole etch has some of the longest recovery times of any process module**.

---

## 9.6 Compensating for Ageing

```
Monitor                         Signal                         Action
─────────────────────────────────────────────────────────────────────────────────
Bias voltage at constant power  Ring wear, wall film           Alarm; power offset
Ring-hours                      Tilt trend                     Edge-zone offset; replacement
Electrode-hours                 Rate and selectivity drift     Recipe offset by hours
OES (CN, CO, F, Ar)             Wall state, product            Trend; stage-end endpoint
He leak rate                    ESC seal, bow                  Edge zone offset
Particle monitor                Flaking, arcs                  Clean interval; arc check
```

Hole-etch chambers are among the most heavily instrumented, because a failure is expensive: a lost wafer represents about 41 minutes of the most expensive chamber time in the array flow, plus the stack that was deposited before it.

---

## 9.7 Summary & Key Takeaways

1. **The walls remember.** A wafer deposits about 0.15 µm of polymer, which sets the polymer supply to the next wafer.

2. **Clean and season go together.** WAC takes 1.5 min and seasoning 45 s per wafer; skipping the seasoning shifts the first wafer's bottom CD by about 3 nm.

3. **The first-wafer effect decays in 2–3 wafers.** Seasoning cuts it by a factor of 2.5 or more.

4. **Particles become blind holes.** The adder budget is about 5 per wafer at ≥ 80 nm.

5. **Consumables set drift.** The edge ring (500 RF h) sets tilt; the upper electrode (1500 RF h) sets chemistry drift.

6. **Recovery is slow.** Qualification takes 4–10 wafers at 41 min each.

---

## Study Questions

1. A chamber deposits 0.2 µm per wafer and flakes at 4 µm. With WAC removing 80 nm/min, find the clean time per wafer, and the maximum wafers between cleans.

2. A first-wafer bottom CD shift is −3.5 nm with n_s = 2.5 wafers. At which wafer index does it fall within ±1.0 nm? If seasoning reduces ΔCD₁ by 65%, what is the new index?

3. A hole etch has an adder budget of 4 per wafer for ≥ 80 nm, each blocking 2 holes. Compute the blind-hole probability per hole and the die yield loss for 1.33×10⁹ holes per die and 750 dies, if the adders are spread randomly.

4. An edge ring wears 0.06 mm per 100 RF hours with a tilt sensitivity of 0.4°/mm. How many RF hours until the tilt reaches 0.1°, and how many 41-min wafers is that?

5. Compute the R_N/R_ox ratio after 1500 RF hours for the electrode drift in Section 9.4.1. What CH₂F₂ flow change (0.045 per sccm) compensates?

---

**Previous Chapter:** [Chapter 8: Cryogenic Wafer Temperature, ESC Design & Heat Load](./08-cryogenic-esc-heat.md)  
**Next Chapter:** [Chapter 10: Profile Control — Taper, Bowing & Bottom CD](./10-profile-control.md)

---

**Chapter 9 Development Status:** Complete  
**Version:** 1.0
