# Black-Box Optimisation Capstone Project

## Imperial College London – Professional Certificate in Machine Learning and Artificial Intelligence

This repository documents my work on the **Black-Box Optimisation (BBO) Capstone Project**, completed as part of the Professional Certificate in Machine Learning and Artificial Intelligence.

The project involved optimising **eight unknown objective functions** with different dimensionalities. The underlying mathematical form of each function was hidden, and only the output generated from each submitted query was available.

The challenge therefore required decisions to be made under uncertainty and with a very limited evaluation budget.

---


## Project Objective

The objective was to identify input combinations that maximise each of the eight unknown functions.

For a function

[
f(x_1,x_2,\ldots,x_n)
]

the goal was to find an input vector

[
x^*=\arg\max_x f(x)
]

without knowing the analytical form of (f).

Each submitted query returned only the corresponding function value. This created a sequential optimisation problem in which every observation had to be used to decide where to search next.

The eight functions ranged from **2 to 8 dimensions**, making the optimisation challenge progressively harder as dimensionality increased.

---

## Non-Technical Summary

This project explores how good decisions can be made when very little information is available. I was given eight hidden functions and had to find input values that produced the highest possible outputs. Each round revealed only the result of the submitted inputs, so the next decision had to be based on patterns learned from previous attempts. My strategy gradually moved from broad exploration towards more focused searches around promising regions. The project demonstrates how machine learning techniques such as Bayesian optimisation can support decision-making when experiments are limited, outcomes are uncertain and trying every possible option is impractical.

---

## Optimisation Approach

My strategy evolved throughout the capstone rather than relying on a single fixed model.

The overall process followed the cycle:

**Observe → Model → Evaluate uncertainty → Select query → Receive feedback → Update**

Early rounds placed greater emphasis on **exploration**, allowing different areas of the search space to be sampled.

As more observations became available, the strategy increasingly favoured **exploitation**, refining regions associated with stronger outputs while retaining some exploration where uncertainty remained high.

The main techniques considered or used during the project included:

* exploratory data analysis and visualisation;
* Gaussian Process surrogate modelling;
* Bayesian optimisation;
* Expected Improvement (EI);
* Upper Confidence Bound (UCB);
* Sobol sampling for candidate generation;
* kernel-based modelling;
* simple regression baselines;
* SVM-based reasoning during exploratory stages;
* neural-network experimentation for selected functions;
* clustering-inspired interpretation of promising regions;
* PCA-inspired reasoning about influential dimensions and redundant exploration;
* reinforcement-learning concepts for interpreting exploration and exploitation.

The final strategy remained deliberately pragmatic. More complex models were introduced only when the available data justified them.

---

## Evolution of the Strategy

### Early rounds: exploration

The initial observations were sparse, so confidently identifying promising regions was difficult.

The emphasis was therefore on:

* understanding the scale and behaviour of each function;
* maintaining broad coverage of the search space;
* visualising lower-dimensional functions where possible;
* avoiding premature commitment to apparent local optima.

### Middle rounds: model-based refinement

As additional observations became available, Gaussian Process models became increasingly useful because they could represent both:

* predicted function values; and
* uncertainty around those predictions.

Acquisition functions were then used to balance exploration and exploitation.

Expected Improvement was useful for identifying locations with potential to improve the current best observation, while UCB provided another mechanism for balancing predicted performance against uncertainty.

### Later rounds: targeted exploitation

During the later stages, the limited number of remaining evaluations changed the risk/reward balance.

Instead of attempting to map large unexplored areas, I increasingly refined promising regions identified by earlier observations.

Ideas from clustering and PCA also influenced this stage. Repeated strong observations could be viewed as local clusters, while dimensions or movements producing little additional information could receive less attention.

### Final round

The final submission placed greater emphasis on exploiting evidence accumulated over the previous rounds.

Where recent movement had reduced performance, I was willing to move back towards previously stronger regions rather than continue searching in the same direction.

This was particularly useful for Functions 2 and 7, while Function 5 continued to benefit from local refinement.

---

## Final Round Results

| Function   |    Final Output |
| ---------- | --------------: |
| Function 1 |    1.812495e-08 |
| Function 2 |        0.543109 |
| Function 3 |       -0.023874 |
| Function 4 |       -3.055181 |
| Function 5 | **1368.742373** |
| Function 6 |       -0.563543 |
| Function 7 |    **1.346625** |
| Function 8 |    **9.570335** |

Function 5 showed particularly strong improvement during the later optimisation rounds and reached **1368.742373** in the final round.

Function 7 also demonstrated why retaining historical information matters. After performance declined during several later queries, returning towards an earlier promising region produced a final value of **1.346625**.

