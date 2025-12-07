Trump Twitter Analysis: Populist Discourse & Social Media
Unsupervised Machine Learning Analysis of Populist Rhetoric Patterns in Political Communication

Table of Contents

Project Overview
Business Understanding
Data Understanding
Data Preparation
Modeling
Evaluation
Deployment
Technical Stack
Project Structure
Results
Limitations and Future Work
References


Project Overview
This project applies the CRISP-DM (Cross-Industry Standard Process for Data Mining) methodology to analyze populist discourse patterns in Donald Trump's Twitter communication from 2009 to 2021. Using unsupervised machine learning techniques, we automatically discover semantic categories in 50,000+ tweets without predefined labels, providing an objective analysis of political rhetoric on social media.
Author: Created for bachelor's thesis research on political communication
Duration: December 2024
Status: Complete

1. Business Understanding
Problem Statement
My girlfriend approached me with a challenge for her bachelor's thesis: she needed comprehensive visualizations and quantitative analysis of Donald Trump's Twitter communication patterns, specifically focusing on populist rhetoric and discourse strategies. Her thesis examines the relationship between populism, political communication, and social media platforms.
Initial Vision
Rather than conducting a limited qualitative analysis, we decided to create a comprehensive data science project analyzing Trump's entire Twitter archive. The goal was to produce publication-quality visualizations and insights that could support academic research while demonstrating advanced NLP and machine learning techniques.
Research Questions
We formulated four primary research questions to guide our analysis:
RQ1: Word Frequency Analysis
Which words does Trump use most frequently in his posts, and what does this reveal about his communication priorities?
RQ2: Semantic Categorization of Words
What semantic categories emerge when we analyze individual words using unsupervised clustering? Can we identify thematic groupings without predefined labels?
RQ3: Tweet-Level Classification
How can complete tweets be categorized into populist discourse patterns? Which rhetorical strategies dominate his communication?
RQ4: Temporal Evolution
How does Trump's discourse change over time, particularly during critical periods such as:

2016 Presidential Campaign
COVID-19 pandemic
2020 Election cycle
Post-presidency period

Success Criteria

Discover 12-15 distinct discourse categories through unsupervised learning
Achieve less than 20% outlier rate in topic modeling
Generate Power BI-ready datasets for visualization
Produce publication-quality analysis suitable for academic citation


2. Data Understanding
Data Source Discovery
I located a comprehensive dataset containing Trump's complete Twitter archive through multiple sources:
Primary Source: Trump Twitter Archive (thetrumparchive.com)
Alternative Sources:

GitHub: MarkHershey/CompleteTrumpTweetsArchive
Harvard Dataverse: Trump Tweet Dataset (2009-2019)
Kaggle: Trump Twitter Archive

Dataset Characteristics
Initial Dataset:

Total records: 57,331 tweets
Date range: 2009-05-04 to 2021-01-08 (account suspension)
Format: CSV with JSON-structured metadata

Data Structure:
Columns: 9
- id: Unique tweet identifier (int64)
- text: Full tweet content (string)
- isRetweet: Retweet flag (boolean)
- isDeleted: Deletion flag (boolean)
- device: Posting device (string)
- favorites: Like count (int64)
- retweets: Retweet count (int64)
- date: Timestamp (datetime)
- isFlagged: Flagged content marker (boolean)

Initial Data Quality Assessment
Positive Findings:

Well-structured CSV format
Consistent schema across all records
Temporal coverage spans entire Twitter presence
Engagement metrics (likes, retweets) available
Device information reveals posting patterns

Identified Issues:

Encoding Problems: UTF-8 corruption (e.g., "a..." instead of ellipsis)
HTML Entities: Unescaped characters (& instead of &)
Retweets: 22% of records are retweets (not original content)
Deleted Tweets: 3% marked as deleted
URL Clutter: Many tweets contain only shortened URLs
Duplicate Content: Near-duplicate tweets with minor variations
Missing Context: Tweets referencing images/videos without content

Exploratory Data Analysis Insights
Temporal Distribution:

Pre-campaign (2009-2015): 8,432 tweets (general business/celebrity content)
Campaign 2016: 12,876 tweets (peak activity)
Presidency (2017-2020): 28,543 tweets (official communication)
Post-election 2020: 7,480 tweets (contested election period)

Engagement Patterns:

