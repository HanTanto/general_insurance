# General Insurance Claim Distribution and Aggregate Simulation

## Overview
This project explores loss distribution modelling, claim frequency modelling, and aggregate claim simulation for general and health insurance products. It combines statistical distribution fitting, actuarial policy modifications (deductible, policy limit, coinsurance), and Monte Carlo simulation to represent insurance risk and estimate risk measures such as Value-at-Risk (VaR) and Expected Shortfall (ES).

The project is organized into three notebooks, each covering a distinct part of the analysis:

| Notebook | Focus |
|---|---|
| `Modifikasi_besar_klaim.ipynb` | Loss severity distribution fitting and policy modifications |
| `Pemodelan_distribusi.ipynb` | Claim frequency modelling using discrete distributions |
| `Simulasi_Agregate_Klaim.ipynb` | Aggregate claim simulation and risk measures |

---

## 1. Loss Distribution & Policy Modifications — `Modifikasi_besar_klaim.ipynb`

Models the severity (size) of hospitalization insurance claims and explores how common actuarial policy modifications affect the resulting loss distribution.

- **Dataset:** [Insurance Claim Dataset (Kaggle)](https://www.kaggle.com/code/kerneler/starter-insurance-claim-dataset-efa06de4-6/input)
- Fitted candidate distributions (Gamma, Weibull, Lognormal) to claim severity data; selected **Lognormal** based on RMSE and validated with a **Kolmogorov-Smirnov goodness-of-fit test**
- Derived and implemented, both analytically and numerically, the distribution function, expectation, variance, quantile function, and random sample generation for three policy modifications:
  - **Deductible** — insurer only pays the loss exceeding a threshold *d*
  - **Policy limit** — insurer caps payout at a maximum *u*
  - **Combined deductible–policy limit–coinsurance** — insurer shares a proportion *c* of the loss within a bounded range
- Compared how each modification affects the mean and variability of claims paid by the insurer, with implications for product design and target policyholder segments

## 2. Claim Frequency Modelling — `Pemodelan_distribusi.ipynb`

Models the frequency of insurance events using discrete probability distributions, estimated via maximum likelihood and validated against empirical data.

- **Dataset:** BPJS Kesehatan (Indonesian national health insurance) claims data, 2015–2016, West Java province. *Note: this dataset was provided for academic coursework and is not included in this repository due to data confidentiality; only the modelling approach and code are shared.*
- **Poisson distribution** — modelled length-of-stay (days hospitalized); identified overdispersion in the real data and addressed it with a **zero-modified Poisson** distribution
- **Binomial distribution** — modelled the number of female claimants in samples of policyholders via bootstrap simulation
- **Geometric distribution** — modelled the number of claims from one gender occurring before the other, using order-of-arrival data
- Estimated parameters via maximum likelihood estimation (MLE) and evaluated fit visually and via empirical vs. theoretical probability mass functions

## 3. Aggregate Claim Simulation — `Simulasi_Agregate_Klaim.ipynb`

Simulates and compares two candidate models for aggregate claims, and evaluates their tail risk.

- Proved analytically (via moment generating functions) that **S_N = X₁+X₂+...+X_N is not identical to N·X** in distribution, and derived their respective means and variances
- Simulated both models under **N ~ Poisson(2)**, **X ~ Exponential(mean 100)** with 1,000,000 iterations to visualize the difference in distribution shape and tail behaviour
- Computed and compared **Value-at-Risk (VaR)** and **Expected Shortfall (ES)** at 90%, 95%, 97.5%, and 99% confidence levels between the two models
- Demonstrated that the correct simplification of aggregate claims is **S_N = N · X̄** (N times the *average* claim size), and validated this via simulation

---

## Key Insights
- Standard accuracy-style intuition can be misleading in actuarial modelling — e.g., naively assuming *S_N = N·X* materially understates tail risk compared to the correct formulation
- Deductibles reduce mean claims but leave insurer exposure unbounded; policy limits bound exposure but reduce product attractiveness; combining deductible, policy limit, and coinsurance balances both
- Real-world claim frequency data can violate textbook assumptions (e.g., Poisson's equidispersion) — overdispersion required a zero-modified distribution to fit properly

## Tech Stack
Python | pandas | NumPy | Matplotlib | SciPy (`scipy.stats`)

## How to Run
```bash
pip install -r requirements.txt
jupyter notebook Modifikasi_besar_klaim.ipynb
```
> Note: `Pemodelan_distribusi.ipynb` depends on the private BPJS dataset and Google Colab/Drive mounting, and will not run end-to-end outside that environment. The Poisson-fitting logic on synthetic data (Section 1 of that notebook) will still run standalone.
