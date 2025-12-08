# 📊 Trump Twitter Analysis: Populist Discourse & Social Media

## Unsupervised Machine Learning Analysis of Populist Rhetoric Patterns in Political Communication

---

### 🚀 Project Overview

This project applies the **CRISP-DM** (Cross-Industry Standard Process for Data Mining) methodology to analyze populist discourse patterns in Donald Trump's Twitter communication from 2009 to 2021. We use unsupervised machine learning techniques to automatically discover semantic categories in **50,000+ tweets** without predefined labels, providing an objective analysis of political rhetoric on social media.

* **Author:** Created for bachelor's thesis research on political communication
* **Duration:** December 2025
* **Status:** In progress

### ✅ Success Criteria

* Discover 12-15 distinct discourse categories through unsupervised learning.
* Achieve **less than 20% outlier rate** in topic modeling.
* Generate Power BI-ready datasets for visualization.
* Produce publication-quality analysis suitable for academic citation.

---

### 1. 💡 Business Understanding

#### Problem Statement
The project addresses the need for comprehensive visualizations and quantitative analysis of Donald Trump's Twitter communication, focusing specifically on populist rhetoric, to support a bachelor's thesis.

#### Research Questions
| RQ | Focus | Description |
| :--- | :--- | :--- |
| **RQ1** | Word Frequency Analysis | Which words does Trump use most frequently, and what does this reveal about his priorities? |
| **RQ2** | Semantic Categorization | What thematic groupings emerge from unsupervised word clustering? |
| **RQ3** | Tweet-Level Classification | Which rhetorical strategies dominate his communication? |
| **RQ4** | Temporal Evolution | How does Trump's discourse change over time, especially during critical periods (e.g., 2016 Campaign, COVID-19)? |

---

### 2. 💾 Data Understanding

#### Data Source Discovery
The comprehensive dataset of Trump's Twitter archive was sourced primarily from the **Trump Twitter Archive (thetrumparchive.com)**.

#### Dataset Characteristics
* **Total Initial Records:** 57,331 tweets
* **Date Range:** 2009-05-04 to 2021-01-08 (account suspension)

#### Dataset Culomns
* <img width="481" height="25" alt="kép" src="https://github.com/user-attachments/assets/3a6a0723-99a2-40e5-8051-f00303c90b5a" />
* Key Columns: The dataset includes text, date, device, and engagement metrics like favorites and retweets.

#### Initial Data Quality Assessment (Identified Issues)
* Encoding Problems (e.g., UTF-8 corruption).
* 22% of records were Retweets (excluded from core content analysis).
* 3% marked as Deleted Tweets.
* HTML Entities and URL clutter.

#### Exploratory Data Analysis Insights
* **Peak Activity:** Presidency (2017-2020) with 28,543 tweets.
* **Engagement:** Average favorites: 47,832; Average retweets: 13,245.
* **Posting Device:** **Twitter for iPhone** accounted for 67% of posts (likely personal).

---

### 3. 🧹 Data Preparation

The data cleaning pipeline was the most time-intensive component.

#### Data Cleaning Steps
1.  **Encoding Correction:** Solved UTF-8 corruption issues (e.g., smart quotes, ellipses).
2.  **Removal:** Retweets, deleted tweets, and outlier tweets (length < 3 words) were removed.

#### Feature Engineering (Step 6)
* **Temporal Features:** `year`, `month`, `day_of_week`.
* **Period Labels:** Categorical labels created for analysis (e.g., `Campaign 2016`, `COVID & Election 2020`).

#### Final Dataset Statistics
* **Total Tweets:** 44,836 (78.2% of original).
* **Total Words:** 534,291.
* **Encoding errors corrected:** 100%.

---

### 4. 🧠 Modeling

#### 4.1. Word-Level Analysis: Word2Vec + K-Means (RQ2)
* **Objective:** Discover semantic word clusters.
* **Word2Vec Configuration:** Skip-gram architecture, 100 vector dimensions, Min word frequency: 10.
* **K-Means:** 15 clusters used (to match discourse categories).
* **Result Example:** Clusters successfully identified themes like **"Media Delegitimization"** (*fake, news, media, lying, corrupt*) and **"Immigration/Policy"** (*border, wall, illegal, mexico, immigration*).

#### 4.2. Tweet-Level Analysis: BERTopic (RQ3)
* **Objective:** Classify complete tweets into discourse categories.
* **Embeddings:** `all-MiniLM-L6-v2` (Sentence-BERT).
* **Clustering (HDBSCAN):** Min cluster size 50, Min samples 10.
* **Topic Representation:** c-TF-IDF (N-gram range: 1-3).

#### Initial Results:
* ** Discovered topics:** 14
* ** Outliers:** 16,327 tweets (36.4%)
* ** Average confidence:** 0.73

