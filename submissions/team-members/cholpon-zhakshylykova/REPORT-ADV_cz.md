# 📄 SpineScope – My Project Journey  
**Track:** 🔴 Advanced | **Role:** Data Science Explorer  

Welcome to my SpineScope project report!  
This was a challenging and rewarding experience that pushed me to think critically, explore data deeply, and make modeling decisions like a real-world data scientist. This write-up captures my journey through each phase — from asking the right questions to building and refining predictive models for spinal health.

---

## ✅ Phase 1: Setup & Exploratory Data Analysis (EDA)

This phase was all about understanding the dataset. I focused on quality, patterns, relationships, and potential red flags before modeling anything.

### 🔑 Q1: Which features are most strongly linked to spinal abnormalities?

After running statistical tests and digging into feature interactions, I found all  biomechanical features with strong signals:

| Feature                    | F-Statistic | P-Value | Effect Size | Significant |
|----------------------------|-------------|---------|-------------|-------------|
| `pelvic_incidence`         | 98.54       | 0.0     | 0.243       | ✅ Yes       |
| `pelvic_tilt`              | 21.30       | 0.0     | 0.065       | ✅ Yes       |
| `lumbar_lordosis_angle`    | 114.98      | 0.0     | 0.272       | ✅ Yes       |
| `sacral_slope`             | 89.64       | 0.0     | 0.226       | ✅ Yes       |
| `pelvic_radius`            | 16.87       | 0.0     | 0.052       | ✅ Yes       |
| `degree_spondylolisthesis` | 119.12      | 0.0     | 0.280       | ✅ Yes       |

These results confirmed my suspicion that spinal alignment and posture measurements are key indicators.

---

### 🔑 Q2: Any strong linear relationships?

This heatmap shows pairwise correlations among numerical features, masked for the upper triangle.
![Correlation Heatmap](plots/correlation_heatmap.png)

A couple stood out for very high correlation with each other:

| Feature Pair                               | Correlation (r) |
|--------------------------------------------|------------------|
| `pelvic_incidence` ↔ `lumbar_lordosis_angle` | 0.717            |
| `pelvic_incidence` ↔ `sacral_slope`          | 0.815            |

These strong correlations hinted at redundancy and potential multicollinearity — something I’d need to address later.

---

### 🔑 Q3: Do features cluster based on spinal health?

I tried PCA and t-SNE to visualize whether biomechanical features naturally separated different spinal conditions. The results:
![Dimension in the SpineScope data](plots/dimensionality_reduction_class.png)
- 🧭 **PCA** showed some separation, but not enough for clean clusters.
- 🌈 **t-SNE** gave better visual hints, but overlap remained.
- 🧩 Conclusion: There's some pattern, but no hard boundaries between conditions.

---

### 🔑 Q4: Any multicollinearity issues?

Oh yes — big ones! Some features had extremely high VIFs:

| Feature                 | VIF     |
|------------------------|---------|
| `pelvic_incidence`     | ∞       |
| `pelvic_tilt`          | ∞       |
| `lumbar_lordosis_angle`| 18.94   |
| `sacral_slope`         | ∞       |
| `pelvic_radius`        | 12.28   |

📌 I made note to simplify the feature set and avoid confusing the model later.

---

## ✅ Phase 2: Model Development

### 📆 Week 1: Feature Engineering & Preprocessing

This was one of my favorite parts — transforming raw data into something models could learn from.

---

### 🔑 Q1: Handling categorical features

- `class` had only 3 values: Normal, Hernia, and Spondylolisthesis.  
- I used simple one-hot encoding (or LabelEncoder when needed).  
- No need for fancy embeddings here — low cardinality made this easy.

---

### 🔑 Q2: Which features seem most predictive?
There were clear differences of features in different spinal conditions from the plots. 
![Measurements in different spinal conditions](plots/numerical_features_boxplots_class.png)

Based on plots and feature importance analysis, I ranked them like this:


| Feature                    | Why It Matters |
|----------------------------|----------------|
| `degree_spondylolisthesis` | Clear signal for Spondylolisthesis |
| `pelvic_incidence`         | Elevated for Spondylolisthesis |
| `pelvic_tilt`              | Varies clearly across classes |
| `sacral_slope`             | Moderate separation |
| `lumbar_lordosis_angle`    | Some overlap, but useful |
| `pelvic_radius`            | Noisy — not so helpful |

Visuals like boxplots and violin plots really helped guide these insights.