Function 8 showed comparatively small changes during the later rounds, suggesting that the search had entered a relatively stable region.

---

## Key Lessons

### Exploration and exploitation must change over time

The appropriate balance was not constant.

Early in the project, exploration was valuable because little was known about the functions. Later, with fewer evaluations remaining, exploiting established high-performing regions became increasingly important.

### Uncertainty is useful information

A prediction alone does not indicate how trustworthy it is.

Gaussian Processes were particularly useful because uncertainty could be incorporated directly into query selection.

### More complex models are not automatically better

With very small data sets, highly flexible models can introduce additional tuning requirements without necessarily improving optimisation.

This reinforced the value of starting with interpretable approaches and increasing complexity only when justified.

### Dimensionality changes the problem

Two-dimensional functions could be inspected visually, making patterns easier to recognise.

For higher-dimensional functions, intuition became less reliable and model-based search became substantially more important.

### Historical results should not be ignored

Later rounds demonstrated that continuing to move in one direction simply because it was the most recent strategy can be counterproductive.

Previous high-performing regions remained valuable evidence and occasionally justified returning to an earlier part of the search space.

---

## Repository Structure

```text
BBO-Capstone-Project/
│
├── README.md
├── DATASHEET.md
├── MODEL_CARD.md
├── METHODOLOGY.md
├── OPTIMISATION_HISTORY.md
├── requirements.txt
│
├── data/
│   ├── inputs.csv
│   └── outputs.csv
│
├── notebooks/
│   └── BBO_Optimisation.ipynb
│
└── presentation/
    └── BBO_Capstone_Presentation.pdf
```

Additional weekly notebooks may be included where the historical analysis can be reconstructed reliably from the recorded query history.

---

## Documentation

### Datasheet

[`DATASHEET.md`](DATASHEET.md)

Documents the origin, composition, collection process, preprocessing, intended use and limitations of the BBO data set.

### Model Card

[`MODEL_CARD.md`](MODEL_CARD.md)

Documents the optimisation approach, intended use, modelling decisions, assumptions, limitations and performance.

### Methodology

[`METHODOLOGY.md`](METHODOLOGY.md)

Provides a more detailed technical explanation of the modelling and optimisation strategy.

### Optimisation History

[`OPTIMISATION_HISTORY.md`](OPTIMISATION_HISTORY.md)

Records how the optimisation strategy and results evolved across the query rounds.

### Reproducible Analysis

[`notebooks/BBO_Optimisation.ipynb`](notebooks/BBO_Optimisation.ipynb)

Provides a reproducible implementation of the core analysis and optimisation workflow using the recorded BBO observations.

---

## Technologies

The project uses the Python scientific and machine-learning ecosystem, including:

* Python
* NumPy
* Pandas
* Matplotlib
* SciPy
* scikit-learn
* Jupyter Notebook

Gaussian Process modelling is primarily implemented using `GaussianProcessRegressor` from scikit-learn.

---

## Reproducibility

The repository records the query and output history used during the optimisation challenge.

The analysis notebook demonstrates how the recorded observations can be loaded, analysed and used to construct surrogate models and evaluate candidate query points.

Because this was a sequential black-box challenge, the original hidden objective functions are not available in this repository. The supplied data therefore represents observations returned by the capstone platform rather than functions that can be evaluated locally.

---

## Assumptions and Limitations

The optimisation approach assumes that the objective functions contain enough underlying structure for observations in nearby or related regions to provide useful information.

Important limitations include:

* a very small number of observations;
* increasingly sparse coverage as dimensionality grows;
* uncertainty about the true objective-function structure;
* sensitivity of surrogate models to kernel and hyperparameter choices;
* risk of premature exploitation;
* limited ability to validate models using conventional train/test procedures;
* inability to guarantee that the global optimum has been identified.

The results should therefore be interpreted as outcomes of a **limited-budget sequential optimisation process**, rather than proof that the true global maxima were found.

---

## Broader ML Relevance

The BBO challenge reflects a common real-world problem: making decisions when experimentation is expensive and complete information is unavailable.

Similar principles appear in:

* hyperparameter optimisation;
* engineering design;
* scientific experimentation;
* operational optimisation;
* resource allocation;
* A/B testing;
* automated machine-learning systems.

The project reinforced that successful optimisation is not simply about selecting the most sophisticated algorithm. It requires balancing uncertainty, available evidence, computational effort and the cost of making an unsuccessful decision.

---

## Project Status

**Capstone optimisation completed.**

All query rounds have been completed and the repository represents the final documented version of the project.
