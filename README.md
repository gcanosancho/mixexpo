# MixExpo · Chemical mixture exposure calculator

**MixExpo v1 (25.11.2026)** — a pedagogical web tool to explore dietary exposure to chemical mixtures and the associated risks, based on the French Second Total Diet Study (TDS2 / EAT2, ANSES).

👉 **Open the calculator:** https://gcanosancho.github.io/mixexpo/

> ⚠️ **For pedagogical purposes only.** MixExpo illustrates the reasoning of dietary exposure and mixture risk assessment. It is not a diagnostic or regulatory tool, and a single 24-hour recall does not represent chronic exposure.

Author: **German Cano-Sancho** (INRAE, LABERCA, Nantes, France)

---

## What students do

The tool follows six steps (interface in Spanish, French and English):

1. **My 24 h diet:** a 24-hour dietary recall, meal by meal (breakfast, lunch, snack, dinner, other), with household portions or grams. A live panel shows exposure and risk while the diet is being entered.
2. **Exposure:** daily exposure per substance (ng/kg bw/day) with lower/middle/upper-bound (LB/MB/UB) uncertainty, shown next to the risk quotient to contrast *how much* with *how risky*.
3. **Contributing foods:** a food × substance map of percentage contributions, distinguishing "not analysed" from "zero".
4. **Risk & mixtures:** a tiered mixture assessment following the EFSA (2019) guidance on combined exposure:
   - Tier 0: hazard index (HI) of all threshold substances;
   - Tier 1: HI by common toxicological effect (neurotoxicity, nephrotoxicity, reproduction/development, immunotoxicity, other);
   - genotoxic carcinogens combined as a total margin of exposure (MOE<sub>T</sub>).
5. **Compare scenarios:** save up to four diets and compare exposure and risk profiles (e.g. canned vs fresh foods, fish choices, ultra-processed foods).
6. **Methods & data:** data sources, assumptions and editable toxicological reference values.

Four example diets are included (original spreadsheet example, Mediterranean with fish, fast food, vegetarian). Results can be exported as CSV, and diets/scenarios saved and shared as JSON.

### Suggested classroom exercises

- Compare the ranking of substances by **exposure** versus by **risk**: why is the most abundant chemical not the most concerning one?
- Switch between **LB and UB**: which conclusions depend on analytical sensitivity rather than on measured contamination?
- Replace canned foods with fresh ones and follow the change in **bisphenol A** exposure.
- Change a reference value (e.g. BPA: EFSA 0.2 ng/kg bw/day vs BfR 200 ng/kg bw/day) and discuss how hazard characterisation drives the conclusion.
- Explore how the **hazard index** and the **Tier-1 groups** change the interpretation of the same diet.

---

## Data sources

