# Datasheet – BBO Capstone Dataset 

## 1. Motivation 

This dataset was created within the context of the Black-Box Optimization (BBO) Challenge Capstone Project, which aims to study and apply Bayesian Optimization (BO) strategies and models in an environment of incomplete information and costly evaluations. The dataset's construction simulates real-world problems such as chemical process optimization or experimental design, which require maximizing an objective function by identifying input values that yield the best outcome. This approach is used when faced with functions for which there is no analytical form or access to the gradient, necessitating an iterative pattern-learning approach. 

## 2. Composition 

The dataset is composed of the inputs/outputs of the 8 independent functions that comprise it: 

- Inputs: NumPy arrays of varying dimensions (2D to 8D) 
- Outputs: 1D arrays with scalar evaluations 

Each function has an initial number of observations (10-40), which is increased through weekly iterations. 

The final dataset is incremental, with: 

- 12 additional observations per function (one per week) 
- Approximate total: 20–50 observations per function 

The dataset has no missing data. However, the observations have a skewed distribution, progressively concentrating in optimal regions (a bias towards exploitation). 

## 3. Data Collection Process 

The initial data were distributed at the start of the challenge. Data with weekly increments were generated via an iterative Bayesian optimization process, where each new observation corresponds to an evaluation of the function at a given point.

### Optimisation Strategy: 

The optimization strategy evolved from an exploratory approach focused on the results obtained by the optimization models (GP+EI, GP+UCB) to a much more structural approach to the function. 

In the first iterations, exploration was prioritized using only Bayesian Optimization (BO) to identify regions where the function's maxima could be located. However, with each iteration, more information was obtained, and patterns began to emerge in the results, mainly compact clusters of maximum values and clear separations between high- and low-value regions. Therefore, additional layers of analysis, such as clustering, SVM, and Neural Networks (NN), were used to gain a much deeper structural understanding of the functions in subsequent iterations. The changes described were primarily driven by trends in the data, which, although captured by the BO models, could be better represented by the complementary models. 

All this integration of models in an iterative system allowed the standardization of the decision-making process guided by clear heuristics, where exploitation will be carried out as long as there is evidence that shows that improvements in the maximum value are achieved within the cluster or that there are possible improvements. Exploration will be applied when a promising region has not been located or when marginal improvements are obtained within the cluster.

### Time frame: 

- 12 weeks 
- Frequency: 1 evaluation per function per week 

There are no ethical considerations regarding personal data, as the dataset is entirely synthetic. 

## 4. Preprocessing and Uses 

### Preprocessing 

- Inputs in the range [0,1) 
- No transformations were applied to the inputs/outputs 

### Intended Uses 

- Evaluation of Bayesian Optimization strategies 
- Study of exploration vs. exploitation trade-offs 
- Benchmarking of surrogate models 

### Inappropriate Uses 

- Global statistical inference of the space (dataset is not representative) 
- Generalization outside the explored domain 

## 5. Distribution and Maintenance 

### Distribution:

This dataset is part of the Black-Box Optimization (BBO) Challenge Capstone Project; therefore, its distribution and maintenance are the responsibility of the organizing institution. 

- Format: .npy files (NumPy arrays) 
- Access: Restricted to the challenge environment or associated repository 
- License: Assigned to participants in the academic program 

### Maintenance: 

- Incremental updates per iteration 
- Implicit weekly versioning (v1–v12)

## 6. Data Set Reflections 

I want to emphasize that the final data set does not constitute a representative sample of the space of each function, but rather is the result of an iterative optimization strategy where each incremental observation responds to a process of estimation, evaluation, and decision-making, seeking a balance between exploitation and exploration, guided by statistical and ML models (GP, HEBO, clustering, and classification). Likewise, I want to mention that there are some important assumptions about the continuity of the function space, as well as about the existence of few local optima, which justify the final structure of the data set and, above all, the selection of the models ultimately integrated into the methodology, as well as the biases identified, especially the concentration of data in regions with maximum values resulting from prioritizing progressive exploitation. Finally, the datasheet reflects a group of accumulated decisions, as well as limitations that shaped its distribution and results.
