# Index: Book #24 Navigation Guide

## Quick Navigation

**Total Content:** 16 chapters + 7 appendices + glossary  
**Estimated Read Time:** 22–30 hours for the complete book; 6–10 hours for a focused reading path

| Part | Chapters | Theme |
|------|----------|-------|
| I | 1–4 | Fundamentals: architecture, stack, transport physics, chemistry |
| II | 5–9 | Hardware: reactor, ion energy and pulsing, recipes, cryogenic chuck, conditioning |
| III | 10–14 | Phenomena: profile, mask and clogging, landing, twist and tilt, multi-deck |
| IV | 15–16 | Production: metrology and control, integration, yield, cost |

---

## Part I: Fundamentals (Chapters 1–4)

### Chapter 1: [3D NAND Architecture & the Memory Channel Hole](./chapters/01-channel-hole-architecture.md)
**Estimated Time:** 60 min | **Difficulty:** Foundation | **Reading Level:** All roles  
**Focus:** Why is the cell built in a hole, and what must the hole deliver?

**Key Topics:**
- Planar to vertical NAND: the string, the cell in a hole, the replacement gate
- Aspect ratio and how it scales with layer count
- Specification sheet of a memory hole
- Place of the hole etch in the flow
- Holes per die and wafer; yield per defective hole

**Prerequisites:** None (foundational)  
**Cross-References:** Book #23 (Staircase Etch); slit etch companion volume  
**Critical Equations:** AR = H/CD; bits per die; Y = exp(−N·p)  
**Study Questions:** 5 calculations on stack height, hole count, film fit, and yield

---

### Chapter 2: [The ONON Stack — Materials, Deposition & Stress](./chapters/02-onon-stack-materials.md)
**Estimated Time:** 70 min | **Difficulty:** Intermediate | **Reading Level:** Process/Integration roles  
**Focus:** What is the hole cut into, and what does the stack ask of the etch?

**Key Topics:**
- ON and OP stacks; PECVD film properties and hydrogen
- Oxide-to-nitride rate matching and wall modulation
- Stack stress and wafer bow (Stoney)
- Height variation and overetch margin
- Top and bottom layers; particles in the stack

**Prerequisites:** Chapter 1  
**Cross-References:** Silicon Nitride Etch companion volume; Books #6–10  
**Critical Equations:** Wall step = 2d(ρ−1)/(ρ+1); Stoney bow; recess = OE/S_Si  
**Data Tables:** Film properties (Appendix A)  
**Study Questions:** 5 calculations on stress, bow, wall steps, and landing

---

### Chapter 3: [High-Aspect-Ratio Etch Physics — Ion and Neutral Transport](./chapters/03-har-etch-physics.md)
**Estimated Time:** 90 min | **Difficulty:** Advanced | **Reading Level:** Process/Research roles  
**Focus:** What can reach the bottom of an 8 µm hole, and why does the rate fall?

**Key Topics:**
- Ion angular spread, acceptance cone, pass fraction
- Clausing transmission and diffusion with sticking
- ARDE and the depth-versus-time integral
- Differential charging and ion deflection
- Grazing reflection and bowing; product density

