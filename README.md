# MixExpo · Chemical mixture exposure calculator

**MixExpo v1.2 (25.11.2026)** — a pedagogical web tool to explore dietary exposure to chemical mixtures and the associated risks, based on the French Second Total Diet Study (TDS2 / EAT2, ANSES).

👉 **Open the calculator:** https://gcanosancho.github.io/mixexpo/

> ⚠️ **For pedagogical purposes only.** MixExpo illustrates the reasoning of dietary exposure and mixture risk assessment. It is not a diagnostic or regulatory tool, and a single 24-hour recall does not represent chronic exposure.

Author: **German Cano-Sancho** (INRAE, LABERCA, Nantes, France)

---

## What students do

The tool follows six steps (interface in English by default, with Spanish and French; a link such as `…/mixexpo/?lang=es` or `?lang=fr` opens it directly in another language). The 17 chemicals were selected because they were **quantified in at least 10 % of TDS2 samples**, and are grouped and colour-coded by family: metals, process contaminants, mycotoxins, persistent organic pollutants, pesticides, food-contact materials, food additives and phytoestrogens.

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
- Switch the bisphenol A reference value from the BfR (200 ng/kg bw/day, default) to EFSA's (0.2 ng/kg bw/day) and discuss how hazard characterisation drives the conclusion.
- Explore how the **hazard index** and the **Tier-1 groups** change the interpretation of the same diet.

---

## Data sources

