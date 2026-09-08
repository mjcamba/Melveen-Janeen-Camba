# Marginal Analysis Capability

## Summary

This document specifies the "marginal-analysis" capability: a reusable component that computes and reports marginal effects/impacts for model outputs and input features across datasets. It is intended to support reproducible analysis workflows, clear data contracts, and integration with the repository's AI conventions described in AGENTS.md.

## Motivation

Marginal analysis helps quantify how changes in individual inputs affect model outputs (e.g., probability changes, predicted value deltas). This capability standardizes how marginal effects are computed, reported, and validated across projects.

## Scope

- Compute marginal effects for tabular data and models producing scalar outputs (classification probability, regression predictions).
- Support local (per-instance) and average marginal effects (AME).
- Produce numeric summaries, diagnostics, and optional visualizations.
- Provide a minimal tool/CLI/API contract so other components can call it.

## Inputs

- dataset: path or in-memory representation (CSV, DataFrame) containing feature columns and optional sample weights.
- model: a callable that accepts a batch of rows and returns model outputs (probabilities or scalar predictions). The model interface must be documented by the caller.
- features: list of feature names to analyze.
- baseline: for categorical or discrete features, a baseline value to compare against (optional; default: dataset mode/median where applicable).
- deltas or value_grid: specification of changes to apply to each feature (e.g., +1, -1 for numeric; explicit set for categorical).
- batch_size: optional, for large datasets.
- random_seed: for deterministic sampling when using subsets.

Input contract (example YAML):

```yaml
dataset: data/processed/train.csv
model: "models/predict:predict_fn" # opaque reference — caller documents how to resolve
features:
  - age
  - income
baseline:
  age: 30
  income: 50000
value_grid:
  age: [-5, 0, +5]
  income: [0.9, 1.1]
batch_size: 1024
random_seed: 42
``` 

## Outputs

- numeric summary table (CSV/JSON) with columns: feature, perturbed_value, baseline_value, mean_output_change, std_output_change, ci_lower, ci_upper, sample_count
- per-instance marginal effects (optional, large) as parquet/ndjson
- diagnostic plots (e.g., marginal effect vs. feature value, distribution of changes)
- human-readable report (Markdown) describing methods, assumptions, and key findings

Output contract (example):

- capabilities/marginal-analysis/results/<run-id>/summary.json
- capabilities/marginal-analysis/results/<run-id>/per_instance.parquet
- capabilities/marginal-analysis/results/<run-id>/report.md

## Methods / Algorithms

- Finite-difference approach for numeric features: evaluate model at x and x + delta then compute difference.
- One-hot replacement for categorical features: set feature to each category value and compute change from baseline.
- For probability outputs, report absolute and relative change as appropriate.
- Support sample-weighted averages when sample weights supplied.
- Bootstrap (configurable n_bootstrap) to provide confidence intervals on mean effects.

Implementation notes:
- Avoid changing correlated features unless the caller intentionally supplies counterfactual rebalancing logic.
- For correlated features, include a warning and offer an optional conditional marginal approach (requires model or conditional sampler).

## API / CLI

Provide a minimal CLI wrapper and a programmatic function signature example:

Function signature (Python pseudocode):

```python
def run_marginal_analysis(dataset, model_fn, features, *, value_grid=None, baseline=None, batch_size=1024, n_bootstrap=0, seed=None, output_dir):
    """Runs marginal analysis and writes outputs to output_dir."""
```

CLI example:

```bash
python -m capabilities.marginal_analysis \
  --dataset data/processed/test.csv \
  --model models/predict:predict_fn \
  --features age income \
  --output-dir capabilities/marginal-analysis/results/run-2026-09-08
```

## Data contracts & privacy

- Input data should avoid PII unless analysis is allowed by policy. Document any sensitive fields used.
- When storing per-instance outputs, prefer hashed or anonymized identifiers.

## Evaluation & Tests

- Unit tests for numeric finite-difference computation (small synthetic model where expected deltas are known).
- Integration tests with a toy model (logistic regression) on synthetic data to verify AME behavior and CI coverage.
- CI should run a lightweight test to ensure no regressions to the computation API.

## Error handling & logging

- Validate required inputs (dataset, model, features) and fail fast with clear error messages.
- Log deterministic run metadata: timestamp, seed, feature list, model reference, input checksum.
- On non-fatal issues (e.g., missing categories in baseline), emit warnings and include them in the report.

## Outputs and reproducibility

- Each run writes a manifest (manifest.json) with parameters, environment (python package versions), and a checksum of inputs and outputs.
- Use the random_seed option to ensure reproducible bootstrap samples and subsampling.

## CI / Performance considerations

- Support streaming evaluation and batching to handle large datasets without full in-memory materialization.
- Provide a `--sample-frac` or `--max-samples` option to create fast CI-friendly runs.

## Conventions / Integration with AGENTS.md

- Follow AGENTS.md for AI agent prompt formats, system message conventions, and logging when marginal-analysis is orchestrated by an agent.
- If an AI agent is used to recommend analysis configurations, record the agent transcript per AGENTS.md rules and include references in the run manifest.

## Example run

A short example describing a typical run and interpreting outputs.

1. Caller provides model and dataset, requests features [age, income].
2. The capability computes predicted probability at baseline and after +5 years of age and +10% income.
3. Results: mean probability change for +5 age = +0.03 (CI 0.02–0.04); for +10% income = -0.01 (CI -0.02–0.00).
4. Report highlights statistical significance and caveats about correlated covariates.

## Next steps

- Implement a minimal Python reference implementation in capabilities/marginal-analysis/
- Add unit and integration tests in tests/capabilities/test_marginal_analysis.py
- Wire a simple CLI entrypoint and document usage in README.md under capabilities/marginal-analysis/


