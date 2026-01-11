# 🏠 House Price Prediction

This project builds an end-to-end machine learning pipeline to predict housing prices using multiple regression-based approaches. The focus is **not only final accuracy**, but a **systematic, reproducible model selection and optimization process**, similar in spirit to the task-oriented format of [ai-dev-tasks](https://github.com/snarktank/ai-dev-tasks).

---

## 🎯 Project Goals

* Compare multiple regression models under the **same data and validation protocol**
* Diagnose **underfitting, overfitting, and convergence issues** using learning curves
* Apply **regularization, feature engineering, and optimization strategies** to improve poorly performing models
* Produce a clean, explainable workflow suitable for GitHub and interviews

Target outcome:

> A stable model with **~80% explanatory power (R²)** and well-behaved training dynamics

---

## 📊 Dataset

* **Task**: Regression (house price prediction)
* **Features**: Median income, house age, average rooms, population, geographic features, etc.
* **Target**: House price
* **Data split**:

  * Train: 80%
  * Test: 20% (held out, never used during model selection)

---

## 🧠 Methodology Overview

The project follows a **model-first → diagnosis → optimization** loop:

1. Establish baselines
2. Compare multiple model families using cross-validation
3. Identify weak or unstable models
4. Diagnose failure modes (bias, variance, optimization)
5. Apply targeted fixes
6. Re-evaluate under identical conditions

---

## 🧪 Step 1: Baseline Models

We begin with four conceptually different regression approaches:

### Models Evaluated

1. **Linear Regression**

   * Baseline, low bias control

2. **Polynomial Regression (degree=2)**

   * Captures non-linear feature interactions

3. **SGD Regressor (Linear + Regularization)**

   * Tests stochastic optimization and scalability

4. **Polynomial + SGD Regressor**

   * High-capacity model with stochastic optimization

All models are implemented using **scikit-learn Pipelines** to ensure:

* No data leakage
* Identical preprocessing during cross-validation

---

## 🔁 Step 2: Cross-Validation for Fair Model Comparison

To ensure fair comparison:

* A **fixed KFold split** is shared across all models
* Metrics:

  * R² (primary)
  * MSE (diagnostic)

We use:

* `KFold(n_splits=5, shuffle=True, random_state=42)`
* `cross_validate(..., return_train_score=True)`

This allows us to observe:

* Mean validation performance
* Train vs validation gap (bias–variance insight)
<img width="889" height="490" alt="image" src="https://github.com/user-attachments/assets/53769244-7bff-4f29-859e-c331783cb02f" />

---

## 📈 Step 3: Initial Results & Observations

### Summary of Findings

| Model                         | Train R² | Val R²              | Diagnosis            |
| ----------------------------- | -------- | ------------------- | -------------------- |
| Linear Regression             | Medium   | Medium              | Slight underfitting  |
| Polynomial Regression (deg=2) | High     | Very Low / Negative | Severe overfitting   |
| SGD Regressor                 | Unstable | Extremely poor      | Optimization failure |
| Poly + SGD                    | Diverged | Diverged            | Non-convergent       |

Key insight:

> **Better training score ≠ better model**

---

## 🔍 Step 4: Diagnosing Model Failures

### 1️⃣ Overfitting & Optimization Instability (Polynomial + SGD)

Symptoms:
Training loss decreases initially but oscillates and fails to converge
Extremely poor or negative validation R²
Large variance across cross-validation folds

Root cause:
Model capacity too high due to polynomial feature expansion (degree=2)
SGD optimization becomes unstable in high-dimensional feature space
Learning rate and regularization insufficient to control variance
<img width="790" height="490" alt="image" src="https://github.com/user-attachments/assets/45136113-4fd0-477a-82e3-4e327a2fe45d" />

---

### 2️⃣ Non-Convergence (SGD-based Models)

Symptoms:
* Extremely large negative R²
* Exploding MSE (up to 1e20)

Root causes:
* Learning rate too large
* Sensitivity to feature scale
* Stochastic updates overshooting optimum
##### before  VS after 
<div style="display: flex; gap: 16px; align-items: center;">
  <img src="https://github.com/user-attachments/assets/a3e47432-a19d-410e-ae94-82b55caf96f7"
       alt="Before"
       style="width: 45%; height: auto;" />
  <img <img width="790" height="490" alt="image" src="https://github.com/user-attachments/assets/1ea9d691-413b-45e6-864c-dd9ad95ebf67" />

</div>

---

## 🛠️ Step 5: Model Optimization Strategy

Instead of random trial-and-error, each fix is **targeted to a diagnosed issue**.

### A. Regularization

* Introduced **Ridge (L2)** regularization
* Tuned `alpha` to control model complexity

Result:

* Validation R² improved from ~-1.8 to ~-1.2

---

### B. Reducing Model Capacity

* Reduced polynomial degree from 2 → 1

Observation:

* Degree = 1 polynomial ≡ linear regression
* Confirms that non-linearity was not helping

---

### C. SGD Convergence Control

Key changes:

* `StandardScaler` inside pipeline
* Smaller `eta0`
* `warm_start=True`
* Manual iteration with `max_iter=1`

---

## 📉 Step 6: Learning Curve Analysis (Loss Curves)

To understand optimization behavior, we plot:

* Training MSE vs iteration
* Validation MSE vs iteration (log scale)

### Interpretation

Observed pattern:

* Both losses decrease smoothly
* Validation loss does not rebound
* Stable gap between train and validation

Conclusion:

> The optimized SGD model **converges properly and generalizes stably**

---

## ✅ Final Model Selection

The final selected model balances:

* Predictive performance
* Stability across folds
* Interpretability
* Training efficiency

Rather than chasing maximum R², the focus is on **robust generalization**.

---

## 🧾 Key Takeaways

* Cross-validation must come **before** model optimization
* Loss curves are more informative than single metrics
* Overfitting and optimization failure are distinct problems
* Regularization and learning rate tuning solve different failure modes



