# Model Card

# Adaptive Sequential Optimisation Strategy (ASOS)

**Version:** 1.0

---

# Model Overview

## Summary

The Adaptive Sequential Optimisation Strategy (ASOS) was developed for the **Imperial College London Professional Certificate in Machine Learning and Artificial Intelligence** Black-Box Optimisation (BBO) Capstone Project.

The objective was to optimise eight unknown black-box functions over ten sequential optimisation rounds. At each round, a single query point was selected for each function using only the historical observations returned by the evaluation system.

Rather than implementing a complete Bayesian Optimisation framework, ASOS applies Bayesian Optimisation-inspired principles to guide sequential decision-making while maintaining a transparent and reproducible optimisation process.

---

# Intended Use

## Suitable applications

ASOS is suitable for:

- Educational demonstrations of sequential optimisation
- Black-box optimisation problems with limited observations
- Exploration versus exploitation studies
- Decision-making under uncertainty
- Sequential optimisation case studies

---

## Not suitable for

ASOS is not intended for:

- Production optimisation systems
- Large-scale industrial optimisation
- High-dimensional optimisation requiring thousands of evaluations
- Problems requiring guaranteed global convergence
- Fully automated Bayesian Optimisation workflows

---

# Optimisation Strategy

The optimisation process evolved throughout the ten optimisation rounds.

## Phase 1 – Initial Exploration (Rounds 1–3)

The initial rounds focused on exploring the search space and identifying promising regions for each objective function.

At this stage, relatively larger changes between query points were used because little information about the objective functions was available.

---

## Phase 2 – Balanced Search (Rounds 4–7)

As additional observations became available, the optimisation became increasingly function-specific.

Functions demonstrating consistent improvement received greater local refinement, while functions with unstable behaviour continued exploring nearby regions.

Each optimisation decision considered:

- Historical objective values
- Previous query locations
- Local improvement trends
- Evidence of convergence
- Remaining uncertainty

---

## Phase 3 – Local Refinement (Rounds 8–10)

The final rounds focused primarily on exploitation.

For functions showing stable convergence, only very small refinements were introduced.

Functions that continued exhibiting unpredictable behaviour retained limited exploration.

---

# Performance

Performance was evaluated using the objective values returned after each optimisation round.

Key observations include:

| Function | Outcome |
|----------|---------|
| F1 | Gradual convergence towards zero |
| F2 | Moderate improvement with some fluctuations |
| F3 | Stable performance with minor refinements |
| F4 | Consistent improvement across later rounds |
| F5 | Strongest overall improvement (approximately 893 → 1312) |
| F6 | Variable behaviour requiring continued exploration |
| F7 | Higher uncertainty due to increased dimensionality |
| F8 | Early convergence with diminishing returns |

The primary performance metric was the improvement in objective value across successive optimisation rounds.

---

# Assumptions

The optimisation strategy assumes:

- The objective functions exhibit some degree of local continuity.
- Neighbouring query points provide meaningful information.
- Historical observations improve future optimisation decisions.
- Limited observations can still reveal useful optimisation trends.

These assumptions guided the transition from exploration towards local refinement.

---

# Limitations

Several limitations should be recognised.

- Only a small number of observations were available.
- Large regions of the search space remain unexplored.
- Higher-dimensional functions increase optimisation uncertainty.
- The underlying objective functions remain unknown.
- The strategy may converge towards local rather than global optima.
- No explicit Gaussian Process surrogate model was implemented.

---

# Transparency and Reproducibility

A key objective of ASOS is transparency.

Every optimisation decision was informed by:

- Historical query points
- Objective values
- Observed optimisation trends
- Documented reasoning

The optimisation history has been recorded throughout the project to enable others to understand how each optimisation decision was reached.

---

# Ethical Considerations

Although this project does not involve human participants or personal data, transparency remains an important consideration.

Documenting assumptions, optimisation decisions and limitations supports:

- Reproducibility
- Interpretability
- Critical evaluation
- Future adaptation

Comprehensive documentation enables others to understand both the strengths and limitations of the optimisation strategy.

---

# Future Improvements

Potential future enhancements include:

- Implementation of Gaussian Process surrogate models
- Acquisition function optimisation
- Automated Bayesian Optimisation frameworks
- Comparison with alternative optimisation strategies
- Uncertainty quantification
- Performance benchmarking against established optimisation libraries

---

# Repository Documentation

Supporting documentation is provided in:

- **README.md** – Project overview
- **DATASHEET.md** – Dataset documentation
- **METHODOLOGY.md** – Detailed optimisation methodology
- **OPTIMISATION_HISTORY.md** – Complete optimisation history

Together these documents provide a transparent and reproducible record of the optimisation process.