---

### 🔑 Q3: What needed scaling?

I checked distribution shapes and used:

| Scenario                     | Scaler Used      |
|------------------------------|------------------|
| Normal-ish distributions     | `StandardScaler` |
| Highly skewed features       | Log-transform + scale |

`degree_spondylolisthesis` was very skewed, so I will log-transform it first and apply StandardScaler() for all features and by this degree_spondlylolistethis will be scaled twice.

---

### 🔑 Q4: Did I create new features?

✅ Yes — and I’m proud of this one!

- **New Feature:** `pi_ss_ratio`  
- **Formula:** `pelvic_incidence / sacral_slope`  
- **Why:** This ratio captures the relationship between spine structure and posture. It helped reduce redundancy *and* added interpretability.

---

### 🔑 Q5: Features I dropped

To simplify the model and reduce multicollinearity:

- **Dropped:** `pelvic_incidence`  
- **Replaced with:** `pi_ss_ratio`  
- **Justification:** `pelvic_incidence` was heavily correlated with other features. The new ratio retained its influence without the mess.

---

### 🔑 Q6: Final schema + class balance

**Input Features:**

- `pelvic_tilt`, `sacral_slope`, `lumbar_lordosis_angle`,  
  `pelvic_radius`, `degree_spondylolisthesis`, `pi_ss_ratio`

**Target:** `class` or `binary_class`

**Class Distribution:**

| Class             | Count | %     |
|-------------------|-------|-------|
| Spondylolisthesis | 150   | 48.4% |
| Normal            | 100   | 32.3% |
| Hernia            | 60    | 19.4% |

⚠️ **Imbalance ratio:** ~2.5  
To handle this, I will explored:

- 🔁 SMOTE / oversampling  
- ⚖️ Class weighting  
- 🎯 Focal loss — to help with harder-to-classify examples

---

### 📆 Week 2 & 3: Model Building & Tuning

> Coming soon: I’ll log my experiments, tuning strategies, and final results here.

---

## ✅ Phase 3: Model Deployment

> Placeholder for deployment strategy and exporting the final model. Thinking about using FastAPI + Docker, or maybe Streamlit for a quick UI!

---

### 🔑 Q1: What neural network architecture did I implement (input shape, number of hidden layers, activation functions, etc.), and what guided my design choices?
🎯 Purpose: Tests ability to structure an FFNN for classification and explain architectural decisions.

💡 Hint:
Describe your model layers: e.g., [Input → Dense(64) → ReLU → Dropout → Dense(32) → ReLU → Output(sigmoid)].
Justify the number of layers/units based on dataset size and complexity.
Explain why ReLU and sigmoid are appropriate.



### 🔑 Q2: What metrics did you track during training and evaluation (e.g., accuracy, precision, recall, F1-score, AUC), and how did your model perform on the validation/test set?
🎯 Purpose: Tests metric understanding for classification and ability to interpret model quality.
💡 Hint:
Log and compare metrics across epochs using validation data.
Plot confusion matrix and/or ROC curve.
Explain where the model performs well and where it struggles (e.g., false positives/negatives).


### 🔑 Q3: How did the training and validation loss curves evolve during training, and what do they tell you about your model's generalization?
🎯 Purpose: Tests understanding of overfitting/underfitting using learning curves.
Include training plots of loss and accuracy.
Overfitting → training loss drops, validation loss increases.
Underfitting → both remain high.
Mention any regularization techniques used (dropout, early stopping).


### 🔑 Q4: How does your neural network’s performance compare to a traditional baseline (e.g., Logistic Regression or Random Forest), and what insights can you draw from this comparison?
🎯 Purpose: Encourages comparative thinking and understanding model trade-offs.
💡 Hint:
Train a classical ML model and compare F1, AUC, and confusion matrix.
Was the neural net better? If so, why (e.g., captured interactions)?
If not, consider whether your DL model is under-tuned or overfitting.


### 🔑 Q5: What did you log with MLflow (e.g., model configs, metrics, training duration), and how did this help you improve your modeling workflow?
🎯 Purpose: Tests reproducibility and tracking practice in a deep learning workflow.

💡 Hint:
Log architecture details (e.g., layer sizes, dropout rate, learning rate), metrics per epoch, and confusion matrix screenshots.
Explain how you used logs to choose the best model or compare runs.




This has been a great learning experience so far. I’m excited to keep improving the model and documenting more insights.

*– Cholpon Zhakshylykova*
