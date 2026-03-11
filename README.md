# DigiCow Farmer Training Adoption Prediction

This repository contains my solution to the **DigiCow Africa Farmer Adoption Challenge** hosted on Zindi.

The objective was to **predict whether farmers would adopt improved agricultural practices after receiving training**, across three time horizons.

🏆 **Final Rank: 11th out of 388 participants**

---

# Problem Overview

Agricultural training programs often struggle with a key issue: farmers attend training sessions but do not always implement the practices afterward.

The task was to predict the probability that a farmer adopts a practice within:

| Target | Description |
|------|------|
| 7 Days | Adoption within one week |
| 90 Days | Adoption within three months |
| 120 Days | Adoption within four months |

The **7-day prediction carried the highest weight** in the competition scoring.

---

# Dataset

The competition provided four datasets:

| File | Description |
|-----|-------------|
| `Train.csv` | Labeled training sessions |
| `Test.csv` | Sessions requiring predictions |
| `Prior.csv` | Historical adoption records |
| `SampleSubmission.csv` | Required submission format |

Each record included information such as:

- Farmer group
- Trainer
- County
- Training topics
- Demographics
- Cooperative membership

---

# Key Challenge

Only **~1.13% of farmers adopted within 7 days**, creating an extreme class imbalance.

The competition evaluated models using:

- **Log Loss**
- **AUC (Area Under the ROC Curve)**

---

# Approach

The final solution consisted of several key components:

### Feature Engineering

Most of the performance improvement came from engineered features, including:

- **Peer adoption features** – capturing how adoption behavior spreads within farmer groups
- **Farmer historical behavior** – time-decayed adoption statistics based on past sessions
- **Topic features** – measuring how different training topics influence adoption
- **Group / trainer priors** – historical effectiveness of trainers, groups, and regions
- **Target encoding** for categorical variables

---

### Modeling

A **diverse ensemble of models** was trained, including:

- HistGradientBoosting
- LightGBM (GBDT & DART)
- CatBoost
- Logistic Regression

Models were trained using **5-fold cross-validation and multiple random seeds** to increase diversity.

---

### Ensemble Blending

Predictions from multiple models were combined using **optimized blend weights** to minimize out-of-fold log loss.

---

### Probability Calibration

Since positive cases were extremely rare, probability calibration was applied using:

- Platt scaling
- Beta calibration
- Isotonic regression

Calibration significantly improved log loss performance.

---

# Final Performance

Out-of-fold model performance:

| Target | Log Loss | AUC |
|------|------|------|
| 7-Day | 0.020448 | 0.991267 |
| 90-Day | 0.027401 | 0.988192 |
| 120-Day | 0.030665 | 0.987013 |

Estimated leaderboard score:

0.9813


---

# Leaderboard Results

| Leaderboard | Score |
|-------------|-------|
| Public | 0.958938 |
| Private | 0.953083 |

Final placement:

**11th out of 388 participants**

---
