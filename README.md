# 📧 Email Threat Classifier

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.11-3776AB?style=flat-square&logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/scikit--learn-1.4-F7931E?style=flat-square&logo=scikit-learn&logoColor=white" />
  <img src="https://img.shields.io/badge/XGBoost-Planned-orange?style=flat-square" />
  <img src="https://img.shields.io/badge/DistilBERT-Planned-yellow?style=flat-square" />
  <img src="https://img.shields.io/badge/FastAPI-0.110-009688?style=flat-square&logo=fastapi&logoColor=white" />
  <img src="https://img.shields.io/badge/SHAP-Explainability-6C3483?style=flat-square" />
  <img src="https://img.shields.io/badge/status-active_development-brightgreen?style=flat-square" />
</p>

> **Extended and iterative version of my undergraduate thesis project.**
> A hybrid phishing email detection system evolving to combine structural metadata analysis (XGBoost) with deep semantic understanding (DistilBERT), featuring a short-circuit gating mechanism and SHAP-based explainability.

## 📌 Project Overview
This repository is an active evolution of the phishing detection system developed for my Intelligent Computing Engineering thesis. The goal is to iteratively improve the original baseline (Random Forest + Logistic Regression) into an advanced architecture (XGBoost + DistilBERT), documenting each architectural decision as a reproducible step.

Modern phishing attacks are sophisticated enough to bypass rule-based filters. A model that understands both the structural signals (metadata, URL patterns) and the semantic content (email body) is significantly harder to fool. 

## 🏗️ Target Architecture
```text
email (subject + body + sender)
        │
        ▼
┌───────────────────┐
│  FeatureExtractor │  → 32 metadata features 
└───────────────────┘
        │
        ▼
┌───────────────────┐
│  Metadata Model   │  → score_meta ∈ [0, 1]
│  (XGBoost)        │
└───────────────────┘
        │
score_meta ≥ θ_meta? ──── YES ──► label = PHISHING  (gating activation)
        │
        NO
        ▼
┌───────────────────┐
│  Semantic Model   │  → score_text ∈ [0, 1]
│  (DistilBERT)     │
└───────────────────┘
        │
        ▼
score_final = α·score_meta + (1−α)·score_text
        │
        ▼
score_final ≥ θ_final → label ∈ {phishing, legitimate}
        │
        ▼
SHAP explanation (top-K features → phishing)

```

## 🗺️ Development Roadmap

Each stage is developed in its own branch and merged via Pull Request, ensuring the evolution is traceable in the commit history.

| Stage | Branch | Description | Status |
| --- | --- | --- | --- |
| 00 | `main` | **Original Baseline:** Random Forest (Metadata) + TF-IDF/Logistic Regression (Text) | ✅ Done |
| 01 | `feat/01-xgboost-upgrade` | Upgrade metadata submodel to XGBoost | 🔄 In progress |
| 02 | `feat/02-word-embeddings` | Introduce FastText / Word2Vec semantic features | ⏳ Planned |
| 03 | `feat/03-distilbert-semantic` | Replace TF-IDF with DistilBERT fine-tuned on email corpus | ⏳ Planned |
| 04 | `feat/04-dynamic-fusion` | Implement dynamic α weights based on email profile | ⏳ Planned |
| 05 | `feat/05-docker-compose` | Full containerized deployment | ⏳ Planned |

## 📚 Dataset

The baseline utilizes a highly curated, deduplicated corpus of **164,563 records**, consolidated from 7 public datasets and split into stratified 70/15/15 partitions:

| Dataset | Records | Description |
| --- | --- | --- |
| phishing_email.csv | ~82k | Primary phishing corpus |
| Enron.csv | ~33k | Legitimate email corpus |
| CEAS_08.csv | ~17k | Spam filtering challenge |
| SpamAssasin.csv | ~6k | SpamAssassin public corpus |
| Ling.csv | ~2k | Ling-Spam corpus |
| Nazario.csv | ~1.5k | Phishing-only corpus |
| Nigerian_Fraud.csv | ~1k | 419 fraud corpus |

*Note: Raw CSVs are tracked via Git LFS.*

## 🔍 Explainability

One of the core goals is not just predicting whether an email is phishing, but explaining *why*. **SHAP (SHapley Additive exPlanations)** is integrated directly into the FastAPI response to:

* Identify the top contributing technical features per prediction.
* Provide exact (not approximated) human-readable justification alongside the model output.

## 🚀 Quickstart

**1. Clone and set up environment**

```bash
git lfs install
git clone [https://github.com/YOUR_USERNAME/email-threat-classifier.git](https://github.com/YOUR_USERNAME/email-threat-classifier.git)
cd email-threat-classifier

python -m venv .venv
source .venv/bin/activate  # Linux / macOS
# .venv\Scripts\activate   # Windows

pip install -r requirements.txt
git lfs pull

```

**2. Standardize data and create splits**

```bash
python scripts/standardize_datasets.py
python scripts/make_splits.py

```

**3. Train current baseline models**

```bash
python scripts/train_metadata_model.py
python scripts/train_text_model.py

```

**4. Start API**

```bash
uvicorn phishguard.api.main:app --reload --port 8000

```

Interactive docs available at `http://localhost:8000/docs`.

## 🎓 Academic Context

This repository extends the work developed for my undergraduate thesis in Intelligent Computing Engineering at Universidad Autónoma de Aguascalientes (UAA). The original thesis establishes the baseline late-fusion architecture and gating mechanism. This public repo expands on that work iteratively, with an emphasis on transitioning to deep learning architectures, reproducibility, and deployment.

## 👤 Author

**Carlos Daniel Torres Macías**
Intelligent Computing Engineering — UAA

```

```
