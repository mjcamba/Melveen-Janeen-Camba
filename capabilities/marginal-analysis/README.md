# Marginal Analysis

This capability implements a farm profit optimization model for a diversified vegetable farm operating under perfect competition.

The workbook evaluates tomatoes, carrots, and mesclun using crop-specific revenue, fertilizer costs, labor requirements, and diminishing-return labor functions. Labor is allocated using a farmer-hours-first convention, where the first 720 field hours are costed at the farmer labor rate and additional hours are costed at the temporary-worker rate. Crop labor costs are assigned using a farm-wide blended labor rate.

The model supports optimization of crop bed allocations subject to:

- Total bed capacity
- Crop-specific maximum bed limits
- Temporary worker capacity
- Nonnegative integer planting decisions

The workbook includes:

- A **Model** worksheet containing inputs, decision variables, calculations, outputs, and constraint checks
- A **MC Schedules** worksheet containing standalone marginal-cost and average-variable-cost schedules for tomatoes, carrots, and mesclun
- Formula-based PASS/FAIL validation checks
- Price-versus-marginal-cost charts for each crop
- Solver-ready decision variables and objective function

## Objective

Maximize seasonal profit (`PROFIT`) by choosing the optimal values for:

- `TOM_BEDS`
- `CAR_BEDS`
- `MES_BEDS`

subject to the model constraints documented in `spec.md`.

## Files

- `spec.md` — authoritative model specification and audit record
- `model.xlsx` — Excel implementation of the specification

Capability: Marginal analysis and farm-profit optimization using nonlinear labor-response functions and Excel Solver.
