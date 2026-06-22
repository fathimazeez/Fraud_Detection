Click here for code link : https://drive.google.com/drive/folders/1ukb7MozqcuRwNtNvmTX6Yl5IT-557o02?usp=drive_link
# Fraud_Detection
Credit Card Fraud Detection is a machine learning project that identifies fraudulent transactions using a real-world dataset. It applies SMOTE for data balancing, compares Logistic Regression and Random Forest models, evaluates performance with fraud-specific metrics, and deploys the best model as a Streamlit web app for real-time fraud prediction.
# 💳 Credit Card Fraud Detection

A machine learning project that detects fraudulent credit card transactions and deploys the model as an interactive web app using **Streamlit**.

---

## 📌 Project Description

Credit card fraud causes billions of dollars in losses every year, and fraudulent transactions make up a tiny fraction of all transactions — making them hard to detect with simple rules. This project builds a complete, beginner-friendly machine learning pipeline that:

1. Loads real-world, anonymized credit card transaction data
2. Explores the data to understand patterns between fraud and legitimate transactions
3. Preprocesses and balances the data (since fraud cases are extremely rare)
4. Trains and compares two classification models
5. Evaluates them using metrics suited for imbalanced data (not just accuracy)
6. Saves the best-performing model
7. Deploys it as a live, interactive **Streamlit web app** where anyone can upload transactions or enter values manually and instantly get a fraud prediction

The goal is to demonstrate an end-to-end ML workflow — from raw data to a usable, deployed tool — in a way that's easy to follow and modify.

---

## 📂 Dataset

This project uses the **[Credit Card Fraud Detection dataset](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud)** from Kaggle.

- ~284,800 transactions made by European cardholders over 2 days
- Only **0.17%** of transactions are fraudulent — a heavily imbalanced dataset
- Features:
  - `Time` — seconds elapsed since the first transaction
  - `V1`–`V28` — anonymized features generated via PCA (original details hidden for privacy)
  - `Amount` — transaction amount
  - `Class` — target label (`0` = legitimate, `1` = fraud)

> ⚠️ The dataset (`creditcard.csv`) is **not included** in this repo due to size/licensing. Download it from Kaggle and place it in the project folder before running the training script.

---

## 🧠 How It Works

### 1. Exploratory Data Analysis (EDA)
Visualizes the severe class imbalance and the distribution of transaction amounts for fraud vs. legitimate transactions.

### 2. Preprocessing
- Scales the `Time` and `Amount` columns using `StandardScaler` (the `V1`–`V28` features are already PCA-scaled)
- Splits data into training and test sets, preserving the fraud/legit ratio (stratified split)

### 3. Handling Class Imbalance — SMOTE
Since fraud cases are so rare, the model would otherwise barely learn from them. **SMOTE** (Synthetic Minority Over-sampling Technique) generates synthetic fraud examples in the training set so the model can learn fraud patterns more effectively, without touching the test set (to keep evaluation realistic).

### 4. Model Training
Two models are trained and compared:
| Model | Why it's used |
|---|---|
| **Logistic Regression** | Simple, fast, interpretable baseline |
| **Random Forest** | Captures non-linear patterns, usually more accurate |

An **Isolation Forest** (unsupervised anomaly detector) is also trained as a bonus model that flags unusual transactions without needing labeled fraud examples.

### 5. Evaluation
Because fraud is rare, accuracy alone is misleading (a model predicting "legit" every time would still be ~99.8% accurate). Instead, the project evaluates models using:
- **ROC-AUC** — how well the model ranks fraud vs legit transactions
- **Average Precision** — performance specifically on the rare fraud class
- **F1 Score** — balance between precision and recall
- A full **classification report** (precision, recall per class)

The model with the highest ROC-AUC is automatically selected and saved as the deployment model.

### 6. Deployment — Streamlit App
The saved model is loaded into a **Streamlit** web app with two ways to use it:
- **📁 Upload CSV** — score a batch of transactions at once and see predictions in a table
- **✍️ Manual Entry** — enter a single transaction's values and get an instant fraud/legit prediction with a probability score

---

## 🗂️ Project Structure

```
├── train_fraud_model.py        # Full training pipeline (EDA → preprocessing → SMOTE → training → evaluation → saving)
├── app.py                      # Streamlit app for live predictions
├── creditcard.csv              # Dataset (download from Kaggle, not included)
└── fraud_model_outputs/        # Generated after training
    ├── fraud_model.pkl         # Best trained model
    ├── anomaly_model.pkl       # Isolation Forest (unsupervised)
    ├── scaler.pkl              # Fitted StandardScaler
    ├── columns.pkl             # Feature column order
    └── eda.png                 # EDA visualization
```

---

## 🚀 How to Run

### Option A: Google Colab
1. Upload `creditcard.csv` to your Colab session
2. Run the training script:
   ```python
   !python train_fraud_model.py
   ```
3. Write and launch the Streamlit app using `pyngrok` to get a public link (see deployment steps in the project notes)

### Option B: Local Machine
```bash
pip install streamlit pandas scikit-learn imbalanced-learn joblib

# 1. Train the model
python train_fraud_model.py

# 2. Launch the app
streamlit run app.py
```
Then open the local URL Streamlit prints (usually `http://localhost:8501`).

---

## 📊 Example Output

After training, you'll see console output like:

```
--- Random Forest ---
ROC-AUC: 0.97xx | Avg Precision: 0.85xx | F1: 0.86xx
              precision    recall  f1-score   support
       Legit       1.00      1.00      1.00     56864
       Fraud       0.xx      0.xx      0.xx        98
```

And the Streamlit app lets you interactively test transactions and see results like:

> 🚨 Likely FRAUDULENT transaction — probability: 92.4%

---

## ⚠️ Limitations & Next Steps

- The `V1`–`V28` features are anonymized PCA components, so the model can't explain *why* a transaction looks fraudulent in real-world terms.
- SMOTE creates synthetic data only for training — real-world deployment would need ongoing retraining as fraud patterns evolve.
- Possible improvements: add XGBoost/LightGBM, threshold tuning for business-specific cost tradeoffs (false positives vs false negatives), or integrating the Isolation Forest's anomaly score as a secondary risk signal in the app.

---

## 📜 Credits

- Dataset: [ULB Machine Learning Group](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud) via Kaggle
- Built with: `scikit-learn`, `imbalanced-learn`, `pandas`, `matplotlib`, `seaborn`, `Streamlit`