| Data | Source | Notes |
|---|---|---|
| Food concentrations (acrylamide, PAHs, dioxins & PCBs, brominated flame retardants, mycotoxins, pesticides, additives, phytoestrogens) | ANSES, Second French Total Diet Study (EAT2), regional data, 2007–2009. [data.gouv.fr](https://www.data.gouv.fr/datasets/donnees-regionales-eat2-etude-de-l-alimentation-totale/) | Composite samples of 15 sub-samples, foods analysed "as consumed" |
| Bisphenol A | ANSES / LABERCA campaign on EAT2 samples, plus bottled waters (LHN). [data.gouv.fr](https://www.data.gouv.fr/datasets/bisphenol-a) | Samples collected before the French ban of BPA in food-contact materials (2015) |
| Cd, Hg, Pb | EAT2 trace-element values from the original teaching spreadsheet (45 foods) | Not available for the full catalogue |

The concentration data belong to ANSES and are reproduced with attribution for teaching purposes. They are **not** covered by the MIT licence of this repository.

---

## Methods (summary)

- **Exposure:** `E = Σ (food intake [g/day] × mean concentration [ng/g]) / body weight [kg]`, using the mean of all TDS2 samples for each food.
- **Left-censored data:** non-detects (< LOD) and non-quantified results (< LOQ) are replaced by 0 (LB), half the limit (MB) or the limit (UB). "Not analysed" results are excluded, not set to zero.
- **Selection of chemicals:** all 427 analytes in the TDS2 and BPA files were screened; chemicals quantified in ≥ 10 % of analysed samples were kept (metals: ≥ 10 % of foods in the teaching sheet). Left out because rarely quantified: PFAS (PFOS 9 %, others < 5 %), ochratoxin A (0.5 %), chlorpyrifos (1.2 %), carbendazim (0.5 %), dimethoate (0.3 %), nitrites (3.7 %). Total arsenic is frequent but not assessed (mostly non-toxic arsenobetaine in fish).
- **Aggregated substances:** PAH4 = BaP + BaA + BbF + CHR; PBDEs = Σ BDE-28, 47, 99, 100, 153, 154, 183; HBCDD = α + β + γ; non-dioxin-like PCBs = PCB 28, 52, 101, 138, 153, 180; dioxins and dl-PCBs as WHO-2005 TEQ (17 PCDD/Fs + 12 dl-PCBs); isoflavones = daidzein + genistein + glycitein; lignans = secoisolariciresinol + enterolactone. Sums are computed per sample before averaging.
- **Risk characterisation:** risk quotient RQ = exposure / health-based guidance value; for margin-of-exposure substances, RQ = MOE threshold / MOE. Concern if RQ ≥ 1. A result exceeding the reference only under UB is flagged as *uncertain*.

### Reference values (editable in the tool)

| Substance | Type | Value | Critical effect / source | Quantified in TDS2 |
|---|---|---|---|---|
| Cadmium | TWI | 2.5 µg/kg bw/week | Kidney toxicity — EFSA 2009/2011 | 98 % of foods* |
| Mercury (as methylmercury) | TWI | 1.3 µg/kg bw/week | Neurodevelopment — EFSA 2012 | 14 % of foods* |
| Lead | MOE | BMDL<sub>10</sub> 0.63 µg/kg bw/day; MOE ≥ 10 | Nephrotoxicity in adults — EFSA 2010 | 72 % of foods* |
| Acrylamide | MOE | BMDL<sub>10</sub> 0.17 mg/kg bw/day; MOE ≥ 10,000 | Tumours (genotoxic) — EFSA 2015 | 74 % |
| PAH4 | MOE | BMDL<sub>10</sub> 0.34 mg/kg bw/day; MOE ≥ 10,000 | Carcinogenicity — EFSA 2008 | 88 % |
| Deoxynivalenol | Group TDI | 1 µg/kg bw/day | Reduced body-weight gain — EFSA 2017 | 45 % |
| Dioxins + dl-PCBs | TWI | 2 pg WHO-TEQ/kg bw/week | Sperm quality — EFSA 2018 | 100 % |
| Non-dioxin-like PCBs (Σ6) | TDI | 10 ng/kg bw/day | AFSSA 2007 (no EFSA value) | 100 % |
| PBDEs (Σ7) | MOE | 3.2 ng/kg bw/day; MOE ≥ 25 | Neurodevelopment — EFSA 2024, tier 1 (to be verified) | 99 % |
| HBCDD | MOE | 2.35 µg/kg bw/day; MOE ≥ 24 | Neurodevelopment — EFSA 2021 (threshold to be verified) | 87 % |
| Piperonyl butoxide | ADI | 0.2 mg/kg bw/day | Liver — JMPR 1995 | 18 % |
| Chlorpropham | ADI | 0.05 mg/kg bw/day | Blood, thyroid — EFSA 2017 (to be verified) | 10 % |
| Bisphenol A | TDI | 200 ng/kg bw/day | BfR 2023 (default). ⚠ EFSA 2023: 0.2 ng/kg bw/day, 1000 times lower; the tool shows both | 57 % |
| Sulfites (as SO₂) | MOE | BMDL 38 mg/kg bw/day; MOE ≥ 80 | Neurotoxicity — EFSA 2022 (threshold to be verified) | 14 % |
| Tartaric acid (E334) | Group ADI | 240 mg/kg bw/day | No adverse effect at highest dose — EFSA 2020 | 17 % |
| Isoflavones | TRV | 0.02 mg/kg bw/day | Reproductive system — ANSES 2025 (0.01 for vulnerable groups) | 34 % |
| Lignans | — | no reference value | Exposure described only | 76 % |

\* Metals: share of the 43 foods with trace-element values in the original teaching sheet (sample-level data not available).
Reference values should be checked against the latest scientific opinions before any use beyond teaching.

### Main limitations

- One day of intake does not represent usual (chronic) exposure.
- Concentration data date from 2007–2009 and may not reflect current contamination levels.
- Not every substance was analysed in every food; metals are only available for 45 foods.
- Selection by occurrence favours chemicals measured with sensitive methods; a rarely quantified chemical (e.g. PFAS with 2007–2009 detection limits) is not necessarily of low concern.
- Dietary route only (no air, dust or dermal exposure).
- Tier-1 groupings are a simplification of cumulative assessment groups, intended for teaching.

---

## Using the tool

No installation is needed: `index.html` is a single self-contained file with no external dependencies. Open it in any modern browser, online or offline. Diets and scenarios are stored only in the user's own browser; nothing is sent to a server.

## How to cite

> Cano-Sancho G. (2026). *MixExpo: a pedagogical calculator of dietary exposure to chemical mixtures based on French TDS2 data.* Version 1.2 (25.11.2026). https://github.com/gcanosancho/mixexpo

## Versions

- **v1.2 (25.11.2026)** — chemicals selected by occurrence (quantified in ≥ 10 % of TDS2 samples): adds non-dioxin-like PCBs, HBCDD, piperonyl butoxide, chlorpropham, sulfites, tartaric acid, isoflavones and lignans; removes PFAS, chlorpyrifos, dimethoate and carbendazim; new liver/thyroid mixture group; occurrence shown in the Methods tab.
- **v1.1 (25.11.2026)** — English by default; chemicals grouped and colour-coded by family with full names; bisphenol A assessed against the BfR TDI with a warning on the EFSA value; clearer display of substances never detected (LB = 0).
- **v1 (25.11.2026)** — first web version, replacing the earlier Excel calculator; adds bisphenol A, LB/MB/UB scenarios, tiered mixture assessment and scenario comparison.

## Licence

The code is released under the [MIT licence](LICENSE). The ANSES concentration data are reproduced with attribution and remain subject to their original terms.

Code developed with the assistance of an AI assistant (Claude, Anthropic).
