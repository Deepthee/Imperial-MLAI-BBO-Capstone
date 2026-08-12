# Black-Box Optimisation Capstone Project

## Imperial College London Professional Certificate in Machine Learning and Artificial Intelligence

This repository documents my Black-Box Optimisation (BBO) capstone project completed as part of the Imperial College London Professional Certificate in Machine Learning and Artificial Intelligence.

The project involved sequentially optimising eight unknown objective functions using a limited number of evaluations. Across Stage 2 of the capstone, I completed 13 optimisation rounds, progressively refining my approach as new observations became available.

The project explores Bayesian optimisation, Gaussian Process surrogate modelling, acquisition functions, exploration versus exploitation, hyperparameter tuning, clustering, PCA-inspired reasoning and reinforcement-learning concepts.

---

## Non-Technical Summary

The goal of this project was to find strong solutions to eight unknown mathematical functions without being able to see how the functions worked internally. Each time I selected a set of input values, the system returned a result, and that new information was used to decide what to try next. Rather than relying on random trial and error, I used machine-learning techniques to estimate which areas were promising while also considering unexplored regions. Over successive rounds, the strategy became increasingly focused on the strongest areas identified by the data. The project demonstrates how intelligent decisions can be made when information is limited and experimentation is costly.

---

## Project Objective

The capstone was structured as a black-box optimisation problem.

For each of eight unknown functions, I was provided with an initial set of input-output observations. The internal mathematical form of the functions was hidden.

The task was to select new query points that would maximise the returned objective value.

This created three key challenges:

- only a small number of observations were available;
- the search spaces ranged from two to eight dimensions;
- every query had an opportunity cost because the number of evaluations was limited.

The problem therefore required a balance between:

**Exploration:** testing uncertain or previously unexplored regions.

**Exploitation:** refining regions that had already produced strong results.

---

## Objective Functions

The challenge contained eight functions of increasing dimensionality:

| Function | Input dimensions |
| --- | ---: |
| Function 1 | 2 |
| Function 2 | 2 |
| Function 3 | 3 |
| Function 4 | 4 |
| Function 5 | 4 |
| Function 6 | 5 |
| Function 7 | 6 |
| Function 8 | 8 |

The increasing dimensionality made the later functions progressively harder to understand and visualise.

---

## Optimisation Approach

My primary approach was based on **Bayesian optimisation using Gaussian Process (GP) surrogate modelling**.

The overall process was:

1. Analyse the observations collected so far.
2. Fit or update a Gaussian Process surrogate.
3. Estimate the predicted performance and uncertainty of unseen candidates.
4. Generate candidate points across the search space.
5. Apply an acquisition strategy such as Expected Improvement or Upper Confidence Bound.
6. Compare the model recommendation with historical observations.
7. Select and submit the next query.
8. Receive the black-box evaluation.
9. Add the result to the dataset.
10. Repeat the process.

A **Matérn-family kernel** was used as the main GP kernel because it provides flexibility when modelling functions that may not be perfectly smooth.

Sobol sampling was also used to generate structured candidate points across multidimensional search spaces.

For a detailed technical explanation, see [`METHODOLOGY.md`](METHODOLOGY.md).

---

## How the Strategy Evolved

The optimisation strategy changed considerably as additional observations became available.

### Early rounds: exploration

Initially, relatively little was known about the objective functions.

The priority was therefore to learn about the search space and avoid becoming committed too early to a potentially suboptimal region.

### Middle rounds: model-guided refinement

As the dataset grew, Gaussian Process predictions and uncertainty became more informative.

The strategy increasingly combined:

- GP predictions;
- acquisition-function scores;
- uncertainty;
- previous high-performing observations; and
- local behaviour around promising regions.

Concepts introduced throughout the programme also influenced how I interpreted the search, including hyperparameter tuning, clustering and dimensionality reduction.

### Final rounds: selective exploitation

As the number of remaining evaluations decreased, broad exploration became less valuable because there were fewer future rounds in which to exploit newly discovered information.

