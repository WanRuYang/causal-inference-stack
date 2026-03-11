# Causal Inference Stack

A layered reference for causal inference — from foundational theory to identification, estimation, practical applications, and common interview topics.

# Theory
```
├─ Potential Outcomes
└─ Structural Causal Models (DAG, do-calculus)

DAG Structures
├─ Confounder
├─ Mediator
├─ Collider
├─ Instrument
└─ Selection Bias

Estimand
├─ ATE
├─ ATT
├─ CATE
├─ LATE
└─ QTE

Identification
├─ Randomization
├─ Backdoor
├─ Frontdoor
├─ Instrumental Variables
├─ Regression Discontinuity
├─ Difference-in-Differences
└─ Synthetic Control

Estimation
├─ Regression
├─ Matching / Weighting
├─ Doubly Robust
├─ ML-based Causal Methods
└─ Panel / Synthetic Methods

Applications
├─ Experiments
├─ Marketing
├─ Marketplace
├─ Recommendation
├─ Pricing
└─ Policy

Interview Topics
├─ A/B Testing
├─ Quasi-experiments (Geo / Geo-Time / Time)
├─ Observational Causal Inference
├─ Uplift Modeling
└─ IV / DiD / RD
```


---

# Layer 1 — Causal Theory

| Framework | Key Concept | What it Defines |
|---|---|---|
| Rubin Causal Model | Potential Outcomes | Defines treatment effects |
| Rubin Causal Model | ATE / ATT / CATE | Formal estimands |
| Structural Causal Model | DAG | Graphical causal structure |
| Structural Causal Model | do-operator | Intervention distribution |
| Structural Causal Model | do-calculus | Identification rules |
| Structural Causal Model | Backdoor criterion | Adjustment for confounding |
| Structural Causal Model | Frontdoor criterion | Identification via mediators |

---

# Layer 2 — DAG Structures

| Structure | Graph | Description |
|---|---|---|
| Confounder | T ← X → Y | Variable affects both treatment and outcome |
| Mediator | T → M → Y | Variable lies on causal pathway |
| Collider | T → C ← U | Conditioning introduces bias |
| Instrument | Z → T → Y | Instrument affects outcome only through treatment |
| Selection Bias | T → S ← Y | Conditioning on selection variable introduces bias |
| M-bias | T ← X → M ← U → Y | Adjusting for M creates spurious association |

---

# Causal Graph Patterns Cheat Sheet

| Pattern | DAG | Effect of Adjustment |
|---|---|---|
| Confounder | T ← X → Y | Must adjust |
| Mediator | T → M → Y | Adjusting removes indirect effect |
| Collider | T → C ← U | Adjusting introduces bias |
| Instrument | Z → T → Y | Use IV methods |
| Selection bias | T → S ← Y | Conditioning induces correlation |
| M-bias | T ← X → M ← U → Y | Adjusting introduces bias |

---

# Layer 3 — Estimands

| Estimand | Definition | Typical Context |
|---|---|---|
| ATE | Average Treatment Effect | Population-level policy effect |
| ATT | Average Treatment Effect on Treated | Impact on treated users |
| CATE | Conditional Average Treatment Effect | Personalization |
| LATE | Local Average Treatment Effect | Instrumental variables |
| QTE | Quantile Treatment Effect | Distributional impact |

---

# Layer 4 — Identification

| Identification Strategy | Pearl Mapping | Key Assumption |
|---|---|---|
| Randomized Experiment | do(T) | Random assignment |
| Selection on Observables | Backdoor criterion | Conditional exchangeability |
| Frontdoor Adjustment | Frontdoor criterion | Mediator structure |
| Instrumental Variables | Exclusion restriction | Valid instrument |
| Regression Discontinuity | Threshold assignment | Continuity |
| Difference-in-Differences | Parallel trends | No differential trends |
| Synthetic Control | Convex donor combination | Stable counterfactual |

---

# DAG → Identification → Assumption → Estimator

| DAG Structure | Identification Strategy | Key Assumption | Estimator |
|---|---|---|---|
| Confounder | Backdoor adjustment | No unobserved confounding | Regression, Matching, IPW |
| Mediator | Frontdoor adjustment | Mediator fully captures causal pathway | Two-stage regression |
| Unobserved confounding | Instrumental variables | Relevance & exogeneity | 2SLS, GMM |
| Threshold rule | Regression discontinuity | Continuity of potential outcomes | Local linear RD |
| Time policy change | Difference-in-differences | Parallel trends | TWFE, group-time ATT |
| Constructed counterfactual | Synthetic control | Convex combination of donor units | SC, augmented SC |

---

# Layer 5 — Estimation Methods

## Experimental Estimators

| Identification | Estimator | Method Type |
|---|---|---|
| Randomized | Diff-in-means | Experimental |
| Randomized | t-test / Welch | Statistical |
| Randomized | Mann–Whitney U | Nonparametric |
| Randomized | χ² test | Categorical outcomes |
| Randomized | CUPED | Variance reduction |

---

## Backdoor Adjustment Methods

| Identification | Estimator | Method Type |
|---|---|---|
| Backdoor | Regression Adjustment | Parametric |
| Backdoor | Propensity Score Matching | Matching |
| Backdoor | Propensity Score Weighting (IPW) | Weighting |
| Backdoor | Stratification | Subclassification |

---

## Doubly Robust Methods

| Identification | Estimator | Method Type |
|---|---|---|
| Backdoor | AIPW | Doubly robust |
| Backdoor | TMLE | Semi-parametric |
| Backdoor | Double Machine Learning (DML) | Orthogonal ML |
| Backdoor | DR-Learner | Meta-learning |

---

## Covariate Balancing Methods

