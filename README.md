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
<p float="left"> <div style="display: flex; gap: 16px; align-items: center;"> <img src="https://github.com/user-attachments/assets/a3e47432-a19d-410e-ae94-82b55caf96f7" alt="Before" style="width: 45%; height: auto;" /> <img <img width="790" height="490" alt="image" src="https://github.com/user-attachments/assets/1ea9d691-413b-45e6-864c-dd9ad95ebf67" /> </p>

After tuning, SGD models converge smoothly, but final performance remains capped.

🧠 Final Conclusion

Linear and regularized linear models plateau at R² ≈ 0.6

Polynomial expansion does not help

Optimization issues can be fixed, but model capacity remains insufficient

👉 This indicates that housing prices exhibit strong non-linear and interaction effects not captured by linear models.
