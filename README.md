🏠 House Price Prediction — Model Selection & Diagnosis

This project focuses on how to systematically evaluate, diagnose, and iterate on regression models, rather than maximizing accuracy through blind tuning.

The core question is:

If multiple models perform poorly, how do we know whether to tune further or switch model families?

🔍 Workflow

Establish linear baselines

Compare models using fixed cross-validation splits

Diagnose bias, variance, and optimization behavior

Apply targeted fixes

Decide whether linear models are fundamentally sufficient

🧪 Models Compared

All models use identical preprocessing and CV splits.

Linear Regression

Polynomial Regression

SGD Regressor

Polynomial + SGD

📊 Key Observations
Model	Validation Behavior	Interpretation
Linear Regression	Stable, R² ≈ 0.6	Limited expressiveness
Polynomial Regression	Train ↑, Val ↓	Overfitting
SGD Regressor	Negative R²	Optimization instability
Poly + SGD	Diverged	Capacity + optimization failure

Higher model complexity did not improve generalization.

🔧 Optimization & Diagnosis

Instead of random tuning, fixes were mapped to specific failure modes:

Overfitting → L2 regularization, reduced polynomial degree

Non-convergence → smaller learning rate, feature scaling, warm-start SGD

Optimization instability → loss curve inspection (train vs validation)

Loss Curve Insight (Before vs After)
<p float="left"> <img src="https://github.com/user-attachments/assets/45136113-4fd0-477a-82e3-4e327a2fe45d" width="45%" /> <img src="https://github.com/user-attachments/assets/0245ab65-a328-4471-909f-a7eacc6d0097" width="45%" /> </p>
Loss Curve Insight (Before vs After)
<p float="left">
  <img src="https://github.com/user-attachments/assets/a3e47432-a19d-410e-ae94-82b55caf96f7" width="45%" alt="Before" />
  <img src="https://github.com/user-attachments/assets/1ea9d691-413b-45e6-864c-dd9ad95ebf67" width="45%" alt="After" />
</p>


After tuning, SGD models converge smoothly, but final performance remains capped.

🧠 Final Conclusion

Linear and regularized linear models plateau at R² ≈ 0.6

Polynomial expansion does not help

Optimization issues can be fixed, but model capacity remains insufficient

👉 This indicates that housing prices exhibit strong non-linear and interaction effects not captured by linear models.

🔜 Next Step

To better model these non-linear effects, the next phase of this project explores tree-based ensemble models, including:

RandomForestRegressor – variance reduction via bagging and feature randomness

GradientBoostingRegressor – sequential error correction with additive weak learners

These models are better suited for capturing complex interactions and are expected to significantly improve predictive performance.

GradientBoostingRegressor
outpu is accuracy around 82% foe the testing set, which dramatically increase compare to the liear regression models.
<img width="922" height="654" alt="image" src="https://github.com/user-attachments/assets/034d830d-58b2-4120-a018-2f8d6aa50218" />
RandomForestRegressor
<img width="946" height="646" alt="image" src="https://github.com/user-attachments/assets/d9bd6a02-0b7b-4023-9f28-ed81999354bf" />

