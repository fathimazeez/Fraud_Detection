Click here for code link : https://drive.google.com/drive/folders/1ukb7MozqcuRwNtNvmTX6Yl5IT-557o02?usp=drive_link

# 🔍 FinTech Compliance System

A end-to-end financial crime detection system built to simulate real-world fintech compliance workflows.

> Built as a portfolio project targeting fintech compliance roles — covers Fraud Detection, AML Monitoring, GenAI Alert Summarization, RAG Policy Q&A, and KYC Verification.

---

## 🎯 What This Project Does

Most fraud detection projects stop at predicting whether a transaction is fraudulent. This project goes further by adding four additional compliance layers on top — combining everything into a single live dashboard.

| Layer | What It Does |
|-------|-------------|
| 🤖 ML Fraud Detection | XGBoost model scores every transaction for fraud probability |
| 🚨 AML Monitoring | Rule-based engine catches money laundering patterns ML misses |
| 💬 GenAI Alerts | Gemini LLM writes compliance alert paragraphs for flagged transactions |
| 📚 RAG Policy Q&A | Ask AML questions answered from FATF policy documents via Pinecone |
| 🪪 KYC Verification | Extract and validate customer identity fields from documents |

---

## 🖥️ Dashboard Preview

| Page | Description |
|------|-------------|
| 📊 Transaction Scoring | Upload CSV → get fraud scores, AML flags, risk levels |
| 🚨 Alert Summary | Click any HIGH/CRITICAL transaction → auto-generated compliance alert |
| 💬 AML Policy Q&A | Ask compliance questions grounded in FATF policy |
| 🪪 KYC Verification | Upload KYC document → extract fields → validate risk |

---

## 🏗️ Project Structure

```
fintech-compliance-system/
│
├── fraud_detection.ipynb       # Module 1 — ML fraud model
├── aml_layer.ipynb             # Module 2 — AML rule engine
├── genai_alert.ipynb           # Module 3 — GenAI alert generation
├── rag_aml_policy.ipynb        # Module 4 — RAG pipeline
├── kyc_module.ipynb            # Module 5 — KYC verification
├── app.py                      # Streamlit dashboard
│
├── fraud_detector_model.pkl    # Saved XGBoost model
├── scaler.pkl                  # Saved StandardScaler
│
└── README.md
```

---

## ⚙️ How Each Module Works

### Module 1 — ML Fraud Detection
- Dataset: Kaggle Credit Card Fraud (284,807 transactions, 0.17% fraud)
- Handled class imbalance using SMOTE
- Trained XGBoost classifier
- Output: fraud probability score (0.0 to 1.0) per transaction
- Performance: ROC-AUC > 0.99

### Module 2 — AML Rule Engine
Three rules applied on top of the ML scores:

| Rule | What It Catches |
|------|----------------|
| Structuring Flag | Amount between $8,000–$9,999 (below reporting threshold) |
| Round Amount Flag | Amount exactly divisible by 500 |
| Velocity Flag | Same pattern repeated 3+ times in short window |

Combined risk formula:
```
combined_risk = (fraud_probability × 0.7) + (aml_flag_count × 0.1)
```

| Score | Risk Level | Action |
|-------|-----------|--------|
| 0.00 – 0.29 | 🟢 LOW | ALLOW |
| 0.30 – 0.59 | 🟡 MEDIUM | ALLOW |
| 0.60 – 0.84 | 🟠 HIGH | REVIEW |
| 0.85 – 1.00 | 🔴 CRITICAL | BLOCK |

### Module 3 — GenAI Alert Summarization
- Takes every HIGH/CRITICAL transaction
- Sends transaction details to Gemini LLM
- Generates a 2-sentence professional compliance alert
- Stored alongside each flagged transaction

### Module 4 — RAG Pipeline
- FATF 40 Recommendations PDF processed using PyMuPDF
- Chunked and embedded using all-MiniLM-L6-v2
- Stored in Pinecone vector database
- User asks question → Pinecone retrieves relevant policy → Gemini answers

### Module 5 — KYC Verification
- Reads PDF or TXT KYC documents
- Gemini extracts: Name, DOB, National ID, Address, Expiry Date
- Validates: age (must be 18+), ID expiry, missing fields
- Returns: CLEAR / MEDIUM RISK / HIGH RISK / REVIEW

---

## 🛠️ Tech Stack

| Category | Tools |
|----------|-------|
| ML Model | XGBoost, Scikit-learn, imbalanced-learn (SMOTE) |
| Data | Pandas, NumPy |
| PDF Reading | PyMuPDF (fitz) |
| Embeddings | Sentence Transformers (all-MiniLM-L6-v2) |
| Vector DB | Pinecone |
| LLM | Google Gemini API (gemini-2.0-flash-lite) |
| Dashboard | Streamlit |
| Deployment | Pyngrok + Google Colab |
| Storage | Google Drive, Joblib |

---

## 🚀 How to Run

**Step 1 — Clone the repo**
```bash
git clone https://github.com/yourusername/fintech-compliance-system.git
```

**Step 2 — Install dependencies**
```bash
pip install xgboost scikit-learn imbalanced-learn pandas numpy pymupdf
pip install sentence-transformers pinecone-client google-generativeai streamlit pyngrok joblib
```

**Step 3 — Add your API keys in app.py**
```python
GEMINI_API_KEY   = "your_gemini_api_key"
PINECONE_API_KEY = "your_pinecone_api_key"
```

**Step 4 — Run notebooks in order**
1. `fraud_detection.ipynb` — trains and saves the model
2. `aml_layer.ipynb` — scores transactions with AML flags
3. `genai_alert.ipynb` — generates compliance alerts
4. `rag_aml_policy.ipynb` — uploads FATF policy to Pinecone
5. `kyc_module.ipynb` — processes KYC documents

**Step 5 — Launch the dashboard**
```bash
streamlit run app.py
```

---

## 🔑 Get Free API Keys

| Service | Link |
|---------|------|
| Gemini API | https://aistudio.google.com |
| Pinecone | https://app.pinecone.io |
| Ngrok | https://dashboard.ngrok.com |

---

## 📊 Model Performance

| Metric | Score |
|--------|-------|
| ROC-AUC | > 0.99 |
| Precision (Fraud) | > 0.85 |
| F1 Score (Fraud) | > 0.85 |

---

## 💡 Key Insight

A transaction can have **low fraud probability but still be HIGH risk** due to AML flags.

Example: A $9,500 transaction scores 0.15 fraud probability (looks clean to ML) but triggers the structuring flag, pushing the combined risk score to 0.65 → **HIGH RISK → REVIEW**.

This is exactly the gap between fraud detection and AML compliance — and why both layers are needed.

---

## 👤 Author

**Data Science Trainer | FinTech Compliance Portfolio Project**

Built to demonstrate applied data science for financial crime prevention roles in fintech.

---

## 📄 License

MIT License
