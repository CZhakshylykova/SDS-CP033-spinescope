# 📄 SpineScope – Project Report - 🔴 **Advanced Track**

Welcome to your personal project report!  
Use this file to answer the key reflection questions for each phase of the project. This report is designed to help you think like a data scientist, guide AI tools more effectively, and prepare for real-world job interviews.

---

## ✅ Phase 1: Setup & Exploratory Data Analysis (EDA)

> Answer the EDA questions provided in the project materials here. Focus on data quality, trends, anomalies, and relationships.

### 🔑 Question 1: Which features are most strongly correlated with spinal abnormalities?

### 🔑 Question 2: Are any features linearly dependent on others (e.g., sacral slope and pelvic incidence)?

### 🔑 Question 3: Do biomechanical measurements cluster differently for normal vs. abnormal cases?

### 🔑 Question 4: Are there multicollinearity issues that impact modeling?

---

## ✅ Phase 2: Model Development

> This phase spans 3 weeks. Answer each set of questions weekly as you build, train, evaluate, and improve your models.

---

### 📆 Week 1: Feature Engineering & Data Preprocessing

#### 🔑 Question 1:
**Which categorical features are high-cardinality, and how will you encode them for use with embedding layers?**  

💡 **Hint:**  
Use `.nunique()` and `.value_counts()` to inspect cardinality.  
Use `LabelEncoder` or map categories to integer IDs.  
Think about issues like rare categories, overfitting, and embedding size selection.

✏️ *Your answer here...*
class: 3 unique values
class
Spondylolisthesis    150
Normal               100
Hernia                60
Name: count, dtype: int64
class mapped to integer IDs for embedding.
Embedding size for 'class': 2

High-cardinality features are those with many unique values (e.g., >10).
- Integer encoding is required for embedding layers.
- Grouping rare categories can help prevent overfitting and reduce embedding size.

**`class: 3 unique values` means the 'class' column has 3 different categories: Spondylolisthesis, Normal, and Hernia.**
**The value counts show how many samples belong to each class.**
**`class mapped to integer IDs for embedding.` means each class label was converted to a unique integer (e.g., Spondylolisthesis=2, Normal=1, Hernia=0).**
**`Embedding size for 'class': 2` means that, following the rule-of-thumb (`min(50, (num_categories + 1) // 2)`), the recommended embedding vector size for this feature is 2.**

- None of the classes in the 'class' column have high cardinality.
- The 'class' feature has only 3 unique values: Spondylolisthesis, Normal, and Hernia.
- High cardinality typically refers to features with many unique categories (e.g., >10), which is not the case here.
---

#### 🔑 Question 1:
**Which biomechanical features are likely to have the most predictive power for classifying spinal conditions, and how did you determine that?**

- Features such as **pelvic_incidence**, **lumbar_lordosis_angle**, and **pelvic_tilt** are likely to have the most predictive power.
- This was determined by:
  - Examining boxplots for each feature by class, which showed clear separation in medians and ranges for these features between normal and abnormal cases.
  - Reviewing the correlation matrix, which indicated strong relationships between these features and the target.
  -  In summary, features that consistently differ between classes and show strong separation in visualizations or model importances are most predictive for spinal condition classification.

---

#### 🔑 Question 2:
**What numerical features in the dataset needed to be scaled before being input into a neural network, and what scaling method did you choose?**

All numerical features are scaled before input into the neural network, as neural networks are sensitive to feature scale.  
- I used `df.describe()` and histograms to check the spread and skew of each feature.
- Based on the distributions, I chose `StandardScaler` (z-score normalization) to standardize the features to zero mean and unit variance.
- This ensures all features contribute equally during training and helps the model converge faster and more reliably.
- The advantage of using `StandardScaler` is that it standardizes features to have zero mean and unit variance.
- This helps neural networks and many machine learning algorithms converge faster and more reliably, prevents features with larger scales from dominating the learning process, and improves numerical stability during optimization.

- `MinMaxScaler` would have scaled each numerical feature to a fixed range, typically [0, 1]. This is useful when features have different ranges and you want to preserve the shape of the original distribution.
- A log-transform would have reduced the skewness of features with long tails or large outliers, making their distributions more symmetric and potentially improving model performance for highly skewed data.

- I found that most numerical features had approximately symmetric or moderately skewed distributions, with values spread around their means and no extreme outliers or long tails.
- Because of this, `StandardScaler` (z-score normalization) was appropriate, as it centers and scales features without distorting their distribution.
- If the features had been highly skewed or had large outliers, a log-transform or `MinMaxScaler` might have been more suitable.

---

#### 🔑 Question 3:
**Did you create any new features based on domain knowledge or feature interactions? If yes, what are they and why might they help the model better predict spinal conditions?**

💡 **Hint:**  
Try combining related anatomical angles or calculating ratios/differences (e.g., `pelvic_incidence - sacral_slope`).  
Think: which combinations might reflect spinal misalignment patterns?

✏️ *Your answer here...*

---

#### 🔑 Question 4:
**Which features, if any, did you choose to drop or simplify before modeling, and what was your justification?**

💡 **Hint:**  
Check for highly correlated or constant features using `.corr()` and `.nunique()`.  
Avoid overfitting by removing redundant signals.  
Be cautious about leaking target-related info if any engineered features are overly specific.

✏️ *Your answer here...*
I did not drop any features. In deep learning I learnt that neural networks are very sensitive and hence any features could help increase the performance. Perhaps during testing, we can fine tune by testing with some less features
---

#### 🔑 Question 5:
**After preprocessing, what does your final input schema look like (i.e., how many numerical and categorical features)? Are there class imbalance or sparsity issues to be aware of?**

- After preprocessing, the final input schema consists of **6 numerical features** (all biomechanical measurements) and **no high-cardinality categorical features**.
- All features are continuous and have been scaled using `StandardScaler`.
- The target variable (`class`) has 3 classes: Spondylolisthesis, Normal, and Hernia.
- Using `.value_counts()` on the target shows some class imbalance:
  - Spondylolisthesis: 150 samples
  - Normal: 100 samples
  - Hernia: 60 samples
- There are no features with excessive sparsity or many near-zero values.
- **Note:** The class imbalance (especially fewer Hernia cases) may require handling via class weights, resampling, or specialized loss functions during model training.

---

### 📆 Week 2: Model Development & Experimentation

### 🔑 Question 1:

### 🔑 Question 2:

### 🔑 Question 3:

### 🔑 Question 4:

### 🔑 Question 5:

---

### 📆 Week 3: Model Tuning

### 🔑 Question 1:

### 🔑 Question 2:

### 🔑 Question 3:

### 🔑 Question 4:

### 🔑 Question 5:

---

## ✅ Phase 3: Model Deployment

> Document your approach to building and deploying the Streamlit app, including design decisions, deployment steps, and challenges.

### 🔑 Question 1:

### 🔑 Question 2:

### 🔑 Question 3:

### 🔑 Question 4:

### 🔑 Question 5:

---

## ✨ Final Reflections

> What did you learn from this project? What would you do differently next time? What did AI tools help you with the most?

✏️ *Your final thoughts here...*

---
