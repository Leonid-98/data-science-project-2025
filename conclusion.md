# Final thoughts

- The baseline Ridge run showed the data pipeline works, so I kept that notebook as a sanity check.
- The improved notebook stacked smarter features (TotalSF, bath counts, logs/ratios) plus HistGB/GBR/RF grids, which got log-RMSE down to ~0.137 on the validation split.
- The final notebook doubled down on those ideas: more crafted features, 5x2 repeated CV, and a full registry of ensembles (bagging, stacking, voting, log-target) so I could export multiple submissions and blends.
- Best single models so far: `Bagging_HGB` for Kaggle’s metric and `GBR_depth3` for plain RMSE; blended submissions mix those top performers for leaderboard tries.

## Leaderboard snapshot (lower = better log-RMSE)

| Submission file | LB score | Notes |
| --- | --- | --- |
| `blend_weighted_submission.csv` | **0.12595** | Weighted mix of top four CV models; currently best performer. |
| `blend_top3_submission.csv` | 0.12614 | Equal-weight version of the strongest trio; barely trails the weighted blend. |
| `Bagging_HGB_submission.csv` | 0.12718 | Single-model winner on Kaggle’s metric, so it anchors most blends. |
| `HGB_tuned_2_submission.csv` | 0.12985 | Old improved-model champ; still solid but eclipsed by bagging + blends. |
| `GBR_2_submission.csv` | 0.13144 | Best RMSE model from the improved notebook; useful for variety in blends. |
| `GBR_depth3_submission.csv` | 0.13331 | Final-model RMSE pick; adds diversity but needs blending to match top scores. |

Takeaways: ensembles alone still beat single models, but blending high-performing trees closes the gap to ~0.126 log-RMSE. Keeping a few distinct single submissions (bagging HGB, GBR variants) is still handy for diagnosing shifts, yet leaderboard-wise the blended CSVs are now the go-to uploads.

```
Raw CSVs
    │
    v
Feature Engineering (ratios, logs, flags)
    │
    v
ColumnTransformer (num + cat pipelines)
    │
    v
Model Zoo (HistGB / GBR / RF / ExtraTrees / KernelRidge / Blends)
    │
    v
Repeated K-Fold CV → Leaderboard table
    │
    v
Full-train fit → Submission CSVs -> Kaggle CLI
```