The final strategy therefore placed greater emphasis on promising regions while retaining limited exploration where uncertainty remained useful.

Historical evidence also became increasingly important. A recent observation was not automatically considered more useful than an earlier strong result.

---

## Exploration and Exploitation

The exploration-exploitation trade-off became one of the most important lessons from the project.

Early exploration was valuable because information discovered at that stage could influence many future queries.

Later in the project, the value of exploitation increased because the remaining query budget was limited.

This is closely related to ideas from reinforcement learning and multi-armed bandits: actions produce feedback, feedback changes expectations and those updated expectations influence future actions.

The optimisation process therefore became increasingly adaptive rather than following one fixed policy throughout the challenge.

---

## Other ML Concepts Considered

Although Gaussian Process Bayesian optimisation remained the main foundation, other machine-learning concepts influenced the project.

### Linear Regression

Linear regression was considered as a simple baseline for identifying possible directional relationships between inputs and outputs.

### Support Vector Machines

SVM concepts influenced my thinking around boundaries between stronger and weaker regions of the search space.

### Neural Networks

Neural networks were considered as flexible nonlinear surrogate models. However, the extremely small datasets created a significant risk of overfitting, so they did not replace Gaussian Processes as the primary model.

### Hyperparameter Tuning

Kernel parameters and acquisition-function settings were examined to understand their effect on the exploration-exploitation balance.

### Clustering

As more observations accumulated, I considered whether successful queries formed local clusters or recurring promising regions.

### PCA

PCA introduced a useful way of thinking about higher-dimensional functions: focus attention on the directions that appear to contain meaningful variation while avoiding redundant exploration.

### Reinforcement Learning

The final rounds highlighted similarities between sequential BBO and reinforcement learning, particularly feedback-driven adaptation and exploration versus exploitation.

---

## Stage 2 Optimisation History

Stage 2 consisted of **13 sequential query rounds from Modules 12 to 24**.

Each round produced one new evaluation for each of the eight objective functions.

The complete recorded history is available in:

- [`data/queries.csv`](data/queries.csv)
- [`data/results.csv`](data/results.csv)

A narrative explanation of how the strategy evolved is available in:

[`OPTIMISATION_HISTORY.md`](OPTIMISATION_HISTORY.md)

---

## Final-Round Observations

The final round reinforced the fact that different functions required different optimisation behaviours.

### Function 5

Function 5 responded particularly well to continued local refinement and produced a final-round value of approximately **1368.74**.

This was one of the clearest examples of successful exploitation.

### Function 2

Function 2 demonstrated sensitivity to relatively small changes in its inputs. Returning towards a previously successful neighbourhood produced a substantial recovery in the final round.

### Function 7

Function 7 showed the value of retaining historical evidence. After several weaker results, moving back towards an earlier promising region improved the final result to approximately **1.35**.

### Function 8

Function 8 converged towards approximately **9.5703**, with increasingly small improvements suggesting diminishing returns in the region being explored.

These results reinforced the importance of using **function-specific strategies rather than applying exactly the same optimisation policy to every objective**.

---

## Repository Structure

```text
.
├── README.md
├── DATASHEET.md
├── MODEL_CARD.md
├── METHODOLOGY.md
├── OPTIMISATION_HISTORY.md
├── BBO_Optimisation.ipynb
│
└── data/
    ├── README.md
    ├── queries.csv
    ├── results.csv
    │
    └── initial/
        ├── README.md
        ├── function1_initial_inputs.npy
        ├── function1_initial_outputs.npy
        ├── ...
        ├── function8_initial_inputs.npy
        └── function8_initial_outputs.npy
```

---

## Jupyter Notebook

The main reproducible analysis is available here:

[`BBO_Optimisation.ipynb`](BBO_Optimisation.ipynb)

The notebook demonstrates:

- loading the initial BBO datasets;
- loading all Stage 2 query results;
- constructing the complete optimisation history;
- Gaussian Process surrogate modelling;
- Matérn kernels;
- Expected Improvement;
- Upper Confidence Bound;
- Sobol candidate generation;
- retrospective query recommendation;
- two-dimensional surrogate visualisation;
- analysis of optimisation progress; and
- final observations and limitations.

