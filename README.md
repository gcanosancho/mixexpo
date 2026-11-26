# MixExpo · Chemical mixture exposure calculator

**MixExpo v1.4 (26.11.2026)** — a pedagogical web tool to explore dietary exposure to chemical mixtures and the associated risks, based on the French Second Total Diet Study (TDS2 / EAT2, ANSES).

👉 **Open the calculator:** https://gcanosancho.github.io/mixexpo/

> ⚠️ **For pedagogical purposes only.** MixExpo illustrates the reasoning of dietary exposure and mixture risk assessment. It is not a diagnostic or regulatory tool, and a single 24-hour recall does not represent chronic exposure.

Author: **German Cano-Sancho** (INRAE, LABERCA, Nantes, France)

---

## What students do

The tool opens on a **Start** page that explains the steps, then follows them in tabs (interface in English by default, with Spanish and French; a link such as `…/mixexpo/?lang=es` or `?lang=fr` opens it directly in another language). The 36 chemicals were selected because they were **quantified in at least 10 % of TDS2 samples overall, or in at least 10 % of the samples of at least one food group** (groups with ≥ 10 samples), and are grouped and colour-coded by family: metals, process contaminants, mycotoxins, persistent organic pollutants, pesticides, food-contact materials, food additives and phytoestrogens.

0. **Start:** instructions, body weight and (optional) age. Under 12 years, the stricter ANSES isoflavone value for children is applied.
1. **My 24 h diet:** a 24-hour dietary recall, meal by meal, with household portions or grams. A live panel shows exposure and risk while the diet is being entered.
2. **Several days:** up to 7 days with the same editor, to move from a single day towards **chronic exposure**: a day × substance grid of risk quotients, the mean of the days, and the trajectory of any substance (or of the hazard index) with its running mean. A header switch lets all other tabs show results either for day 1 or for the mean of the days.
3. **Exposure:** daily exposure per substance (ng/kg bw/day) with lower/middle/upper-bound (LB/MB/UB) uncertainty, shown next to the risk quotient to contrast *how much* with *how risky*.
4. **Contributing foods:** a food × substance map of percentage contributions, distinguishing "not analysed" from "zero".
5. **Risk & mixtures:** a tiered mixture assessment following the EFSA (2019) guidance on combined exposure:
   - Tier 0: hazard index (HI) of all threshold substances;
   - Tier 1: HI by common toxicological effect (neurotoxicity, nephrotoxicity, liver/thyroid, reproduction/development, immunotoxicity, other);
   - genotoxic carcinogens combined as a total margin of exposure (MOE<sub>T</sub>).
6. **Compare scenarios:** save up to four diets (single day or mean of days) and compare exposure and risk profiles.
7. **Occurrence:** a heatmap of the share of TDS2 samples in which each chemical was quantified, by food group (or food), with the criterion each chemical meets. Three views: calculator chemicals × food groups, individual analytes × food groups, chemicals × foods.
8. **Methods & data:** data sources, selection criterion, occurrence of each chemical, assumptions and editable toxicological reference values.

Example diets: a **balanced day following the French PNNS 4 recommendations** (at least 5 fruit and vegetables, wholegrain starch, 2 dairy products, a handful of nuts, legumes, poultry, rapeseed/olive oil, water), Mediterranean with fish, fast food and vegetarian; and two multi-day examples (3 balanced PNNS days with poultry, fatty fish and lean fish/legumes; 3 contrasted days). Results can be exported as CSV (including all days, age and the results basis), and diets/days/scenarios saved and shared as JSON.

### Suggested classroom exercises

