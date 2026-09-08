## Solver Decision Variables

The following workbook-level named ranges are Solver changing cells:

TOM_BEDS

CAR_BEDS

MES_BEDS

These cells represent the number of beds planted for each crop and must be constrained as nonnegative integers.

---

## Solver Starting Points

### Starting Point 1

TOM_BEDS = 0

CAR_BEDS = 0

MES_BEDS = 0

### Starting Point 2

TOM_BEDS = 14

CAR_BEDS =20

MES_BEDS = 30

Solver Method:

GRG Nonlinear

Integer decisions required

Objective: Maximize PROFIT

---

## Farm Inputs

| Name | Value | Unit | Source |
|--------|--------|--------|--------|
| WEEKS | 36 | weeks | Case scenario |
| FIXED_COSTS | 20000 | USD per season | Case scenario |
| TOTAL_BED_CAP | 64 | beds | Case scenario |
| FARMER_HOURS | 720 | field hours | Case scenario |
| FARMER_SALARY | 50000 | USD per season | Case scenario |
| FARMER_LABOR_RATE | 34.72 | USD per hour | Case scenario |
| TEMP_WORKER_SALARY | 25000 | USD per season | Case scenario |
| TEMP_LABOR_RATE | 17.36 | USD per hour | Case scenario |
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
| TOM_MAX_BEDS | 14 | beds | Crop table |

---

## Carrot Inputs

| Name | Value | Unit | Source |
|--------|--------|--------|--------|
| CAR_PRICE | 2094 | USD per bed | Crop table |
| CAR_HRS | 0.83 | hours per week per bed | Crop table |
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

TOM_LABOR_HRS(q) =
q × TOM_HRS × WEEKS × (1 + TOM_DIM_PCT)^q

CAR_LABOR_HRS(q) =
q × CAR_HRS × WEEKS × (1 + CAR_DIM_PCT)^q

MES_LABOR_HRS(q) =
q × MES_HRS × WEEKS × (1 + MES_DIM_PCT)^q

### Calculated Crop Labor

TOM_LABOR_HOURS =
TOM_LABOR_HRS(TOM_BEDS)

CAR_LABOR_HOURS =
CAR_LABOR_HRS(CAR_BEDS)

MES_LABOR_HOURS =
MES_LABOR_HRS(MES_BEDS)

### Revenue

TOM_REVENUE =
TOM_BEDS × TOM_PRICE

CAR_REVENUE =
CAR_BEDS × CAR_PRICE

MES_REVENUE =
MES_BEDS × MES_PRICE

TOTAL_REVENUE =
TOM_REVENUE +
CAR_REVENUE +
MES_REVENUE

### Fertilizer Costs

TOM_FERT_COST =
TOM_BEDS × TOM_FERTILIZER

CAR_FERT_COST =
CAR_BEDS × CAR_FERTILIZER

MES_FERT_COST =
MES_BEDS × MES_FERTILIZER

TOTAL_FERTILIZER_COST =
TOM_FERT_COST +
CAR_FERT_COST +
MES_FERT_COST

### Total Labor

TOTAL_LABOR_HOURS =
TOM_LABOR_HOURS +
CAR_LABOR_HOURS +
MES_LABOR_HOURS

### Labor Allocation

PERM_HRS_USED =
MIN(TOTAL_LABOR_HOURS, FARMER_HOURS)

TEMP_HRS =
MAX(TOTAL_LABOR_HOURS − FARMER_HOURS, 0)

TEMP_WORKERS_NEEDED =
TEMP_HRS / TEMP_WORKER_HOURS

### Labor Cost

LABOR_COST =
(PERM_HRS_USED × FARMER_LABOR_RATE)
+
(TEMP_HRS × TEMP_LABOR_RATE)

### Blended Labor Rate

BLENDED_RATE =
LABOR_COST / TOTAL_LABOR_HOURS

### Crop Labor Allocation

TOM_LABOR_COST =
TOM_LABOR_HOURS × BLENDED_RATE