Average favorites: 47,832 per tweet
Average retweets: 13,245 per tweet
Peak engagement: Controversial statements and attack rhetoric
Lowest engagement: Policy details and routine announcements

Device Analysis:

Twitter for iPhone: 67% (likely personal tweets)
Twitter Web Client: 18% (staff-managed content)
Android: 12% (earlier period)
Other: 3%

3. Data Preparation
3.1 Data Cleaning Pipeline
The data preparation phase was the most time-intensive component, requiring multiple iterations to achieve analysis-ready data quality.
Step 1: Encoding Correction
Problem: UTF-8 encoding errors produced corrupted characters that would contaminate text analysis.
Examples of corruption:

"a..." should be "..." (ellipsis)
"a'" should be "'" (apostrophe)
"a"" should be """ (smart quotes)
"A " should be "" (empty space artifacts)


Step 6: Feature Engineering
Created temporal and categorical features for analysis:
Temporal Features:

year: Extract year for annual trends
month: Monthly granularity
year_month: Period identifier (YYYY-MM)
day_of_week: Weekday pattern analysis

Period Labels:

Pre-Campaign: Before 2016-06-16
Campaign 2016: 2016-06-16 to 2017-01-20
Presidency (Pre-COVID): 2017-01-20 to 2020-01-01
COVID & Election 2020: 2020-01-01 to 2020-11-03
Post-Election: After 2020-11-03

Text Features:

word_count: Number of tokens per tweet
cleaned_text: Space-joined cleaned tokens
cleaned_tokens: List of tokens for modeling

3.2 Final Dataset Statistics
After cleaning pipeline:

Total tweets: 44,836 (78.2% of original)
Date range: 2009-05-04 to 2021-01-08
Total words: 534,291
Unique words: 23,847
Average words per tweet: 11.9
Median words per tweet: 11.0

Data Quality Metrics:

Encoding errors corrected: 100%
Missing values: 0%
Duplicate tweets removed: 156
Outlier tweets (length < 3 words): 892 (excluded)


4. Modeling
4.1 Word-Level Analysis: Word2Vec + K-Means
Objective: Discover semantic word clusters without predefined categories.
Methodology:
Step 1: Word Embeddings (Word2Vec)

Architecture: Skip-gram (better for infrequent words)
Vector dimensions: 100
Context window: 5 words
Minimum word frequency: 10 occurrences
Training epochs: 20

Rationale: Skip-gram captures semantic relationships by predicting context words from target words, creating dense vector representations where similar words cluster together.
Step 2: Clustering (K-Means)

Number of clusters: 15 (matching discourse categories)
Initialization: k-means++ (20 iterations)
Distance metric: Euclidean
Max iterations: 500

Step 3: Dimensionality Reduction (t-SNE)

Components: 2D for visualization
Perplexity: 30
Learning rate: Auto
Iterations: 1000

Results:

Vocabulary size: 8,932 words (min_count=10)
Clear semantic clusters visible in t-SNE plot
Examples:

Cluster 0: fake, news, media, lying, corrupt
Cluster 4: border, wall, illegal, mexico, immigration
Cluster 8: economy, jobs, trade, china, manufacturing



Challenges Encountered:
Challenge 1: Stopword Dominance
Initial run showed clusters dominated by function words (the, and, to).
Solution: Enhanced stopword list and added min_df filtering in subsequent analyses.
Challenge 2: Ambiguous Clusters
Some clusters mixed unrelated words (e.g., personal attacks + policy terms).
Solution: Increased cluster count from 10 to 15, allowing finer granularity.
Challenge 3: Cluster Interpretation
Raw clusters required manual interpretation to map to discourse categories.
Solution: Created systematic mapping process:

Extract top 30 words per cluster
Review representative tweets
Match to theoretical populism categories
Validate with domain expert (thesis advisor)

4.2 Tweet-Level Analysis: BERTopic
Objective: Classify complete tweets into discourse categories using state-of-the-art topic modeling.
Methodology:
Step 1: Sentence Embeddings

Model: all-MiniLM-L6-v2 (Sentence-BERT)
Embedding dimensions: 384
Context-aware representations

Rationale: BERT-based embeddings capture semantic meaning of entire tweets, not just word co-occurrence.
Step 2: Dimensionality Reduction (UMAP)

Components: 5 (from 384)
Neighbors: 15
Min distance: 0.0
Metric: Cosine similarity

