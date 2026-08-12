# Methodology

## Black-Box Optimisation Capstone Project

## 1. Methodology Overview

The Black-Box Optimisation (BBO) capstone project required the optimisation of eight unknown objective functions using a limited number of sequential evaluations.

For each function, the analytical form was hidden. The only information available was:

- the dimensionality of the function;
- previously submitted input vectors;
- the corresponding observed outputs; and
- limited descriptive information supplied as part of the challenge.

The optimisation problem can be expressed as:

\[
x^* = \arg\max_{x \in [0,1]^d} f(x)
\]

where \(f(x)\) is unknown and can only be evaluated through submitted queries.

Because exhaustive search was impossible, the project required a strategy that could learn from a small number of observations and decide where to evaluate next.

My methodology therefore centred on **Bayesian optimisation using Gaussian Process surrogate modelling**, supported by exploratory analysis, structured candidate generation and function-specific judgement.

---

## 2. Why Bayesian Optimisation?

Bayesian optimisation is particularly suitable when:

- the objective function is unknown;
- evaluations are limited or expensive;
- derivatives are unavailable;
- relatively few observations exist;
- uncertainty about unexplored regions matters.

These characteristics closely matched the BBO challenge.

Instead of attempting to directly optimise the unknown function, Bayesian optimisation builds a surrogate approximation from the observations collected so far.

The process used throughout the project can be represented as:

1. collect existing input-output observations;
2. fit or update a surrogate model;
3. generate potential candidate points;
4. predict candidate performance and uncertainty;
5. calculate an acquisition score;
6. inspect the strongest candidates;
7. submit a new query;
8. observe the returned function value;
9. update the data set;
10. repeat.

This created a sequential learning process in which every new observation influenced subsequent decisions.

---

## 3. Gaussian Process Surrogate

Gaussian Process (GP) regression formed the main surrogate-modelling foundation.

For observed data:

\[
D = \{(x_i,y_i)\}_{i=1}^{n}
\]

where:

\[
y_i = f(x_i)
\]

the GP provides a predictive distribution for an unseen candidate \(x\):

\[
f(x) \sim \mathcal{N}(\mu(x),\sigma^2(x))
\]

where:

- \(\mu(x)\) represents the predicted objective value;
- \(\sigma(x)\) represents predictive uncertainty.

This distinction was important.

A candidate with a high predicted value could be attractive for exploitation, while a candidate with greater uncertainty could be useful for exploration.

This made GP regression more useful for the project than relying only on a point-prediction regression model.

---

## 4. Kernel Selection

A Matérn-family kernel was used as the main GP modelling choice during the project.

The Matérn kernel was appropriate because it allows less restrictive assumptions about smoothness than a very smooth kernel such as the RBF kernel.

Conceptually, the kernel determines how observations influence predictions at other locations in the search space.

Kernel length scales also provide a useful interpretation of sensitivity:

- shorter length scales imply faster variation;
- longer length scales imply smoother behaviour.

Noise handling was considered where appropriate because some function behaviour could not confidently be distinguished from local irregularity using the limited observations available.

The precise kernel configuration was treated as a modelling choice rather than an assumption that all eight functions shared identical behaviour.

---

## 5. Exploration and Exploitation

A central problem throughout the project was deciding whether to:

**exploit** a region already associated with strong performance,

or

**explore** a region where the surrogate remained uncertain.

The preferred balance changed as the project progressed.

### Early stage

Exploration received greater weight because observations were sparse.

Information collected early could influence many later queries.

### Middle stage

The strategy became more balanced as promising regions began to emerge.

Surrogate predictions, uncertainty and previous performance were considered together.

### Late stage

Exploitation became increasingly important because only a small number of evaluations remained.

At this point, broad exploration had less opportunity to generate useful future decisions.

The remaining evaluation budget therefore became an implicit part of the optimisation strategy.

---

## 6. Expected Improvement

Expected Improvement (EI) was one of the acquisition strategies considered during the project.

For maximisation, EI measures the expected improvement of a candidate over the best objective value observed so far.

Conceptually:

\[
EI(x) =
E[\max(f(x)-f(x^+)-\xi,0)]
\]

where:

- \(f(x^+)\) is the current best observed value;
- \(\xi\) controls the exploration-exploitation tendency.

A small value of \(\xi\) favours refinement around promising areas, while increasing it can encourage broader exploration.

EI was useful because it considers both the predicted value and uncertainty rather than selecting candidates using the GP mean alone.

---

## 7. Upper Confidence Bound

Upper Confidence Bound (UCB) provided another way of combining prediction and uncertainty.

The acquisition principle can be expressed as:

