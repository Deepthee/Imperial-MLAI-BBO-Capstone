# Optimisation History

## Black-Box Optimisation Capstone Project

This document records the evolution of my optimisation strategy throughout Stage 2 of the Black-Box Optimisation (BBO) capstone project.

The challenge involved eight unknown objective functions with dimensionalities ranging from two to eight variables. Each function could only be evaluated by submitting a query and receiving the corresponding output.

The optimisation therefore followed a sequential process:

**submit query → observe result → update understanding → refine strategy → submit next query**

Rather than using one fixed strategy throughout the project, I adapted the approach as more observations became available.

---

# 1. Initial Strategy

At the beginning of Stage 2, very few observations were available for each function.

The priority was therefore **exploration rather than aggressive optimisation**.

My initial objectives were to:

* understand the approximate scale of each function;
* identify potentially promising regions;
* avoid concentrating queries too early;
* use visualisation where dimensionality allowed it;
* establish simple modelling baselines;
* preserve enough diversity in the observations to support later surrogate modelling.

For the two-dimensional functions, visualisation was particularly useful because relationships between inputs and outputs could be inspected directly.

For the higher-dimensional functions, this quickly became impractical and greater reliance had to be placed on statistical modelling.

---

# 2. Introduction of Gaussian Process Modelling

As additional observations became available, I increasingly used **Gaussian Process (GP) regression** as the primary surrogate modelling framework.

GPs were attractive for this challenge because the data sets remained extremely small.

Unlike a conventional regression prediction alone, a GP provides both:

* a predictive mean; and
* predictive uncertainty.

This made it possible to reason not only about where the function might have a high value, but also about where the model remained uncertain.

A Matérn-based kernel was used as the main modelling foundation, with noise handling considered where appropriate.

This became the core of the optimisation workflow for much of the project.

---

# 3. Acquisition-Based Query Selection

Once surrogate modelling was established, acquisition functions were used to guide candidate selection.

The two main ideas considered were:

## Expected Improvement

Expected Improvement (EI) evaluates candidate points according to their potential to improve on the best value observed so far.

The parameter ( \xi ) provided a mechanism for controlling how aggressively the strategy explored alternatives rather than concentrating only on the predicted optimum.

## Upper Confidence Bound

Upper Confidence Bound (UCB) combines predicted performance and uncertainty.

The exploration parameter ( \beta ) controls the relative importance placed on uncertainty.

Higher values encourage exploration, while lower values favour exploitation.

These parameters were not treated as universally optimal settings. Their usefulness depended on the function, the observations available and the stage of the optimisation process.

---

# 4. Candidate Generation

Because exhaustive search of the continuous input spaces was impossible, candidate points were generated computationally.

Sobol sequences were useful because they provide more systematic coverage of a multidimensional search space than purely independent random sampling.

The candidate-generation process could therefore be summarised as:

1. generate candidate points across the valid search domain;
2. predict their values using the surrogate model;
3. estimate uncertainty;
4. calculate the acquisition score;
5. inspect the strongest candidates;
6. select the next query.

This provided a repeatable framework while still allowing judgement to be applied when the observed behaviour of a function suggested that the model should not be followed blindly.

---

# 5. Moving Beyond a Single Strategy

One of the most important developments during the project was recognising that the eight functions should not necessarily be treated identically.

Some functions responded well to local refinement.

Others were much more sensitive to relatively small changes in their inputs.

As a result, the later optimisation rounds became increasingly **function-specific**.

Instead of asking:

> What is the best optimisation strategy for all eight functions?

the more useful question became:

> Given everything observed for this function, what type of query provides the most useful next experiment?

This represented an important shift from algorithm-driven optimisation towards evidence-driven optimisation.

---

# 6. Neural Networks and Model Complexity

As the programme introduced neural networks, I considered whether a neural surrogate could improve the optimisation.

Neural networks offer substantial flexibility and can represent complex nonlinear relationships. However, the number of observations available in the BBO challenge remained very small.

This created a significant overfitting risk.

A highly flexible neural network could fit the observed points extremely well without learning a useful approximation of the underlying function.

For this reason, neural networks were treated primarily as an exploratory modelling option rather than replacing the GP as the main optimisation surrogate.

This reinforced an important principle followed throughout the project:

**model complexity should be justified by the available evidence.**

---

# 7. Hyperparameter Tuning

