# **Gallstone Clinical Dataset — Binary Classification**

Gallstone disease is a common cause of abdominal pain, but diagnosis is often delayed because early symptoms are nonspecific and imaging resources are not always immediately available. A reliable ML model built from routine clinical and bioimpedance measurements can help **identify high-risk patients earlier**, prioritize diagnostic imaging, and reduce unnecessary referrals.

This project evaluates the Gallstone Clinical Dataset using a **statistically robust, reproducible machine-learning workflow**, focusing on model comparison, cross-validation stability, and feature-subset performance.

# ⭐ **Results Summary (10-Fold Stratified Cross-Validation)**

<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Model</th>
      <th>Accuracy Mean</th>
      <th>Accuracy Std</th>
      <th>Precision Mean</th>
      <th>Recall Mean</th>
      <th>F1 Mean</th>
      <th>AUC Mean</th>
      <th>AUC Std</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>SVM (Linear)</td>
      <td>0.808871</td>
      <td>0.087594</td>
      <td>0.840930</td>
      <td>0.760000</td>
      <td>0.794853</td>
      <td>0.882907</td>
      <td>0.060758</td>
    </tr>
    <tr>
  </tbody>
</table>
</div>

**Why SVM wins:**
The dataset is small (319 samples) and contains moderately correlated, mostly linear-separable clinical features. Under these conditions, **maximum-margin linear models** often generalize better than tree-based or neural models.

# **Dataset Citation**

> UCI Machine Learning Repository — *Gallstone Clinical Dataset*
> [https://archive.ics.uci.edu/dataset/1150/gallstone-1](https://archive.ics.uci.edu/dataset/1150/gallstone-1)


# **Introductory Paper**

> Esen I. et al., *Early prediction of gallstone disease with a machine learning-based method from bioimpedance and laboratory data*, 2025.
> [https://pubmed.ncbi.nlm.nih.gov/38394521/](https://pubmed.ncbi.nlm.nih.gov/38394521/)
# **Dataset Overview**

The Gallstone dataset contains:

* **319 patients**
* **38 clinical and bioimpedance features**
* **Binary label:**

  * `0` — gallstone disease
  * `1` — healthy

### **Why this dataset is challenging**

* Small sample size → prone to overfitting
* Mix of linear and weakly nonlinear relationships

# **Improvements Over the Original Study**

The original study evaluated models using a **single, non-stratified 70/30 train–test split** (The paper does not mention stratification), reporting the following performance for their best model (Random Forest):

| Model         | Precision | Recall | F1   | AUC  | Accuracy |
| ------------- | --------- | ------ | ---- | ---- | -------- |
| Random Forest | 0.91      | 0.81   | 0.86 | 0.85 | 0.8542   |


### **1️⃣ Reproducing Their Evaluation: Same Split Ratio, but Stratified for Fairness**

To make the comparison fair and avoid skewed class distributions, I reproduced their evaluation using a **stratified 70/30 split**. Under this comparable setup, my strongest model (XGBoost) achieved:

| Model      | Precision | Recall | F1   | AUC  | Accuracy |
| ---------- | --------- | ------ | ---- | ---- | -------- |
| XGBoost    | 0.836     | 0.854  | 0.845| 0.90 | 0.8437   |

**Interpretation:**

* Accuracy is similar (0.8437 vs. 0.8542)
* AUC is **higher** (0.90 vs. 0.85), indicating stronger class separability
* Recall and F1 are comparable

This already shows the dataset is **small and highly sensitive to the particular train/test split**.
A non-stratified split (as in the paper) can easily inflate metrics if the test set ends up easier.

### **2️⃣ Why Single-Split Evaluation Is Unreliable Here**

With only 319 patients, both non-stratified and stratified 70/30 splits suffer from major limitations:

* Test performance varies heavily depending on which patients fall into the test set
* A single split **cannot** provide a reliable measure of generalization
* Medical datasets often have overlapping feature distributions, further increasing variance
* This is visible in my reproduction: **Precision, recall, F1, and AUC shift significantly depending on the split**

This instability is why the original study’s reported accuracy (85.42%) is likely **optimistic**.

### **3️⃣ A More Reliable Evaluation: 10-Fold Stratified Cross-Validation**

To obtain a more trustworthy performance estimate, all models were evaluated using **10-fold stratified cross-validation**.

This yields:

* Much more stable accuracy and AUC estimates
* Lower variance
* Fairer comparison across models
* A realistic measure of generalization for a small dataset

**10-Fold CV Results:**

<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Model</th>
      <th>Accuracy Mean</th>
      <th>Accuracy Std</th>
      <th>Precision Mean</th>
      <th>Recall Mean</th>
      <th>F1 Mean</th>
      <th>AUC Mean</th>
      <th>AUC Std</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>SVM (Linear)</td>
      <td>0.808871</td>
      <td>0.087594</td>
      <td>0.840930</td>
      <td>0.760000</td>
      <td>0.794853</td>
      <td>0.882907</td>
      <td>0.060758</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Random Forest</td>
      <td>0.796169</td>
      <td>0.058272</td>
      <td>0.809883</td>
      <td>0.779583</td>
      <td>0.788645</td>
      <td>0.858825</td>
      <td>0.059220</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Logistic Regression</td>
      <td>0.783770</td>
      <td>0.089805</td>
      <td>0.826988</td>
      <td>0.715417</td>
      <td>0.763403</td>
      <td>0.860460</td>
      <td>0.064797</td>
    </tr>
    <tr>
      <th>4</th>
      <td>XGBoost</td>
      <td>0.783669</td>
      <td>0.055013</td>
      <td>0.802843</td>
      <td>0.773750</td>
      <td>0.778995</td>
      <td>0.857255</td>
      <td>0.056884</td>
    </tr>
    <tr>
      <th>5</th>
      <td>MLP Neural Network</td>
      <td>0.774496</td>
      <td>0.063210</td>
      <td>0.781460</td>
      <td>0.760417</td>
      <td>0.765661</td>
      <td>0.842521</td>
      <td>0.070628</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Gradient Boosting</td>
      <td>0.767843</td>
      <td>0.066589</td>
      <td>0.792643</td>
      <td>0.735833</td>
      <td>0.757129</td>
      <td>0.857914</td>
      <td>0.051992</td>
    </tr>
  </tbody>
</table>
</div>

### **Why K-Fold Accuracy Is Lower but AUC Remains Strong**

* Accuracy fluctuates heavily with small medical datasets
* AUC measures **ranking quality**, which is more stable
* So when using 10-fold CV:

  * **Accuracy drops slightly** (removing lucky splits)
  * **AUC stays high**, confirming genuine discriminative ability

This is the *expected* behavior when moving from a single split to a robust CV pipeline.

# **How to Use This Repository**

### **1. Install dependencies**

```bash
pip install -r requirements.txt
```

### **2. Run notebooks in order**

```
01_EDA.ipynb  
02_Baseline_Models.ipynb  
03_Cross_Validation.ipynb  
04_TopK_Features.ipynb
```

**Notebook overview:**

* **01_EDA** — variable profiles, correlations, distribution analysis
* **02_Baseline_Models** — stratified train/test comparison
* **03_Cross_Validation** — 10-fold stratified CV
* **04_TopK_Features** — impact of top predictors on SVM / Random Forest performance