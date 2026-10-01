Sources, model, figures, success criteria

# Spec — economic research

This is the focus of my research on the 2023 Maui wildfires and the disproportionate economic and health burdens experienced by lower-income households after August 8, 2023.  

## Status

Skeleton. Sections below are placeholders for me to fill in.

## Research question

These consequences raise an important economic question: were the burdens of the disaster distributed equally among households with different financial resources?  Following the Maui wildfires on August 8, 2023, did households below the poverty line experience greater difficult accessing food, medical care and medications than households above the poverty line?  

**Hypothesis (H1).** Households below the federal poverty line experienced greater
difficulty accessing food, medical care, and medications after the Maui wildfires
than households above the poverty line.

## Scope and boundaries

This paper examines whether households below the poverty line experienced food insecurity and difficulty accessing healthcare/medications after the Maui wildfires than households above the poverty line. I expect that households below the poverty line faced greater barriers to care because they had fewer resources to absorb lost income, displacement, transportation costs, and other disaster-related expenses. When money becomes tight, families may have to choose between necessities. They may need to decide whether to pay for food, gas, a prescription refill, a medical appointment, or another urgent expense.

> **Scope warning, stated up front.** The hypothesis as worded has two limbs — *food*
> and *medical care/medications*. Only the medical limb is testable from published
> sources. No source in this set cross-tabulates food security by poverty line. The
> food limb must either be dropped from the hypothesis, restated as a cohort-level
> claim, or deferred pending the MauiWES dashboard / microdata. Acceptance criterion
> **AC-7** enforces this; **G-1** under Gaps records it as the one blocking item.

## Data sources

Awotona, A. A. (2017). Planning for community-based disaster resilience worldwide : learning from case studies in six continents. Routledge.
Hawaiʻi State Department of Health. (2023, October). Maui wildfires public health rapid needs assessment preliminary report. https://health.hawaii.gov/news/files/2023/10/Maui-Wildfire-RNA-Preliminary-Report.pdf
Hudson, S. W. (2016). Insights in public health: Climate change: A public health challenge and
opportunity for Hawai‘i. Hawai‘i Journal of Medicine & Public Health, 75(8), 245–250
Institute of Medicine (US) Committee on Enhancing Environmental Health Content in Nursing Practice. (1995). Nursing, health, & environment: Strengthening the relationship to improve the public's health (A. M. Pope, M. A. Snyder, & L. H. Mood, Eds.). National Academies Press.
University of Hawaiʻi Economic Research Organization. (2024). Maui wildfire exposure study: Community health, wellbeing, and resilience. University of Hawaiʻi at Mānoa. https://uhero.hawaii.edu/wp-content/uploads/2024/05/MauiExposureStudy.pdf
U.S. Bureau of Labor Statistics. (2023). Consumer Price Index, Honolulu area. U.S. Department of Labor. https://www.bls.gov/regions/west/news-release/consumerpriceindex_honolulu.htm

Retried on September 26, 2026

## Method

| ID | Source | Vintage | Used for |
|---|---|---|---|
| S1 | UHERO, *Maui Wildfire Exposure Study: Community Health, Wellbeing, and Resilience* (MauiWES) | 15 May 2024; fieldwork Jan–Mar 2024 | H1 test, cohort poverty share, food-security marginals |
| S2 | Hawai‘i DOH, *2023 Maui Wildfire Rapid Needs Assessment — Preliminary Report* | Oct 2023; fieldwork 9–11 Oct 2023 | Corroboration; safety-net friction |
| S3 | BLS CPI-U, Food at home, Urban Hawai‘i (`CUUSA426SAF11`) via FRED | Annual, 1984–2025 | Cost-pressure context |
| S4 | BLS CPI-U, Food at home, U.S. city average (`CUUR0000SAF11`) via FRED | Annual, 1984–2025 | Mainland comparison |
| S5 | U.S. Census ACS via USAFacts — Maui County, Hawai‘i, U.S. poverty rates | County 2020–2024 (5-yr); state/national 2024 | Exposure benchmark |

### Step 1 — Reconstruct the contingency table (S1)

S1 publishes the healthcare item only as rounded percentages by poverty line. Counts
are rebuilt as:

```
N          = 679                      (published cohort size)
n_below    = round(0.27 × 679) = 183  (published: 27% below FPL)
n_above    = 679 − 183       = 496
cell_count = round(published_pct × n_group)
residual cell ("no difficulty") = n_group − other two cells
```

Published row percentages, item *"Did you have any difficulties accessing medical
care or medications?"*:

| Group | Before **and** since | Only since | No difficulty |
|---|---|---|---|
| Overall | 13% | 28% | 60% |
| Below poverty line | 18% | 30% | 52% |
| At/above poverty line | 10% | 27% | 63% |

**Internal consistency check (must pass before any test runs):**
`0.27 × 48% + 0.73 × 37% = 40.0%`, against the published overall "any difficulty"
of `13% + 28% = 41%`. Agreement within rounding tolerance (≤1 pp). → **AC-1**

### Step 2 — Test statistics

All two-sided, α = 0.05, respondents treated as independent and unweighted.

- **Two-proportion z-test**, pooled variance for the test statistic:
  `z = (p̂₁ − p̂₂) / √(p̄(1−p̄)(1/n₁ + 1/n₂))`
- **Risk ratio CI**, log method:
  `exp(ln(RR) ± 1.96 × √((1−p̂₁)/x₁ + (1−p̂₂)/x₂))`
- **Risk difference CI**, unpooled: `(p̂₁−p̂₂) ± 1.96 × √(p̂₁(1−p̂₁)/n₁ + p̂₂(1−p̂₂)/n₂)`
- **Pearson χ²** on the full 2×3 table; effect size Cramér's V.
- **Cochran–Armitage trend test** where an ordered exposure exists.

### Step 3 — The three tests that decide H1

| Test | Contrast | Why it matters |
|---|---|---|
| **H1** | Any difficulty: below vs. above FPL | The headline claim |
| **H1a** | Difficulty *before and since*: below vs. above | Pre-existing disparity |
| **H1b** | Difficulty *only since*: below vs. above | **The "after the fires" claim** |

H1b is the test that actually matches the hypothesis's wording. H1 pools pre-existing
and new barriers and therefore cannot, on its own, support a claim about what the
fires did.

### Step 4 — Sensitivity (required, not optional)

1. **Rounding envelope.** Every published percentage is treated as ±0.5 pp; the test
   is re-run over the grid of corners. The *least favourable* corner is reported.
2. **Poverty-split sensitivity.** `n_below` re-derived at shares 22/25/27/30/33%.
3. **Minimum detectable effect** at 80% power for each test, to distinguish "no
   effect" from "no power."

### Step 5 — Context panels (not tests of H1)

- **Cost pressure (S3, S4).** Both series indexed to 2019 = 100. Counterfactual is a
  log-linear OLS fit to Hawai‘i 2010–2019: `ln(CPI) = α + β·year`, trend rate
  `exp(β) − 1 = 1.82%/yr`; excess = `actual / fitted − 1`.
- **Exposure benchmark (S5, S1).** One-sample z of the cohort poverty share against
  each fixed population rate: `z = (p̂ − p₀)/√(p₀(1−p₀)/n)`.

### Computed results (current run)

| Test | Below | Above | Effect | 95% CI | p | Verdict |
|---|---|---|---|---|---|---|
| H1 any difficulty | 48.1% | 37.1% | RR 1.30 | 1.07–1.57 | 0.0095 | Reject H₀ |
| H1a pre-existing | 18.0% | 10.1% | RR 1.79 | 1.19–2.68 | 0.0050 | Reject H₀ |
| **H1b only since fires** | 30.1% | 27.0% | RR 1.11 | 0.85–1.45 | **0.4331** | **Retain H₀** |
| H2 full 2×3 distribution | — | — | V 0.12 | — | 0.0066 | Reject H₀ |

Sensitivity: H1 survives the entire rounding envelope (worst corner p = 0.0179) and
every poverty split from 22% to 33% (p = 0.0049–0.0123). H1a survives (worst
p = 0.0139). **H1b is null across the whole envelope** — its most favourable corner
reaches only p = 0.2782. Minimum detectable RR for H1b is 1.42 against an observed
1.11, so H1b is underpowered and must be reported as *no detectable difference*.

---

## Assumptions

## Assumptions