Later modules encouraged more explicit consideration of hyperparameter tuning.

For the GP-based optimisation framework, important choices included:

* kernel configuration;
* kernel length scales;
* noise assumptions;
* EI exploration parameter ( \xi );
* UCB exploration parameter ( \beta );
* number and distribution of candidate points.

Rather than performing very large searches over these settings, tuning remained relatively conservative because of the extremely small amount of data available.

The goal was not to optimise the surrogate perfectly against the existing observations, as this could itself lead to overfitting.

Instead, tuning was used to understand how sensitive the next-query recommendation was to modelling assumptions.

---

# 8. Clustering Perspective

As more observations accumulated, I began considering the search space from a clustering perspective.

Repeated strong observations within nearby regions could indicate a **promising local cluster**, while isolated observations might represent either genuinely interesting areas or noise.

This perspective encouraged me to consider:

* distances between successful queries;
* whether several strong results occupied similar regions;
* whether a new query tightened the boundary of a promising region;
* whether repeated sampling was becoming redundant.

The clustering perspective did not replace Bayesian optimisation. Instead, it provided another way of interpreting what the accumulated observations were showing.

---

# 9. PCA-Inspired Reasoning

Principal Component Analysis introduced another useful conceptual perspective.

In higher-dimensional functions, not every input direction necessarily contributes equally to variation in the output.

Although PCA was not used to directly transform every BBO function, its underlying principles influenced the optimisation strategy.

In particular, I began asking:

* Which dimensions appear to matter most?
* Which changes repeatedly produce little improvement?
* Are new queries adding information or repeating existing observations?
* Can attention be concentrated on the directions that appear to drive the greatest variation?

This became increasingly important for the higher-dimensional functions, where intuitive visualisation was impossible.

---

# 10. Exploration Versus Exploitation

The exploration–exploitation balance changed substantially during the project.

## Earlier rounds

Exploration had high value because information collected early could influence many subsequent decisions.

A query that did not immediately produce a strong result could still be useful if it revealed something important about the response surface.

## Middle rounds

The strategy became more balanced.

Promising regions were refined, but uncertainty and unexplored regions were still considered.

## Final rounds

The value of broad exploration decreased because fewer future evaluations remained.

The strategy therefore shifted increasingly towards exploitation.

This did not mean blindly selecting the latest high-performing region. Historical results remained important, particularly when recent movements produced deteriorating outputs.

---

# 11. Function-Specific Observations

## Function 1

Function 1 produced extremely small values in the later stages.

The final two recorded values illustrate that even changes producing an order-of-magnitude improvement could still leave the absolute objective value extremely small.

Week 12 produced approximately:

`1.84645 × 10^-9`

while the final round produced approximately:

`1.81250 × 10^-8`

The function remained difficult to optimise confidently despite the relative improvement.

---

## Function 2

Function 2 demonstrated considerable sensitivity to relatively small changes in its two inputs.

The Week 12 query:

`[0.692000, 0.722000]` was for Function 1, while Function 2 used:

`[0.739000, 0.747000]`

and returned approximately:

`0.274540`

In the final round, the strategy returned towards a previously stronger neighbourhood.

The final output recovered to:

**0.543109**

This reinforced the importance of retaining historical evidence rather than assuming that the most recent direction should always be continued.

---

## Function 3

Function 3 remained challenging during the final stages.

Week 12 returned:

`-0.034936`

while the final round improved this to approximately:

`-0.023874`

The improvement was useful, but the remaining uncertainty made it difficult to conclude that the underlying optimum had been located.

---

## Function 4

Function 4 also showed relatively difficult local behaviour.

Week 12 produced approximately:

`-3.086528`

and the final round improved this to:

`-3.055181`.

This demonstrated modest improvement but also illustrated the limited gains available from late-stage refinement when the function structure remained uncertain.

---

## Function 5

Function 5 became the clearest example of successful exploitation.

By Week 12, the selected query:

`[0.239000, 0.856000, 0.900000, 0.900000]`

returned:

**1348.847228**

The evidence suggested that the current region remained productive, so the final strategy continued refining rather than abandoning it for broad exploration.

The final round produced:

**1368.742373**

This was a strong validation of the decision to exploit the established region late in the challenge.

Function 5 therefore became one of the clearest examples of how accumulated observations could progressively narrow the search towards stronger solutions.

---

## Function 6

Function 6 demonstrated the danger of assuming that a promising trend would necessarily continue.

