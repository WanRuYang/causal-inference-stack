# Causal Inference Interview Playbook
This playbook summarizes common identification strategies, assumptions, and validation checks used in causal inference—both for Data Science interviews and real-world experimentation.
- **Which causal estimand can we identify?**
```
RCT → ATE
DiD → ATT
IV → LATE
Uplift → CATE
Synthetic Control → ATT
```

# Strategic Decision Matrix
| Scenario                                              | Method                                                            | Effect Type Identified                             | Key Assumption                                                                | Robustness / Validation                                                                     |
| ----------------------------------------------------- | ----------------------------------------------------------------- | -------------------------------------------------- | ----------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| Randomized experiment available                       | **A/B Testing (RCT)**                                             | **ATE** (sometimes ATT depending on exposure)      | Randomization removes confounding; SUTVA                                      | Sample ratio mismatch check; Balance test; CUPED variance reduction                         |
| Geographic rollout                                    | **Synthetic Control (SC) / Synthetic DiD**                        | **ATT** (effect on treated region)                 | Convexity: treated unit approximated by weighted donor units; no interference | Placebo tests (in-space / in-time); Leave-one-out donor test; Pre-treatment fit diagnostics |
| Full rollout (no control)                             | **Interrupted Time Series (ITS)**                                 | **ATT** (treated time period)                      | Stable trend: outcome would follow same trajectory absent intervention        | Segmented regression; Event study; Include external covariates                              |
| Group + time variation                                | **Difference-in-Differences (DiD)**                               | **ATT** (treated group over time)                  | Parallel trends; SUTVA                                                        | Pre-trend tests; Event study plots; Placebo timing test                                     |
| Eligibility threshold                                 | **Regression Discontinuity (RD)**                                 | **Local ATE (LATE around cutoff)**                 | Continuity: units near cutoff comparable                                      | McCrary density test; Bandwidth sensitivity; Covariate balance near cutoff                  |
| Imperfect compliance                                  | **Instrumental Variables (IV)**                                   | **LATE (compliers)**                               | Relevance; Exclusion restriction; Monotonicity                                | Weak instrument test (F-stat > 10); Reduced-form estimation; Overidentification tests       |
| Personalization / targeting                           | **CATE / Uplift Modeling**                                        | **CATE** (heterogeneous treatment effects)         | Unconfoundedness; overlap                                                     | AUUC / Qini curve; Calibration plots; Policy simulation                                     |
| Observational causal effect with measured confounders | **Backdoor adjustment** (Regression / Matching / IPW / Double ML) | **ATE / ATT / CATE** depending on weighting scheme | Conditional exchangeability                                                   | Covariate balance; sensitivity analysis; overlap diagnostics                                |
| Mediated causal pathway observable                    | **Frontdoor adjustment / mediation analysis**                     | **ATE decomposed into direct + indirect effects**  | Mediator captures causal path; no unblocked backdoor paths                    | Two-stage estimation; mediation effect decomposition                                        |
| Product funnel causal decomposition                   | **Mediation / path analysis** (Recom → Click → Purchase)          | **Indirect effect / mediated effect**              | Sequential ignorability                                                       | Compare direct vs indirect effects; experiment validation                                   |
| Recommendation evaluation under bias                  | **Counterfactual learning / IPS / Off-policy evaluation**         | **Policy value / expected reward**                 | Correct logging propensity                                                    | Self-normalized IPS; doubly robust OPE                                                      |
| Ranking relevance estimation                          | **Randomized ranking / interleaving experiments**                 | **ATE on engagement metrics**                      | Random exposure eliminates position bias                                      | Online A/B test; interleaving win rate                                                      |

# Key Effect Types (Causal Estimands)
| Effect Type | Mathematical Form | Meaning | Typical Use Case |
|---|---|---|---|
| **ITE** | $ITE_i = Y_i(1) - Y_i(0)$ | Individual treatment effect | theoretical causal quantity |
| **ATE** | $ATE = \mathbb{E}[Y(1) - Y(0)]$ | Average Treatment Effect across population | RCT experiments |
| **ATT** | $ATT = \mathbb{E}[Y(1) - Y(0) \mid D=1]$ | Average Treatment Effect on Treated | DiD / Synthetic Control |
| **CATE** | $CATE(x) = \mathbb{E}[Y(1) - Y(0) \mid X=x]$ | Conditional Treatment Effect by subgroup | personalization / targeting |
| **LATE** | $LATE = \mathbb{E}[Y(1)-Y(0) \mid \text{Compliers}]$ | Local treatment effect for compliers | Instrumental Variables |
| **Policy Value** | $V(\pi)=\mathbb{E}[Y(\pi(X))]$ | Expected outcome under decision policy | recommendation / RL |


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
| Situation                               | Method                    | Effect Identified |
| --------------------------------------- | ------------------------- | ----------------- |
| Randomized experiment                   | A/B testing               | ATE               |
| Geographic rollout                      | Synthetic Control         | ATT               |
| Time-only intervention                  | Interrupted Time Series   | ATT               |
| Group + time variation                  | Difference-in-Differences | ATT               |
| Eligibility threshold                   | Regression Discontinuity  | Local ATE         |
| Compliance issue                        | Instrumental Variables    | LATE              |
| Observational with confounders measured | Backdoor adjustment       | ATE / ATT         |
| Observational with mediator observed    | Frontdoor adjustment      | ATE               |
| Personalized treatment decision         | CATE / Uplift modeling    | CATE              |

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


