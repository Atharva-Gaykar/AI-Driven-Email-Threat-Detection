

---

# 🧠 AI-Driven Email Threat Detection

This repository contains the implementation of an **AI-based hybrid system** for detecting and classifying **phishing, spam, and malicious emails** using **NLP (DistilBERT)** and **URL-based feature engineering**.
The project fine-tunes transformer embeddings for textual understanding and combines them with handcrafted numerical features for enhanced threat detection accuracy.

---

## 📂 Repository Structure

### 1️⃣ `Fine_tune_DistilBert_For_Embedding.ipynb`

Contains the **fine-tuning pipeline** for the DistilBERT model on the *Safe vs Suspicious* classification task.

* Custom tokens (`[SSUB]`, `[SBODY]`, `[LINK]`, `[PHONE]`) were introduced to preserve structure and privacy.
* The fine-tuned model generates semantic embeddings used later in the hybrid system.

### 2️⃣ `Hybrid_model_preparation.ipynb`

Implements **feature extraction and classification**:

* Extracts **URL-based numerical features** (length, subdomain count, suspicious keywords, etc.)
* Combines them with **DistilBERT embeddings**.
* Trains an **XGBoost classifier** achieving **99.35% accuracy** on test data.

### 3️⃣ `Phising_data_collection.ipynb`

Handles **data gathering and preprocessing** for the email corpus.

* Collects and cleans raw phishing, spam, and safe email samples.
* Extracts URLs and text for model-ready input.

---

## ⚙️ Tech Stack

* Python · Pandas · Scikit-learn · XGBoost
* Hugging Face Transformers (DistilBERT)
* Regex · URL parsing · NLP preprocessing



  



