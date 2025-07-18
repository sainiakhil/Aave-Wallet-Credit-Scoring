
# 💳 Aave Wallet Credit Scoring

This project is focused on building a machine learning pipeline to assign **credit scores** to wallets interacting with the **Aave V2 protocol**, based on their transaction behavior.

---

## 🧠 Problem Statement

Given a dataset of wallet-level transaction records including actions like `deposit`, `borrow`, `repay`, and `liquidationcall`, we aim to:

* Engineer behavior-based features from raw transaction logs.
* Use unsupervised learning to detect anomalous patterns.
* Translate these anomalies into **credit scores** (0–1000 scale), where higher scores represent more reliable wallet behavior.

---

## 🏗️ Methodology & Architecture

### ✅ Step 1: Data Loading & Preprocessing

* Loaded nested JSON-like structures from the Aave V2 data dump.
* Flattened transaction records into a wallet-level format.
* Removed irrelevant fields and cleaned malformed entries.

### ✅ Step 2: Feature Engineering

Engineered features to capture transactional behavior:

* `total_volume`: Sum of all transaction amounts.
* `transaction_count`: Number of transactions by wallet.
* `average_volume`: Mean transaction amount.
* `duration_active`: Time between first and last transaction.

These features help characterize wallet activity in terms of **scale**, **frequency**, and **consistency**.

### ✅ Step 3: Data Preparation

* Applied **StandardScaler** to normalize feature distributions.
* Split the data into **training** and **testing** sets.

### ✅ Step 4: Unsupervised Model Training

* Chose **Isolation Forest**, an unsupervised anomaly detection model, as:

  * The problem lacked labeled data.
  * We aimed to score unusual (potentially risky) wallet behaviors.

* Trained on the scaled feature set to learn normal behavior and score anomalies.

### ✅ Step 5: Credit Score Assignment

* Converted **anomaly scores** from Isolation Forest into a **credit score** between 0 and 1000.
* Used a **linear transformation**, where:

  * Lower anomaly score → Higher credit score.
  * Higher anomaly score → Lower credit score.

---

## 🔁 Processing Flow

```plaintext
Raw Aave V2 Transaction Data
        ↓
Flatten & Clean Data
        ↓
Feature Engineering (volume, count, frequency)
        ↓
Feature Scaling (StandardScaler)
        ↓
Isolation Forest (Unsupervised Learning)
        ↓
Anomaly Score Calculation
        ↓
Linear Transformation to Credit Score (0–1000)
        ↓
Wallet-Level Credit Scoring Output
```

---

## 📊 Evaluation & Insights

* Visualized distribution of credit scores using histogram bins.

* High-scoring wallets (800–1000) tend to:

  * Have higher transaction volumes.
  * Be more active over time.
  * Show regular patterns across borrow/repay cycles.

* Low-scoring wallets exhibit sporadic or minimal activity—flagged as risky.

---

