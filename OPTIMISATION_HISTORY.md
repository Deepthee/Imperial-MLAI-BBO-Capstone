# Optimisation History

# Adaptive Sequential Optimisation Strategy (ASOS)

## Overview

This document summarises the evolution of the optimisation strategy throughout the Imperial College Black-Box Optimisation (BBO) Capstone Project.

Rather than documenting every query submitted, this history focuses on how the optimisation strategy developed as additional observations became available. Each optimisation round built upon the evidence gathered from previous evaluations, allowing progressively more informed decisions under conditions of uncertainty.

---

# Evolution of the Strategy

## Initial Optimisation Rounds

The optimisation process began with only a limited number of observations for each of the eight objective functions.

At this stage, the primary objective was to understand the behaviour of each function by exploring different regions of the search space. Since little information was available, query selection favoured broader exploration over local refinement.

The early rounds established an initial understanding of:

- promising search regions;
- objective value trends;
- relative stability of each function; and
- differences between lower- and higher-dimensional optimisation problems.

---

## Intermediate Optimisation Rounds

As more observations became available, optimisation decisions became increasingly informed by historical evidence.

Rather than treating every function identically, each function began to develop its own optimisation strategy.

Functions showing consistent improvement gradually transitioned towards local refinement, while functions displaying greater variability continued to explore nearby regions.

During these rounds the optimisation process increasingly relied on:

- historical objective values;
- previous query locations;
- observed convergence trends; and
- remaining uncertainty within the search space.

This marked the transition from a general exploration strategy to function-specific optimisation.

---

## Later Optimisation Rounds

By the later optimisation rounds, clear differences had emerged between the objective functions.

Some functions demonstrated relatively stable convergence and required only minor adjustments to query locations.

Other functions remained more difficult to characterise due to their higher dimensionality or inconsistent optimisation behaviour.

Rather than applying a single optimisation rule, the strategy adapted to the behaviour of each function individually.

---

# Key Observations

Several important patterns emerged throughout the optimisation process.

### Function 1

Objective values steadily approached zero, suggesting that the optimisation strategy was successfully refining a promising region.

### Function 2

Performance fluctuated throughout the optimisation, demonstrating that local improvements were not always monotonic and occasionally required renewed exploration.

### Function 3

The function showed relatively stable behaviour with incremental improvements over successive rounds.

### Function 4

Consistent improvements were observed during later optimisation rounds as local refinement became more effective.

### Function 5

Function 5 demonstrated the strongest and most consistent improvement throughout the project.

Successive local refinements produced steady increases in the objective value, providing strong evidence that exploitation was appropriate for this function.

### Function 6

Despite periods of improvement, optimisation remained relatively unstable, suggesting that additional exploration continued to be valuable.

### Function 7

The higher dimensionality of this function increased optimisation uncertainty and limited confidence in local refinement.

### Function 8

Function 8 appeared to converge relatively early, with later optimisation rounds producing only small improvements, indicating diminishing returns.

---

# Lessons Learned

Several important lessons emerged from the optimisation process.

- Exploration remains essential during the early stages of optimisation.
- Exploitation becomes increasingly effective as confidence improves.
- Different optimisation problems require different search behaviours.
- Historical observations become progressively more valuable over time.
- Higher-dimensional optimisation problems generally require greater exploration.

Perhaps the most important lesson was that effective optimisation is an iterative learning process rather than a sequence of isolated decisions.

---

# Future Updates

This optimisation history will continue to be updated as additional optimisation rounds are completed during the capstone project.

Each new round provides additional evidence that further refines the optimisation strategy and improves understanding of the underlying objective functions.
