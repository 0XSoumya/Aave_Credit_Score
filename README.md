# 🧠 Credit Score Prediction from Aave Wallet Activity

## 📌 Problem Statement

Given historical on-chain activity of user wallets on the Aave V2 protocol, assign a **credit score (0–1000)** to each wallet that reflects its financial health and responsible behavior, without access to repayment history.

---

## 🛠️ Approach Summary

We leveraged unsupervised learning (KMeans clustering) to identify wallet behavior patterns and map them to credit score bands.

### 🔄 Pipeline Overview

1. **Data Loading**

   * Loaded JSON-formatted data into a structured Pandas DataFrame.

2. **Preprocessing & Feature Engineering**

   * Extracted features from nested `actionData`.
   * Computed wallet-level features like:

     * Number of transactions
     * Total borrowed & repaid amounts
     * Number of liquidations, collateral usage, etc.

3. **Clustering with KMeans**

   * Scaled features using StandardScaler.
   * Used Elbow method to select optimal `k=5` clusters.
   * Mapped each cluster to a score band (100 to 900).

4. **Credit Score Assignment**

   * Mapped clusters to credit score bands.
   * Smoothened scores within bands using MinMaxScaler.

5. **Export**

   * Saved final credit scores to `wallet_credit_scores_ml.csv`

---

## 📈 Dependencies

* Python 3.8+
* pandas
* numpy
* matplotlib
* seaborn
* scikit-learn

Install using:

```bash
pip install -r requirements.txt
```

---

## 🚀 How to Run

1. Open `task1` in Jupyter Notebook.
2. Run all cells sequentially.
3. Final scores will be exported as `wallet_credit_scores_ml.csv`

---

## 🤔 Assumptions & Notes

* Data contains wallets from the Aave V2 protocol.
* No direct credit history is used—only transaction patterns.
* Clustering is an approximation of behavior segmentation.

---

## 📬 Contact

For questions, reach out to the author via GitHub issues.
