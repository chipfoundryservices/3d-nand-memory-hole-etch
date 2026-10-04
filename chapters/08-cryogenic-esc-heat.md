# Chapter 8: Cryogenic Wafer Temperature, ESC Design & Heat Load

## Overview

The ions that etch the bottom of a memory hole deliver their energy to the wafer as heat. At 4 keV mean energy, the reference ion flux puts 3 kW into a 300 mm wafer, through a film of helium a few microns thick, into a chuck held at −70 °C. The wafer temperature that results is not a setpoint. It is the sum of the chuck temperature and a heat-load term that changes with every recipe stage, and it controls adsorption, polymer formation, bowing, and the bottom CD. This chapter derives the wafer temperature from the heat load, shows how it moves across a stepped recipe, sets the uniformity requirement from the etch-rate sensitivity, and describes the chuck and the chilling hardware that make it possible.

**Learning Objectives:**
- Compute wafer heat load from ion flux and energy and the resulting temperature rise above the chuck
- Calculate the wafer's thermal time constant and the temperature trajectory across stages
- Convert an etch-rate temperature sensitivity into a wafer-temperature uniformity requirement
- Size the electrostatic clamp against backside helium pressure
- Describe the effect of wafer bow on helium leak and edge temperature
- List the hardware and safety requirements of a −70 °C chuck

---

## 8.1 Where the Heat Comes From

```
Source                      Reference power         Share
──────────────────────────────────────────────────────────────────────
Ion bombardment             3.0 kW  (4.2 W/cm²)     ~ 90%
Neutral recombination and   0.2–0.3 kW              ~ 7%
  surface reactions
Plasma radiation, electron   0.1 kW                  ~ 3%
  heating, exothermic etch
Total on the wafer          ≈ 3.3 kW (4.7 W/cm²)
```

At fixed ion flux Γ_i = 6.6×10¹⁵ cm⁻² s⁻¹, the ion heat per unit area is Γ_i · e · E:

```
Stage          Mean ion energy    Ion heat       Wafer ion power
                (keV)              (W/cm²)        (707 cm²)
────────────────────────────────────────────────────────────────
Stage 1         1.0                1.06           0.75 kW
Stage 2         1.5                1.59           1.12 kW
Stage 3         3.0                3.17           2.24 kW
Stage 4         4.5                4.76           3.36 kW
Overetch        4.0                4.23           2.99 kW
```

(Ion flux is held constant for this illustration. In practice the flux changes with source power and pressure.)

A recipe that steps up its bias with depth therefore also steps up its heat load by more than a factor of four over the etch.

---

## 8.2 From Heat Load to Wafer Temperature

### 8.2.1 Heat Transfer Through the Backside Gas

Helium fills the microscopic gap between the wafer and the chuck. At low pressure the heat transfer is in the free-molecular regime and rises with pressure, then saturates as the gap becomes a continuum.

```
He backside pressure (Torr)      h (W/m² K)  (illustrative)
────────────────────────────────────────────────────────────
  2                                  300
  5                                  700
 10                                 1100
 20                                 1500
 40                                 1800
```

The steady-state temperature rise of the wafer above the chuck surface is

```
ΔT = q / h
     q = heat flux (W/m²)

Reference: q = 4.24 W/cm² = 42,400 W/m², h = 1500 W/m²K  →  ΔT = 28 K
  h = 1000: ΔT = 42 K    h = 700: ΔT = 61 K    h = 2000: ΔT = 21 K
```

### 8.2.2 Thermal Time Constant

The wafer's heat capacity per unit area, ρ c t, divided by h gives the time constant:

```
ρ c t = 2329 kg/m³ × 700 J/kg·K × 775×10⁻⁶ m = 1263 J/m²K
τ = ρ c t / h = 1263 / 1500 = 0.84 s
```

The wafer reaches its new temperature within about 2–3 s of a heat-load change (about 3τ). The chuck's thermal mass, in contrast, responds over minutes, so **the chuck holds a constant setpoint, and the wafer temperature follows the heat load**.

### 8.2.3 Wafer Temperature Across the Stepped Recipe

```
ESC surface setpoint: −70 °C,  h = 1500 W/m²K
Stage          q (W/m²)     ΔT (K)    Wafer T (°C)
─────────────────────────────────────────────────────
Stage 1        10,600        7.1       −62.9
Stage 2        15,900       10.6       −59.4
Stage 3        31,700       21.1       −48.9
Stage 4        47,600       31.7       −38.3
Overetch       42,300       28.2       −41.8
```

**The wafer temperature moves by about 25 K across the recipe.** That is not a flaw, but it is a feature that has to be designed with. Using the adsorption model of Chapter 4.5.2 (E_ads = 0.30 eV), the adsorption constant K changes with T as dK/K = E_ads/(k_B T²) dT:

```
At 225 K (−48 °C):  dK/K = 3481 / 225² = 6.9% per K
K(−63 °C = 210 K) / K(−38 °C = 235 K) = exp[3481 × (1/210 − 1/235)] = exp(1.76) = 5.8
```

The wafer is about six times more "adsorbing" at the start than at the end. In a real recipe, the stage-by-stage gas and bias settings take this into account: the cooler top-half wafer gets more polymer and adsorbate, which protects the mask and wall where the hole is shallow, and the warmer bottom wafer sheds polymer, which helps keep the bottom open. The temperature trajectory is **part of the recipe**.

---

## 8.3 Temperature Uniformity Requirements

### 8.3.1 Sensitivity

```
Parameter (near −50 °C, illustrative)            Sensitivity
────────────────────────────────────────────────────────────────
Etch rate (bottom)                               +2% / K
Bottom CD                                        +0.6 nm / K
Bowing amplitude                                 +0.3 nm / K
Mask loss rate                                   −1% / K  (warmer: less polymer)
```

(The etch-rate sensitivity is lower than the raw adsorption sensitivity of 6.9%/K because the coverage saturates at higher θ and a share of the supply is ion-driven.)

### 8.3.2 Required Uniformity

```
Depth uniformity target: 1.0% (3σ) from temperature alone
  → T uniformity = 1.0% / 2% per K = ±0.5 K  (3σ)

Bottom CD: ±1.0 nm (3σ) from temperature alone
  → ±1.0 / 0.6 = ±1.7 K — less strict than depth

Binding requirement: ±0.5 K across the wafer, within the 40-minute etch
```

Delivering ±0.5 K across a wafer at 4.2 W/cm² of heat flux needs both a uniform heat transfer coefficient (±5%: since ΔT = 28 K, 5% is 1.4 K!) and a radially tunable chuck. The heat flux itself also varies across the wafer (edge ion flux, edge ring), so uniformity is a **tuning** problem, with zones.

```
Example: ΔT = 28 K at the center. If h varies ±3% across a zone: ±0.84 K.
  Required: ±0.5 K → must compensate by zone setpoints, or h uniformity of ±1.8%.
```

### 8.3.3 Zone Design

```
Zone                      Function
──────────────────────────────────────────────────────────────────────────────
Center / mid / edge       Independent chuck coolant or heater trim, 3–5 zones
  chuck zones             (setpoint offsets of ±3–5 K)
Backside He zones         Center and edge He pressure set independently to tune h
Edge ring cooling         Ring temperature affects edge etch and polymer
```

A zone offset of 1 K moves the local depth by 2%, so the offset range of ±3–5 K is enough to correct the largest across-wafer variations, but the calibrations (sensor wafers, patterned monitors) must resolve 0.3 K.

---

## 8.4 Clamping the Wafer

### 8.4.1 The Clamp Pressure

An ESC holds the wafer with an electrostatic pressure across a dielectric of thickness d and relative permittivity ε_r:

```
P_clamp = ½ ε₀ ε_r (V / d)²

Reference: ε_r = 9 (Al₂O₃ or AlN-type ceramic), d = 0.3 mm
  V = 2 kV:   E = 6.7×10⁶ V/m,  P = ½ × 8.85×10⁻¹² × 9 × (6.7×10⁶)² = 1.8 kPa = 13 Torr
  V = 3 kV:   P = 4.0 kPa = 30 Torr
  V = 4 kV:   P = 7.1 kPa = 53 Torr
```

The clamp pressure must exceed the backside He pressure with margin:

```
P_He = 20 Torr (2.7 kPa) → required P_clamp ≥ 1.3 × 2.7 = 3.5 kPa → V ≥ 2.8 kV
```

Many production chucks use the **Johnsen–Rahbek effect**, in which a weakly conducting dielectric produces a much smaller effective gap and a stronger clamp for the same voltage. At low temperature the resistivity of the dielectric rises, which changes the clamp behavior with temperature and requires verification at the operating setpoint.

### 8.4.2 RF and DC on the Same Electrode

The ESC clamp voltage (a few kV DC) must sit on top of RF of about 6 kV peak (Chapter 5.5.1). The DC supply is filtered from the RF, and the wafer self-bias is measured to keep the clamp balanced. The He-filled chuck near Paschen's minimum is the main arc risk (Chapter 5.5.2).

---

## 8.5 Bowed Wafers

```
Bow (µm)    Gap at edge    He leak (illustrative)    Edge temperature effect
───────────────────────────────────────────────────────────────────────────
 50         small          0.3 sccm                   < 0.5 K
150         moderate       1.5 sccm                   +1–3 K (hot edge)
250         large          6 sccm                     +5–10 K (needs staged chucking)
```

