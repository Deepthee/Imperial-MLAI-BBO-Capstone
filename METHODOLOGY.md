# Methodology

## Black-Box Optimisation Capstone Project

This document describes the technical approach used throughout the Black-Box Optimisation (BBO) capstone project and explains the reasoning behind the main modelling and query-selection decisions.

---

## 1. Problem Definition

The capstone project involved optimising eight unknown objective functions with different dimensionalities.

The analytical form of each function was hidden. For each function, I only had access to:

- previously submitted input values;
- the corresponding returned outputs;
- the dimensionality of the function; and
- limited descriptions provided as part of the challenge.

The objective was to identify input combinations that maximised each function while working with a very limited number of evaluations.

This made the problem suitable for a sequential black-box optimisation approach:

**Observe → Model → Select Query → Evaluate → Update**

Each new result therefore became additional evidence for deciding where to search next.

---

## 2. Core Optimisation Approach

My main approach was based on **Bayesian optimisation using Gaussian Process (GP) surrogate modelling**.

Bayesian optimisation was appropriate because:

- the objective functions were unknown;
- gradients were unavailable;
- the number of evaluations was limited;
- the datasets remained small;
- exhaustive search was impossible; and
- uncertainty about unexplored regions was important.

Instead of attempting to optimise the hidden function directly, I used the observations collected so far to approximate its behaviour.

The general workflow was:

1. Load the available input-output observations.
2. Analyse the existing results.
3. Fit or update a surrogate model.
4. Generate candidate points.
5. Predict candidate performance and uncertainty.
6. Use an acquisition strategy to rank candidates.
7. Review the recommendation against historical results.
8. Submit the selected query.
9. Add the returned result to the dataset.
10. Repeat the process in the next round.

---

## 3. Gaussian Process Surrogate Modelling

Gaussian Process regression formed the main modelling foundation of the project.

For an unseen candidate, a GP provides both:

- a **predicted mean**, representing the expected function value; and
- a **predictive uncertainty**, representing how confident the model is in that prediction.

This was particularly useful for black-box optimisation.

A candidate with a high predicted value could be selected for **exploitation**, while a candidate with greater uncertainty could be considered for **exploration**.

This ability to model uncertainty was one of the main reasons I preferred Gaussian Processes over conventional regression models.

---

## 4. Kernel Selection

A **Matérn-family kernel** was used as the main GP kernel.

The Matérn kernel was suitable because it can represent functions that are not perfectly smooth and therefore makes less restrictive smoothness assumptions than some alternatives such as the RBF kernel.

Kernel length scales also provided a useful way of thinking about sensitivity:

- shorter length scales suggest that relatively small changes in an input may produce substantial changes in output;
- longer length scales suggest smoother behaviour.

Noise handling was considered where appropriate because, with such limited data, it was not always possible to distinguish genuine local variation from noise.

---

## 5. Exploration vs Exploitation

The exploration-exploitation trade-off was central to the project.

**Exploration** involved testing less-understood areas of the search space.

**Exploitation** involved refining areas that had already produced promising results.

The balance changed as the project progressed.

### Early rounds

I placed greater emphasis on exploration because very little was known about the functions.

An exploratory query early in the project could provide information that influenced many subsequent decisions.

### Middle rounds

As more observations became available, the strategy became more balanced.

Promising regions could be refined while uncertainty was still used to identify potentially valuable unexplored areas.

### Final rounds

The strategy moved increasingly towards exploitation.

With only a few evaluations remaining, broad exploration had less opportunity to produce information that could subsequently be exploited.

The remaining query budget therefore became an important part of the decision-making process.

---

## 6. Expected Improvement

**Expected Improvement (EI)** was one of the acquisition strategies considered during the project.

EI evaluates potential query points according to how much improvement they may provide over the best result observed so far.

The exploration parameter `xi` influences how aggressively the acquisition function searches beyond the currently promising region.

A smaller value generally encourages greater exploitation, while increasing it can encourage additional exploration.

EI was useful because it considered both predicted performance and uncertainty rather than simply choosing the point with the highest GP prediction.

---

## 7. Upper Confidence Bound

**Upper Confidence Bound (UCB)** provided another mechanism for balancing exploration and exploitation.

Conceptually:

`UCB = predicted mean + beta × predictive uncertainty`

The parameter `beta` determines how much importance is placed on uncertainty.

- Higher `beta` values encourage exploration.
- Lower `beta` values favour exploitation.

