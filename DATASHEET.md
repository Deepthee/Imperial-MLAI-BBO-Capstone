# Datasheet for the Black-Box Optimisation Dataset

## Dataset Overview

This dataset was created as part of the **Imperial College London Professional Certificate in Machine Learning and Artificial Intelligence** Black-Box Optimisation (BBO) Capstone Project.

The dataset records the sequential optimisation history for eight unknown objective functions. Stage 2 of the project consisted of **13 optimisation rounds from Modules 12 to 24**, with one new query submitted for each function during every round.

Each submitted query produced an objective value from the external BBO evaluation system. That result was then incorporated into the available evidence and used to inform subsequent optimisation decisions.

Unlike a conventional static machine-learning dataset, this dataset was generated **sequentially and adaptively**. Later observations therefore depend partly on the results obtained in earlier rounds.

---

# 1. Motivation

## Why was the dataset created?

The dataset was created to support the Black-Box Optimisation Capstone Project.

Its primary purpose is to document the optimisation process and enable analysis of how query-selection strategies evolve as additional observations become available.

The dataset supports learning and experimentation in:

- Sequential optimisation
- Bayesian optimisation
- Gaussian Process surrogate modelling
- Exploration versus exploitation
- Optimisation under uncertainty
- Acquisition-function reasoning
- Data-driven decision-making
- Reproducible optimisation workflows

The dataset also provides a record of both successful and unsuccessful query decisions, allowing the evolution of the optimisation strategy to be examined retrospectively.

---

## Who created the dataset?

The Stage 2 optimisation history was generated and documented by **Deepthee Kasal** during completion of the Imperial College London Professional Certificate in Machine Learning and Artificial Intelligence.

The initial observations and subsequent objective-function evaluations were supplied by the capstone platform.

---

# 2. Composition

## What does the dataset contain?

The repository contains data for eight independent black-box objective functions.

Each Stage 2 observation records:

- Optimisation round
- Module
- Function identifier
- Query coordinates
- Number of input dimensions
- Objective value returned by the evaluator

The functions have different dimensionalities:

| Function | Dimensions |
| --- | ---: |
| F1 | 2 |
| F2 | 2 |
| F3 | 3 |
| F4 | 4 |
| F5 | 4 |
| F6 | 5 |
| F7 | 6 |
| F8 | 8 |

All query coordinates and objective values are numerical.

---

## Dataset Size

Stage 2 contains:

- **13 sequential optimisation rounds**
- **8 objective functions**
- **1 submitted query per function per round**
- **104 Stage 2 query evaluations**

In addition to these Stage 2 observations, the repository contains the original initial input and output datasets supplied at the beginning of the BBO challenge.

The initial datasets are stored separately in:

`data/initial/`

The Stage 2 history is stored in:

- `data/queries.csv`
- `data/results.csv`

---

## Data Format

### `queries.csv`

Contains the submitted Stage 2 query points.

Fields include:

- `round`
- `module`
- `function`
- `dimensions`
- `query`
- `x1` to `x8`

Unused coordinate columns are blank for lower-dimensional functions.

### `results.csv`

Contains the objective values returned by the BBO evaluation system.

Fields include:

- `round`
- `module`
- `function`
- `output`

### Initial datasets

The initial observations are retained in their original NumPy `.npy` format.

For each function there is an input and output file, for example:

```text
function1_initial_inputs.npy
function1_initial_outputs.npy
...
function8_initial_inputs.npy
function8_initial_outputs.npy
```

---

## Missing Information

The mathematical forms of the eight objective functions are intentionally unknown.

The repository therefore does not contain:

- source code for the hidden functions;
- analytical expressions for the functions;
- known global optima;
- complete representations of the search spaces; or
- exhaustive evaluations of candidate points.

Large areas of the search spaces remain unexplored, particularly for the higher-dimensional functions.

---

# 3. Collection Process

## Initial Data

The initial observations were supplied by the capstone platform.

These provided the starting evidence from which optimisation decisions could be made.

---

## Stage 2 Data Collection

Stage 2 consisted of 13 sequential query rounds from **Module 12 through Module 24**.

During each round:

1. Existing observations were reviewed.
2. The behaviour of each function was analysed.
3. Surrogate-model predictions and/or optimisation trends were considered.
4. One new query point was selected for each function.
5. The eight queries were submitted to the external capstone portal.
6. The portal returned one objective value for each query.
7. The new observations were incorporated into the optimisation history.
8. The expanded dataset informed the following round.

This means the observations are **not independent random samples**.

They reflect an adaptive optimisation policy in which previous outcomes influenced subsequent query locations.

---

## Query-Selection Strategy

The query-selection process evolved throughout the project.

The primary framework was based on Bayesian optimisation principles and included:

- Gaussian Process surrogate modelling;
- Matérn-family kernels;
- Expected Improvement;
- Upper Confidence Bound reasoning;
- Sobol candidate generation;
- predictive uncertainty;
- historical objective values;
- local improvement patterns; and
- exploration-exploitation trade-offs.