#### Critical Challenge: Excessive Outlier Rate
* **Problem:** Initial outlier rate was 36.4%.
* **Solution:** Applied HDBSCAN parameter tuning and **Distribution-based outlier reduction**.
* **Final Result:** **18.3% outliers** (acceptable for heterogeneous corpus).

---

### 5. 📈 Results & Academic Contributions


 
### RQ1 (Frequency) :
Top 3 words: *america* (4,523), *great* (3,891), *people* (3,654). Strong emphasis on national identity and "people" framing.  

 
<img width="4363" height="2359" alt="wordcloud" src="https://github.com/user-attachments/assets/0874ceae-3033-4b83-bca0-83789b15379e" />

---
 ### RQ2 (Semantic) :
 Successfully identified **15 distinct semantic clusters** (e.g., *fake, news, media* vs. *border, wall, immigration*), showing clear "us vs. them" dichotomies.   
 
<img width="4390" height="3558" alt="word_clusters_tsne" src="https://github.com/user-attachments/assets/248e7756-d19c-432e-ae9a-2c9a0aedb395" />

---
 ### RQ3 (Classification) :
 **Media Delegitimization** was most prevalent (28.5%), followed by **Immigration/External Threats** (14.4%). 

<img width="5355" height="2355" alt="improved_topic_distribution" src="https://github.com/user-attachments/assets/0440abdb-d835-408e-bc1d-04ecabb2e2cb" />

 ---
 ### RQ4 (Temporal) :
 **Campaign periods** emphasized national identity (+114% vs. baseline). **COVID-19** triggered a conspiracy narrative spike (+132%). 

 ---

#### Academic Contributions
* Quantitative validation of populist communication theory.
* Temporal mapping of discourse evolution.
* Replicable methodology for social media discourse analysis.

---
### 6. Technical Stack

**Programming Language:**
* Python 3.8+

**Core Libraries:**
* pandas 2.0.3 - Data manipulation
* numpy 1.24.3 - Numerical computing
* scikit-learn 1.3.0 - Machine learning utilities

**NLP & Embeddings:**
* nltk 3.8.1 - Text preprocessing
* gensim 4.3.1 - Word2Vec implementation
* sentence-transformers 2.2.2 - BERT embeddings

**Topic Modeling:**
* bertopic 0.15.0 - Main topic modeling framework
* umap-learn 0.5.3 - Dimensionality reduction
* hdbscan 0.8.33 - Density-based clustering

**Visualization:**
* matplotlib 3.7.2 - Static plots
* seaborn 0.12.2 - Statistical visualization
* wordcloud 1.9.2 - Word cloud generation
* plotly 5.16.1 - Interactive plots

**Development Environment:**
* Jupyter Notebook / Google Colab
* Git version control
* GitHub repository hosting

---

### 7. ⚠️ Limitations and Future Work

#### Current Limitations
* **Retweet Exclusion:** Removed 22% of data, potentially missing amplification patterns.
* **Manual Category Mapping:** Topic-to-category assignment required interpretation, posing a risk of researcher bias.
* **Twitter-Only Analysis:** Excludes other platforms (Truth Social, rallies, interviews).
* **Stopword Trade-off:** Aggressive filtering may have removed contextual nuances.
* **Model Stationarity:** Single model may not optimally capture linguistic shifts over the 12-year span.

#### Future Work Recommendations
* **Short-Term:** **Sentiment Analysis** Integration and **Named Entity Recognition (NER)**.
* **Medium-Term:** **Multimodal Analysis** (incorporating image/video content) and **Network Analysis**.
* **Long-Term:** **Predictive Modeling** and **Cross-Platform Analysis**.

### 🛠️ Required Fine-Tuning for Replication

| Parameter / Step | Adjustment Guidance |
| :--- | :--- |
| **HDBSCAN min\_cluster\_size** | Adjust based on dataset size (e.g., **50** for this 44K corpus). |
| **Stopword List** | Requires **2-3 iterative refinements**; expect 50-100 domain-specific additions. |
| **Topic-to-Category Mapping** | Fully **manual process** requiring domain expertise (allow 4-6 hours). |
| **Outlier Threshold** | Accept **15-20%** outlier rate for heterogeneous political corpora. |

---

### 8. 📚 References

The full list of academic, technical, and data sources used in this project is available below:

* **Academic Sources:** Mudde, C. (2004), Moffitt, B. (2016), Ott, B. L. (2017), and others.
* **Technical Methods:** BERTopic (Grootendorst, 2022), Word2Vec (Mikolov et al., 2013), and CRISP-DM 1.0 (Chapman et al., 2000).
* **Data Sources:** Trump Twitter Archive.

---
### Contact
Email: mesternandor04@gmail.com
Last Updated: December 2025