The notebook distinguishes between the **actual recorded submissions** and a **representative reproducible implementation** of the modelling methodology.

---

## Data Documentation

The project data is documented in:

[`DATASHEET.md`](DATASHEET.md)

The datasheet describes:

- motivation;
- composition;
- collection process;
- preprocessing;
- intended uses;
- inappropriate uses;
- distribution;
- limitations; and
- maintenance.

The original challenge datasets have also been retained in NumPy `.npy` format under `data/initial/`.

---

## Model Documentation

The optimisation approach is documented in:

[`MODEL_CARD.md`](MODEL_CARD.md)

The model card describes:

- the optimisation approach;
- intended use;
- modelling strategy;
- performance;
- assumptions;
- limitations;
- potential failure modes; and
- transparency considerations.

---

## Technical Methodology

A detailed explanation of the modelling and decision-making process is provided in:

[`METHODOLOGY.md`](METHODOLOGY.md)

This includes the reasoning behind:

- Gaussian Processes;
- Matérn kernels;
- Expected Improvement;
- Upper Confidence Bound;
- Sobol sampling;
- hyperparameter tuning;
- exploration and exploitation;
- clustering-inspired analysis;
- PCA-inspired reasoning; and
- reinforcement-learning interpretations.

---

## Running the Notebook

The project requires Python 3 and the following libraries:

```text
numpy
pandas
scipy
scikit-learn
matplotlib
jupyter
```

Install the dependencies using:

```bash
pip install numpy pandas scipy scikit-learn matplotlib jupyter
```

Clone the repository and start Jupyter:

```bash
jupyter notebook
```

Open:

```text
BBO_Optimisation.ipynb
```

The hidden objective functions themselves cannot be executed locally because they were evaluated externally through the capstone project portal.

---

## Limitations

Several limitations should be considered when interpreting the project.

### Small datasets

Only a limited number of observations were available for each function.

### High dimensionality

For higher-dimensional functions, the available observations represented only a very small fraction of the possible search space.

### Surrogate assumptions

Gaussian Process modelling assumes that sufficient structure exists for previous observations to provide information about other regions.

### Sequential sampling bias

Later observations became concentrated around areas believed to be promising. The resulting dataset is therefore not a uniformly sampled representation of the complete search space.

### Hyperparameter sensitivity

Different kernels, noise assumptions and acquisition settings can lead to different recommendations.

### Unknown global optimum

Because the objective functions were hidden, the strongest observed result cannot be assumed to be the true global maximum.

---

## Key Learning

The most important lesson from the project was that successful optimisation is not simply about selecting the most sophisticated model.

The quality of the process depends on making good sequential decisions under uncertainty.

The project demonstrated the importance of:

- using uncertainty rather than ignoring it;
- adapting exploration as the evaluation budget changes;
- preserving historical evidence;
- recognising when additional complexity is unsupported by the data;
- learning from unsuccessful evaluations;
- avoiding premature convergence; and
- adapting the strategy to the behaviour of individual functions.

These principles extend beyond black-box optimisation to model tuning, experimentation, engineering design and real-world ML decision-making.

---

## Transparency and Reproducibility

The repository separates:

- raw initial data;
- submitted queries;
- returned evaluations;
- executable analysis;
- methodology;
- optimisation history;
- dataset documentation; and
- model documentation.

The goal is to make it possible for another reader to understand not only **what was submitted**, but also **why the optimisation strategy evolved as it did**.

The hidden functions themselves are not available, so the external objective evaluations cannot be independently reproduced.

---

## Academic Context

This repository was created as part of the **Imperial College London Professional Certificate in Machine Learning and Artificial Intelligence**.

It documents an educational black-box optimisation challenge and should not be interpreted as a production optimisation system.

---

## Project Status

**Capstone optimisation completed.**

Stage 2 covered 13 sequential optimisation rounds across Modules 12–24, followed by final retrospective analysis and repository consolidation.