Later rounds increasingly incorporated function-specific judgement and concepts from hyperparameter tuning, clustering, PCA and reinforcement learning.

The final approach is described in:

- [Model Card](MODEL_CARD.md)
- [Methodology](METHODOLOGY.md)
- [Optimisation History](OPTIMISATION_HISTORY.md)

---

# 4. Preprocessing and Transformations

The original objective observations were preserved without altering their returned values.

No target-value imputation or synthetic objective evaluations were introduced.

For repository analysis, the observations were reorganised into structured tabular form so that the Stage 2 history could be analysed consistently across all eight functions.

Because the functions have different dimensionalities, the query table provides coordinate columns from `x1` to `x8`. Coordinates that do not apply to a lower-dimensional function are left blank.

The original initial datasets are retained separately in `.npy` format to preserve the source data used during the challenge.

---

# 5. Intended Uses

The dataset is intended for:

- Educational demonstrations of black-box optimisation
- Sequential optimisation analysis
- Bayesian optimisation experiments
- Gaussian Process surrogate modelling
- Exploration-exploitation studies
- Analysis of optimisation under limited information
- Reproducible optimisation case studies
- Examination of adaptive query-selection behaviour

It may also be useful for comparing alternative surrogate or acquisition strategies retrospectively.

---

## Inappropriate Uses

The dataset should not be treated as:

- a representative real-world population dataset;
- a conventional supervised-learning benchmark;
- evidence that the global optima of the functions were found;
- a comprehensive map of the hidden functions;
- an unbiased random sample of the search spaces; or
- a validated production optimisation benchmark.

The observations were collected specifically for sequential optimisation and are therefore affected by the query-selection strategy.

---

# 6. Distribution and Access

The dataset forms part of the public GitHub repository for the BBO Capstone Project.

The repository contains:

- the original initial datasets;
- Stage 2 query history;
- Stage 2 objective results;
- Jupyter Notebook analysis;
- methodology documentation;
- optimisation history;
- model card; and
- this datasheet.

The hidden objective functions and external capstone evaluation platform are not included.

Consequently, historical analysis can be reproduced from the stored observations, but the original black-box evaluations cannot be independently rerun from this repository.

---

# 7. Maintenance

The dataset was updated during the active optimisation phase as new query results became available.

The optimisation phase is now **complete**.

The repository therefore serves as the final record of the BBO capstone rather than an actively growing optimisation dataset.

Future changes, if any, would be limited to:

- documentation corrections;
- reproducibility improvements;
- additional retrospective analysis; or
- clearer presentation of the existing results.

No further official BBO query rounds are expected to be added.

---

# 8. Biases and Sampling Considerations

The dataset contains an important form of **sequential sampling bias**.

Later query points were deliberately influenced by earlier results. As promising regions emerged, more evaluations were concentrated around those regions.

Consequently:

- some areas of the search space are sampled much more densely than others;
- poor-performing regions may contain relatively few observations;
- high-performing local regions may be overrepresented;
- later observations increasingly reflect exploitation decisions; and
- unexplored regions may still contain better solutions.

This is expected behaviour in black-box optimisation, but it limits the use of the dataset for purposes requiring uniform or representative sampling.

---

# 9. Limitations

Several limitations should be recognised.

## Small Sample Size

The number of observations is small relative to the size of the search spaces.

## High Dimensionality

Functions with five to eight dimensions contain very large search spaces compared with the number of available evaluations.

## Unknown Objective Functions

The underlying mathematical functions are hidden, preventing analytical verification of optimisation results.

## Unknown Global Optima

The strongest observed value cannot be assumed to represent the true global optimum.

## Adaptive Sampling

The dataset reflects optimisation decisions rather than random sampling, introducing intentional sampling bias.

## Model Dependence

Later query decisions were partly influenced by modelling assumptions and optimisation heuristics. Different surrogate models or acquisition strategies could have produced different datasets.

---

# 10. Transparency and Reproducibility

The repository separates the main components of the project so that the origin and role of each artefact remain clear.

The following files support reproducibility:

- [`README.md`](README.md) – project overview and non-technical explanation
- [`MODEL_CARD.md`](MODEL_CARD.md) – optimisation model documentation
- [`METHODOLOGY.md`](METHODOLOGY.md) – detailed technical methodology
- [`OPTIMISATION_HISTORY.md`](OPTIMISATION_HISTORY.md) – evolution of the optimisation strategy
- [`BBO_Optimisation.ipynb`](BBO_Optimisation.ipynb) – reproducible analysis
- [`data/queries.csv`](data/queries.csv) – Stage 2 submitted queries
- [`data/results.csv`](data/results.csv) – returned objective values
- `data/initial/` – original initial observations

Together, these artefacts document both the **data collected** and the **decision-making process that produced it**.

---

# 11. Dataset Status

**Status: Complete**

The dataset covers all **13 Stage 2 optimisation rounds from Modules 12–24**.

The optimisation phase has ended and the dataset is retained as the final record of the BBO capstone project.
