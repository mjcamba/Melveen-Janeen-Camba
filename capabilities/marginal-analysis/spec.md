# Marginal-Analysis Model — Logic and Acceptance Checks

---

## Solver Decision Variables

The following workbook-level named ranges are Solver changing cells:

- TOM_BEDS
- CAR_BEDS
- MES_BEDS

These cells represent the number of beds planted for each crop and must be constrained as nonnegative integers.

---

## Workbook Structure

- **Model** — inputs, decisions, formulas, outputs, Solver objective/constraints, checks
- **MC Schedules** — standalone crop schedules, MC/AVC, price–MC markers, and charts
All hardcoded inputs, decision variables, primary calculated outputs, PROFIT, and constraint-check cells must use workbook-level named ranges. Marginal-cost schedules may use worksheet tables or r[...]
---

## Farm Inputs

| Name | Value | Unit | Source |
|--------|--------|--------|--------|
| WEEKS | 36 | weeks | Case scenario |
| FIXED_COSTS | 20000 | USD per season | Case scenario |
| TOTAL_BED_CAP | 64 | beds | Case scenario |
| FARMER_HOURS | 720 | field hours | Case scenario |
| FARMER_SALARY | 50000 | USD per season | Case scenario |
| FARMER_LABOR_RATE | 50000 / 1440 | USD per hour | Case scenario |
| TEMP_WORKER_SALARY | 25000 | USD per season | Case scenario |
| TEMP_LABOR_RATE | 25000 / 1440 | USD per hour | Case scenario |
| TEMP_WORKER_HOURS | 1440 | hours per worker per season | Case scenario |
| TEMP_WORKER_CAP | 4 | workers | Case scenario |

---

## Tomato Inputs

| Name | Value | Unit | Source |
|--------|--------|--------|--------|
| TOM_PRICE | 8800 | USD per bed | Crop table |
| TOM_HRS | 2.50 | hours per week per bed | Crop table |
| TOM_FERTILIZER | 880 | USD per bed | Farm Profit Lab |
| TOM_DIM_PCT | 10.0% | diminishing-return rate | Farm Profit Lab |
| TOM_MAX_BEDS | 20 | beds | Crop table |

---

## Carrot Inputs

| Name | Value | Unit | Source |
|--------|--------|--------|--------|
| CAR_PRICE | 2094 | USD per bed | Crop table |
| CAR_HRS | TOM_HRS / 3 | hours per week per bed | Crop table |
| CAR_FERTILIZER | 440 | USD per bed | Farm Profit Lab |
| CAR_DIM_PCT | 2.5% | diminishing-return rate | Farm Profit Lab |
| CAR_MAX_BEDS | 20 | beds | Crop table |

---

## Mesclun Inputs

| Name | Value | Unit | Source |
|--------|--------|--------|--------|
| MES_PRICE | 2700 | USD per bed | Crop table |
| MES_HRS | 1.25 | hours per week per bed | Crop table |
| MES_FERTILIZER | 880 | USD per bed | Farm Profit Lab |
| MES_DIM_PCT | 1.25% | diminishing-return rate | Farm Profit Lab |
| MES_MAX_BEDS | 30 | beds | Crop table |

---

## Calculation Logic

### Crop Labor Functions

TOM_LABOR_HRS(q) = q × TOM_HRS × WEEKS × (1 + TOM_DIM_PCT)^q

CAR_LABOR_HRS(q) = q × CAR_HRS × WEEKS × (1 + CAR_DIM_PCT)^q

MES_LABOR_HRS(q) = q × MES_HRS × WEEKS × (1 + MES_DIM_PCT)^q

### Calculated Crop Labor

TOM_LABOR_HOURS = TOM_LABOR_HRS(TOM_BEDS)

CAR_LABOR_HOURS = CAR_LABOR_HRS(CAR_BEDS)

MES_LABOR_HOURS = MES_LABOR_HRS(MES_BEDS)

### Revenue

TOM_REVENUE = TOM_BEDS × TOM_PRICE

CAR_REVENUE = CAR_BEDS × CAR_PRICE

MES_REVENUE = MES_BEDS × MES_PRICE

TOTAL_REVENUE = TOM_REVENUE + CAR_REVENUE + MES_REVENUE

### Fertilizer Costs

TOM_FERT_COST = TOM_BEDS × TOM_FERTILIZER

CAR_FERT_COST = CAR_BEDS × CAR_FERTILIZER

MES_FERT_COST = MES_BEDS × MES_FERTILIZER

