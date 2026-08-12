# Model Card

# Adaptive Sequential Optimisation Strategy (ASOS)

**Version:** 2.0  
**Project Status:** Final

---

# Model Overview

## Summary

The Adaptive Sequential Optimisation Strategy (ASOS) was developed for the **Imperial College London Professional Certificate in Machine Learning and Artificial Intelligence** Black-Box Optimisation (BBO) Capstone Project.

The objective was to maximise eight unknown black-box functions using a limited sequential evaluation budget.

Stage 2 consisted of **13 optimisation rounds from Modules 12 to 24**. In each round, one new query point was submitted for each of the eight functions. The returned objective values were then incorporated into the historical dataset and used to inform the following round.

The strategy was primarily based on **Bayesian optimisation principles**, using Gaussian Process (GP) surrogate modelling, uncertainty estimates, acquisition-function reasoning and historical observations to balance exploration and exploitation.

The approach evolved throughout the project rather than applying one fixed optimisation policy to all functions and rounds.

---

# Intended Use

## Suitable Applications

ASOS is suitable for:

- Educational demonstrations of sequential optimisation
- Black-box optimisation with limited observations
- Bayesian optimisation experiments
- Exploration versus exploitation studies
- Decision-making under uncertainty
- Surrogate-model-based optimisation
- Sequential optimisation case studies

---

## Not Suitable For

ASOS is not intended for:

- Safety-critical optimisation
- Production systems without further validation
- Problems requiring guaranteed global convergence
- Very large-scale optimisation without modifications
- Applications where the objective function changes rapidly over time
- Situations where surrogate-model assumptions cannot reasonably be justified

---

# Optimisation Strategy

The strategy evolved over the 13 Stage 2 rounds as additional observations became available.

## Phase 1 – Broad Exploration

### Early Rounds

The initial objective was to learn about the search spaces without committing too quickly to apparently promising regions.

At this stage, the limited number of observations meant that uncertainty was high.

Query decisions therefore placed relatively greater emphasis on:

- exploring different regions;
- identifying possible directional trends;
- understanding the scale of each objective function; and
- avoiding premature convergence.

---

## Phase 2 – Model-Guided Search

### Middle Rounds

As more observations became available, the strategy became increasingly structured and function-specific.

Gaussian Process surrogate modelling was used to estimate both:

- predicted objective values; and
- uncertainty in unexplored regions.

Acquisition-function concepts such as **Expected Improvement (EI)** and **Upper Confidence Bound (UCB)** provided a framework for balancing promising predictions against uncertainty.

Sobol candidate generation also provided structured coverage of multidimensional candidate spaces.

At this stage, decisions increasingly considered:

- GP predictions;
- predictive uncertainty;
- acquisition scores;
- previous high-performing observations;
- local improvement trends; and
- evidence of diminishing returns.

---

## Phase 3 – Function-Specific Refinement

As the history grew, it became clear that a single search behaviour was not appropriate for every function.

Some functions benefited from increasingly local exploitation, while others remained unstable enough to justify continued exploration.

Concepts introduced in later programme modules also influenced interpretation of the optimisation history.

These included:

- hyperparameter tuning;
- clustering and local grouping;
- PCA-inspired dimensional reasoning;
- prompt and model robustness;
- sequential decision-making; and
- reinforcement-learning concepts.

The purpose was not to replace Bayesian optimisation with each newly introduced technique, but to use these concepts to improve interpretation and decision-making.

---

## Phase 4 – Final Exploitation and Validation

### Final Rounds

As the remaining evaluation budget decreased, the value of broad exploration also decreased.

A newly discovered region near the end of the project would provide little opportunity for subsequent refinement.

The strategy therefore shifted towards **selective exploitation** of regions supported by accumulated evidence.

However, historical results remained important. Recent observations were not automatically considered more valuable than earlier strong observations.

This was particularly relevant for functions where repeated movement away from a previously successful region resulted in declining performance.

The final rounds therefore combined:

- exploitation of strong regions;
- limited uncertainty-driven exploration;
- backtracking where recent trends deteriorated;
- smaller local refinements where convergence appeared likely; and
- function-specific judgement.

---

# Core Technical Components

## Gaussian Process Surrogate

Gaussian Process regression was used as the primary surrogate-modelling framework.

GPs were appropriate because the BBO challenge involved:

- small datasets;
- expensive/limited evaluations;
- unknown objective functions; and
- a need to quantify predictive uncertainty.

A **Matérn-family kernel** was used to provide flexible modelling without assuming that the hidden functions were perfectly smooth.

---

## Expected Improvement

Expected Improvement was used as an acquisition concept for identifying candidates that offered a useful combination of predicted performance and uncertainty.

This helped formalise the exploration-exploitation trade-off.

---

## Upper Confidence Bound

Upper Confidence Bound was also considered as an alternative acquisition strategy.

UCB combines the surrogate prediction with predictive uncertainty, allowing the degree of exploration to be adjusted.

---

## Sobol Sampling

Sobol low-discrepancy sampling was used to generate structured candidate points across the bounded multidimensional search spaces.

This provided more systematic candidate coverage than purely random sampling.

---

# Performance

Performance was assessed primarily using the objective values returned by the external BBO evaluation system.