Rather than assuming one exploration setting was appropriate throughout the project, I considered the stage of the optimisation and the behaviour of each function when deciding how much exploration was justified.

---

## 8. Candidate Generation

Because the input spaces were continuous, it was impossible to evaluate every possible input combination.

Candidate points therefore had to be generated computationally.

I used **Sobol sampling** as a structured method for generating candidate points across the search space.

Sobol sequences provide relatively even coverage of multidimensional spaces and can therefore be more useful than relying entirely on independent random samples.

A representative implementation is:

    from scipy.stats import qmc

    sampler = qmc.Sobol(d=n_dimensions, scramble=True)
    candidates = sampler.random_base2(m=12)

The surrogate model could evaluate these candidates without requiring actual black-box evaluations.

The acquisition strategy was then used to identify promising candidates for the next real submission.

---

## 9. Representative Query-Selection Workflow

The following illustrates the core GP and Expected Improvement approach used during the project:

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

This represents the main model-based reasoning rather than an exact record of every historical query.

Individual rounds were also influenced by the accumulated behaviour of each function.

---

## 10. Model Recommendations and Human Judgement

I did not treat the surrogate model's recommendation as an unquestionable answer.

With very small datasets, GP predictions can be sensitive to:

- kernel choice;
- kernel length scales;
- assumptions about noise;
- acquisition-function parameters;
- candidate-generation density; and
- individual observations.

For this reason, I compared model recommendations with the historical behaviour of each function.

This became particularly important during the later rounds.

If movement away from a previously successful region repeatedly reduced performance, historical evidence could justify returning towards the earlier region rather than continuing to follow the latest direction.

The final strategy therefore combined **model-based recommendations with evidence from previous evaluations**.

---

## 11. Alternative Models Considered

### Linear Regression

Linear regression was considered as a simple and interpretable baseline.

It could provide indications of whether particular variables had an approximately linear relationship with the output.

However, the unknown functions were likely to contain nonlinear relationships, limiting the usefulness of a purely linear model.

### Support Vector Machines

SVM concepts influenced how I thought about observations close to transitions between stronger and weaker regions.

Points where relatively small input changes produced substantial output changes could be viewed as behaving similarly to support-vector or boundary observations.

SVM reasoning therefore helped with interpretation but did not replace the GP as the primary surrogate.

### Neural Networks

Neural networks were considered because they can represent complex nonlinear relationships.

However, the BBO datasets remained extremely small.

Using a highly flexible neural network with so few observations created a significant risk of overfitting.

For this reason, neural-network concepts informed the analysis but did not replace Gaussian Process modelling as the core optimisation approach.

---

## 12. Hyperparameter Tuning

Hyperparameter tuning was deliberately conservative.

Relevant parameters included:

- GP kernel configuration;
- kernel length scales;
- noise assumptions;
- Expected Improvement `xi`;
- UCB `beta`; and
- number of candidate points.

With such small datasets, aggressive hyperparameter optimisation could make a model fit the existing observations extremely well without improving its ability to identify useful new query points.

I therefore used tuning primarily to understand the sensitivity and stability of the optimisation strategy rather than attempting to maximise conventional predictive accuracy.

---

## 13. Clustering-Inspired Reasoning

As more observations accumulated, I began considering whether successful queries formed local groups or **clusters**.

This helped me think about:

- recurring promising regions;
- distances between high-performing observations;
- whether a new query represented genuine exploration;
- whether it was tightening an existing promising region; and
- whether repeated queries were becoming redundant.

Clustering did not replace Bayesian optimisation.

Instead, it provided another perspective for interpreting the growing dataset and identifying potentially meaningful regions.

---

## 14. PCA-Inspired Reasoning

Principal Component Analysis (PCA) influenced how I thought about higher-dimensional functions.

As dimensionality increased, visualising the complete search space became impossible.

The underlying PCA principle of retaining important variation while reducing redundancy encouraged me to ask:

- Which dimensions appear to influence the output most?
- Which movements repeatedly produce little change?
- Are new queries adding genuinely new information?
- Could attention be concentrated on the directions associated with greater variation?

PCA was therefore primarily used as a conceptual framework rather than being applied as a universal preprocessing transformation.

---

## 15. Reinforcement-Learning Perspective

The final stages of the programme introduced another useful interpretation of the optimisation process.

A submitted query can be viewed as an **action**.

The resulting function value provides **feedback or reward**.

That feedback changes expectations about which regions of the search space are promising.

The next query is then selected using the updated information.

