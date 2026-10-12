# 09_27 fair comparison: 400,000 train / 100,000 test

One shared `train_test_split(test_size=0.2, random_state=42)` of all 500,000 rows.

- Linear Regression, LightGBM, XGBoost, the hybrid, Random Forest, and CatBoost each train on all 400,000 rows.
- TabPFN v2 and TabICLv2 cannot hold a 400,000-row CPU context, so they partition the same 400k training rows into 10k contexts and score the same 100,000 test rows.

| Model | R² | MAE | MSE | RMSE | MAPE (%) |
|---|---:|---:|---:|---:|---:|
| Linear Regression | 0.0847 | 0.2795 | 0.5569 | 0.7463 | 123.3653 |
| LightGBM | 0.2147 | 0.1592 | 0.4778 | 0.6913 | 160.0843 |
| XGBoost | 0.1884 | 0.1622 | 0.4939 | 0.7027 | 151.7027 |
| LightGBM + XGBoost Hybrid | 0.2092 | 0.1600 | 0.4812 | 0.6937 | 154.8249 |
| Random Forest | 0.2197 | 0.1593 | 0.4748 | 0.6891 | 167.1634 |
| CatBoost | 0.2293 | 0.1591 | 0.4690 | 0.6848 | 164.5762 |
| TabICLv2 | 0.2296 | 0.1496 | 0.4687 | 0.6846 | 164.9302 |
| TabPFN | 0.0592 | 0.1153 | 0.5724 | 0.7566 | 108.3488 |

CSV copy: `results/09_27_comparison_400k_train_100k_test.csv`