Because the true objective functions and global maxima were hidden, performance represents **observed optimisation progress rather than proof of global optimality**.

Key behaviours included:

| Function | Observed behaviour |
| --- | --- |
| F1 | Values moved extremely close to zero, with later refinements showing very small changes |
| F2 | Sensitive local behaviour; performance fluctuated and improved when returning towards previously successful regions |
| F3 | Relatively small changes with limited evidence of large late-stage gains |
| F4 | Challenging landscape with negative objective values and continued uncertainty |
| F5 | Strongest sustained improvement; local refinement eventually produced values above 1,300 |
| F6 | Variable behaviour; local exploitation did not consistently produce improvement |
| F7 | Late deterioration followed by strong recovery after moving back towards an earlier promising region |
| F8 | Converged around 9.57, with increasingly small changes suggesting diminishing returns |

Function 5 provided the clearest example of successful exploitation, while Functions 6 and 7 demonstrated why optimisation decisions should not assume that neighbouring queries will always improve performance.

---

# Evaluation Metrics

The primary metric was:

**Objective value returned by the black-box evaluator**

Additional diagnostic measures included:

- improvement relative to previous observations;
- best value observed so far;
- stability of results around local regions;
- predictive uncertainty;
- acquisition-function values; and
- evidence of diminishing returns.

Since the true global optima were unavailable, conventional error metrics against a known optimum could not be calculated.

---

# Assumptions

The optimisation strategy assumes that:

- the objective functions contain some exploitable structure;
- nearby observations can provide useful information about neighbouring regions;
- historical observations remain informative for future decisions;
- Gaussian Process uncertainty provides a useful approximation of uncertainty in the search space; and
- repeated strong performance within a region provides evidence that further refinement may be worthwhile.

These assumptions may not hold for highly discontinuous, chaotic or strongly non-stationary functions.

---

# Limitations

Several limitations should be recognised.

## Small Data

The number of observations remained small, particularly relative to the dimensionality of Functions 6–8.

## High Dimensionality

As dimensionality increased, the search space grew rapidly and visual interpretation became impractical.

## Surrogate Assumptions

Gaussian Process behaviour depends on choices such as:

- kernel family;
- length scales;
- noise assumptions; and
- acquisition parameters.

Different modelling choices could therefore generate different recommendations.

## Sequential Sampling Bias

Later queries became increasingly concentrated around regions believed to be promising.

The resulting dataset is therefore not a uniformly sampled representation of each function's complete search space.

## Local Optima

Increasing exploitation can produce convergence around a strong local region without discovering a better region elsewhere.

## Unknown Global Optimum

Because the hidden functions were not available, the project cannot demonstrate that any observed value represents the true global maximum.

---

# Transparency and Reproducibility

Transparency was an important part of the final project.

The repository records:

- original initial datasets;
- Stage 2 submitted query points;
- returned objective values;
- optimisation methodology;
- strategy evolution;
- data documentation;
- model documentation; and
- reproducible analysis code.

The Jupyter Notebook distinguishes between the **actual recorded capstone submissions** and a **representative reproducible implementation** of the final modelling methodology.

This distinction is important because the optimisation approach evolved during the project rather than being generated by one unchanged script across all 13 rounds.

---

# Ethical Considerations

The project does not contain personal or sensitive data, so the primary ethical considerations relate to transparency and responsible interpretation.

In particular:

- observed performance should not be presented as proof of global optimality;
- model assumptions should be documented;
- uncertainty should not be hidden;
- retrospective code should not be misrepresented as the exact historical implementation; and
- limitations should remain visible when adapting the approach to other problems.

These practices support reproducibility and responsible use of optimisation techniques.

---

# Future Improvements

Potential future enhancements include:

- systematic comparison of EI and UCB;
- automated acquisition-function hyperparameter tuning;
- alternative Gaussian Process kernels;
- automatic relevance determination for higher-dimensional functions;
- comparison with random-search and other baseline strategies;
- deep kernel learning where sufficient data is available;
- more systematic sensitivity analysis;
- improved visualisation of high-dimensional search behaviour; and
- benchmarking against established Bayesian optimisation libraries.

With a larger evaluation budget, a more deliberate initial space-filling design could also improve global coverage before transitioning towards exploitation.

---

# Key Learning

The project demonstrated that successful optimisation is not simply a matter of selecting the candidate with the highest model prediction.

Effective sequential optimisation requires balancing:

- predicted performance;
- uncertainty;
- historical evidence;
- remaining evaluation budget;
- dimensionality; and
- the behaviour of the individual objective function.

The strategy therefore evolved from broad exploration towards increasingly selective and function-specific exploitation.

---

# Repository Documentation

Supporting documentation is provided in:

- **README.md** – Project overview and non-technical summary
- **DATASHEET.md** – Dataset documentation
- **METHODOLOGY.md** – Detailed optimisation methodology
- **OPTIMISATION_HISTORY.md** – Evolution of the optimisation strategy
- **BBO_Optimisation.ipynb** – Reproducible analysis and representative optimisation implementation
- **data/queries.csv** – Stage 2 submitted queries
- **data/results.csv** – Corresponding objective values

Together, these resources provide a transparent record of the data, modelling approach, optimisation decisions, outcomes and limitations of the BBO capstone project.