This resembles the exploration-exploitation problem found in reinforcement learning and multi-armed bandits.

The comparison also demonstrates why an unsuccessful query is not necessarily wasted. Even a poor result provides information that can influence future decisions.

---

## 16. Function-Specific Strategy

One of the most important changes during the project was moving away from the idea that all eight functions should use exactly the same optimisation behaviour.

By the later rounds, several different patterns had emerged.

### Function 2

Function 2 showed sensitivity to relatively small changes in its inputs.

This made aggressive local movement risky and demonstrated the importance of retaining historical results when selecting subsequent queries.

### Function 5

Function 5 responded particularly well to local refinement.

Continued exploitation of the promising region produced a final-round value of approximately **1368.74**.

### Function 7

Function 7 demonstrated the value of returning towards an earlier successful region after later movements produced weaker results.

The final-round value recovered to approximately **1.35**.

### Function 8

Function 8 showed increasingly small improvements during the later rounds.

The final value of approximately **9.570335** suggested that the search had reached a region of diminishing returns.

These differences reinforced the need for **function-specific optimisation decisions rather than a rigid global policy**.

---

## 17. Tools and Libraries

The project used a lightweight Python scientific-computing stack.

| Library | Purpose |
| --- | --- |
| NumPy | Numerical operations and arrays |
| Pandas | Managing query and result data |
| SciPy | Statistical functions and Sobol sampling |
| scikit-learn | Gaussian Process and baseline ML models |
| Matplotlib | Visualisation |
| Jupyter | Reproducible analysis and documentation |

This stack was appropriate for the relatively small datasets involved and avoided unnecessary framework complexity.

---

## 18. Reproducibility

The repository separates data, analysis and documentation.

### Data

- `data/inputs.csv`
- `data/outputs.csv`

These files contain the recorded query and evaluation history.

### Analysis

- `notebooks/BBO_Optimisation.ipynb`

This notebook demonstrates the core modelling and query-selection workflow.

### Documentation

- `README.md`
- `DATASHEET.md`
- `MODEL_CARD.md`
- `METHODOLOGY.md`
- `OPTIMISATION_HISTORY.md`

Together, these files explain what was done, why the decisions were made and how the approach evolved.

The hidden objective functions themselves cannot be reproduced because they were evaluated externally through the capstone platform.

---

## 19. Limitations

### Small Dataset

The number of observations remained very small for learning potentially complex objective functions.

### Curse of Dimensionality

As dimensionality increased, the observations represented an increasingly small proportion of the possible search space.

### Surrogate Assumptions

Gaussian Process modelling assumes that sufficient underlying structure exists for previous observations to provide useful information about nearby or related regions.

### Hyperparameter Sensitivity

Different kernels and acquisition-function settings can generate different query recommendations.

### Sequential Sampling Bias

Later observations became increasingly concentrated around regions believed to be promising.

The final dataset is therefore not a uniformly sampled representation of each complete search space.

### No Guarantee of Global Optimality

The strongest value observed during the challenge cannot be assumed to represent the true global maximum.

---

## 20. Key Methodological Lessons

The project reinforced several important principles:

1. **Start with relatively simple models.**  
   Additional complexity should be introduced only when supported by the available data.

2. **Treat uncertainty as useful information.**  
   Knowing where the model is uncertain can be as valuable as knowing where it predicts strong performance.

3. **Change the exploration-exploitation balance over time.**  
   The value of exploration changes as the remaining evaluation budget decreases.

4. **Preserve historical evidence.**  
   Recent observations should not automatically replace information from earlier successful regions.

5. **Use models to support decisions rather than replace judgement.**  
   Every surrogate recommendation depends on assumptions.

6. **Recognise the opportunity cost of each experiment.**  
   With a limited evaluation budget, every query matters.

7. **Adapt the strategy to individual functions.**  
   A method that performs well for one objective function may not be appropriate for another.

---

## 21. Conclusion

The final methodology was a **small-data, uncertainty-aware and adaptive optimisation approach** centred on Gaussian Process surrogate modelling and Bayesian optimisation.

The approach evolved as additional observations became available, but the central principle remained consistent:

**Use every evaluation to improve the quality of the next decision.**

Concepts from neural networks, hyperparameter tuning, clustering, PCA and reinforcement learning provided additional ways of interpreting the optimisation problem without unnecessarily replacing the GP-based foundation.

The most important development was therefore not increasing model complexity. It was learning **when to explore, when to exploit, when to trust the surrogate and when historical evidence justified challenging its recommendation**.