| Identification | Estimator | Method Type |
|---|---|---|
| Backdoor | Entropy balancing | Covariate balancing |
| Backdoor | Stable balancing weights | Covariate balancing |
| Backdoor | Overlap weights | Overlap weighting |
| Backdoor | Kernel balancing | Covariate balancing |

---

## Heterogeneous Treatment Effects

| Identification | Estimator | Method Type |
|---|---|---|
| CATE | S-Learner | Meta learner |
| CATE | T-Learner | Meta learner |
| CATE | X-Learner | Meta learner |
| CATE | R-Learner | Meta learner |
| CATE | Causal Forest | ML causal |
| CATE | BART | Bayesian causal |
| CATE | Uplift trees | Targeting |

---

## Instrumental Variable Methods

| Identification | Estimator | Method Type |
|---|---|---|
| IV | Wald estimator | Simple IV |
| IV | 2SLS | Econometric |
| IV | LIML | Econometric |
| IV | GMM | Econometric |

---

## Regression Discontinuity

| Identification | Estimator | Method Type |
|---|---|---|
| RD | Sharp RD | Local causal |
| RD | Fuzzy RD | Imperfect compliance |
| RD | Local linear RD | Modern RD |
| RD | Local polynomial RD | Robust RD |

---

## Panel / Time-based Methods

| Identification | Estimator | Method Type |
|---|---|---|
| Panel | TWFE DiD | Classical DiD |
| Panel | Sun & Abraham | Staggered DiD |
| Panel | Callaway & Sant’Anna | Group-time ATT |
| Panel | Event study | Dynamic treatment timing |

---

## Synthetic Control

| Identification | Estimator | Method Type |
|---|---|---|
| Synthetic | Synthetic Control | Counterfactual |
| Synthetic | Augmented SC | Bias corrected |
| Synthetic | Generalized SC | Panel extension |

---

## Distributional Effects

| Identification | Estimator | Method Type |
|---|---|---|
| Distributional | Quantile Treatment Effects | Distributional causal |
| Distributional | RIF regression | Distributional analysis |

---

## Partial Identification

| Identification | Estimator | Method Type |
|---|---|---|
| Weak identification | Manski bounds | Partial identification |
| Attrition bias | Lee bounds | Partial identification |

---

# Causal Inference Landscape

| Category | Methods |
|---|---|
| Experimental Methods | Diff-in-means, t-test, CUPED |
| Matching Methods | Nearest neighbor, Mahalanobis, optimal matching, genetic matching, coarsened exact matching |
| Propensity Score Methods | Matching, weighting, stratification |
| Covariate Balancing | Entropy balancing, stable balancing weights, overlap weights, kernel balancing |
| Doubly Robust | AIPW, TMLE, DML, DR-Learner |
| Meta-learners | S-, T-, X-, R-Learner |
| ML Causal | Causal forest, BART, uplift trees |
| IV Methods | Wald, 2SLS, LIML, GMM |
| RD Methods | Sharp RD, fuzzy RD, local linear RD |
| Panel Methods | TWFE, Sun & Abraham, Callaway & Sant’Anna |
| Synthetic Methods | Synthetic control, augmented SC, generalized SC |
| Distributional | QTE, RIF regression |
| Partial ID | Manski bounds, Lee bounds |

---

# Method Selection Guide

| Data Setting | Target Quantity | Typical Method |
|---|---|---|
| Randomized experiment | ATE | Diff-in-means / CUPED |
| Observational data | ATE / ATT | Regression, IPW, AIPW |
| High-dimensional covariates | ATE / CATE | Double ML |
| Personalization | CATE | Meta-learners, causal forest |
| Policy threshold | Local effect | RD |
| Natural experiment | LATE | IV |
| Panel rollout | ATT | DiD |
| Geo intervention | ATE | Synthetic control |

---

# Practical Use Cases

| Business Problem | Typical Method |
|---|---|
| A/B testing | Diff-in-means / CUPED |
| Marketing attribution | IPW / DML |
| Coupon targeting | Uplift / CATE |
| Personalized recommendation | Causal forest |
| Marketplace pricing | IV |
| Dynamic pricing | IV / structural models |
| Policy change | DiD |
| Eligibility threshold | RD |
| Geo experiment | Synthetic control |
| Churn intervention | CATE |

---

# Interview Layer

| Topic | Typical Questions |
|---|---|
| A/B testing | experiment design, sample size |
| Observational causal inference | treatment effect estimation |
| Propensity scores | matching vs weighting |
| Doubly robust | why AIPW works |
| Heterogeneous effects | uplift modeling |
| IV | when to use IV |
| DiD | parallel trends |
| RD | bandwidth selection |
| Interference | spillover effects |
| Cluster experiments | geo / switchback experiments |

---

# Common Pitfalls

| Pitfall | Description |
|---|---|
| Selection bias | Treatment correlated with outcome |
| Post-treatment bias | Conditioning on mediator |
| Collider bias | Conditioning on common effect |
| Interference | One unit affects another |
| Lack of overlap | Extreme propensity scores |

---

# Key Libraries

| Library | Description |
|---|---|
| econml | ML causal inference estimators |
| dowhy | Graph-based causal modeling |
| causalml | Uplift modeling |
| grf | Generalized random forests |
| DoubleML | Double machine learning framework |

---

# Planned Repository Structure

```

causal-inference-map/

README.md

examples/
├─ ab_testing.ipynb
├─ propensity_score.ipynb
├─ doubly_robust.ipynb
├─ double_ml.ipynb
├─ causal_forest.ipynb
└─ did.ipynb

simulations/
├─ confounding_simulation.py
├─ collider_bias_simulation.py
├─ overlap_violation_simulation.py
├─ iv_simulation.py
└─ did_simulation.py

```
