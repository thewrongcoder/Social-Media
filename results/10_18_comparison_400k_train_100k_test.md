# 10_18 comparison (400,000 train / 100,000 test)

Same split for every model: `test_size=0.2`, `random_state=42` on all 500,000 rows (train fingerprint `99980142373`, test fingerprint `25019607627`).

Linear Regression, LightGBM, XGBoost, the hybrid, Random Forest, and CatBoost are fit once on the full 400,000 training rows. TabICLv2 and TabPFN cannot hold a 400,000-row CPU context, so that same training split is partitioned into 40 contexts of 10,000 rows; each context scores its slice of the same 100,000 test rows. TabPFN uses public v2 weights and 32-row CPU predict batches, splitting any unstable window (min < -1 or max > 15) down to 8 rows.

| Model | R² | MAE | MSE | RMSE |
| --- | ---: | ---: | ---: | ---: |
| Linear Regression | 0.0847 | 0.2795 | 0.5569 | 0.7463 |
| LightGBM | 0.2147 | 0.1592 | 0.4778 | 0.6913 |
| XGBoost | 0.1884 | 0.1622 | 0.4939 | 0.7027 |
| LightGBM + XGBoost Hybrid | 0.2092 | 0.1600 | 0.4812 | 0.6937 |
| Random Forest | 0.2197 | 0.1593 | 0.4748 | 0.6891 |
| CatBoost | 0.2293 | 0.1591 | 0.4690 | 0.6848 |
| **TabICLv2** | **0.2296** | 0.1496 | **0.4687** | **0.6846** |
| TabPFN | 0.0043 | **0.1214** | 0.6058 | 0.7783 |

Higher R² is better; lower MAE/MSE/RMSE is better. Best value in each column is bold.