| Data | Source | Notes |
|---|---|---|
| Food concentrations (acrylamide, PAHs, dioxins & PCBs, PFAS, PBDEs, mycotoxins, pesticides) | ANSES, Second French Total Diet Study (EAT2), regional data, 2007–2009. [data.gouv.fr](https://www.data.gouv.fr/datasets/donnees-regionales-eat2-etude-de-l-alimentation-totale/) | Composite samples of 15 sub-samples, foods analysed "as consumed" |
| Bisphenol A | ANSES / LABERCA campaign on EAT2 samples, plus bottled waters (LHN). [data.gouv.fr](https://www.data.gouv.fr/datasets/bisphenol-a) | Samples collected before the French ban of BPA in food-contact materials (2015) |
| Cd, Hg, Pb | EAT2 trace-element values from the original teaching spreadsheet (45 foods) | Not available for the full catalogue |

The concentration data belong to ANSES and are reproduced with attribution for teaching purposes. They are **not** covered by the MIT licence of this repository.

---

## Methods (summary)

- **Exposure:** `E = Σ (food intake [g/day] × mean concentration [ng/g]) / body weight [kg]`, using the mean of all TDS2 samples for each food.
- **Left-censored data:** non-detects (< LOD) and non-quantified results (< LOQ) are replaced by 0 (LB), half the limit (MB) or the limit (UB). "Not analysed" results are excluded, not set to zero.
- **Aggregated substances:** PAH4 = BaP + BaA + BbF + CHR; PFAS4 = PFOS + PFOA + PFNA + PFHxS; PBDEs = Σ BDE-28, 47, 99, 100, 153, 154, 183; dioxins and dl-PCBs as WHO-2005 TEQ (17 PCDD/Fs + 12 dl-PCBs). Congener sums are computed per sample before averaging.
- **Risk characterisation:** risk quotient RQ = exposure / health-based guidance value; for margin-of-exposure substances, RQ = MOE threshold / MOE. Concern if RQ ≥ 1. A result exceeding the reference only under UB is flagged as *uncertain*.

### Reference values (editable in the tool)

| Substance | Type | Value | Critical effect / source |
|---|---|---|---|
| Cadmium | TWI | 2.5 µg/kg bw/week | Kidney toxicity — EFSA 2009/2011 |
| Mercury (as methylmercury) | TWI | 1.3 µg/kg bw/week | Neurodevelopment — EFSA 2012 |
| Lead | MOE | BMDL<sub>10</sub> 0.63 µg/kg bw/day; MOE ≥ 10 | Nephrotoxicity in adults — EFSA 2010 |
| Acrylamide | MOE | BMDL<sub>10</sub> 0.17 mg/kg bw/day; MOE ≥ 10,000 | Tumours (genotoxic) — EFSA 2015 |
| PAH4 | MOE | BMDL<sub>10</sub> 0.34 mg/kg bw/day; MOE ≥ 10,000 | Carcinogenicity — EFSA 2008 |
| Deoxynivalenol (DON) | Group TDI | 1 µg/kg bw/day | Reduced body-weight gain — EFSA 2017 |
| Dioxins + dl-PCBs | TWI | 2 pg WHO-TEQ/kg bw/week | Sperm quality — EFSA 2018 |
| PBDEs (Σ7) | MOE | 3.2 ng/kg bw/day; MOE ≥ 25 | Neurodevelopment — EFSA 2024, tier-1 approach (reference point of the most potent congener applied to the sum) |
| PFAS (Σ4) | TWI | 4.4 ng/kg bw/week | Immunotoxicity — EFSA 2020 |
| Chlorpyrifos | ADI | 0.001 mg/kg bw/day | Developmental neurotoxicity — EFSA 2014; no safe level could be set in 2019 |
| Dimethoate | ADI | 0.001 mg/kg bw/day | Cholinesterase inhibition — EFSA |
| Carbendazim | ADI | 0.02 mg/kg bw/day | Reproductive toxicity — EFSA 2010 |
| Bisphenol A | TDI | 0.2 ng/kg bw/day | Immune system (Th17 cells) — EFSA 2023 (BfR 2023 proposes 200 ng/kg bw/day) |

Reference values should be checked against the latest scientific opinions before any use beyond teaching.

### Main limitations

- One day of intake does not represent usual (chronic) exposure.
- Concentration data date from 2007–2009 and may not reflect current contamination levels.
- Not every substance was analysed in every food; metals are only available for 45 foods.
- Dietary route only (no air, dust or dermal exposure).
- Tier-1 groupings are a simplification of cumulative assessment groups, intended for teaching.

---

## Using the tool

No installation is needed: `index.html` is a single self-contained file with no external dependencies. Open it in any modern browser, online or offline. Diets and scenarios are stored only in the user's own browser; nothing is sent to a server.

## How to cite

> Cano-Sancho G. (2026). *MixExpo: a pedagogical calculator of dietary exposure to chemical mixtures based on French TDS2 data.* Version 1 (25.11.2026). https://github.com/gcanosancho/mixexpo

## Versions

- **v1 (25.11.2026)** — first web version, replacing the earlier Excel calculator; adds bisphenol A, LB/MB/UB scenarios, tiered mixture assessment and scenario comparison.

## Licence

The code is released under the [MIT licence](LICENSE). The ANSES concentration data are reproduced with attribution and remain subject to their original terms.

Code developed with the assistance of an AI assistant (Claude, Anthropic).
