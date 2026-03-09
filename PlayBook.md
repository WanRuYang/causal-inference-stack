# Causal Inference Interview Playbook
This playbook summarizes common identification strategies, assumptions, and validation checks used in causal inference—both for Data Science interviews and real-world experimentation.

# Strategic Decision Matrix
| Scenario                            | Method                                         | Key Assumption                                                                                                    | Robustness / Validation                                                                             |
| ----------------------------------- | ---------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| **Rollout to One Region**           | **Synthetic Control (SC)** / **Synthetic DiD** | **Convexity**: Treated unit can be approximated by weighted donor units; **No interference (SUTVA)**              | **Placebo tests** (in-space / in-time); **Leave-one-out donor test**; Pre-treatment fit diagnostics |
| **Full Rollout (No Control)**       | **Interrupted Time Series (ITS)**              | **Stable trend**: Outcome would continue following the same trajectory absent treatment; **No concurrent shocks** | **Segmented regression** (level/slope change); **Event study**; Include external covariates         |
| **Quasi-experiment (Group + Time)** | **Difference-in-Differences (DiD)**            | **Parallel trends**: Treatment and control would evolve similarly without intervention; **SUTVA**                 | **Pre-trend tests**; **Event study plots**; Placebo timing test                                     |
| **Eligibility Threshold**           | **Regression Discontinuity (RD)**              | **Continuity**: Units around cutoff are comparable in absence of treatment                                        | **McCrary density test**; **Bandwidth sensitivity**; Covariate balance near cutoff                  |
| **Imperfect Compliance**            | **Instrumental Variables (IV)**                | **Exclusion restriction**; **Relevance**; **Monotonicity**                                                        | **Weak instrument test** (F-stat > 10); Reduced-form estimation; Overidentification tests           |
| **Personalization / Targeting**     | **CATE / Uplift Modeling**                     | **Unconfoundedness** (selection on observables); **Overlap**                                                      | **AUUC / Qini curve**; Calibration plots; Policy simulation (Top-K targeting gain)                  |

# Limitations
Mentioning these limitations during interviews signals practical experience with causal inference.
| Method                | Common Risks / Limitations                                                                           |
| --------------------- | ---------------------------------------------------------------------------------------------------- |
| **Synthetic Control** | Small or poor **donor pool** can lead to unstable synthetic weights or overfitting                   |
| **ITS**               | Cannot rule out **concurrent events** that occur at the same time as intervention                    |
| **DiD**               | **Parallel trend violation**; heterogeneous treatment timing requires modern DiD estimators          |
| **RD**                | **Manipulation near cutoff**; treatment effect only locally identified                               |
| **IV**                | **Weak instruments** or violation of exclusion restriction                                           |
| **CATE**              | Hidden **confounding** may cause uplift estimates to capture correlation instead of treatment effect |

# If you have X, you use Y.
| Situation                       | Method                        |
| ------------------------------- | ----------------------------- |
| Randomized experiment available | **A/B testing (RCT)**         |
| Geographic rollout              | **Synthetic Control**         |
| Time-only intervention          | **Interrupted Time Series**   |
| Group + time variation          | **Difference-in-Differences** |
| Eligibility threshold rule      | **Regression Discontinuity**  |
| Treatment compliance problem    | **Instrumental Variables**    |
| Personalized treatment decision | **CATE / Uplift Modeling**    |

### Pre-analysis checks
- Covariate balance
- Overlap / common support
- Pre-treatment trend diagnostics

### Post-analysis sensitivity
- Placebo tests
- Rosenbaum bounds
- Selection-on-unobservables analysis (Oster's δ)

### Work Flow
```
Theory (DAG)
    ↓
Identification Strategy
    ↓
Estimation Method
```
- example 
```
Confounding DAG
    ↓
Backdoor Adjustment
    ↓
Regression / Matching / IPW
```