Step 3: Clustering (HDBSCAN)

Min cluster size: 50 tweets
Min samples: 10
Metric: Euclidean
Selection method: Excess of mass (EOM)

Step 4: Topic Representation (c-TF-IDF)

N-gram range: 1-3 (unigrams, bigrams, trigrams)
Custom stopwords: 247 words
Min document frequency: 10
Max document frequency: 70%

Initial Results:

Discovered topics: 14
Outliers: 16,327 tweets (36.4%)
Average confidence: 0.73

Critical Challenges and Solutions:
Challenge 1: Excessive Outlier Rate (36.4%)
Problem: Over one-third of tweets were unclassified, indicating poor clustering.
Root Cause Analysis:

Dataset heterogeneity: Trump's tweets span business, celebrity, and political content
HDBSCAN sensitivity: Default min_cluster_size=30 too strict
Diverse vocabulary: 23,847 unique words create sparse vector space

Solution Attempts:
Attempt 1: Reduce min_cluster_size to 30
Result: Outliers decreased to 28.3% but cluster quality degraded
Attempt 2: Increase min_cluster_size to 50
Result: Outliers remained high (35.1%) but cleaner clusters
Attempt 3: Add min_samples parameter

hdbscan_model = HDBSCAN(
    min_cluster_size=50,
    min_samples=10,  # Require 10 confirming samples
    metric='euclidean'
)

Result: Outliers reduced to 24.7%, acceptable quality
Attempt 4: Outlier reduction post-processing

new_topics = topic_model.reduce_outliers(
    documents,
    topics,
    strategy="distributions"
)
```
Result: Final outlier rate: 18.3%

**Final Configuration:**
- Min cluster size: 50
- Min samples: 10
- Outlier reduction: Distribution-based
- Result: 18.3% outliers (acceptable for heterogeneous corpus)

**Challenge 2: Stopword Contamination in Topics**

**Problem:** Initial topics showed generic words as top descriptors:
```
Topic 0: the(0.05), and(0.03), to(0.03), is(0.03), of(0.03)
Topic 1: https(0.10), co(0.09), you(0.05), thank(0.06)


Root Cause: BERTopic's default CountVectorizer doesn't remove stopwords.
Solution is in my jupiternotebook!

9. Results
Key Findings
RQ1: Word Frequency

Top 3 words: america (4,523), great (3,891), people (3,654)
Strong emphasis on national identity and populist "people" framing
Negative terms dominate: fake (2,543), bad (1,876), terrible (1,234)

RQ2: Semantic Word Categories

Successfully identified 15 distinct semantic clusters
High alignment with theoretical populism categories
Clear "us vs. them" dichotomies visible in cluster structure

RQ3: Tweet Classification

Media Delegitimization most prevalent (28.5%)
Immigration/External Threats second largest (14.4%)
18.3% outliers reflect content diversity beyond politics

RQ4: Temporal Evolution

Campaign periods emphasize national identity (+114% vs. baseline)
COVID-19 triggered conspiracy narrative spike (+132%)
Post-impeachment showed "under attack" surge (+276%)

Academic Contributions
This analysis provides:

Quantitative validation of populist communication theory
Temporal mapping of discourse evolution
Engagement pattern insights for political communication research
Replicable methodology for social media discourse analysis


10. Limitations and Future Work
Current Limitations
Methodological Limitations:

Retweet Exclusion

Decision to exclude retweets removes 22% of data
May miss amplification patterns and shared narratives
Future work could analyze retweet networks separately


Manual Category Mapping

Topic-to-category assignment requires interpretation
Potential for researcher bias in labeling
Inter-rater reliability not assessed (single researcher)


Twitter-Only Analysis

Excludes other platforms (Truth Social, rallies, interviews)
Platform-specific communication norms may influence results


Outlier Rate

18.3% of tweets remain uncategorized
Acceptable for heterogeneous corpus but indicates model limitations
May contain important edge cases worth deeper analysis


Stopword Removal Trade-off

Aggressive filtering may remove contextual nuances
Phrases like "very bad" reduced to "bad", losing emphasis



Technical Limitations:

Computational Resources

BERTopic training requires 10-20 minutes on standard hardware
Full dataset analysis impractical for real-time applications


Model Stationarity