**Prerequisites:** Chapters 1–2; plasma physics (Books #1–5)  
**Critical Equations:** σ_θ = √(T_⊥/E); f = 1 − exp(−θ_c²/2σ²); λ = D/√(3s); t = (w̄/R₀)(A + A²/2A₀); δθ = √(eΔV/E)  
**Study Questions:** 5 calculations on pass fraction, decay length, etch time, deflection

---

### Chapter 4: [Etch Chemistries — Fluorocarbons, Hydrofluorocarbons & Cryogenic HF](./chapters/04-etch-chemistries.md)
**Estimated Time:** 75 min | **Difficulty:** Advanced | **Reading Level:** Process/Research roles  
**Focus:** How do chemistry and temperature balance etch, passivation, and the mask?

**Key Topics:**
- F/C ratio, C₄F₆, mask selectivity
- Hydrogen and the nitride; balancing R_N/R_ox
- Delivering passivation deep in the hole
- Cryogenic adsorption and HF transport
- OP-stack halogen chemistry

**Prerequisites:** Chapter 3; fluorocarbon etch (Books #6–10)  
**Critical Equations:** θ = Kp/(1+Kp); K ∝ exp(E_ads/kT); λ_HF vs λ_CFx  
**Study Questions:** 5 calculations on blends, adsorption, decay length, mask

---

## Part II: Hardware Design (Chapters 5–9)

### Chapter 5: [Reactor Architecture for Multi-keV High-Aspect-Ratio Etch](./chapters/05-reactor-architecture.md)
**Estimated Time:** 70 min | **Difficulty:** Intermediate | **Reading Level:** Equipment roles  
**Focus:** Why CCP, why 400 kHz, and what does a kV sheath do?

**Key Topics:** CCP versus ICP; frequency stack; sheath thickness and collisions; edge ring and tilt; 10 kW bias delivery; platform matching  
**Prerequisites:** Chapters 3–4  
**Critical Equations:** s = (√2/3)λ_D(2V/T_e)^(3/4); τ_i = 3s/v_f; tilt displacement = H·θ  
**Study Questions:** 5 calculations on sheath, transit time, ring wear, bias network, throughput

---

### Chapter 6: [Ion Energy, Bias Power & Pulsing](./chapters/06-ion-energy-pulsing.md)
**Estimated Time:** 75 min | **Difficulty:** Advanced | **Reading Level:** Process/Equipment roles  
**Focus:** How much energy does a hole need, and what does pulsing change?

**Key Topics:** E_req ∝ A²; energy trade-offs; IED broadening; pulsed bias model; bias ramp with depth  
**Prerequisites:** Chapters 3, 5  
**Critical Equations:** E_req = 2T_⊥ln[1/(1−f)]A²; E_on = E₀/√d; mask loss ∝ Γ√E  
**Study Questions:** 5 calculations on energy, duty cycle, ramp, heat, drift

---

### Chapter 7: [Gas Delivery, Pressure & Multi-Step Recipes](./chapters/07-gas-pressure-recipes.md)
**Estimated Time:** 70 min | **Difficulty:** Intermediate | **Reading Level:** Process roles  
**Focus:** Why is the recipe divided into stages, and how sensitive is it?

**Key Topics:** Stage logic; reference stepped recipe with stage times; mask loss of the recipe; flow sensitivities; pressure; transitions; center/edge tuning  
**Prerequisites:** Chapters 3, 4, 6  
**Critical Equations:** Stage time from t(A); τ = pV/Q; mask loss ∝ Σ√E·t  
**Study Questions:** 5 calculations on mask loss, stage times, MFC error, residence time

---

### Chapter 8: [Cryogenic Wafer Temperature, ESC Design & Heat Load](./chapters/08-cryogenic-esc-heat.md)
**Estimated Time:** 80 min | **Difficulty:** Advanced | **Reading Level:** Equipment/Process roles  
**Focus:** What temperature is the wafer, and how uniform must it be?

**Key Topics:** Ion heat load; ΔT = q/h; thermal time constant; stage-by-stage wafer temperature; ±0.5 K uniformity; clamp pressure; bow and He leak; cryogenic hardware  
**Prerequisites:** Chapters 2, 4, 6  
**Critical Equations:** q = Γ·e·E; ΔT = q/h; τ = ρct/h; P = ½ε₀ε_r(V/d)²  
**Study Questions:** 5 calculations on heat, time constant, uniformity, clamp, He leak

---

### Chapter 9: [Chamber Conditioning, Wall Memory & Consumables](./chapters/09-chamber-conditioning.md)
**Estimated Time:** 60 min | **Difficulty:** Intermediate | **Reading Level:** Equipment/Process roles  
**Focus:** How does the chamber change over a wafer, a lot, and a ring lifetime?

**Key Topics:** Wall inventory; clean interval; first-wafer effect and seasoning; particles; consumables; PM and qualification; compensating for ageing  
**Prerequisites:** Chapters 4, 5, 7  
**Critical Equations:** ΔCD_k = ΔCD₁ exp[−(k−1)/n_s]; consumable life in wafers  
**Study Questions:** 5 calculations on clean time, first-wafer shift, adders, ring life

---

## Part III: Process Phenomena (Chapters 10–14)

### Chapter 10: [Profile Control — Taper, Bowing & Bottom CD](./chapters/10-profile-control.md)
**Estimated Time:** 80 min | **Difficulty:** Advanced | **Reading Level:** Process roles  
**Focus:** What sets the shape of the hole, and how tight is the bottom?

**Key Topics:** CD(z); etch-front taper; bowing estimate; bottom CD and film fit; ellipticity and roughness; bottom-CD error budget  
**Prerequisites:** Chapters 3, 4, 7  
**Critical Equations:** T = ∫g_f dt; bow ∝ (1−f)·Y_w·t; RSS budget  
**Study Questions:** 5 calculations on taper, bowing, sensitivities, core, roughness

---

### Chapter 11: [Mask Budget — Selectivity, Faceting & Clogging](./chapters/11-mask-budget-clogging.md)
**Estimated Time:** 75 min | **Difficulty:** Advanced | **Reading Level:** Process/Integration roles  
**Focus:** How thick must the mask be, and when does the hole close itself?

**Key Topics:** Mask stack; t_mask equation; instantaneous selectivity; faceting; clogging margin; mask materials; budget with statistics  
**Prerequisites:** Chapters 3, 6, 7, 10  
**Critical Equations:** t_mask ≥ H/S + t_cap + t_margin; S(A) = R(A)/R_mask  
**Study Questions:** 5 calculations on mask thickness, selectivity, facet growth, clogging

---

### Chapter 12: [Etch Rate Decay, Bottom Landing & Overetch](./chapters/12-bottom-landing.md)
**Estimated Time:** 80 min | **Difficulty:** Advanced | **Reading Level:** Process/Integration roles  
**Focus:** How do you make every hole reach the bottom without punching through?

**Key Topics:** dz/dw̄ = 20; overetch from a tail; selectivity and recess; bottom shape; endpoint plus overetch  
**Prerequisites:** Chapters 2, 3, 10  
**Critical Equations:** dz/dw̄ = [A²/2A₀]/(1 + A/A₀); OE = d_sys + z_p σ_r; recess = d₀ + OE/S_Si  
**Study Questions:** 5 calculations on sensitivity, overetch, recess, time

---

### Chapter 13: [Twisting, Tilt & Uniformity Across the Wafer](./chapters/13-twisting-tilt-uniformity.md)
**Estimated Time:** 75 min | **Difficulty:** Advanced | **Reading Level:** Process/Equipment roles  
**Focus:** Why do holes wander, lean, and differ from place to place?

**Key Topics:** Twist as a random walk; edge tilt decay; radial tuning as a 2×2 problem; array-edge loading and dummy holes  
**Prerequisites:** Chapters 3, 5, 8, 12  
**Critical Equations:** σ_x² = 2σ_s²ℓz; θ(r) = θ_e exp[−(R−r)/λ_t]  
**Study Questions:** 5 calculations on twist, tilt, tuning, loading, dummies

---

### Chapter 14: [Advanced Schemes — Multi-Deck, Joints & 400-Layer Stacks](./chapters/14-advanced-multideck.md)
**Estimated Time:** 70 min | **Difficulty:** Advanced | **Reading Level:** Integration/Research roles  
**Focus:** Why stack decks, and how tight is the joint?

**Key Topics:** Single deep hole versus two decks; multi-deck sequence; joint alignment budget; layer pitch and decks; wafer bonding; cyclic etch  
**Prerequisites:** Chapters 3, 6, 11, 12, 13  
**Critical Equations:** t(A); E_req ∝ A²; joint RSS; chamber time ∝ decks  
**Study Questions:** 5 calculations on deep-hole time, joint margin, layers, cyclic etch

---

## Part IV: Production Scale (Chapters 15–16)

### Chapter 15: [Metrology, Inspection & Advanced Process Control](./chapters/15-metrology-apc.md)
**Estimated Time:** 75 min | **Difficulty:** Advanced | **Reading Level:** All roles  
**Focus:** How do you control a process whose failures you cannot see?

**Key Topics:** OES signal budget; metrology precision-to-tolerance; why inspection cannot verify 10⁻¹¹; feed-forward and EWMA; FDC; wafers at risk  
**Prerequisites:** Chapters 9, 12  
**Critical Equations:** P/T; u_{k+1} = u_k − λe/G; variance factor 1 + λ/(2−λ)  
**Study Questions:** 5 calculations on exhaust concentration, P/T, inspection time, EWMA, risk

---

### Chapter 16: [Integration, Yield & Cost of Ownership](./chapters/16-integration-yield-coo.md)
**Estimated Time:** 75 min | **Difficulty:** Advanced | **Reading Level:** All roles  
**Focus:** What does the hole hand to the next steps, and what does it cost?

**Key Topics:** Hole outcomes and downstream failures; web margin; yield budget; cost of ownership; chamber count; cryogenic trade-off; wafer-map signatures  
**Prerequisites:** All earlier chapters  
**Critical Equations:** Web σ and tail; Y = exp(−ΣNp); $/wafer = capex/(yr·wpy) + consumables  
**Study Questions:** 5 calculations on web, depreciation, yield, chambers, signatures

---

## Back Matter

- [Glossary](./GLOSSARY.md)
- [Appendix A: Stack, Mask & Gas Material Properties](./appendices/A-stack-material-properties.md)
- [Appendix B: Chemistry & Reaction Data](./appendices/B-chemistry-reaction-data.md)
- [Appendix C: Standard Procedures](./appendices/C-standard-procedures.md)
- [Appendix D: Process Windows & Lookup Tables](./appendices/D-process-windows.md)
- [Appendix E: Geometry & Error-Budget Calculations](./appendices/E-geometry-error-budget-calculations.md)
- [Appendix F: Endpoint & Metrology Reference](./appendices/F-endpoint-metrology-reference.md)
- [Appendix G: Troubleshooting Guide](./appendices/G-troubleshooting-guide.md)

---

## Reading Paths by Role

**Process Engineers (≈ 10 h):** Ch. 3, 4, 6, 7, 10, 11, 12 → Appendices B, D, G  
**Equipment Engineers (≈ 8 h):** Ch. 5, 6, 8, 9, 15 → Appendices C, F, G  
**Integration Engineers (≈ 7 h):** Ch. 1, 2, 12, 14, 16 → Appendices A, E  
**Device Engineers (≈ 5 h):** Ch. 1, 10, 13, 16  
**Researchers (≈ 8 h):** Ch. 3, 4, 6, 13, 14 → Appendices B, E

---

## Study Problem Themes

- Compute the ion pass fraction and required energy for a hole of given A (Ch. 3, 6)
- Integrate ARDE for etch time and stage times (Ch. 3, 7)
- Estimate wafer temperature and uniformity from ion heat load (Ch. 8)
- Size a mask and its stepped bias budget (Ch. 6, 7, 11)
- Build overetch and recess from a CD distribution (Ch. 12)
- Estimate twist, tilt, and joint margin (Ch. 13, 14)
- Combine killer modes into a die-yield estimate and a cost per wafer (Ch. 16)

---

## How to Use This Index

1. **First time?** Read PREFACE.md, then this INDEX, then Chapter 1.
2. **Focused reading?** Pick your role from the reading paths above.
3. **Reference mode?** Jump to the chapter. Use Appendix G for symptoms and Appendix D for lookup tables.
4. **Deep dive?** Read Chapters 1–16 in order and work the study questions.

---

**Index Version:** 1.0  
**Last Updated:** 2026-10-04  
**Next:** Begin [Chapter 1: 3D NAND Architecture & the Memory Channel Hole](./chapters/01-channel-hole-architecture.md)