- Compare the ranking of substances by **exposure** versus by **risk**: why is the most abundant chemical not the most concerning one?
- Switch between **LB and UB**: which conclusions depend on analytical sensitivity rather than on measured contamination?
- Replace canned foods with fresh ones and follow the change in **bisphenol A** exposure.
- Switch the bisphenol A reference value from the BfR (200 ng/kg bw/day, default) to EFSA's (0.2 ng/kg bw/day) and discuss how hazard characterisation drives the conclusion.
- Explore how the **hazard index** and the **Tier-1 groups** change the interpretation of the same diet.
- Record several days: how does the **mean** compare with the worst day, and why do chronic reference values apply to the mean?
- In the **Occurrence** tab, find chemicals that are rare in the diet overall but frequent in one food group (pirimiphos-methyl in bread, PFAS in shellfish and offal): how would a selection based on the whole diet change the risk picture?
- Even the **balanced PNNS diet** exceeds some references (acrylamide from coffee and bread, lead, PBDEs on fish days): what does this say about diet quality versus contaminant exposure?

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
- **Selection of chemicals (v1.4):** the 425 analytes of the TDS2 files were screened. An analyte is kept if it was quantified in **≥ 10 % of all analysed samples, or in ≥ 10 % of the samples of at least one INCA2 food group with at least 10 analysed samples** (metals: ≥ 10 % of foods in the teaching sheet; bisphenol A: 57 % of samples). The minimum of 10 samples avoids selecting a chemical because of one positive sample out of two. 102 analytes meet the rule and are combined into 36 chemicals. The 14 PAHs other than PAH4 meet the rule but are only shown in the Occurrence tab (PAH4 is EFSA's indicator). Not kept: ochratoxin A, coumestrol and biochanin A (≥ 10 % only in groups with fewer than 10 samples), carbendazim (8 % of fruit), nitrites (6 % of cured meats), dimethoate. Total arsenic is frequent but not assessed (mostly non-toxic arsenobetaine in fish).
- **Aggregated substances:** PAH4 = BaP + BaA + BbF + CHR; PBDEs = Σ BDE-28, 47, 99, 100, 153, 154, 183; HBCDD = α + β + γ; non-dioxin-like PCBs = PCB 28, 52, 101, 138, 153, 180; dioxins and dl-PCBs as WHO-2005 TEQ (17 PCDD/Fs + 12 dl-PCBs); PFAS4 = PFOS + PFOA + PFNA + PFHxS (EFSA 2020); other long-chain PFAS = PFDA + PFUnDA + PFDoDA + PFTrDA; PBBs = PBB 52 + 101 + 153; isoflavones = daidzein + genistein + glycitein; other isoflavones = equol + formononetin; lignans = secoisolariciresinol + enterolactone + matairesinol. Sums are computed per sample before averaging.
- **Pesticide ADIs:** those in force at the time of TDS2 (EFSA 2008 compilation), except chlorpyrifos (lower EFSA 2014 value). In Tier 1, the three organophosphates are grouped under neurotoxicity (acetylcholinesterase inhibition); the other pesticides are placed under "other effects".
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
| Chlorpropham | ADI | 0.05 mg/kg bw/day | Blood, thyroid — EC 2003; not renewed since 2019 | 10 % (potatoes 88 %) |
| Pirimiphos-methyl | ADI | 0.004 mg/kg bw/day | Acetylcholinesterase inhibition — EFSA 2005 | 5 % (bread 93 %) |
| Chlorpyrifos-methyl | ADI | 0.01 mg/kg bw/day | Acetylcholinesterase inhibition — EC 2005; not renewed since 2020 | 1 % (bread 21 %) |
| Chlorpyrifos (-ethyl) | ADI | 0.001 mg/kg bw/day | Developmental neurotoxicity — EFSA 2014 (0.01 at the time of TDS2); not approved since 2020 | 1 % (fruit 17 %) |
| Propargite | ADI | 0.007 mg/kg bw/day | EU draft assessment report 2007 | 1 % (fruit 13 %) |
| Cyprodinil | ADI | 0.03 mg/kg bw/day | EFSA 2005 | 6 % (fruit 24 %) |
| Boscalid | ADI | 0.04 mg/kg bw/day | JMPR 2006 (same in the EU) | 3 % (fruit 20 %) |
| Iprodione | ADI | 0.06 mg/kg bw/day | EC 2002; not renewed since 2017 | 4 % (fruit 27 %) |
| Diphenylamine | ADI | 0.075 mg/kg bw/day | EFSA 2008; not approved since 2012 | 4 % (fruit 32 %) |
| Thiabendazole | ADI | 0.1 mg/kg bw/day | EC 2001 | 4 % (fruit 29 %) |
| Pyrimethanil | ADI | 0.17 mg/kg bw/day | EFSA 2006 | 4 % (fruit 11 %) |
| Fenhexamid | ADI | 0.2 mg/kg bw/day | EC 1998 | 2 % (fruit 17 %) |
| Fludioxonil | ADI | 0.37 mg/kg bw/day | EFSA 2007 | 4 % (fruit 32 %) |
| 2-Phenylphenol | ADI | 0.4 mg/kg bw/day | JMPR 1999 (same in the EU) | 4 % (soft drinks & juices 14 %) |
| PFAS (sum of 4) | TWI | 4.4 ng/kg bw/week | Immunotoxicity — EFSA 2020 | 12 % (shellfish 73 %) |
| Other long-chain PFAS | — | no reference value | Exposure described only | 10 % (offal 88 %) |
| Polybrominated biphenyls | — | no reference value | EFSA 2010: very low concern; exposure only | 16 % (offal 81 %) |
| BDE-209 | — | no reference value here | Assessed separately by EFSA; exposure only | 85 % |
| Annatto (E160b) | ADI | 0.3 mg/kg bw/day (norbixin) | EFSA 2016 (bixin: 6) | 3 % (cheese 14 %) |
| Other isoflavones (equol + formononetin) | — | no reference value | Exposure described only | 16 % (milk 100 %) |
| Bisphenol A | TDI | 200 ng/kg bw/day | BfR 2023 (default). ⚠ EFSA 2023: 0.2 ng/kg bw/day, 1000 times lower; the tool shows both | 57 % |
| Sulfites (as SO₂) | MOE | BMDL 38 mg/kg bw/day; MOE ≥ 80 | Neurotoxicity — EFSA 2022 (threshold to be verified) | 14 % |
| Tartaric acid (E334) | Group ADI | 240 mg/kg bw/day | No adverse effect at highest dose — EFSA 2020 | 17 % |
| Isoflavones | TRV | 0.02 mg/kg bw/day | Reproductive system — ANSES 2025 (0.01 for vulnerable groups) | 34 % |
| Lignans (incl. matairesinol) | — | no reference value | Exposure described only | 76 % |

\* Metals: share of the 43 foods with trace-element values in the original teaching sheet (sample-level data not available).
Reference values should be checked against the latest scientific opinions before any use beyond teaching.

### Main limitations

- One day of intake does not represent usual (chronic) exposure; the mean of a few days is a better, but still limited, estimate (dietary surveys use statistical modelling of several non-consecutive days).
- Concentration data date from 2007–2009 and may not reflect current contamination levels.
- Not every substance was analysed in every food; metals are only available for 45 foods.
- Selection by occurrence favours chemicals measured with sensitive methods; a rarely quantified chemical is not necessarily of low concern. The food-group condition depends on how foods are grouped (INCA2 nomenclature) and on the number of samples per group.
- Dietary route only (no air, dust or dermal exposure).
- Tier-1 groupings are a simplification of cumulative assessment groups, intended for teaching.

---

## Using the tool

No installation is needed: `index.html` is a single self-contained file with no external dependencies. Open it in any modern browser, online or offline. Diets and scenarios are stored only in the user's own browser; nothing is sent to a server.

## How to cite

> Cano-Sancho G. (2026). *MixExpo: a pedagogical calculator of dietary exposure to chemical mixtures based on French TDS2 data.* Version 1.4 (26.11.2026). https://github.com/gcanosancho/mixexpo

## Versions

- **v1.4 (26.11.2026)** — new selection rule: quantified in ≥ 10 % of samples overall **or** in ≥ 10 % of the samples of at least one food group (≥ 10 samples), bringing the calculator from 17 to 36 chemicals (PFAS, long-chain PFAS, PBBs, BDE-209, 13 pesticides, annatto, equol and formononetin; matairesinol added to lignans); new **Occurrence** tab with a heatmap of chemicals × food groups / analytes × food groups / chemicals × foods; Methods table shows the food group with the highest occurrence; HI drivers chart limited to the top 12.
- **v1.3 (26.11.2026)** — Start page with step-by-step instructions; age input; "Several days" tab (up to 7 days, day × substance grid, mean of days, trajectory with running mean) and a header switch to analyse day 1 or the mean; balanced PNNS example days replace the original spreadsheet example; English always the default unless a language is explicitly chosen; mobile layout fixes.
- **v1.2 (25.11.2026)** — chemicals selected by occurrence (quantified in ≥ 10 % of TDS2 samples): adds non-dioxin-like PCBs, HBCDD, piperonyl butoxide, chlorpropham, sulfites, tartaric acid, isoflavones and lignans; removes PFAS, chlorpyrifos, dimethoate and carbendazim; new liver/thyroid mixture group; occurrence shown in the Methods tab.
- **v1.1 (25.11.2026)** — English by default; chemicals grouped and colour-coded by family with full names; bisphenol A assessed against the BfR TDI with a warning on the EFSA value; clearer display of substances never detected (LB = 0).
- **v1 (25.11.2026)** — first web version, replacing the earlier Excel calculator; adds bisphenol A, LB/MB/UB scenarios, tiered mixture assessment and scenario comparison.

## Licence

The code is released under the [MIT licence](LICENSE). The ANSES concentration data are reproduced with attribution and remain subject to their original terms.

Code developed with the assistance of an AI assistant (Claude, Anthropic).