TOTAL_FERTILIZER_COST = TOM_FERT_COST + CAR_FERT_COST + MES_FERT_COST

### Total Labor

TOTAL_LABOR_HOURS = TOM_LABOR_HOURS + CAR_LABOR_HOURS + MES_LABOR_HOURS

### Labor Allocation

PERM_HRS_USED = MIN(TOTAL_LABOR_HOURS, FARMER_HOURS)

TEMP_HRS = MAX(TOTAL_LABOR_HOURS − FARMER_HOURS, 0)

TEMP_WORKERS_NEEDED = TEMP_HRS / TEMP_WORKER_HOURS

### Labor Cost

LABOR_COST = (PERM_HRS_USED × FARMER_LABOR_RATE) + (TEMP_HRS × TEMP_LABOR_RATE)

### Blended Labor Rate

BLENDED_RATE = IF(TOTAL_LABOR_HOURS = 0, 0, LABOR_COST / TOTAL_LABOR_HOURS)

The zero-hours case (no beds planted for any crop) must resolve to a defined rate of 0 rather than a division error.

### Crop Labor Allocation

TOM_LABOR_COST = TOM_LABOR_HOURS × BLENDED_RATE

CAR_LABOR_COST = CAR_LABOR_HOURS × BLENDED_RATE

MES_LABOR_COST = MES_LABOR_HOURS × BLENDED_RATE

### Total Cost

TOTAL_COST = LABOR_COST + TOTAL_FERTILIZER_COST + FIXED_COSTS

### Profit

PROFIT = TOTAL_REVENUE − LABOR_COST − TOTAL_FERTILIZER_COST − FIXED_COSTS

### Constraint Calculations

TOTAL_BEDS = TOM_BEDS + CAR_BEDS + MES_BEDS

BEDS_CHECK = IF(TOTAL_BEDS <= TOTAL_BED_CAP, "PASS", "FAIL")

TEMP_CHECK = IF(TEMP_WORKERS_NEEDED <= TEMP_WORKER_CAP, "PASS", "FAIL")

TOM_CAP_CHECK = IF(TOM_BEDS <= TOM_MAX_BEDS, "PASS", "FAIL")

CAR_CAP_CHECK = IF(CAR_BEDS <= CAR_MAX_BEDS, "PASS", "FAIL")

MES_CAP_CHECK = IF(MES_BEDS <= MES_MAX_BEDS, "PASS", "FAIL")

### Marginal Cost and Average Variable Cost Schedules

**Marginal Cost Definition for Standalone Crop Schedules:**

For each crop evaluated at quantity q (with the other two crops held at zero beds):

1. **Schedule Basis:** The schedule is standalone — this crop at q beds, the other two crops at zero beds.

2. **Cost Basis:** The cost basis includes labor and fertilizer only. Fixed costs are excluded because they cancel in the marginal-cost difference, and excluding them avoids reusing the farm-level TOTAL_COST name.

3. **Labor Cost Calculation:** Labor uses the permanent-first split (permanent workers at FARMER_LABOR_RATE, then temporary workers at TEMP_LABOR_RATE), not a blended rate.

4. **Intermediate Cost Name:** Define an intermediate schedule-level cost as **TOM_SCHED_COST(q)**, **CAR_SCHED_COST(q)**, or **MES_SCHED_COST(q)** (depending on crop) to represent labor cost plus fertilizer cost at quantity q in the standalone schedule. This distinct name prevents confusion with the farm-level TOTAL_COST.

5. **Marginal Cost Formula:** MC(q) = TOM_SCHED_COST(q) − TOM_SCHED_COST(q−1) (or the corresponding crop-specific schedule cost difference).

6. **Average Variable Cost Formula:** AVC(q) = CROP_SCHED_COST(q) / q for q ≥ 1. At q = 0, leave AVC blank rather than displaying a division error.

Calculated for:

- 1 through TOM_MAX_BEDS
- 1 through CAR_MAX_BEDS
- 1 through MES_MAX_BEDS

For each crop's standalone schedule:

- The other two crop bed counts are set to zero.
- The focal crop is evaluated from zero through its maximum quantity.
- Labor (using permanent-first allocation), fertilizer, schedule cost, marginal cost, and average variable cost are recalculated at each quantity.
- The marginal-cost change is measured from the immediately preceding quantity.
- The schedule visibly marks the largest quantity where the crop price remains at least as high as MC.

---

## Definitions

### Marginal Cost Schedule