| Assumption | Basis | How it could be falsified |
|---|---|---|
| A1. Reconstructed counts approximate the true cell counts | Published N = 679 and 27/73 split; consistency check reproduces the published 40% marginal to within 1 pp | Obtain MauiWES microdata or the dashboard's own n's; if any cell differs from the reconstruction by >5 units, Step 1 is wrong |
| A2. The 27% below-FPL share applies to the healthcare item's respondents | S1 reports poverty distribution and the healthcare item on the same cohort | Item-level non-response differs by poverty status; falsified if the dashboard reports an item-specific denominator ≠ 679 |
| A3. Respondents are independent observations | Standard for a z/χ² test | Cohort contains multiple adults per household; falsified by any household identifier in the microdata showing clustering (would require cluster-robust SEs and would widen all CIs) |
| A4. "Difficulty accessing medical care or medications" measures access as the hypothesis intends | Face validity of the published item wording | Item is a single self-report with no validated scale; falsified if S1's instrument appendix shows it captures, e.g., satisfaction rather than access |
| A5. Poverty classification is correct and consistent | S1 states Hawai‘i-specific FPL by household size ($22,680 for 2; $40,410 for 5) | Falsified if income was collected in bands too coarse to assign FPL status, or if household size was self-reported inconsistently |
| A6. Urban Hawai‘i CPI proxies grocery cost pressure on Maui | No Maui-specific CPI exists; Urban Hawai‘i is the closest published series | Falsified by any Maui-specific price series showing a materially different path; note the series is Honolulu-weighted |
| A7. Census benchmarks and the MauiWES cohort measure poverty comparably | Both use federal poverty thresholds | ACS uses Census money-income definitions; MauiWES uses self-reported income bands. Falsified by a definitional audit showing non-comparable income concepts |
| A8. The pre-fire food-security comparison group is a valid counterfactual | UHERO Rapid Survey, Maui subsample, June 2023 | S1 itself states this sample skews to higher education and income. Falsified — **already known to be weakly satisfied**; treat Panel D as between-survey, not causal |

### Assumptions that would *change the conclusion* if violated

A1 and A3 are load-bearing. A1 failing changes every number; A3 failing widens CIs
and could push H1 (p = 0.0095) past 0.05. A8 is already known to be imperfect and is
why no causal claim is made from the food-security panel.

---

## Falsification

The hypothesis is falsifiable at three levels. State which one a given result
addresses.

**F1 — Direct falsification of H1.** If below-FPL households show *equal or lower*
difficulty than above-FPL households, with a 95% CI on the risk ratio that excludes
1.0 in the opposite direction, H1 is false. *Current status: not falsified.* RR 1.30,
CI 1.07–1.57.

**F2 — Falsification of the causal "after the fires" reading.** If the disparity in
barriers arising *only since* the fires is null while the pre-existing disparity is
significant, then the hypothesis as literally worded — that the fires produced the
differential — is **not supported**, even though H1 passes. *Current status: this is
what the data show.* H1b p = 0.4331, null across the entire rounding envelope, while
H1a is robust at p = 0.0050. The honest conclusion is that the fires loaded onto a
pre-existing access gap rather than creating one. **The paper must say this.**

**F3 — Falsification by better data.** H1 is overturned if MauiWES microdata yields
counts materially different from the reconstruction (A1), if clustering materially
widens the CIs (A3), or if the dashboard's own poverty-line cut contradicts the
reconstructed table.

**What would make the food limb falsifiable at all:** a cross-tabulation of the NCHS
six-item food-security scale by poverty line within the cohort. Until that exists,
the food limb is untested, not supported and not refuted.

## Outputs

| Output | Where it lands |
|---|---|
| Figures | `analysis/figures/` |
| Final paper | `analysis/research-paper.pdf` |
| Working drafts | `drafts/` |
| Source data (CPI CSVs) | `analysis/data/` |
| Analysis scripts | `analysis.js`, `alt-groups.js`, `benchmark.js`, `sensitivity.js` |
| Machine-readable results | `results.txt`, `alt-groups-results.txt`, `benchmark-results.txt`, `sensitivity-results.txt`, `chartdata.json` |
| Interactive figure | `maui-figure.html` |

Figure inventory, each traceable to a source above or a Method step:

| Figure | Content | Traces to |
|---|---|---|
| Panel A | Hawai‘i vs. U.S. food CPI, 2019 = 100, with trend counterfactual | S3, S4; Step 5 |
| Panel B | Poverty-rate benchmark ladder | S5, S1; Step 5 |
| Panel C | Healthcare access by poverty line (2×3 stacked) | S1; Steps 1–3 |
| Panel D | Food security, cohort vs. pre-fire Maui, and by race | S1; descriptive only |
| Table 1 | Hypothesis test results | Steps 2–4 |
| Table 2 | RNA corroboration indicators | S2 |

---


PASS/FAIL. The paper does not ship with any FAIL.

