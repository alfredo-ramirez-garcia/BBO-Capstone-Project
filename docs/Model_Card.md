# Model Card – BBO Optimization System 

## 1. Model Overview 

Model name: BBO Optimization Framework 
Version: v1.0
Developers: BBO Capstone Practitioner 
License: Academic 

This system implements Bayesian Optimization (BO) for optimizing black-box functions, using a hybrid approach that combines surrogate models (Gaussian Process (GP)), evolutionary models (HEBO), and complementary structural analysis models (neighborhood, clustering, SVM, and NN). Its objective is to identify regions where a global maximum of a function can be found under costly evaluation conditions. The system is intended to complement a sequential decision-making process under uncertainty. 

## 2. Intended Use 

### Primary task: 

Optimization of black-box functions using Bayesian Optimization. 

### Target users: 

- Data scientists 
- ML engineers 
- Optimization researchers 
- Decision makers 

### Recommended use cases: 

- Hyperparameter tuning 
- Optimization under limited evaluations

### Not recommended for: 

- Highly discontinuous or non-smooth functions 
- Contexts where a global optimum guarantee is required 

This system should be used as a decision support tool, not as an automated decision-making mechanism. 

## 3. Training Data 

### Data sources: 

Dataset generated during the BBO challenge (8 black-box functions) 

### Size: 

- Initial: 10–40 points per function 
- Final: ~20–50 observations per function 

### Modalities: 

- Structured numerical data (NumPy arrays) 

### Preprocessing steps: 

- Inputs normalized to [0,1) 
- No transformations were applied to the inputs/outputs 

The dataset is adaptive; it reflects the optimization path rather than a representative sample of the space. 

## 4. Evaluation Metrics

### Metrics used: 

- Estimated output (function maximization) 
- Local neighborhood (k-NN) 
- Distance to centroids (clustering) 
- Classification scores (SVM, NN) 

### Performance results: 

The system showed three clear regimes: 

- Complete convergence: functions 4, 5, 8 
- Partial convergence: functions 1, 3, 7 
- Fuzzy exploration: functions 2, 6 

### Fairness/bias checks: 

Not applicable in demographic terms, although the following do exist: 

- Bias towards exploited regions 
- Concentration of samples in local optima 

## 5. Ethical Considerations 

### Potential biases or risks: 

- Bias towards local exploitation 
- Dependence on smoothness assumptions 
- Risk of ignoring unexplored global optima 

### Mitigation strategies: 

- Integration of multiple signals (GP, clustering, classification) 
- Manual evaluation (Practitioner override) 

### Privacy concerns: 
Not applicable (synthetic dataset). 

## 6. Model Life Cycle 

### Date of last update: 
04/28/2026 

### Version control/repository: 
GitHub (capstone project repository) 

Monitoring plan: 

- Iterative evaluation by function 
- Validation using clustering and neighborhood analysis

## 7. Performance

During the final weeks, my results improved across several functions, particularly F1, F6, and F8, where I finished in the top 20. In this regard, I consider the structure-driven approach to have been very effective, since the candidate estimations using GP were complemented by structural analysis (neighborhood, clustering, and classification), which allowed me to identify a region with promising values ​​, which were then exploited. However, results from other functions revealed significant limitations in the strategy: functions F3 and F4 still have difficulty handling complex space, with too many local maxima and high volatility. Furthermore, it is now clear that the transition from exploration to exploitation was made too soon due to the optimization strategy's design, leading to an implicit bias in candidate selection and limiting the ability of the GP and HEBO models to identify more dominant regions. If I had more time to continue developing the strategy, or even if I could start over, I would implement the following from the first iterations: ARD kernels to detect relevance by dimension, formal stopping/exploration criteria, and RL-type adaptive policies to adjust the exploration-exploitation balance dynamically. All these improvements focus on improving the management of the exploration-exploitation trade-off, which has proven to be one of the strategy's biggest limitations so far.

## 8. Limitations



## 9. Trade-offs

The most significant trade-off was deciding when (and under what criteria) to transition from aggressively exploring to exploiting promising regions. During the initial iterations, exploring new regions was essential because the function's patterns were completely unknown, which risked premature convergence to local maxima. However, once clusters and patterns are identified, further exploration could prove inefficient, leading to lost queries and no significant improvement in results. Another trade-off relates to immediate performance improvements versus long-term learning of the function's structure. In functions F5 and F8, micro-refinements around promising regions yielded better results in the short term. This was not the case with functions like F6, where applying local exploration reduced performance but provided critical information about the function's structure. The solution to these imbalances was pursued through a structure-driven optimization strategy that proposes maintaining exploitation when structural support (compact clusters and stable neighbors) persists and exploration when high dispersion or uncertainty persists; unfortunately, the strategy still has many opportunities for improvement.

## 7. Model Card Reflections 

The system was designed to structure a sequential process in which decisions do not depend on a single model, but rather on the convergence of multiple results to select candidate optimization points. The decision is not automated; the practitioner synthesizes information, evaluating the consistency, proximity, and stability of optimal regions to make a decision. One of its main strengths is the integration of complementary models, which reduces uncertainty in scenarios with limited information by capturing elements that allow inferring the problem's structure instead of simply estimating an output. However, its main weakness is its bias toward local exploitation, which can hinder the identification of global maxima if extensive initial exploration is not conducted. Regarding the model card, the current structure is adequate because it balances clarity and depth.
