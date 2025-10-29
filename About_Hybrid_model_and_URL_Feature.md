


## 🌐 **Project Report: URL Feature Extraction and Hybrid XGBoost Classification**

---

### **1️⃣ Objective**

* Enhance email threat classification by combining **semantic embeddings** (from fine-tuned DistilBERT) with **numerical URL-based features**.
* Capture malicious patterns hidden in links such as obfuscation, shortening, and suspicious keywords.

---

### **2️⃣ URL Feature Extraction Overview**

Each email’s extracted URLs were analyzed using handcrafted rule-based functions.
URLs were separated by `[NEXT]` tokens (from preprocessing).

Example schema after extraction:

```
['text_combined', 'URL', 'URL_COUNT', 'label',
 'USE_OF_IP', 'url_length_max', 'url_length_avg', 'url_subdom_max', 'url_subdom_avg',
 'short_url_count', 'sus_url_count', 'sus_url_flag',
 'dot_count_max', 'dot_count_avg', 'perc_max', 'perc_avg',
 'ques_max', 'ques_avg', 'hyphen_max', 'hyphen_avg',
 'equal_max', 'equal_avg']
```

---

### **3️⃣ Key Feature Categories**

| **Feature Name**         | **Description**                                          | **Purpose**                                    |
| ------------------------ | -------------------------------------------------------- | ---------------------------------------------- |
| **URL_COUNT**            | Number of URLs in email body                             | Helps detect link-heavy phishing emails        |
| **USE_OF_IP**            | Presence of IP-based URLs                                | Common in malicious redirections               |
| **url_length_max / avg** | Maximum & average length of URLs                         | Longer URLs often indicate obfuscation         |
| **url_subdom_max / avg** | Number of subdomains                                     | Too many subdomains suggest hidden redirects   |
| **dot_count_max / avg**  | Number of “.” dots                                       | Detects fake nested domains                    |
| **perc_max / avg**       | Count of “%” characters                                  | Indicates encoding tricks or payloads          |
| **ques_max / avg**       | Count of “?”                                             | Used in parameterized phishing links           |
| **hyphen_max / avg**     | Count of “–”                                             | Common in domain mimicry (`pay-pal-login.com`) |
| **equal_max / avg**      | Count of “=”                                             | Captures suspicious query strings              |
| **short_url_count**      | Number of shortened URLs (`bit.ly`, `t.co`, etc.)        | Shorteners hide final destination              |
| **sus_url_count**        | Count of suspicious keywords (`login`, `bank`, `update`) | Semantic signal for phishing                   |
| **sus_url_flag**         | Binary (1/0) flag if any suspicious term detected        | Quick threat indicator                         |

---

### **4️⃣ Example Functions**

**Split URLs and Count Structural Elements**

```python
def split_urls(sample):
    if pd.isna(sample): return []
    return [u.strip() for u in sample.split('[NEXT]') if u.strip()]

def dot_count(url): return url.count('.') if url else 0
def url_length(url): return len(url) if url else 0
```

**Suspicious Keyword & Shortener Detection**

```python
suspicious_pattern = re.compile(r'paypal|login|bank|update|bonus', re.IGNORECASE)
shortening_pattern = re.compile(r'bit\.ly|tinyurl|t\.co|lnkd\.in', re.IGNORECASE)

def suspicious_words_count(urls):
    return sum(1 for u in urls if suspicious_pattern.search(u))

def shortened_count(urls):
    return sum(1 for u in urls if shortening_pattern.search(u))
```

**Feature Extraction Wrapper**

```python
def extract_url_features(sample):
    urls = split_urls(sample)
    if not urls: return pd.Series({all zero features})
    # compute max/avg for structural counts and keyword signals
```

---

### **5️⃣ Integration with BERT Embeddings**

* The **textual semantic embeddings** (768-dim) were generated from **fine-tuned DistilBERT** `[CLS]` token.
* These were **concatenated** with **engineered URL numerical features**:

  ```
  X_final = [BERT_embeddings ⊕ URL_features]
  ```
* Resulting hybrid vectors were used as input to **XGBoost** for final prediction.

---

### **6️⃣ XGBoost Configuration**

```python
xgb_model = XGBClassifier(
    n_estimators=300,
    learning_rate=0.07,
    max_depth=6,
    subsample=0.9,
    colsample_bytree=0.9,
    reg_lambda=1,
    reg_alpha=0.001,
    use_label_encoder=False,
    eval_metric='logloss',
    random_state=42,
    n_jobs=-1
)
```

* **n_estimators:** Number of boosting rounds
* **max_depth:** Limits overfitting
* **learning_rate:** 0.07 for steady convergence
* **reg_lambda / reg_alpha:** L2/L1 regularization
* **subsample & colsample_bytree:** Randomness for generalization

---

### **7️⃣ Results**

| Metric        | Value      |
| ------------- | ---------- |
| **Accuracy**  | **99.35%** |
| **Precision** | 0.99       |
| **Recall**    | 0.99       |
| **F1-score**  | 0.99       |

✅ **Excellent hybrid model performance** — the combination of structural URL patterns + deep semantic embeddings enabled near-perfect detection of suspicious content.

---

### **8️⃣ Summary**

* Designed **interpretable handcrafted URL features** capturing structural & semantic cues.
* Combined with **DistilBERT embeddings** → rich hybrid representation.
* Trained with **XGBoost**, achieving 99.35% accuracy.
* Provided both **explainability** (via URL stats) and **deep contextual understanding** (via BERT).

---