A table showing MC(q) and AVC(q) for each crop from bed quantity 1 through the crop's maximum bed limit.
Build separate standalone schedules for tomatoes, carrots, and mesclun.
For each schedule, set the other two crop quantities to zero.
Evaluate the focal crop from q = 0 through its own maximum-bed limit.
Exclude FIXED_COSTS; include only labor (using permanent-first allocation) and fertilizer cost in the schedule-level cost function.
Calculate labor hours, labor cost (permanent-first), fertilizer cost, variable cost, schedule cost, MC, and AVC for every quantity.
Define variable cost as VARIABLE_COST(q) = LABOR_COST(q) + FERTILIZER_COST(q).
Define schedule cost as CROP_SCHED_COST(q) = LABOR_COST(q) + FERTILIZER_COST(q) (the same as variable cost for the schedule).
Define marginal cost as MC(q) = CROP_SCHED_COST(q) − CROP_SCHED_COST(q−1).
Define average variable cost as AVC(q) = VARIABLE_COST(q) / q for q ≥ 1.
At q = 0, leave AVC blank rather than displaying an error.
Mark the largest quantity where the focal crop's price per bed is at least MC.
Create one chart per crop showing price per bed, MC, and AVC by quantity.

### Marginal Cost

MC(q) = CROP_SCHED_COST(q) − CROP_SCHED_COST(q−1), where CROP_SCHED_COST is the standalone schedule cost (labor using permanent-first allocation + fertilizer, excluding fixed costs).

### Average Variable Cost

AVC(q) = CROP_SCHED_COST(q) / q for q ≥ 1, where CROP_SCHED_COST is the standalone schedule cost (labor using permanent-first allocation + fertilizer, excluding fixed costs).

At q = 0, AVC is undefined and should be left blank. AVC represents the per-unit variable cost and is used to evaluate the shutdown decision: a crop should not be planted if its price falls below AVC at all feasible quantities, because revenue would not cover the variable costs incurred.

### Standalone P ≈ MC Point

The largest bed quantity q at which PRICE_PER_BED ≥ MC(q), evaluating that crop independently from the other crops.

### Binding Constraint

A constraint is binding when it is satisfied exactly at the optimal solution and prevents additional profit-generating activity.

### Optimization Run

One complete Solver execution using a specified starting point and the GRG Nonlinear solving method.

### Solver Solution

The final values returned by Solver for the decision variables.

### Acceptance Criteria

The published check figures and validation rules that the workbook must satisfy.

### Constraint-Check Cell

A workbook cell that displays PASS or FAIL based on whether a specified constraint is satisfied.

---

## Conventions

1. The farmer's labor is consumed first.
2. The first 720 field hours are permanent labor.
3. Any labor beyond 720 hours is temporary labor.
4. Permanent labor is costed before temporary labor.
5. Labor is allocated to crops using the blended labor rate.
6. Crop labor allocation must not use separate labor rates by crop.
7. Solver uses GRG Nonlinear.
8. Decision variables must be integers.
9. Bed counts cannot be negative.
10. All currency values are USD.
11. All displayed values use standard Excel rounding for presentation.
12. All calculations use full precision.
13. TEMP_WORKERS_NEEDED may be fractional.
14. Temporary workers are not rounded for calculations.
15. Season profit (PROFIT) must equal $42,762 when rounded to the nearest whole dollar.
16. All optimization outputs are the final Solver solution values.
17. The authoritative bed caps are:
    - TOM_MAX_BEDS = 20
    - CAR_MAX_BEDS = 20
    - MES_MAX_BEDS = 30

    These limits override conflicting values from other materials.
Item	Display precision
Bed counts	Whole numbers
Currency, revenue, cost, profit, MC, and AVC	Two decimal places
Labor hours	Two decimal places
Labor rates and blended labor rate	Two decimal places
Temporary workers needed	Two decimal places
Percentages	Two decimal places
---

## Validation Rules

### Structural Validation

- No #REF! errors
- No #DIV/0! errors
- No #NAME? errors
- No #VALUE! errors
- No #N/A errors
- Every calculated cell contains a formula

### Constraint Checks

- TOTAL_BEDS <= 64
- TEMP_WORKERS_NEEDED <= 4
- TOM_CAP_CHECK, CAR_CAP_CHECK, MES_CAP_CHECK each evaluate TOM_BEDS/CAR_BEDS/MES_BEDS against their respective maximum

All constraint-check cells (BEDS_CHECK, TEMP_CHECK, TOM_CAP_CHECK, CAR_CAP_CHECK, MES_CAP_CHECK) must display PASS or FAIL, with conditional formatting applying a green fill to PASS and a red fil[...]

### Hand Calculation Check

