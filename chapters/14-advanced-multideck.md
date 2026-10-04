# Chapter 14: Advanced Schemes — Multi-Deck, Joints & 400-Layer Stacks

## Overview

The physics of Chapters 3 and 6 say that deeper holes get harder faster than linearly: the ion energy needed grows as the square of aspect ratio, etch time grows faster than depth, and the mask has to grow with it. The industry's answer was to stop etching deeper holes and to **stack decks**: etch one 128-pair hole, fill it, deposit a second stack on top, etch a second hole aligned to the first, and open the plug between them. This chapter compares single-deck and multi-deck routes quantitatively, works through the joint alignment budget, shows how layer thinning and decks combine to reach 300–400+ layers, and examines which advanced etch techniques actually reduce time or aspect-ratio pressure.

**Learning Objectives:**
- Compare a 16 µm single-deck hole with two 8 µm decks on time, ion energy, and mask
- Describe the multi-deck process sequence and the role of the sacrificial plug
- Compute the joint alignment budget and the resulting ledge
- Relate layer pitch and deck count to total layers, etch time, and chamber hours per wafer
- Evaluate cyclic (time-multiplexed) etching against ARDE and etch time

---

## 14.1 Why Not One Very Deep Hole?

Consider 256 layers at the reference pitch (60 nm): a single 16 µm stack, with the same mean CD of 100 nm.

```
Metric (reference model)             One 16 µm hole     Two 8 µm decks
────────────────────────────────────────────────────────────────────────────
Final aspect ratio A (w̄ = 100 nm)    160                80 (each)
Bottom rate R(A) = 300/(1 + A/80)    100 nm/min         150 nm/min
Etch time  (w̄/R₀)(A + A²/160)        106.7 min          2 × 40 = 80 min
Ion energy for f_pass = 46.5%        0.625 A² = 16 keV   4 keV
Wafer heat load (fixed flux)         4× per ion          reference
Mask (6:1, 0.36 µm cap + margin)     3.03 µm             1.69 µm per deck
Mask-open aspect ratio               28:1                15:1
Acceptance cone θ_c = 1/A            6.3 mrad            12.5 mrad
```

Two facts decide the matter. First, single-deck etch needs 16 keV, 4× the heat and an unworkable bias system. Second, it takes 107 minutes versus 80 for two decks, so even before the additional deposition and CMP steps, the single hole is slower. The extra steps for two decks (fill, planarize, deposit again, align, second etch, plug removal) cost throughput and money, but they trade a physics problem with no solution for an integration problem with known ones.

---

## 14.2 The Multi-Deck Process Sequence

```
Step                                              Etch-related issues
────────────────────────────────────────────────────────────────────────────────
 1  Deposit deck 1 (128 pairs + joint oxide)       Stack stress and bow
 2  Etch deck 1 holes                               Reference etch (8 µm)
 3  Fill with sacrificial plug (poly-Si or carbon)  Void-free fill at A = 80
 4  Planarize (CMP) to the top of deck 1             Plug exposed, flat top
 5  Deposit deck 2 (128 pairs)                       Alignment marks visible
 6  Etch deck 2 holes                                Land on plug, aligned
 7  Remove plug through the deck-2 hole              Selective to oxide, nitride
 8  Continue (films, channel) through both decks     Continuous channel
```

Steps 3, 4, 5, 7 do not exist in a single-deck flow. The etch in step 6 has the same physics as step 2 but must **land on the plug** rather than polysilicon at the base, with a tolerance on lateral position (Section 14.3). Step 7 removes the plug through a hole 80 nm wide and 8 µm deep, which is itself a high-aspect-ratio selective etch with its own ARDE.

---

## 14.3 The Joint

### 14.3.1 Lateral Alignment Budget

The bottom of a deck-2 hole (CD 80 nm) must sit on the top of the plug in deck 1 (CD about 118 nm after the CMP removed the top 2 nm and its facet). The available lateral room per side is

```
Available margin = (118 − 80) / 2 = 19 nm per side
```

A misalignment δ leaves a **ledge** of width δ at the joint, where the cell films are deposited over a step and the channel can thin or break. The contributors to δ (3σ):

