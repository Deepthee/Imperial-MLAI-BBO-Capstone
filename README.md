# Imperial College Machine Learning & AI Black-Box Optimisation Capstone

## Overview

This repository documents my Black-Box Optimisation capstone project for the **Imperial College London Professional Certificate in Machine Learning and Artificial Intelligence**.

The project involves optimising eight unknown objective functions through successive rounds of sequential query selection. I developed an **Adaptive Sequential Optimisation Strategy (ASOS)**, inspired by Bayesian Optimisation principles, to balance exploration of uncertain regions with exploitation of promising areas.

The underlying mathematical functions remain unknown throughout the exercise. Each optimisation decision is therefore based on the query history, returned objective values and observed behaviour of each function.

---

## Project Objectives

The objectives of this project were to:

- Optimise eight unknown black-box objective functions using sequential query selection.
- Apply Bayesian Optimisation concepts to balance exploration and exploitation.
- Analyse optimisation behaviour as additional observations became available.
- Develop transparent and reproducible optimisation decisions.
- Document the project using industry-standard ML documentation practices, including Datasheets for Datasets and Model Cards.

---

## Optimisation Strategy

The optimisation strategy evolved throughout the ten rounds.

### Rounds 1–3
- Initial exploration of the search space.
- Identification of promising regions.

### Rounds 4–7
- Balanced exploration and local refinement.
- Function-specific optimisation decisions.

### Rounds 8–10
- Predominantly local exploitation while maintaining limited exploration where uncertainty remained.

The strategy was inspired by Bayesian Optimisation principles while remaining intentionally simple and transparent, allowing optimisation decisions to evolve as more observations became available.

---

## Results Summary

Some notable outcomes include:

- **Function 5** demonstrated consistent improvement throughout the optimisation, increasing from approximately **893** to over **1312**.
- **Function 1** steadily converged towards zero.
- **Function 8** showed clear signs of convergence with diminishing returns.
- Higher-dimensional functions highlighted the challenges associated with sparse sampling and limited observations.

The optimisation demonstrates how sequential decision-making can progressively improve solutions even when the underlying objective functions remain unknown.

---

## Repository Structure

```text
Imperial-MLAI-BBO-Capstone/
├── README.md
├── docs/
│   ├── DATASHEET.md
│   ├── MODEL_CARD.md
│   ├── METHODOLOGY.md
│   └── OPTIMISATION_HISTORY.md
└── data/
    ├── queries.csv
    └── results.csv
## Documentation

| Document | Description |
|----------|-------------|
| [Datasheet](docs/DATASHEET.md) | Describes the composition, collection, intended use and limitations of the optimisation dataset |
| [Model Card](docs/MODEL_CARD.md) | Documents the Adaptive Sequential Optimisation Strategy, its intended use, performance, assumptions and limitations |
| [Methodology](docs/METHODOLOGY.md) | Explains the sequential optimisation workflow and how the strategy evolved |
| [Optimisation History](docs/OPTIMISATION_HISTORY.md) | Summarises the observations and optimisation behaviour identified across the project |
---
> **Project status:** This repository is being updated as additional optimisation rounds are completed through the remaining capstone modules.


## Technologies

- Python
- NumPy
- Bayesian Optimisation concepts
- Git & GitHub
- Markdown documentation

---

## Key Learning Outcomes

This project provided practical experience in:

- Sequential optimisation
- Black-box optimisation
- Bayesian Optimisation concepts
- Exploration versus exploitation
- Optimisation under uncertainty
- Transparent machine learning documentation
- Reproducible experimentation

---

## Future Improvements

Potential future enhancements include:

- Implementing a full Gaussian Process surrogate model.
- Comparing different acquisition functions.
- Benchmarking against established Bayesian Optimisation libraries.
- Extending the optimisation framework to larger and higher-dimensional search spaces.

---


## Acknowledgements

This project was completed as part of the **Imperial College London Professional Certificate in Machine Learning and Artificial Intelligence**.

The optimisation problem and evaluation environment were provided as part of the programme's Black-Box Optimisation Capstone.