Tomato labor at q = 1:

1 × 2.5 × 36 × 1.10 = 99 labor hours

This must match the workbook calculation.

### Marginal Cost Cross-Check

Compare at least one marginal-cost value generated by the workbook against the corresponding value from the Farm Profit Lab. Compare the tomato standalone marginal cost at q = 1 with the Farm Pro[...]

The relevant checks are:

TOM_LABOR_HOURS(1) = 1 × 2.5 × 36 × 1.10 = 99.00

MC_Tom(1) = (99 × (50,000 / 1,440)) + 880 = 4,317.50

### Average Variable Cost Cross-Check

Compare the tomato standalone AVC at q = 1 with hand calculation:

AVC_Tom(1) = TOM_SCHED_COST(1) / 1 = [(99 × (50,000 / 1,440)) + 880] / 1 = 4,317.50

---

## Solver Configuration and Constraints

**Objective:** Maximize PROFIT

**Changing cells:** TOM_BEDS, CAR_BEDS, MES_BEDS

**Method:** GRG Nonlinear

**Constraints:**

- TOM_BEDS >= 0 and integer
- CAR_BEDS >= 0 and integer
- MES_BEDS >= 0 and integer
- TOM_BEDS <= TOM_MAX_BEDS
- CAR_BEDS <= CAR_MAX_BEDS
- MES_BEDS <= MES_MAX_BEDS
- TOTAL_BEDS <= TOTAL_BED_CAP
- TEMP_WORKERS_NEEDED <= TEMP_WORKER_CAP

### Solver Starting Points

**Starting Point 1**

- TOM_BEDS = 0
- CAR_BEDS = 0
- MES_BEDS = 0

**Starting Point 2**

- TOM_BEDS = 20
- CAR_BEDS = 0
- MES_BEDS = 0

### Solver Validation

Run Solver from both starting points (0/0/0 and 20/0/0) and record whether the two runs converge to the same result.

---

## Acceptance Criteria

**Optimal Mix**

- Tomatoes = 10 beds
- Carrots = 20 beds
- Mesclun = 30 beds

**Beds Used:** 60

**PROFIT:** $42,762 when rounded to the nearest whole dollar

**Standalone P ≈ MC**

- Tomatoes ≈ 10 beds
- Carrots ≈ 10 beds
- Mesclun ≈ 6 beds

---

## Outputs

- PROFIT
- OPT_TOM_BEDS = final Solver value of TOM_BEDS
- OPT_CAR_BEDS = final Solver value of CAR_BEDS
- OPT_MES_BEDS = final Solver value of MES_BEDS
- TOTAL_BEDS
- TOTAL_REVENUE
- TOM_LABOR_HOURS
- CAR_LABOR_HOURS
- MES_LABOR_HOURS
- TOTAL_LABOR_HOURS
- PERM_HRS_USED
- TEMP_HRS
- TEMP_WORKERS_NEEDED
- LABOR_COST
- BLENDED_RATE
- TOM_FERT_COST
- CAR_FERT_COST
- MES_FERT_COST
- TOTAL_FERTILIZER_COST
- TOTAL_COST
- BEDS_CHECK
- TEMP_CHECK
- TOM_CAP_CHECK
- CAR_CAP_CHECK
- MES_CAP_CHECK
- TOM_MC_SCHEDULE
- CAR_MC_SCHEDULE
- MES_MC_SCHEDULE

---

## Audit Status

## Audit Findings

### Formula and Naming Check
- TOM_BEDS: Verified workbook-level named range.
- CAR_BEDS: Verified workbook-level named range.
- MES_BEDS: Verified workbook-level named range.
- PROFIT: Verified workbook-level named range.
- Error scan with all bed counts set to zero: [result].

### Hand Calculation Check
- TOM_LABOR_HOURS(1) = [actual value].
- Expected = 99.00.
- Result: Pass/Fail.

### Marginal Cost Cross-Check
- Tomato MC(1) workbook value = [actual value].
- Farm Profit Lab value = $4,317.50.
- Result: Pass/Fail.

### Average Variable Cost Cross-Check
- Tomato AVC(1) workbook value = [actual value].
- Expected = $4,317.50.
- Result: Pass/Fail.

### Solver Validation
Run 1:
- Result: [actual solution].

Run 2:
- Result: [actual solution].

Converged to same solution:
- Yes/No.

### Acceptance Criteria
- Tomatoes = [actual]
- Carrots = [actual]
- Mesclun = [actual]
- Total Beds = [actual]
- Profit = [actual]

Result:
- Pass/Fail