```
Contributor                                              δ (nm, 3σ)
───────────────────────────────────────────────────────────────────────
Lithography overlay, deck 2 to deck-1 top                 5
Deck-2 tilt at the edge (0.1° over 8 µm)                  14
Deck-2 twist (3 nm rms → 9 nm at 3σ)                      9
─────────────────────────────────────────────────────────────────────
RSS = √(5² + 14² + 9²)                                    17.4
Margin 19 − 17.4 = 1.6 nm
```

The result is a **very tight joint**. Tilt dominates, which is why the edge ring and edge tilt (Chapters 5.4 and 13.2) are even more important for multi-deck: tilt in deck 1 does not matter for the joint (the deck-2 lithography aligns to the deck-1 hole tops), but tilt in deck 2 moves its bottom directly off the plug. Reduce the tilt to 0.07° (9.8 nm) and the RSS drops to 14.2 nm, tripling the margin to 4.8 nm.

### 14.3.2 The Joint Layer

The joint includes a thicker oxide layer (typically a few hundred nm), which changes the etch conditions suddenly:

```
Joint layer (illustrative):  oxide 400 nm, between decks
  Etch rate through thick oxide: higher than the pair average (no nitride demand)
  Effect on the profile:  a bulge or step at the joint if the recipe is not adapted
  Fix: a recipe stage for the joint (reduced CH₂F₂, adjusted bias) triggered by depth/time
```

The deck-1 hole top, which becomes the joint, is also where the deck-1 mask facet and top-CD growth (Chapter 11.3) are most severe, so the deck-1 top CD is a **joint parameter** and is controlled to ±1.5 nm.

### 14.3.3 Landing on the Plug

During the deck-2 overetch, the plug (poly-Si or carbon) is the landing layer. The same analysis as Chapter 12 applies: overetch is set by the depth spread, recess by selectivity, and plug height by how much recess can be tolerated. A carbon plug has a much larger selectivity problem in fluorocarbon plasma at high O₂ than polysilicon has, so the plug material is part of the etch design.

---

## 14.4 Layer Pitch, Decks, and Total Layers

Increasing layers without adding decks requires a thinner pair:

```
Pair pitch p (nm)    Layers in an 8.0 µm deck    Gain vs 60 nm
───────────────────────────────────────────────────────────────────
60                   (8000 − 320)/60 = 128        1.00
50                   154                          1.20
45                   171                          1.33
40                   192                          1.50
```

At 40 nm pitch, the oxide and nitride would each be about 20 nm, which strains the cell design (gate length, cell-to-cell coupling, charge-trap uniformity). Layer thinning therefore gives a one-time gain of up to about 1.5×; further growth comes from decks.

```
Layers per wafer stack     Option                          Hole-etch chamber time per wafer
─────────────────────────────────────────────────────────────────────────────────────────────
128                        1 deck, 60 nm pitch            41 min
192                        1 deck, 40 nm pitch            41 min (but harder CDs)
256                        2 decks × 128, 60 nm            82 min + joint steps
384                        2 decks × 192, 40 nm            82 min
384                        3 decks × 128, 60 nm            123 min
```

Etch time per wafer scales with the number of decks, not with the number of layers, while the layers per deck are limited by the depth that the etch can handle (about 8 µm in this book).

---

## 14.5 Wafer Bonding and Array-on-CMOS

In wafer-bonded (array-on-CMOS) flows, the memory array is built on its own wafer and bonded face-to-face to a CMOS periphery wafer. For the hole etch this has three effects:

```
Effect                                  Consequence
───────────────────────────────────────────────────────────────────────────
No CMOS on the array wafer              No transistor damage from charging or ion bombardment;
                                        more freedom for ion energy and pulsing
Different thermal budget                Stack and film deposition can use higher temperature,
                                        affecting stress and bow
Wafer thinning and bond alignment       Hole position relative to bond pads; tilt matters
                                        for contact
Layer stack on a plain substrate        Landing layer design similar to CuA
```

The etch physics does not change, but the freedom from device-damage limits can raise the usable ion energy and charging tolerance.