The 150 µm bow of the reference stack (Chapter 2.3.2) is within the range of a standard ESC, but the edge is the weak spot. If the edge seal leaks, the edge-zone He pressure drops, h falls at the edge, and the edge wafer temperature rises. For a ΔT of 28 K, a 10% fall in h at the edge gives +3 K there, which is six times the ±0.5 K budget.

**Staged chucking** applies clamp voltage in steps while the wafer flattens, brings the He up in two stages, and monitors the leak. The results of the leak monitor are logged per wafer and used as a fault signal and as a feed-forward input to the edge zone offset.

---

## 8.6 Cryogenic Hardware and Safety

```
Item                          Requirement
──────────────────────────────────────────────────────────────────────────────
Chiller                       ≥ 4 kW at the chuck setpoint, −70 to −90 °C coolant;
                              margin above the 3.3 kW heat load
Coolant                       Low-viscosity fluid with low freezing point,
                              compatible with chuck seals
Chuck insulation              Vacuum insulation; dry purge in the pedestal
                              housing to avoid condensation
Wafer transfer                Pre-cooling (load-lock cold), and warm-up before
                              exit to avoid condensation on wafers
Chamber walls                 Held warm (60–100 °C) to avoid cold-spot deposits
                              and HF condensation
HF handling                   Gas delivery and abatement designed for HF;
                              detectors; cold-trap management
Warm-up after maintenance     Controlled ramp; do not vent cold hardware
                              to humid air
```

The wall must be warm while the chuck is cold. A temperature step of 130–170 K across the chamber means a thermal design problem of its own in the lower electrode region, including thermal expansion, seal choice, and ring temperature.

---

## 8.7 Temperature as a Profile Knob

```
Lever                          Effect (illustrative)
──────────────────────────────────────────────────────────────────────────────
ESC setpoint −10 K colder      Mask selectivity +10–15%, A₀ +10–15%, bottom CD +4 nm
                               (more passivation); risk of clog
ESC setpoint +10 K warmer      Faster neutral etch on the wall, bowing +3 nm
He pressure 20 → 10 Torr       h 1500 → 1100, wafer +10 K at 4.2 W/cm²
Edge-zone offset +2 K          Edge depth +4%, edge CD +1.2 nm
Backside film (stress comp.)   Lowers bow → better edge contact
```

A cooler wafer is not simply better. More passivation and adsorbate protect the wall and mask but risk closing the bottom, so the setpoint is chosen with the gas and bias stages (Chapter 7).

---

## 8.8 Summary & Key Takeaways

1. **Heat load follows ion energy.** At fixed flux, each kV of ion energy puts about 1 W/cm² (0.7 kW) into the wafer.

2. **Wafer temperature = chuck + q/h.** At 4.2 W/cm² and h = 1500 W/m²K, the wafer sits 28 K above the chuck.

3. **The wafer follows heat load within seconds.** The reference recipe moves wafer temperature by about 25 K, from −63 °C to −38 °C.

4. **Depth uniformity sets ±0.5 K.** At 2%/K, 1% depth spread equals 0.5 K, which is tighter than the 5% variation in h that would arise without zoning.

5. **Clamp must beat He pressure.** A 20 Torr backside needs 3.5 kPa, about 2.8 kV on a thin dielectric.

6. **Bow sets the edge.** A 10% loss of h at the edge raises its temperature by about 3 K.

---

## Study Questions

1. Compute the wafer heat load and ΔT for q from 3.8 keV ions at Γ_i = 7.5×10¹⁵ cm⁻² s⁻¹ with h = 1200 W/m²K. If the ESC is at −75 °C, find the wafer temperature.

2. A 300 mm silicon wafer is 775 µm thick (ρ = 2329 kg/m³, c = 700 J/kg·K). Compute τ for h = 900 W/m²K. How long after a power change does the wafer reach 95% of its new temperature?

3. The etch rate changes 2.2%/K and the depth spec is 0.9% (3σ). What temperature uniformity is required? If h uniformity is ±4% with ΔT = 30 K, what residual uniformity would remain without zone correction?

4. For a ceramic with ε_r = 8 and d = 0.25 mm at 20 Torr He, find the minimum clamp voltage for 1.5× margin.

5. The edge He pressure falls from 20 to 12 Torr. Using the table in Section 8.2.1 (linear interpolation), estimate h and the new edge ΔT for q = 4.2 W/cm². How much does the edge etch rate change at 2%/K?

---

**Previous Chapter:** [Chapter 7: Gas Delivery, Pressure & Multi-Step Recipes](./07-gas-pressure-recipes.md)  
**Next Chapter:** [Chapter 9: Chamber Conditioning, Wall Memory & Consumables](./09-chamber-conditioning.md)

---

**Chapter 8 Development Status:** Complete  
**Version:** 1.0