\[
UCB(x)=\mu(x)+\beta\sigma(x)
\]

where:

- \(\mu(x)\) is the predicted objective value;
- \(\sigma(x)\) represents uncertainty;
- \(\beta\) controls the importance of exploration.

Increasing \(\beta\) places greater emphasis on uncertain regions.

Reducing it favours locations with stronger predicted performance.

The important lesson was that the exploration parameter should not necessarily remain constant throughout a limited-budget sequential optimisation problem.

---

## 8. Candidate Generation

The objective functions operated over continuous multidimensional spaces, making exhaustive evaluation impossible.

Candidate points therefore had to be generated before applying the acquisition function.

Sobol sequences were used as a structured candidate-generation mechanism.

Sobol sampling provides low-discrepancy coverage of a multidimensional domain and was preferable to relying entirely on independent random points.

A simplified version of the process was:

```python
from scipy.stats import qmc

sampler = qmc.Sobol(d=n_dimensions, scramble=True)
candidates = sampler.random_base2(m=12)

The GP surrogate could then evaluate these candidates computationally without requiring actual black-box evaluations.

The acquisition function ranked the candidates, after which the most appropriate point could be considered for submission.

9. Simplified Query-Selection Workflow

A representative implementation is:

import numpy as np
from scipy.stats import norm
from sklearn.gaussian_process import GaussianProcessRegressor
from sklearn.gaussian_process.kernels import Matern

kernel = Matern(nu=1.5)

gp = GaussianProcessRegressor(
    kernel=kernel,
    normalize_y=True,
    n_restarts_optimizer=10,
    random_state=42
)

gp.fit(X, y)

mu, sigma = gp.predict(candidates, return_std=True)

best_observed = np.max(y)

improvement = mu - best_observed - xi

Z = np.divide(
    improvement,
    sigma,
    out=np.zeros_like(improvement),
    where=sigma > 0
)

ei = improvement * norm.cdf(Z) + sigma * norm.pdf(Z)
ei[sigma == 0] = 0

next_query = candidates[np.argmax(ei)]

This represents the core model-based reasoning used in the project.

The exact strategy could vary according to the function and optimisation stage.

10. Why Not Rely Only on the Model?

The GP recommendation was treated as evidence rather than an unquestionable answer.

With such small data sets, surrogate predictions can be highly sensitive to:

kernel selection;
length scales;
noise assumptions;
acquisition parameters;
candidate-generation density;
isolated observations.

Historical results were therefore retained when interpreting recommendations.

This became particularly important during later rounds when some functions deteriorated after moving away from previously successful regions.

The final query decision therefore combined quantitative modelling with accumulated empirical evidence.

11. Alternative Models Considered
Linear Regression

Linear regression provided a simple baseline and a way of examining whether individual dimensions appeared to have strong linear relationships with the objective.

Its main advantage was interpretability.

However, the unknown objective functions could contain substantial nonlinear structure, limiting the usefulness of a purely linear surrogate.

Support Vector Machines

SVM concepts were useful when considering observations near boundaries between relatively strong and weak regions.

Thinking about "support-vector-like" observations encouraged attention to points where relatively small changes in inputs appeared to produce substantial changes in output.

SVMs were therefore useful conceptually, although they did not replace GP regression as the primary surrogate.

Neural Networks

Neural networks were considered because of their ability to represent complex nonlinear interactions.

However, the BBO data remained extremely small.

A neural network introduces many parameters and therefore carries a substantial risk of fitting the existing observations without generalising effectively to unexplored locations.

For this reason, neural-network ideas informed parts of the analysis but did not replace the GP-based optimisation framework.

12. Hyperparameter Tuning

Hyperparameter tuning was approached conservatively.

Relevant parameters included:

GP kernel configuration;
kernel length scales;
noise assumptions;
EI parameter ξ;
UCB exploration parameter β;
candidate sample size.

With very small data sets, aggressive hyperparameter optimisation can produce misleading confidence because the model may simply become highly adapted to the existing observations.

Consequently, tuning was used primarily to investigate the stability of recommendations rather than to maximise conventional predictive performance.

This was an important distinction between standard supervised learning and the BBO problem.

13. Clustering-Inspired Analysis

As observations accumulated, I also considered whether successful queries formed local groups or clusters.

This provided another way to reason about:

recurring promising regions;
distances between high-performing points;
whether a query represented local refinement or genuine exploration;
whether repeated observations were becoming redundant.

The clustering perspective complemented Bayesian optimisation rather than replacing it.

It helped translate a collection of individual observations into regions of interest.

14. PCA-Inspired Analysis

PCA concepts influenced how I thought about higher-dimensional functions.

As dimensionality increased, the search space expanded rapidly and visual interpretation became impractical.

The PCA principle of preserving important variation while reducing redundancy encouraged me to consider whether:

certain dimensions appeared more influential;
repeated movements along some dimensions added little information;
attention could be concentrated on directions associated with larger changes in output.

PCA was therefore primarily used as a conceptual framework for interpreting dimensional relevance rather than as a universal preprocessing transformation.

15. Reinforcement-Learning Perspective

The final stage of the project introduced a useful reinforcement-learning interpretation.

Each submitted query can be viewed as an action.

The resulting objective value provides feedback.

That feedback changes expectations about the usefulness of different regions of the search space.

The next query is then selected using the updated information.

This resembles the exploration-exploitation problem encountered in multi-armed bandits and reinforcement learning.

Early rounds involved greater exploration, while later rounds increasingly exploited strategies and regions that had demonstrated stronger rewards.

The analogy also highlights why unsuccessful evaluations were still useful: they changed the information available for future decisions.

16. Function-Specific Decision Making

By the later rounds, I deliberately avoided applying one identical policy to every function.

For example:

Function 5 responded well to local refinement, supporting continued exploitation.
Function 2 showed sensitivity to relatively small input changes, requiring greater caution.
Function 7 demonstrated the value of returning towards an earlier strong region after later deterioration.
Function 8 showed diminishing improvements, suggesting a late-stage plateau.

These differences reinforced the idea that optimisation strategies should respond to observed function behaviour rather than follow a rigid global rule.

17. Tools and Libraries

The implementation uses the Python scientific computing ecosystem.

Primary libraries include:

numpy
pandas
scipy
scikit-learn
matplotlib
jupyter

Their main roles are:

Library	Purpose
NumPy	Numerical operations and arrays
Pandas	Query and result data management
SciPy	Statistical functions and Sobol sampling
scikit-learn	Gaussian Process and baseline ML models
Matplotlib	Visualisation
Jupyter	Reproducible analysis and documentation

The relatively lightweight stack was appropriate for the scale of the project and avoided introducing unnecessary framework complexity.

18. Reproducibility

The repository separates the project into:

Data

data/inputs.csv

data/outputs.csv

These files record the observed query history.

Analysis

notebooks/BBO_Optimisation.ipynb

This notebook demonstrates the modelling and query-selection workflow.

Documentation

README.md

DATASHEET.md

MODEL_CARD.md

METHODOLOGY.md

OPTIMISATION_HISTORY.md

Together these files explain not only what was done but also why the decisions were made.

The hidden objective functions themselves cannot be reproduced because they were evaluated externally through the capstone platform.

19. Limitations

Several limitations should be considered when interpreting the methodology.

Limited observations

Even at the end of the challenge, the number of observations remained extremely small for learning complex multidimensional functions.

Curse of dimensionality

As dimensionality increased, observations represented an increasingly tiny fraction of the possible search space.

Surrogate assumptions

GP modelling assumes sufficient regularity for relationships between observations to provide useful predictions.

Highly discontinuous or irregular objective functions could violate these assumptions.

Hyperparameter sensitivity

Different kernels or acquisition settings can produce different recommendations.

No guarantee of global optimality

The strongest observed result cannot be assumed to be the true global maximum.

Sequential sampling bias

Later observations were deliberately concentrated around regions considered promising, meaning the final data set is not a uniformly sampled representation of the complete search space.

20. Methodological Lessons

The project reinforced several broader principles.

Start simple.
Model complexity should increase only when the available data supports it.
Treat uncertainty as information.
Knowing where a model is uncertain can be as valuable as knowing where it predicts a strong outcome.
Adapt exploration over time.
The value of exploration decreases as the remaining evaluation budget shrinks.
Preserve historical evidence.
Recent observations should not automatically override earlier strong results.
Use models to support decisions, not replace judgement.
Surrogate recommendations are conditional on modelling assumptions.
Avoid unnecessary experimentation.
With limited evaluations, every query has an opportunity cost.
Different problems may require different policies.
A strategy that works well for one objective function may perform poorly for another.
21. Conclusion

The final methodology was a small-data, uncertainty-aware and adaptive optimisation framework centred on Gaussian Process surrogate modelling and Bayesian optimisation.

The technical approach evolved as additional observations became available, but the central principle remained consistent: use every evaluation to improve the quality of the next decision.

Concepts from neural networks, hyperparameter tuning, clustering, PCA and reinforcement learning added different perspectives to the problem, but they did not justify replacing a method that remained appropriate for the available data.

The most important methodological development was therefore not increasing model complexity. It was learning when to explore, when to exploit, when to trust the surrogate and when the historical evidence justified challenging its recommendation.