---

## 14.6 Cyclic and Time-Multiplexed Etching

An alternative to continuous etch is to cycle: a passivation step (polymer deposition) followed by an etch step (ion-driven removal), in analogy to deep silicon etch. The goal is a better wall protection (separate passivation) and the opportunity to optimize each step. The cost is time, because the duty cycle of etching is less than 100%.

```
Model: etching duty cycle D_e = 2/3 (2 s etch, 1 s passivation)
       Cyclic process: R₀' = 300 × 2/3 = 200 nm/min;  A₀' improved +30% to 104

t = (w̄/R₀')(A + A²/(2A₀')) = (100/200)(80 + 6400/208) = 0.5 × (80 + 30.8) = 55.4 min

Continuous reference: 40 min
```

**Cyclic etching is slower unless the etch step itself is faster.** Even if cycling removed ARDE entirely (A₀ → ∞), the time at R₀' = 200 nm/min would be (100/200) × 80 = 40 min, only equal to the continuous process. Any realistic A₀' leaves it slower, so profile and uniformity benefits must be bought with a longer etch (here 38% longer).

In practice, cyclic approaches are used where profile control justifies time, for example in specific stages (the joint, the bottom), not through the entire etch.

---

## 14.7 Other Techniques

```
Technique                      Aim                                  Status / trade-off
──────────────────────────────────────────────────────────────────────────────────────────
Cryogenic HF chemistry         Lower ARDE, higher selectivity       In production; hardware cost
(Chapters 4, 8)
Pulsed bias, tailored          Narrow IED, charge relief            In production; arc/matching
waveforms (Chapter 6)
Doped carbon masks             Thinner mask                         Adopted selectively; contamination
(Chapter 11)
Hole shaping with ALE trim     Smooth walls, reduce bowing          Research/limited; adds steps
Dual-chamber etch              Separate main etch and bottom open   Handling, throughput
In-situ sidewall liner         Repair bowing mid-etch               Research
```

---

## 14.8 Summary & Key Takeaways

1. **Single deep holes lose on physics.** A 16 µm hole needs 16 keV, four times the heat load, and 107 minutes; two 8 µm decks need 4 keV and 80 minutes.

2. **The joint is a tight alignment problem.** With 19 nm of margin per side and 17.4 nm RSS of contributions, edge tilt in deck 2 is the dominant term.

3. **The deck-1 top CD is a joint parameter.** Facet growth and CMP both affect it.

4. **Layer thinning gives about 1.5×.** Beyond that, layers come from decks: two decks of 128 give 256.

5. **Etch time scales with decks.** 41 min per deck, so 123 min for three.

6. **Cyclic etching costs time.** At 2/3 duty, the example takes 55 min versus 40.

---

## Study Questions

1. Compute the etch time, final bottom rate, and required ion energy for a single hole with A = 120 (z = 12 µm, w̄ = 100 nm) using R₀ = 300 nm/min and A₀ = 80. Compare with two 6 µm decks.

2. A joint has plug top CD of 112 nm and deck-2 bottom CD of 76 nm. If overlay is 6 nm (3σ), tilt 10 nm, and twist 8 nm, compute the RSS misalignment and the margin.

3. At a pair pitch of 48 nm and cap + select of 0.36 µm, how many layers fit in an 8.4 µm deck? How many decks are needed for 400 layers?

4. A three-deck process has 41 min of etch per deck, and each deck pass needs 5 min of clean-and-season overhead in the etch chamber. Find the hole-etch chamber time per wafer, and the wafers per hour for a six-chamber platform.

5. A cyclic process has R₀ = 320 nm/min at 70% duty and A₀ = 100. Compute the etch time for A = 80 (w̄ = 100 nm) and compare with the reference 40 min.

---

**Previous Chapter:** [Chapter 13: Twisting, Tilt & Uniformity Across the Wafer](./13-twisting-tilt-uniformity.md)  
**Next Chapter:** [Chapter 15: Metrology, Inspection & Advanced Process Control](./15-metrology-apc.md)

---

**Chapter 14 Development Status:** Complete  
**Version:** 1.0