CAR_LABOR_COST =
CAR_LABOR_HOURS × BLENDED_RATE

MES_LABOR_COST =
MES_LABOR_HOURS × BLENDED_RATE

### Total Cost

TOTAL_COST =
LABOR_COST +
TOTAL_FERTILIZER_COST +
FIXED_COSTS

### Profit

PROFIT =
TOTAL_REVENUE
− LABOR_COST
− TOTAL_FERTILIZER_COST
− FIXED_COSTS

### Constraint Calculations

TOTAL_BEDS =
TOM_BEDS +
CAR_BEDS +
MES_BEDS

BEDS_CHECK =
IF(TOTAL_BEDS <= TOTAL_BED_CAP,"PASS","FAIL")

TEMP_CHECK =
IF(TEMP_WORKERS_NEEDED <= TEMP_WORKER_CAP,"PASS","FAIL")

### Marginal Cost Schedules

MC(q) =
TOTAL_COST(q) − TOTAL_COST(q−1)

Calculate for:

1 through TOM_MAX_BEDS

1 through CAR_MAX_BEDS

1 through MES_MAX_BEDS

---

## Definitions

### Marginal Cost Schedule

A table showing MC(q) for each crop from bed quantity 1 through the crop's maximum bed limit.

### Marginal Cost

MC(q) =
TOTAL_COST(q) − TOTAL_COST(q−1)

### Standalone P ≈ MC Point

The largest bed quantity q at which:

PRICE_PER_BED ≥ MC(q)

when evaluating that crop independently from the other crops.

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

15. Season profit must equal $42,762 when rounded to the nearest whole dollar.

16. All optimization outputs are the final Solver solution values.

17. The authoritative bed caps are:

TOM_MAX_BEDS = 14

CAR_MAX_BEDS = 20

MES_MAX_BEDS = 30

These limits override conflicting values from other materials.

---

## Validation Rules

### Structural Validation

- No #REF! errors
- No #DIV/0! errors
- No #NAME? errors
- No #VALUE! errors
- No #N/A errors
- Every calculated cell contains a formula

### Hand Calculation

Tomato labor at q = 1:

1 × 2.5 × 36 × 1.10

= 99 labor hours

Must match workbook calculation.

### Constraint Checks

TOTAL_BEDS <= 64

TEMP_WORKERS_NEEDED <= 4

Constraint cells must display PASS or FAIL.

### Intermediate MC Cross-Check

Compare at least one marginal-cost value generated by the workbook with the corresponding value from the Farm Profit Lab.

Values must agree within normal spreadsheet rounding tolerance.

### Solver Validation

Run Solver from:

0 / 0 / 0

and

20 / 0 / 0

Record whether results match.

---

## Acceptance Criteria

Optimal Mix

Tomatoes = 10 beds

Carrots = 20 beds

Mesclun = 30 beds

Beds Used

60

Season Profit

$42,762 when rounded to the nearest whole dollar

Standalone P ≈ MC

Tomatoes ≈ 10 beds

Carrots ≈ 10 beds

Mesclun ≈ 6 beds

---

## Outputs

OPT_TOM_BEDS = final Solver value of TOM_BEDS

OPT_CAR_BEDS = final Solver value of CAR_BEDS

OPT_MES_BEDS = final Solver value of MES_BEDS

TOTAL_BEDS

TOTAL_REVENUE

TOM_LABOR_HOURS

CAR_LABOR_HOURS

MES_LABOR_HOURS

TOTAL_LABOR_HOURS

PERM_HRS_USED

TEMP_HRS

TEMP_WORKERS_NEEDED

LABOR_COST

BLENDED_RATE

TOM_FERT_COST

CAR_FERT_COST

MES_FERT_COST

TOTAL_FERTILIZER_COST

TOTAL_COST

SEASON_PROFIT

BEDS_CHECK

TEMP_CHECK

TOM_MC_SCHEDULE

CAR_MC_SCHEDULE

MES_MC_SCHEDULE
