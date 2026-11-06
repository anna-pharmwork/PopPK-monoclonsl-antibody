# Population Pharmacokinetic Modeling of a Monoclonal Antibody

## Project Overview

This project focuses on the development and evaluation of a population pharmacokinetic (PopPK) model for a hypothetical monoclonal antibody using R and the `nlmixr2` modeling framework.

The objective is to simulate pharmacokinetic data from a virtual patient population, estimate population PK parameters using the FOCEi estimation method, evaluate model performance, and investigate the influence of patient-specific covariates on pharmacokinetic variability.

All data used in this project are simulated and do not represent actual clinical trial observations.

## Objectives

- Generate a virtual population of 100 patients.
- Simulate individual pharmacokinetic profiles following a single intravenous bolus administration.
- Implement a two-compartment pharmacokinetic model.
- Incorporate between-subject variability (BSV) and residual unexplained variability (RUV).
- Estimate population PK parameters using FOCEi.
- Evaluate model performance using goodness-of-fit diagnostics.
- Investigate the effect of body weight on drug clearance.
- Compare baseline and covariate models.

## Methodology

### 1. Virtual Population

A virtual population of 100 individuals was generated with the following demographic characteristics:

- Body weight: mean 70 kg, SD 12 kg.
- Age: mean 50 years, SD 15 years.
- Sex: binary categorical variable.

### 2. Pharmacokinetic Model

A two-compartment model with intravenous administration and linear elimination was implemented.

The structural model is described by:

**Central compartment**

dAc/dt = -(CL/Vc) × Ac - (Q/Vc) × Ac + (Q/Vp) × Ap

**Peripheral compartment**

dAp/dt = (Q/Vc) × Ac - (Q/Vp) × Ap

Where:

- CL — systemic clearance (L/day)
- Vc — central volume of distribution (L)
- Q — intercompartmental clearance (L/day)
- Vp — peripheral volume of distribution (L)
- Ac — amount of drug in the central compartment
- Ap — amount of drug in the peripheral compartment

### 3. Population Variability

Between-subject variability was incorporated using an exponential random-effects model.

Individual clearance:

CLᵢ = CLpop × (WTᵢ / 70)^0.75 × exp(ηCL,i)

Individual central volume:

Vcᵢ = Vcpop × (WTᵢ / 70) × exp(ηVc,i)

Residual variability was simulated using a proportional error model.

### 4. Model Estimation

Population parameters were estimated using the First-Order Conditional Estimation with Interaction (FOCEi) method implemented in `nlmixr2`.

### 5. Model Diagnostics

The baseline model was evaluated using:

- Observed versus population-predicted concentrations (DV vs PRED).
- Observed versus individual-predicted concentrations (DV vs IPRED).
- Conditional weighted residuals versus time (CWRES vs TIME).
- Individual prediction profiles.

### 6. Covariate Analysis

Body weight is investigated as a potential covariate affecting systemic clearance.

The covariate model is defined as:

CLᵢ = CLpop × (WTᵢ / 70)^θWT × exp(ηCL,i)

The estimated covariate model will be compared with the baseline model using changes in the objective function value (OFV), parameter precision, and between-subject variability.

## Preliminary Results

The baseline model was successfully estimated using FOCEi.

| Parameter | Simulation value | Estimated value |
|---|---:|---:|
| CL (L/day) | 0.200 | 0.207 |
| Vc (L) | 3.000 | 2.950 |
| Q (L/day) | 0.100 | 0.0987 |
| Vp (L) | 2.000 | 1.880 |

Baseline model statistics:

- OFV: 28486.03
- AIC: 49268.04
- BIC: 49319.37
- IIV CL: 28.8% CV
- IIV Vc: 26.3% CV

The estimated population parameters were close to the values used to generate the simulated dataset.

## Technology Stack

- R
- nlmixr2
- rxode2
- ggplot2
- dplyr
- tidyr
- FOCEi estimation

## Project Status

- [x] Virtual population generation
- [x] Two-compartment PK simulation
- [x] Residual variability implementation
- [x] Baseline PopPK model estimation
- [x] Goodness-of-fit diagnostics
- [ ] Body weight covariate model estimation
- [ ] Statistical model comparison
- [ ] Final model evaluation

## Limitations

This is an educational modeling project based on simulated data. The model does not represent a validated clinical model for any specific monoclonal antibody.

## Author

Pharmacometrics and quantitative pharmacology portfolio project.