# Causal Identification Methods

## Table of Contents
- [Backdoor Adjustment](#backdoor-adjustment)
- [Propensity Score Methods (IPW)](#propensity-score-methods)
- [Instrumental Variables (IV)](#instrumental-variables)
- [Difference-in-Differences (DiD)](#difference-in-differences-did)
- [Regression Discontinuity (RD)](#regression-discontinuity-rd)
- [Mediation Analysis](#mediation-analysis)
- [Off-Policy Evaluation (OPE)](#off-policy-evaluation)

---

## Backdoor Adjustment
**Identifies:** ATE (Average Treatment Effect)

If a set of covariates $Z$ satisfies the backdoor criterion (blocking all non-causal paths between treatment $D$ and outcome $Y$):

$$ATE = \mathbb{E}_Z [ \mathbb{E}[Y \mid D=1, Z] - \mathbb{E}[Y \mid D=0, Z] ]$$

**Key Assumption**
> **Conditional Exchangeability (Unconfoundedness)**
> $$(Y(1), Y(0)) \perp D \mid Z$$

**Common Implementations**
* **Regression Adjustment:** Modeling $Y = f(D, Z)$.
* **Matching:** Grouping similar units based on $Z$.
* **Double Machine Learning (DML):** Using ML to partial out the effects of $Z$ from both $D$ and $Y$.

---

## Propensity Score Methods

### Inverse Propensity Weighting (IPW)
**Identifies:** ATE

$$ATE = \mathbb{E} \left[ \frac{D Y}{e(Z)} - \frac{(1-D)Y}{1-e(Z)} \right]$$

Where the **Propensity Score** is:
$$e(Z) = P(D=1 \mid Z)$$

**Core Idea**
Reweight observations to create a pseudo-population where the treatment assignment is independent of covariates, simulating a randomized experiment.

---

## Instrumental Variables
### Wald Estimator (Binary Instrument)
**Identifies:** LATE (Local Average Treatment Effect)

$$LATE = \frac{\mathbb{E}[Y \mid Z=1] - \mathbb{E}[Y \mid Z=0]}{\mathbb{E}[D \mid Z=1] - \mathbb{E}[D \mid Z=0]}$$

* $Z$: Instrument (assignment)
* $D$: Treatment (actual uptake)

**Key Assumptions**
1.  **Relevance:** $Z$ has a causal effect on $D$.
2.  **Exclusion Restriction:** $Z$ affects $Y$ *only* through $D$.
3.  **Monotonicity:** No "defiers" (people who do the opposite of their assignment).

**Interpretation**
The effect for **Compliers** (those who take the treatment if and only if they are assigned to it).

---

## Difference-in-Differences (DiD)
**Identifies:** ATT (Average Treatment Effect on the Treated)

$$ATT = (Y_{T, \text{post}} - Y_{T, \text{pre}}) - (Y_{C, \text{post}} - Y_{C, \text{pre}})$$

**Key Assumption**
> **Parallel Trends**
> In the absence of treatment, the average outcomes for the treated and control groups would have followed the same trend over time.

---

## Regression Discontinuity (RD)
**Identifies:** Local ATE at the cutoff

Treatment assignment is determined by whether a running variable $X$ crosses a threshold $c$.

$$\tau_{RD} = \lim_{x \downarrow c} \mathbb{E}[Y \mid X=x] - \lim_{x \uparrow c} \mathbb{E}[Y \mid X=x]$$

**Interpretation**
The causal effect for units **near the cutoff**. It is often considered the most internally valid quasi-experimental method.

---

## Mediation Analysis
**Identifies:** Direct and Indirect Causal Effects

**Total Effect (TE) Decomposition:**
$$TE = \mathbb{E}[Y(1) - Y(0)] = DE + IE$$

* **DE (Direct Effect):** Effect of treatment on outcome holding the mediator constant.
* **IE (Indirect Effect):** Effect of treatment on outcome through the mediator $M$.

**Causal Pathway Example**
`Feature Launch → User Engagement (M) → Retention (Y)`

**Use Cases**
* Understanding *why* a product change worked.
* Attribution modeling.
* Identifying bottlenecks in a conversion funnel.

---

## Off-Policy Evaluation
### Inverse Propensity Scoring (IPS)

**Identifies:** Expected value of a new policy $V(\pi)$

$$V(\pi) = \mathbb{E}_{n} \left[ \frac{\pi(A \mid X)}{b(A \mid X)} Y \right]$$

* $b(A \mid X)$: Behavior Policy (the old policy that collected the data).
* $\pi(A \mid X)$: Target Policy (the new policy we want to evaluate).

**Use Cases**
* **Recommender Systems:** Estimating the revenue of a new ranking algorithm using historical logs.
* **Reinforcement Learning:** Counterfactual evaluation without re-running live experiments.