| ID | Check | Threshold | Status |
|---|---|---|---|
| AC-1 | Reconstruction consistency: implied overall rate vs. published | ≤1 pp | **PASS** (40.0% vs 41%) |
| AC-2 | Every figure traces to a row in the Inputs table or a Method step | 100% | **PASS** (6/6, inventory above) |
| AC-3 | Every number in the paper reproducible by re-running the scripts | 100% | **PASS** — scripts regenerate all `*-results.txt` |
| AC-4 | H1 survives the full ±0.5 pp rounding envelope | worst-corner p < 0.05 | **PASS** (0.0179) |
| AC-5 | H1 survives poverty-split sensitivity 22–33% | all p < 0.05 | **PASS** (0.0049–0.0123) |
| AC-6 | Any null result reported with its minimum detectable effect | 100% of nulls | **PASS** — H1b MDE RR 1.42 stated |
| AC-7 | Food limb of the hypothesis is **not** claimed as tested | no such claim in text | **PASS** — flagged in scope warning and F-section |
| AC-8 | Ecological (area-level) data never used for individual-level inference | no such claim | **PASS** — Panel B caveated in figure |
| AC-9 | Causal language confined to F2's finding | no "the fires caused" on H1b | **PASS** |
| AC-10 | Palette validated for CVD in both light and dark modes | validator all-PASS | **PASS** — `validate_palette.js`, both modes |
| AC-11 | Every chart has a table-view equivalent | 100% | **PASS** — table view in `maui-figure.html` |
| AC-12 | Figures rendered and visually inspected before shipping | both modes | **PASS** — screenshots reviewed |
| AC-13 | Source vintages stated wherever a rate is cited | 100% | **PASS** |
| AC-14 | 5-year ACS window limitation stated where Maui County rate appears | present | **PASS** — in figure method note |

### Open gaps (not failures, but tracked)

| ID | Gap | Blocking? |
|---|---|---|
| G-1 | Food security not cross-tabulated by poverty line in any published source | **Yes — for the food limb only.** Hypothesis must be re-scoped or the data obtained |
| G-2 | Household clustering unknown (A3) | No — but widens CIs if present |
| G-3 | Maui County post-fire poverty rate unavailable (5-yr window) | No — use SAIPE 1-yr if the claim becomes load-bearing |
| G-4 | Filipino subgroup folded into "Asian" in ACS, distinct in MauiWES | No — flag as a data-aggregation finding |

---

## Audit record

*Append only. Do not rewrite prior entries.*

### 2026-09-29 — Initial build
- Retrieved S3/S4 from FRED; extracted S1/S2 from PDFs via `pdf-parse`.
- Built reconstruction (Step 1); AC-1 **PASS**.
- Ran H1/H1a/H1b/H2. H1 and H1a reject H₀; H1b retains.
- Palette validated both modes; AC-10 **PASS**.
- **Corrected:** "2019 = 100" axis annotation was clipped to "19 = 100" at the
  left edge of Panel A. Re-anchored inside the plot area. Caught by visual
  inspection (AC-12), not by the validator.

### 2026-09-30 — Alternative stratifiers
- Inventoried every published cross-tabulation in S1/S2.
- **Finding:** food security is cross-tabulated by *race only*, never by income.
  Logged as G-1.
- Computed MDEs. **Correction to earlier reporting:** H1b was initially described
  as a null result; it is more precisely *underpowered* (MDE RR 1.42 vs. observed
  1.11). AC-6 added to prevent recurrence.

### 2026-10-01 — Benchmarking and spec
- Added S5. Cohort poverty 27% vs. Maui County 9.2% → 2.93×, z = 16.0, p < 0.0001.
- **Corrected:** the supplied Hawaii Health Matters URL (`localeId=116998`)
  resolves to Census Tract 15009030202, **not** Maui County. Verified by rendering
  the dashboard; its own rows compare the tract against "Maui, HI County Value."
  That tract (median HH income $88,581) is not representative of the burn zone.
  Source dropped; USAFacts/Census used instead.
- **Corrected:** Panel numbering shifted (old B/C → new C/D) when the benchmark
  panel was inserted. Figure IDs and captions reconciled.
- Ran full sensitivity suite. AC-4, AC-5, AC-6 **PASS**.
- **Key result for the paper's framing:** F2 is triggered. H1b is null across the
  entire rounding envelope (best corner p = 0.2782) while H1a is robust
  (worst corner p = 0.0139). The hypothesis as worded — that the differential arose
  *after* the fires — is not supported by these data; the differential is
  pre-existing. Paper must be framed accordingly.

### Pending
- [ ] Resolve G-1: request MauiWES dashboard export or microdata for food security
      by poverty line. Until then AC-7 holds the food limb out of scope.
- [ ] Resolve G-2: confirm whether the cohort contains multiple adults per household.
- [ ] Produce `analysis/research-paper.pdf`; re-run AC-2 and AC-3 against the final text.