Vocabulary and discourse patterns evolve over 12-year span
Single model may not capture temporal linguistic shifts optimally


Embedding Model Selection

all-MiniLM-L6-v2 optimized for general text, not political discourse
Domain-specific fine-tuning could improve results



Future Work Recommendations
Short-Term Enhancements:

Sentiment Analysis Integration

Add sentiment dimension to category analysis
Quantify emotional polarity within discourse categories


Named Entity Recognition

Extract person, organization, location mentions
Analyze targeting patterns (which opponents mentioned most)


Comparative Analysis

Apply methodology to other political figures (Biden, Obama, etc.)
Identify unique vs. common populist patterns



Medium-Term Research:

Multimodal Analysis

Incorporate image/video content analysis
Assess alignment between visual and textual messaging


Network Analysis

Analyze mention/reply networks
Map information flow and influence patterns


Causal Impact Analysis

Link tweet content to real-world events
Assess impact on polling, media coverage, policy discourse



Long-Term Directions:

Predictive Modeling

Build classifiers to predict discourse category from new content
Real-time monitoring of political communication


Cross-Platform Analysis

Extend to Truth Social, Facebook, Instagram
Compare platform-specific communication strategies


Longitudinal Tracking

Continue analysis through 2024 campaign
Track discourse evolution post-presidency



Required Fine-Tuning for Replication
Manual Parameters Requiring Adjustment:

HDBSCAN min_cluster_size

Default: 50 (for 44K tweets)
Adjust based on dataset size:

Small (< 10K): 20-30
Medium (10-50K): 40-60
Large (> 50K): 60-100




Stopword List

Requires 2-3 iterative refinements
Process: Run model → inspect top words → add noise → re-run
Expect 50-100 domain-specific additions to standard lists

Topic-to-Category Mapping

Fully manual process
Requires domain expertise in populism theory
Allow 4-6 hours for thorough review and validation


Outlier Threshold

Accept 15-20% outlier rate for heterogeneous political corpora
Higher rates indicate need for parameter tuning or corpus filtering




11. References
Academic Sources

Mudde, C. (2004). The Populist Zeitgeist. Government and Opposition, 39(4), 541-563.
Engesser, S., Ernst, N., Esser, F., & Büchel, F. (2017). Populism and social media: how politicians spread a fragmented ideology. Information, Communication & Society, 20(8), 1109-1126.
Jagers, J., & Walgrave, S. (2007). Populism as political communication style: An empirical study of political parties' discourse in Belgium. European Journal of Political Research, 46(3), 319-345.
Moffitt, B. (2016). The Global Rise of Populism: Performance, Political Style, and Representation. Stanford University Press.
Ott, B. L. (2017). The age of Twitter: Donald J. Trump and the politics of debasement. Critical Studies in Media Communication, 34(1), 59-68.

Technical Methods

Grootendorst, M. (2022). BERTopic: Neural topic modeling with a class-based TF-IDF procedure. arXiv preprint arXiv:2203.05794.
Mikolov, T., Chen, K., Corrado, G., & Dean, J. (2013). Efficient estimation of word representations in vector space. arXiv preprint arXiv:1301.3781.
Reimers, N., & Gurevych, I. (2019). Sentence-BERT: Sentence Embeddings using Siamese BERT-Networks. Proceedings of the 2019 Conference on Empirical Methods in Natural Language Processing.
McInnes, L., Healy, J., & Astels, S. (2017). hdbscan: Hierarchical density based clustering. Journal of Open Source Software, 2(11), 205.
Chapman, P., Clinton, J., Kerber, R., Khabaza, T., Reinartz, T., Shearer, C., & Wirth, R. (2000). CRISP-DM 1.0: Step-by-step data mining guide. SPSS inc, 9, 13.

Data Sources

Trump Twitter Archive. (2021). Retrieved from https://www.thetrumparchive.com/
Hershey, M. (2021). Complete Trump Tweets Archive. GitHub repository. https://github.com/MarkHershey/CompleteTrumpTweetsArchive
Harvard Dataverse. (2019). Trump Twitter Archive Dataset. https://doi.org/10.7910/DVN/KJEBIL


Data Attribution
Trump Twitter Archive data is publicly available and used for academic research purposes.

Contact
For questions, suggestions, or collaboration opportunities:

Email: mesternandor04@gmail.com


Project Status: progress
Last Updated: December 2025
