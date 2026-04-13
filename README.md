# Clinical Trial Design: Digital vs. Face-to-Face CBT for Major Depressive Disorder

**Course:** STA 652 — Spring 2026 | Miami University, Dept. of Statistics  
**Author:** Richel Ohenewaa Attafuah

---

## Overview

This project presents a full statistical design proposal for a randomized controlled non-inferiority trial evaluating whether **therapist-supported digital cognitive behavioral therapy (dCBT)** achieves clinically comparable outcomes to **standard face-to-face CBT (FtF-CBT)** in adults with major depressive disorder (MDD).

Major depressive disorder affects approximately 21 million U.S. adults and 5% of the global adult population. While face-to-face CBT is an established first-line treatment, barriers including cost, therapist availability, and transportation limit access. A digitally delivered alternative could substantially improve reach — but only if clinical equivalence can be established.

---

## Clinical Question

> Is therapist-supported digital CBT **non-inferior** to face-to-face CBT in reducing depressive symptom severity (PHQ-9) at 12 weeks in adults with MDD?

---

## Trial Design

| Design Element | Specification |
|---|---|
| Trial Type | Two-arm parallel-group randomized controlled trial |
| Allocation | 1:1, computer-generated sequence |
| Analysis Principle | Intention-to-treat (ITT) |
| Primary Outcome | PHQ-9 score at 12 weeks |
| Non-Inferiority Margin (Δ) | 2 PHQ-9 points |
| One-Sided α | 0.025 |
| NI Decision Rule | Upper bound of 95% CI for δ = μD − μF < Δ |
| Stratified Randomization | Baseline PHQ-9 severity; antidepressant use |

### Treatment Arms
- **Arm 1 — dCBT:** Structured CBT modules via secure online platform with brief weekly therapist support (messaging or telehealth check-ins)
- **Arm 2 — FtF-CBT:** Standard manualized CBT, weekly 50–60 minute in-person sessions for 12 weeks

---

## Primary Statistical Analysis

The primary analysis uses **ANCOVA** to adjust for baseline PHQ-9, improving precision by reducing residual variance:

```
PHQ9_12wk = β₀ + β₁·Treatment + β₂·PHQ9_baseline + ε
```

Non-inferiority is declared if the **upper bound of the two-sided 95% CI** for the treatment effect β₁ is strictly less than Δ = 2 — equivalent to a one-sided test at α = 0.025.

---

## Simulation-Based Power Analysis

Rather than using closed-form formulas, power was estimated via **Monte Carlo simulation** to directly reflect the ANCOVA design and correlated outcomes.

### Assumptions

| Parameter | Value |
|---|---|
| Non-inferiority margin (Δ) | 2 PHQ-9 points |
| SD of 12-week PHQ-9 (σ) | 6.9 |
| True treatment difference (δ_true) | 0 (equivalence assumed) |
| Baseline–follow-up correlation (ρ) | 0.5 |
| One-sided α | 0.025 |
| Target power | 90% |
| Simulations per sample size | 2,000 |

### Simulation Procedure (per trial)

1. Simulate baseline PHQ-9 scores ~ N(15, 4²)
2. Generate correlated follow-up values using ρ = 0.5
3. Fit ANCOVA model
4. Compute upper 95% CI bound for treatment effect
5. Declare non-inferiority if upper bound < Δ

Power = proportion of simulations concluding non-inferiority.

### Results

| n per group | Estimated Power |
|---|---|
| 120 | ~0.73 |
| 160 | ~0.83 |
| 200 | **≥ 0.90** ✓ |
| 260 | ~0.96 |

**Minimum sample achieving 90% power: 200 per group (400 analyzable)**

Adjusting for **20% anticipated attrition:**

```
n_enroll = 200 / 0.80 = 250 per group → Total enrollment: 500 participants
```

---

## Repository Contents

| File | Description |
|---|---|
| `R Code - Major Depressive Disorder.Rmd` | Full R Markdown: single-trial simulation, power function, Monte Carlo sweep |
| `Report - Major Depressive Disorder.html` | Rendered HTML report with code and output |
| `Final_Report - Major Depressive Disorder.pdf` | Final written proposal (introduction, design, hypotheses, analysis plan, results) |
| `README.md` | This file |

---

## How to Run

1. Clone this repository
2. Open `R Code - Major Depressive Disorder.Rmd` in RStudio
3. Install required packages if needed:
```r
install.packages(c("knitr", "rmarkdown"))
```
4. Knit to HTML or run chunks individually to reproduce the simulation

---

## Key Methods Demonstrated

- Non-inferiority randomized controlled trial design
- ANCOVA with baseline covariate adjustment
- Simulation-based (Monte Carlo) power analysis in R
- Correlated outcome generation using Cholesky decomposition approach
- Attrition-adjusted sample size calculation
- ITT analysis framework and stratified randomization

---

## References

- Kroenke, K., Spitzer, R.L., & Williams, J.B.W. (2001). The PHQ-9: Validity of a brief depression severity measure. *Journal of General Internal Medicine*, 16(9), 606–613.
- National Institute of Mental Health. (2023). Major Depression. https://www.nimh.nih.gov/health/statistics/major-depression
- World Health Organization. (2023). Depression Fact Sheet. https://www.who.int/news-room/fact-sheets/detail/depression
- American Psychological Association. (2019). Clinical practice guideline for the treatment of depression across three age cohorts.
