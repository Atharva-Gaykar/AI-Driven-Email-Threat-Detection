
# 🧠 AI-Driven Email Threat Detection

This repository implements an **AI-based hybrid system** for detecting and classifying **phishing, spam, and malicious emails** using **Natural Language Processing (NLP)** and **URL-based feature engineering**.

The system leverages **DistilBERT embeddings** to understand the semantic context of email content and combines them with **hand-crafted numerical URL features** to improve classification accuracy.

The final model uses **XGBoost** for robust classification and achieves **99.35% accuracy** on the test dataset.

---

# 📂 Repository Structure

## 1️⃣ `Fine_tune_DistilBert_For_Embedding.ipynb`

This notebook contains the **DistilBERT fine-tuning pipeline** for the **Safe vs Suspicious email classification task**.

Key features:

* Introduces **custom structural tokens**

  * `[SSUB]` – Subject
  * `[SBODY]` – Email body
  * `[LINK]` – URL placeholder
  * `[PHONE]` – Phone number placeholder

* These tokens help:

  * Preserve **email structure**
  * Protect **sensitive information**
  * Improve **contextual understanding**

The fine-tuned model is used to generate **semantic embeddings** for downstream classification.

---

## 2️⃣ `Hybrid_model_preparation.ipynb`

This notebook implements the **hybrid classification pipeline**.

Steps included:

### 🔹 URL Feature Engineering

Extracts numerical indicators from URLs such as:

* URL length
* Number of subdomains
* Suspicious keywords
* Presence of IP addresses
* Special characters and redirections

### 🔹 Embedding Integration

Combines:

* **DistilBERT text embeddings**
* **URL numerical features**

### 🔹 Classification

A **XGBoost classifier** is trained on the combined feature set.

**Result**

* Test Accuracy: **99.35%**

---

## 3️⃣ `Phising_data_collection.ipynb`

This notebook handles **dataset collection and preprocessing**.

Main tasks:

* Collect phishing, spam, and legitimate email samples
* Clean and normalize email content
* Extract URLs and textual components
* Prepare structured inputs for the model pipeline

---

# 🧩 System Architecture

<img width="728" height="483" alt="System Architecture" src="https://github.com/user-attachments/assets/c55c031a-42f1-495b-8f1b-6ac57f0a488e" />

---

# 🤗 Live Model Deployment

The trained model is deployed on **Hugging Face Spaces**:

👉 [https://huggingface.co/spaces/Gaykar/ClassifyEmail](https://huggingface.co/spaces/Gaykar/ClassifyEmail)

---

# ⚙️ Tech Stack

### Machine Learning & NLP

* Python
* Hugging Face Transformers (DistilBERT)
* Scikit-learn
* XGBoost

### Data Processing

* Pandas
* Regex
* URL parsing
* NLP preprocessing

### Deployment

* FastAPI
* Docker
* Hugging Face Spaces

---

💡 **Key Idea:**
By combining **semantic email understanding (transformer embeddings)** with **structural URL risk indicators**, the system achieves **highly accurate phishing detection** while remaining lightweight enough for real-world deployment.

---



If you want, I can also make a **🔥 “GitHub-star-worthy” README (with badges, pipeline diagram, results table, dataset description, and architecture diagram)** — which makes projects look **much stronger for internships and hackathons**.
