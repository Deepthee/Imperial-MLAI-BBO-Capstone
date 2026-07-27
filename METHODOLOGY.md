# Methodology

# Adaptive Sequential Optimisation Strategy (ASOS)

## Overview

This document describes the methodology used throughout the Black-Box Optimisation (BBO) Capstone Project.

The optimisation problem consists of eight unknown objective functions. At each optimisation round, a single query point is selected for each function based only on previously observed query points and their corresponding objective values. Since the analytical form of each function is unknown, the optimisation process relies entirely on iterative learning from historical observations.

The Adaptive Sequential Optimisation Strategy (ASOS) was developed to guide these decisions while maintaining a transparent and reproducible optimisation process.

---

# Optimisation Principles

The strategy is based on several key principles.

## Sequential Learning

Each optimisation round builds upon all previous observations.

Rather than treating each query independently, historical objective values and query locations are used to improve subsequent decisions.

As more information becomes available, confidence in the behaviour of each function gradually increases.

---

## Exploration versus Exploitation

A central objective throughout the project was balancing exploration and exploitation.

**Exploration** involves sampling regions where little information is available.

**Exploitation** focuses on refining regions that have already demonstrated promising objective values.

The balance between these two behaviours changes naturally as additional observations become available.

---

## Function-Specific Optimisation

Although every function follows the same optimisation framework, each develops its own optimisation behaviour.

Some functions exhibit consistent improvement and therefore benefit from increasingly local refinement.

Others continue to display uncertain behaviour and require broader exploration throughout the optimisation process.

Consequently, optimisation decisions become progressively more function-specific rather than applying a single global strategy.

---

# Optimisation Workflow

Each optimisation round follows the same general workflow.

1. Review all previous observations.
2. Analyse recent optimisation trends.
3. Identify regions demonstrating improvement.
4. Evaluate whether additional exploration remains necessary.
5. Select a single new query point for each function.
6. Submit the queries.
7. Analyse the returned objective values.
8. Update the optimisation strategy for the next round.

This iterative workflow continues throughout the capstone project.

---

# Evolution of the Strategy

The optimisation strategy evolved naturally as additional observations became available.

### Early optimisation rounds

The primary objective was understanding the behaviour of each objective function.

Larger movements through the search space were used to identify promising regions.

---

### Intermediate optimisation rounds

As more observations accumulated, optimisation decisions became increasingly informed by historical trends.

Functions demonstrating stable improvement transitioned towards local refinement, while uncertain functions continued exploring.

---

### Later optimisation rounds

For functions showing consistent convergence, only small adjustments to query points were required.

Functions with greater uncertainty retained a higher level of exploration to reduce the risk of converging prematurely on suboptimal regions.

---

# Decision-Making Process

Every optimisation decision considered multiple factors.

These included:

- Previous query locations
- Historical objective values
- Local improvement trends
- Evidence of convergence
- Remaining uncertainty
- Dimensionality of each optimisation problem

Rather than relying on a fixed mathematical rule, these factors were considered together to determine the most appropriate query for each function.

---

# Assumptions

The optimisation strategy assumes:

- neighbouring query points provide useful information;
- the objective functions exhibit some degree of local continuity;
- historical observations improve future decisions;
- meaningful optimisation can be achieved using a relatively small number of evaluations.

These assumptions support the gradual transition from exploration towards exploitation.

---

# Strengths

The methodology offers several advantages.

- Transparent decision-making.
- Reproducible optimisation process.
- Adaptable to different optimisation behaviours.
- Easy to interpret.
- Well suited to optimisation with limited observations.

---

# Limitations

Several limitations should also be recognised.

- The mathematical form of the objective functions remains unknown.
- The search space is only sparsely sampled.
- Higher-dimensional functions introduce greater uncertainty.
- Local refinement may overlook better regions elsewhere in the search space.
- The strategy does not implement a formal Gaussian Process surrogate model.

---

# Future Development

Future work could include:

- Gaussian Process surrogate modelling
- Acquisition function optimisation
- Automated Bayesian Optimisation frameworks
- Uncertainty estimation
- Comparison with alternative optimisation strategies

These enhancements would enable more rigorous optimisation while maintaining the transparent decision-making principles established throughout this project.
