# README.md

## Credit Scoring for Aave V2 Wallets

This repository contains a complete solution for assigning credit scores (0–1000) to wallets based on their historical Aave V2 transaction behavior.

---

### 📁 Repository Structure

```
/ (root)
├── task1_refactored.ipynb      # Jupyter Notebook with full analysis and code
├── wallet_credit_scores_ml.csv # Final wallet credit scores (output)
├── README.md                   # This file: project overview and instructions
└── analysis.md                 # Detailed analysis and score distribution
```

---

### 🛠️ Methodology

1. **Data Loading**

   * Raw JSON file loaded using `json` and flattened via `pd.json_normalize`.

2. **Preprocessing & Feature Engineering**

   * Converted raw `actionData.amount` to token units (6 or 18 decimals).
   * Computed USD value per txn: `amount_converted * assetPriceUSD`.
   * Aggregated per wallet to derive features:

     * `num_txns`, `deposit_usd`, `borrow_usd`, `repay_usd`, `net_borrow_usd`.
     * `num_liquidations`, `num_assets`, `avg_days_between_txns`, `repay_to_borrow_ratio`.

3. **Machine Learning: Unsupervised Clustering**

   * Scaled features with `StandardScaler`.
   * Used Elbow and Silhouette methods to select **k=5** clusters.
   * Fitted `KMeans(n_clusters=5)` and assigned each wallet a cluster label.

4. **Credit Score Mapping**

   * Interpreted cluster centroids to understand behavior:

     * Cluster 2: high-volume whales → score 950
     * Cluster 0: solid mid-tier → score 800
     * Cluster 1: low activity → score 600
     * Cluster 3: over-leveraged → score 400
     * Cluster 4: zero-value bots → score 100
   * Smoothed band-center scores into a continuous 0–1000 range with `MinMaxScaler`.

5. **Visualization & Analysis**

   * Elbow and Silhouette plots; feature distribution histograms; cluster profiles heatmap.
   * Score distribution bar chart (bins: 0–100, 100–200, …, 900–1000).

---

### 🚀 Running the Notebook

1. Open `task1_refactored.ipynb` in Jupyter Lab or Notebook.
2. Ensure dependencies are installed:

   ```bash
   pip install pandas numpy seaborn scikit-learn matplotlib
   ```
3. Run all cells sequentially.
4. The final CSV `wallet_credit_scores_ml.csv` will be generated in the root.

---

### 📋 Output File

* `wallet_credit_scores_ml.csv`: two columns (`wallet`, `credit_score_ml`) for each wallet.