Week 12 returned approximately:

`-0.544650`

while the final result was approximately:

`-0.563543`.

The deterioration reinforced the importance of uncertainty and showed that local exploitation does not guarantee improvement.

---

## Function 7

Function 7 provided another important lesson.

Week 12 returned:

`1.263109`.

Rather than continuing indefinitely in the same direction, the final strategy moved back towards an earlier promising region.

The final output increased to:

**1.346625**

This was one of the clearest demonstrations that historical observations should remain part of the decision process.

The most recent point is not necessarily the most informative starting point for the next query.

---

## Function 8

Function 8 showed increasingly small changes during the later stages.

The Week 12 query:

`[0.036900, 0.036900, 0.036900, 0.046900, 0.316900, 0.816900, 0.466900, 0.956900]`

returned:

`9.570285334`.

The final result was:

**9.570335**

The extremely small improvement suggested that the optimisation had reached a region of diminishing returns.

With unlimited evaluations, further exploration might still have been justified. With only one final query available, however, preserving the established high-performing region was a reasonable decision.

---

# 12. Final Round Results

The final outputs were:

| Function   |           Final Output |
| ---------- | ---------------------: |
| Function 1 | 1.8124950340224335e-08 |
| Function 2 |     0.5431086878575929 |
| Function 3 |   -0.02387411493618878 |
| Function 4 |    -3.0551805858271517 |
| Function 5 |     1368.7423730157743 |
| Function 6 |    -0.5635434233499999 |
| Function 7 |     1.3466248033244974 |
| Function 8 |               9.570335 |

The final round illustrates why a single optimisation rule would have been inappropriate.

Some functions benefited from continued exploitation, some benefited from returning towards earlier promising regions and others showed evidence of diminishing returns.

---

# 13. What Changed Most During the Project?

The largest change was not a particular kernel, acquisition function or hyperparameter.

It was the way I interpreted the optimisation problem.

At the beginning, the focus was primarily on finding an algorithm capable of identifying the next query.

By the end, the focus had shifted towards combining:

* surrogate predictions;
* uncertainty;
* historical performance;
* dimensionality;
* local patterns;
* remaining evaluation budget;
* and the behaviour of each individual function.

The optimisation process therefore became increasingly adaptive rather than purely algorithmic.

---

# 14. Key Lessons

The project produced several important lessons.

### Exploration has a time-dependent value

Exploration early in a sequential optimisation problem can influence many later decisions. The same exploration performed in the final round has much less opportunity to generate future value.

### The latest result should not erase earlier evidence

Functions 2 and 7 demonstrated the value of returning towards previously successful regions when more recent movements reduced performance.

### Local exploitation can be extremely effective

Function 5 demonstrated how repeatedly refining a promising region can produce substantial gains.

### Plateaus matter

Function 8 illustrated diminishing returns. Recognising a plateau can be as important as identifying an improving trend.

### Complexity must earn its place

More sophisticated models are not automatically better when observations are scarce.

### High-dimensional optimisation requires greater reliance on modelling

Visual intuition becomes increasingly unreliable as dimensionality grows.

### Uncertainty is part of the result

A prediction without an understanding of its uncertainty can create false confidence.

---

# 15. Reproducibility

The complete numerical query history is stored separately in:

* `data/inputs.csv`
* `data/outputs.csv`

The core optimisation workflow is documented in:

* `notebooks/BBO_Optimisation.ipynb`

Technical modelling decisions are described in:

* `METHODOLOGY.md`

Context, intended use and limitations are documented in:

* `DATASHEET.md`
* `MODEL_CARD.md`

Separating the numerical data, implementation and narrative history keeps the repository readable while allowing the optimisation process to be independently examined.

---

# 16. Final Reflection

The BBO challenge demonstrated that optimisation under uncertainty is fundamentally a process of **sequential decision-making**.

Each query changed what was known about the problem and therefore changed the value of the available future choices.

The most useful strategy was not to commit permanently to exploration or exploitation, nor to continually replace simpler models with more complex ones. Instead, the process became increasingly responsive to the evidence produced by each function.

By the final rounds, the objective was no longer simply to find a mathematically attractive acquisition score. It was to make a defensible decision using the model, uncertainty, historical observations and the limited number of opportunities remaining.

That shift from selecting an algorithm to managing evidence and uncertainty is one of the most important lessons I take from the capstone project.
