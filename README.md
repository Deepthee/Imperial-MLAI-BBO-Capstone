# Imperial College Machine Learning & AI Black-Box Optimisation Capstone

## Overview

This repository contains my capstone project for the **Imperial College London Professional Certificate in Machine Learning and Artificial Intelligence**.

The project focuses on solving a sequential **Black-Box Optimisation (BBO)** problem involving eight unknown objective functions. Across ten optimisation rounds, I developed and refined an Adaptive Sequential Optimisation Strategy (ASOS), inspired by Bayesian Optimisation principles, to balance exploration and exploitation across sequential optimisation rounds.

Unlike traditional machine learning projects, the underlying objective functions remain unknown throughout the exercise. The optimisation process therefore required informed decision-making under uncertainty using progressively collected data.

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

```
Imperial-MLAI-BBO-Capstone/

README.md

docs/
├── DATASHEET.md
├── MODEL_CARD.md
├── METHODOLOGY.md
└── OPTIMISATION_HISTORY.md

data/
├── queries.csv
└── results.csv
```

---

## Documentation

This repository follows recognised machine learning documentation practices.

| Document | Description |
|----------|-------------|
| DATASHEET.md | Documentation describing the optimisation dataset |
| MODEL_CARD.md | Documentation describing the optimisation strategy |
| METHODOLOGY.md | Detailed optimisation methodology |
| OPTIMISATION_HISTORY.md | Summary of optimisation decisions across ten rounds |

---

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

## About Me

I am a Technology Delivery and SAP Transformation Leader with nearly two decades of experience delivering enterprise transformation programmes across utilities, manufacturing and cloud platforms.

Alongside my enterprise technology background, I am developing practical expertise in Machine Learning and Artificial Intelligence through the Imperial College London Professional Certificate programme, with a particular interest in applying AI to enterprise transformation, decision support and intelligent automation.

---

## Acknowledgements

This project was completed as part of the **Imperial College London Professional Certificate in Machine Learning and Artificial Intelligence**.

The optimisation problem and evaluation environment were provided as part of the programme's Black-Box Optimisation Capstone.
