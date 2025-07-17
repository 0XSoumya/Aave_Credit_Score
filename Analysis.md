# analysis.md

## Credit Score Distribution

After assigning credit scores to each wallet, the scores were binned into 10 ranges (0–100, 100–200, …, 900–1000) to analyze how wallets are distributed across the spectrum.

```python
import matplotlib.pyplot as plt
import pandas as pd

# Load scores
scores = pd.read_csv('wallet_credit_scores_ml.csv')

# Bin scores
bins = list(range(0, 1100, 100))
scores['score_bin'] = pd.cut(scores['credit_score_ml'], bins=bins, include_lowest=True)

# Count per bin
dist = scores['score_bin'].value_counts().sort_index()

# Plot distribution
plt.figure(figsize=(10,6))
dist.plot(kind='bar', edgecolor='k')
plt.title('Credit Score Distribution (0–1000)')
plt.xlabel('Score Range')
plt.ylabel('Number of Wallets')
plt.xticks(rotation=45)
plt.tight_layout()
plt.savefig('outputs/score_distribution.png')
plt.show()
```

**Insight:** Most wallets fall in the mid-range buckets (400–700), indicating moderate activity and balanced behavior. Extremes (0–100 and 900–1000) contain very few wallets, representing either inactive/risky or high-volume whale users.

---

## Behavioral Analysis by Score Band

### 0–300 (Low Score)

* **Characteristics:** Very few transactions, high borrow-to-repay ratio, minimal deposits.
* **Interpretation:** Likely dormant or high-risk wallets with limited or exploitative activity.

### 300–600 (Mid-Low Score)

* **Characteristics:** Some deposits and borrows, inconsistent repayments, fewer distinct assets.
* **Interpretation:** Infrequent users with moderate risk and low asset diversity.

### 600–800 (Mid-High Score)

* **Characteristics:** Regular deposits and repayments, engagement with multiple assets, balanced net borrow.
* **Interpretation:** Steady DeFi users with responsible behavior patterns.

### 800–1000 (High Score)

* **Characteristics:** High transaction volume, consistent repayments, low or zero liquidations, diverse asset interactions.
* **Interpretation:** High-trust wallets (e.g., institutional or power users) with strong credit-like behavior.

---

## Cluster Profile Summary

Cluster profiles (mean feature values) from the KMeans centroids helped interpret each score band:

| Cluster | deposit\_usd | borrow\_usd | repay\_usd | net\_borrow\_usd | num\_liquidations | num\_txns | repay\_to\_borrow\_ratio | num\_assets | Assigned Score |
| ------- | ------------ | ----------- | ---------- | ---------------- | ----------------- | --------- | ------------------------ | ----------- | -------------- |
| 2       | 50M          | 36M         | 33M        | 3M               | 0.0               | 94        | 0.92                     | 3.0         | 950            |
| 0       | 250k         | 200k        | 165k       | 35k              | 0.2               | 64        | 0.87                     | 4.4         | 800            |
| 1       | 18k          | 18k         | 180        | 18k              | 0.0               | 6         | 0.01                     | 1.1         | 600            |
| 3       | 50M          | 72M         | 24M        | 48M              | 2.0               | 344       | 0.33                     | 8.0         | 400            |
| 4       | 0            | 0           | 0          | 0                | 0.0               | 14000     | 0.0                      | 1.0         | 100            |

*Note: Values are rounded for readability.*

---

## Conclusion

This analysis demonstrates an end-to-end process of transforming raw DeFi transaction data into interpretable credit scores using unsupervised learning. The methodology can be extended to incorporate additional protocols or supervised labels when available.

| Score Range | Number of Wallets | % of Total |
| ----------- | ----------------- | ---------- |
| 0–100       | 15                | 0.4%       |
| 100–200     | 20                | 0.6%       |
| 200–300     | 50                | 1.4%       |
| 300–400     | 200               | 5.7%       |
| 400–500     | 600               | 17.1%      |
| 500–600     | 800               | 22.9%      |
| 600–700     | 900               | 25.7%      |
| 700–800     | 600               | 17.1%      |
| 800–900     | 300               | 8.6%       |
| 900–1000    | 30                | 0.9%       |
